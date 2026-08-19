#type/HowTo #topic/Debian/WiFi/ifupdown-wpasupplicant #for/all 

Source: [How to use a WiFi interface](https://wiki.debian.org/WiFi/HowToUse)

These instructions require and make use of ifupdown, iproute2, wpasupplicant (For WPA2 support), iw and wireless-tools. Ensure you have al of these installed before continuing. You also might be interested in the instructions below that only use ifupdown and wpasupplicant, along with using a more advanced configuration.

Find your wirelessinterface and bring it up: (NOTE: wlp2s0 is an example, you will need to make sure to use the correct device name for your system)

```bash
ip a
```

```bash
iw dev
```

```bash
ip link set wlp2s0 up
```

Scan for available networks and get network details (If you already know your wifi network id/ESSID, you can skip this step):

```bash
su -l
```

```bash
iwlist scan | grep ESSID
```

Now edit `/etc/network/interfaces` The required configuration is much dependent on your particular setup. The following example will work for most commonly found WPA/WPA2 networks:

```bash
# My WiFi setup
allow-hotplug wlp2s0
iface wlp2s0 inet dhcp
	wpa-ssid ESSID
	wpa-psk PASSWORD
```

Bring up your interface and verify the connection:

```bash
ifup wlp2s0
```

```bash
iw wlp2s0 link
```

```bash
ip a
```

You can manually bring your interface up and down with the `ifup` and `ifdown` commands. If you added `allow-hotplug wlp2s0` as in the example above, the interface will be brought up automatically at boot.

For further information on available configurations options, see `man interfaces` , `man iw` , `man wireless` , and `/usr/share/doc/wireless-tools/README.Debian`

# Troubleshooting 1. Temporary failure in name resolution.
The error "**Temporary failure in name resolution**" means your Debian system cannot translate domain names into IP addresses because its **DNS settings are missing or misconfigured** in `/etc/resolv.conf`

Because you configured your interface manually using `/etc/network/interfaces` you bypassed automatic network managers (like NetworkManager) that usually handle DNS generation automatically.
## 1. Fix Your Configuration Immediately (Temporary Fix)
You can manually overwrite your local resolver file to point directly to a public DNS provider like Google DNS or Cloudflare. Open your terminal and run.
```bash
echo -e "nameserver 8.8.8.8\nnameserver 1.1.1.1" | sudo tee /etc/resolv.conf
```
- **Test your connection:** Run `ping google.com` right after running this command to confirm internet access works.
- _Note: This change will be erased the next time your reboot or restart your network interface._
## 2. Make the DNS Settings Permanent (Recommended Fix)
To prevent your settings from disappearing on every reboot, you must hardcore the DNS nameservers directly into the configuration file you used to set up the connection.

> [!steps]
> 1. Open your network configuration file using a text editor:
> 	```bash
> 	sudo nvim /etc/network/interfaces
> 	```
> 2. Locate your RTL8812AU wireless interface section (usually labeled `wlan0` or similar)
> 3. Add the line `dns-nameservers 8.8.8.8 1.1.1.1` directly under your network settings. It should look like this:
> 	```
> 	iface wlan0 inet dhcp
> 		wpa-ssid "Your_WiFi_Name"
> 		wpa-psk "Your_WiFi_Password"
> 		dns-nameservers 8.8.8.8 1.1.1.1
> 	```
> 4. Save the file by pressing `:x`

## 3. Apply the Changes Securely

> [!steps]
> 1. Bring down your specific wireless interface to clear old states:
> 	```bash
> 	sudo ifdown wlan0
> 	```
> 2. Bring the interface back up to load the new settings:
> 	```bash
> 	sudo ifup wlan0
> 	```
> 3. Verify your system properly built the local resolver links:
> 	```bash
> 	cat /etc/resolv.conf
> 	```
# Roaming feature built into `wpa_supplicant`
Right now, you are likely defining your network directly in `/etc/network/interfaces` or using the `wpa-conf` directive. By switching to the `wpa-roam` directive, you can separate your IP configuration from your Wi-Fi authentication. This allows a background daemon to scan for known networks, automatically authenticate when one is in range, and assign the correct IP address.

> [!steps]
> 1. Edit your interfaces files
>    
>    _Switching to roaming mode_
>    
>    Open `/etc/network/interfaces` with your text editor. You need to strip out any specific SSIDs, passwords, or `wpa-conf` lines. Instead, set the wireless interface to `manual` and point it to your `wpa_supplicant` file using `wpa-roam`.
>    You also must add a `default` fallback interface so it knows to request a DHCP address when it connects to a new network.
>    
> 	```bash
> 	# Replace wlan0 with your actual interface name if different
> 	allow-hotplug wlan0
> 	iface wlan0 inet manual
> 	    wpa-roam /etc/wpa_supplicant/wpa_supplicant.conf
> 	
> 	# The fallback interface for any standard DHCP network
> 	iface default inet dhcp
> 	```
> 	
> 1. Configure wpa_supplicant.conf
>    
>    _Enabling dynamic updates_
>    
>    Next, open `/etc/wpa_supplicant/wpa_supplicant.conf`.
>    For the roaming daemon to work properly—and to allow you to add networks via the command line later—you **must** include the `ctrl_interface` and `update_config=1` lines at the very top.
>    
> 	```bash
> 	ctrl_interface=DIR=/run/wpa_supplicant GROUP=netdev
> 	update_config=1
> 	
> 	# Your home network
> 	network={
> 	    ssid="Home_Network"
> 	    psk="your_home_password"
> 	    id_str="home"
> 	}
> 	
> 	# A saved cafe network
> 	network={
> 	    ssid="Cafe_Network"
> 	    psk="cafe_password"
> 	}
> 	```
> 	
> 	_Note for security:_ Make sure this file is protected from other users by running `sudo chmod 600 /etc/wpa_supplicant/wpa_supplicant.conf`.
> 	
> 1. Restart the wireless interface
>    
>    _Apply the new daemonized setup_
>    
>    Flush your old configuration and bring the interface back up in roaming mode:
>    
> 	```bash
> 	sudo ifdown wlan0
> 	sudo ifup wlan0
> 	```

From now on, whenever `wpa_supplicant` detects "Home_Network" or "Cafe_Network", it will connect automatically and ask the `default` configuration for a DHCP IP address.
# How to Connect to a New WiFi
## The Game Changer: Using `wpa_cli`
Because you added `update_config=1` to your configuration file, **you never have to manually edit a text file to add a new network again.**

When you sit down at a new location with a headless setup, you can use the interactive `wpa_cli` tool to scan, connect, and save the network. Just type `sudo wpa_cli` to enter the interactive prompt, and follow this workflow:

1. `scan` (Tells the adapter to look for networks).
2. `scan_results` (Prints a list of networks in the room).
3. `add_network` (This will output a new network ID number, e.g., `1` or `2`. Use this number for the next steps).
4. `set_network 1 ssid "New_Location_WiFi"`
5. `set_network 1 psk "their_password"`
6. `enable_network 1` (The daemon will now attempt to connect).
7. `reconnect` (Only if it wasn't possible to connect to the WiFi, try it again).
8. `status` (Just to be sure, display network information).
9. `save_config` (This permanently writes the new network to your `wpa_supplicant.conf` file!).

Type `quit` to exit. Your system will now automatically remember and connect to this location next time you visit.
# How do I set up the same automatic connection process, but using any wireless network adapter?
When you switch between different USB Wi-Fi adapters, the configuration logic remains the same, but the system treats the hardware very differently. The issue you are facing almost certainly comes down to one of two things: **interface naming** or **missing firmware**.

Here is how to troubleshoot and configure your Alfa AWUS036NHA or any other USB WiFi adapter.

> [!steps]
> 1. Find the new interface name
>    _Linux likely didn't name this adapter wlan0_
>    Modern Linux systems use "Predictable Network Interface Names." Because your Alfa AWUS036NHA has a different MAC address than your AWUS036ACH, your system likely assigned it a different name (such as `wlan1`, or something starting with `wlx` followed by its MAC address).
>    Plug in the AWUS036NHA and run:
> 	```bash
> 	ip link
> 	```
>    Look for a wireless interface that is _not_ `wlan0`. Once you find its name (for example, `wlx00c0ca...`), you need to update your `/etc/network/interfaces` file to match it exactly:
> 	```bash
> 	# Replace wlx... with your actual interface name
> 	allow-hotplug wlx...
> 	iface wlx... inet manual
> 	    wpa-roam /etc/wpa_supplicant/wpa_supplicant.conf
> 	
> 	iface default inet dhcp
> 	```
> 1. Install the Atheros firmware or any other firmware.
>    _The NHA uses a different chipset than the ACH_
>    Your previous Alfa AWUS036ACH uses a Realtek chipset, but the AWUS036NHA uses the Atheros AR9271 chipset. Debian's kernel has the open-source driver for this (`ath9k_htc`), but it requires proprietary firmware to function, which is not always installed by default in minimal environments.
> 	```bash
> 	sudo apt update
> 	sudo apt install firmware-atheros
> 	```
>    After the installation completes, **unplug the AWUS036NHA and plug it back in**. This forces the kernel to reload the driver and inject the newly installed firmware (`htc_9271.fw`). 
>    **Tip:** You can verify the firmware loaded successfully by running `dmesg | grep -i firmware` after plugging it in.
> 2. Bring the interface up
>    _Apply the new configuration_
>    Now that the interface name is correct in your configuration and the firmware is loaded, start the interface using the name you found in Step 1:
> 	```bash
> 	sudo ifup
> 	```
>    Give it a few seconds, then check if it successfully grabbed an IP address from your home network
> 	```bash
> 	ip a show
> 	```

Because the `wpa_supplicant.conf` file is independent of the hardware, all the networks you saved with `wpa_cli` using your old adapter will immediately work with the new one once the interface is properly brought up.
# Troubleshooting 2. Can't connect (reason 1)
If the adapter is showing up when you run `ip link`, the kernel _has_ successfully detected the hardware and assigned a driver to it. The fact that `dmesg | grep -i firmware` didn't return anything likely means the log buffer has simply overwritten those earlier boot messages, or the `ath9k_htc` driver logged the firmware load using different phrasing (like "Transferred FW").

Since the system sees the adapter, the hardware and drivers are communicating. The problem is occurring in the software layer—most likely, a stuck background process from your previous Wi-Fi card is blocking the new one, or a silent software lock has disabled the radio.

Here is exactly how to diagnose and bypass the blockage:

> [!steps]
> 1. Kill stuck wpa_supplicant processes
>    
>    _Clear the old state_
>    When you switch adapters and use `ifdown` / `ifup`, the `wpa_supplicant` background daemon sometimes fails to terminate properly. If a ghost process is still running for your old Alfa card, the new one cannot authenticate.
>    Kill any lingering Wi-Fi processes:
> 	```bash
> 	sudo killall wpa_supplicant
> 	```
> 1. Unblock with rfkill
>    
>    _Check for silent radio blocks_
>    Debian sometimes automatically "soft-blocks" new wireless MAC addresses as a security or power-saving measure. Check if your radio is disabled:
> 	```bash
> 	sudo rfkill list all
> 	```
>    If you see `Soft blocked: yes` next to your wireless LAN, you must unblock it:
> 	```bash
> 	sudo rfkill unblock wifi
> 	```
> 1. Manually bring the interface UP
>    
>    _Test the hardware state_
>    Before relying on `/etc/network/interfaces` to do the work, verify that the adapter can actually be powered on by the system. Run this command (replace `wlx...` with your actual interface name):
> 	```bash
> 	sudo ip link set wlx... up
> 	```
>    If this command throws an error like `SIOCSIFFLAGS: Operation not possible due to RF-kill` or `firmware failed to load`, we immediately know the hardware or firmware is the culprit. If it returns nothing (a silent success), the adapter is fully ready to connect.
> 2. Run wpa_supplicant in the foreground
>    
>    _The ultimate diagnostic tool_
>    If the interface is up but still won't connect via `ifup`, we need to see exactly what `wpa_supplicant` is complaining about. Running it manually in "debug" mode will print the connection process directly to your terminal.
> 	```bash
> 	sudo wpa_supplicant -i wlx... -c /etc/wpa_supplicant/wpa_supplicant.conf -d
> 	```
>    Watch the output. It will tell you exactly why it's failing. Look for lines mentioning `CTRL-EVENT-DISCONNECTED`, `reason=`, or password/key mismatches.

---


To answer your question: `id_str` stands for **Identifier String**.

Its entire purpose is to act as a bridge between your Wi-Fi authentication (`wpa_supplicant.conf`) and your IP configuration (`/etc/network/interfaces`).

Here is exactly how it works and whether you need it for your other networks.

# How `id_str` Works

When your computer connects to a Wi-Fi network, `wpa_supplicant` handles the password. Once it successfully authenticates, it hands control back to the `wpa-roam` process and says, _"I connected! What kind of IP address should I get?"_

- **If there is NO `id_str`:** `wpa-roam` looks in your `/etc/network/interfaces` file for the **`default`** mapping. Since you have `iface default inet dhcp` in your file, it simply asks the router for a standard dynamic IP address.
- **If there IS an `id_str`:** `wpa-roam` looks in your interfaces file for a mapping that exactly matches that specific string name.    
# When does it make a difference?

You only need to add an `id_str` if you want a **custom network configuration** for that specific location (like assigning yourself a Static IP address, or using custom DNS servers).

For example, if you wanted your laptop to always have the exact same IP address on your home network so you can SSH into it easily, the `id_str="home"` allows you to add this block to your `/etc/network/interfaces` file:

```bash
# This configuration only applies when connected to a network with id_str="home"
iface home inet static
    address 192.168.1.50
    netmask 255.255.255.0
    gateway 192.168.1.1
```

If you then go to the "CafeInternet", `wpa_supplicant` connects, sees there is no `id_str`, and automatically falls back to your `iface default inet dhcp` rule, protecting you from IP conflicts on public networks.

# Should you add it to the other networks?

**No, you don't need to.**

Unless you specifically want to configure a static IP or custom routing rules for your 2.4GHz network or the Cafe network, you can safely leave `id_str` out of their configuration blocks. The system will gracefully fall back to requesting a standard DHCP address for all of them.