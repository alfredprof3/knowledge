#type/HowTo/Configure #topic/Bluetooth/Bluetoothctl #for/Debian 

# Method 1. By leo-aa88 on Github
Source [Guide to Connecting to a Bluetooth Device using bluetoothctl](https://gist.github.com/leo-aa88/d082418f4da83b80701a8369f12f5f41)

This guide walks you through the process of connecting to a Bluetooth device on a Linux system using the `bluetoothctl`command-line tool. It covers initial setup, scanning, pairing, and connecting procedures, along with troubleshooting tips.
# Prerequisites
- Bluez
- Bluez-tools
- Bluez-firmware
- Bluetooth

```bash
sudo apt install bluez bluez-tools bluez-firmware bluetooth
```
# 1. Verify Bluetooth Service and Hardware
First ensure that the bluetooth service is running and your system recognized the Bluetooth adapter.
## A. Check Bluetooth Service Status
```bash
sudo systemctl status bluetooth
```
If service isn't active, start it:
```bash
sudo systemctl start bluetooth
```
Enable the service on boot:
```bash
sudo systemctl enable bluetooth
```
## B. List Available Bluetooth Controllers
```bash
sudo bluetoothctl list
```
This command should list one or more controllers. For example:
```
Controller 98:83:89:D8:70:52 archlinux [default]
```
If no controller is listed, verify hardware connections, enable bluetooth in BIOS/UEFI, and check for necessary drivers.
## C. Check for Soft/Hard Block
```bash
rfkill list bluetooth
```
If the output shows that Bluetooth is blocked, unblock it:
```bash
sudo rfkill unblock bluetooth
```
# 2. Initialize `bluetoothctl` and Power On the Adapter
Start the Bluetooth control tool and prepare your adapter for scanning and pairing.
## A. Launch `bluetoothctl`
```bash
sudo bluetoothctl
```
You will enter in the interactive prompt `[bluetooth]#`
## B. Select and Power On Your Controller
> [!steps]
> 1. List controllers:
>    
> 	```
> 	[bluetooth]# list
> 	```
> 	
> 1. Select the Default Controller (if not already selected)
> 	
> 	```
> 	[bluetooth]# select MAC-address
> 	```
> 	
> 	Replace `MAC-address` with your controller MAC-address, e.g. `98:83:89:D8:70:52`
> 
> 1. Power On the Controller
>    
> 	```
> 	[bluetooth]# power on
> 	```
> 	
> 	You should see a confirmation like `Changing power on succeded`
## C. Set Up Agent and Make Discoverable (Optional)
To handle pairing request:
```
[bluetooth]# agent on
[bluetooth]# default-agent
```
Optionally, make your device discoverable:
```
[bluetooth]# discoverable on
```
# 3. Scanning for Devices
Now that your adapter is powered on, start scanning for nearby Bluetooth devices.
## A. Start Scanning
```
[bluetooth]# scan on
```
The adapter will begin scanning and you will see output lines for every device discovered.
```
[NEW] Device 7D:2B:B5:6A:02:78 Device_name
```
## B. Stop Scanning (After Finding Devices)
When you have identified the device you want:
```
[bluetooth]# scan off
```
# 4. Pairing and Connecting to a Device
With the devices discovered, proceed to pair, trust and connect.
## A. List Discovered Devices
```
[bluetooth]# devices
```
## B. Select a Device
Choose the device you want to interact with:
```
[bluetooth]# select MAC-address
```
Replace `MAC-address` with the MAC address of the target device.
## C. Pair with the Device
```
[bluetooth]# pair MAC-address
```
Follow any on-screen prompts. Some devices may require a PIN confirmation or acceptance.
## D. Trust the Device
To allow automatic reconnections in the future:
```
[bluetooth]# trust MAC-address
```
## E. Connect to the Device
```
[bluetooth]# connect MAC-address
```
The device should now connect. If it's an audio device, additional configuration for audio services (like PulseAudio or BlueALSA) may be needed.
# 5. Additional Commands and Tips
## A. Get Device Information
To view details about a connected or known devices.
```
[bluetooth]# info MAC-address
```
## B. Remove a Device
If you want to remove a paired device from the list:
```
[bluetooth]# remove MAC-address
```
## C. Exiting `bluetoothctl`
When finished:
```
[bluetooth]# quit
```
# 6. Troubleshooting
If you encounter issues during the process:
## A. Check Adapter Status
Within `bluetoothctl` verify adapter details:
```
[bluetooth]# show
```
Confirm `Powered: yes` and appropriate discoverability settings.
## B. User `hciconfig` for Additional Information
```bash
hciconfig -a
```
Ensure the interface (e.g. `hci0`) is `UP` and `RUNNING` if not, bring up.
## C. Verify System Logs for Errors
```bash
sudo dmesg | grep -i bluetooth
```
Look for driver or hardware errors.
## D. Ensure System and Drivers are Up-to-Date
```bash
sudo apt update && sudo apt upgrade -y
```
Check for updated drivers or firmware for your specific bluetooth adapter.