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

1. Open your network configuration file using a text editor:
```bash
sudo nvim /etc7network/interfaces
```
   2. Locate your RTL8812AU wireless interface section (usually labeled `wlan0` or similar)
   3. Add the line `dns-nameservers 8.8.8.8 1.1.1.1` directly under your network settings. It should look like this:
```
iface wlan0 inet dhcp
	wpa-ssid "Your_WiFi_Name"
	wpa-psk "Your_WiFi_Password"
	dns-nameservers 8.8.8.8 1.1.1.1
```
4. Save the file by pressing `:x`
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
## The Game Changer: Using `wpa_cli`
Because you added `update_config=1` to your configuration file, **you never have to manually edit a text file to add a new network again.**

When you sit down at a new location with a headless setup, you can use the interactive `wpa_cli` tool to scan, connect, and save the network. Just type `sudo wpa_cli` to enter the interactive prompt, and follow this workflow:

1. `scan` (Tells the adapter to look for networks)    
2. `scan_results` (Prints a list of networks in the room)
3. `add_network` (This will output a new network ID number, e.g., `1` or `2`. Use this number for the next steps).
4. `set_network 1 ssid "New_Location_WiFi"`
5. `set_network 1 psk "their_password"`
6. `enable_network 1` (The daemon will now attempt to connect)
7. `save_config` (This permanently writes the new network to your `wpa_supplicant.conf` file!)

Type `quit` to exit. Your system will now automatically remember and connect to this location next time you visit.