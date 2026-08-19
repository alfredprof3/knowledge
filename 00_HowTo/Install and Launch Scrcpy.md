#notes/scrcpy

Source: [Genymobile - Scrcpy](https://github.com/Genymobile/scrcpy)
# 1. Install
## macOS

> [!steps]
> 1. First, Scrcpy is available in [Homebrew](https://brew.sh/):
> 	```bash
> 	brew install scrcpy
> 	```
> 2. You need `adb`, accessible from your `PATH`. If you don't have it yet:
> 	```bash
> 	brew install --cask android-platform-tools
> 	```
### Alternative

Alternatively, Scrcpy is also available in [MacPorts](https://www.macports.org/), which sets up `adb` for you:

```bash
sudo port install scrcpy
```

## Linux
### Option 1. From the official release

> [!steps]
> 1. Download a static build of the [latest release](https://github.com/Genymobile/scrcpy/releases/latest):
> 	```bash
> 	curl -L -O https://github.com/Genymobile/scrcpy/releases/download/v4.1/scrcpy-linux-x86_64-v4.1.tar.gz
> 	```
> 2. Extract it
> 	```bash
> 	tar -xvf scrcpy-linux-x86_64-v4.1.tar.gz
> 	```
> 3. Move it to the user bin `PATH`
> 	```bash
> 	mv -v scrcpy ~/.local/bin/
> 	```
### Option 2. From an install script

> [!steps]
> 1. First, you need to install the required packages:
> 	```bash
> 	sudo apt install wget gcc git ffmpeg \
> 	libsdl3-0 libusb-1.0-0 adb pkg-config \
> 	meson ninja-build libsdl3-dev libavcodec-dev \
> 	libavdevice-dev libavformat-dev libavutil-dev \
> 	libswresample-dev libusb-1.0-0-dev libv4l-dev
> 	```
> 2. Then clone the repo and execute the installation script ([source](https://github.com/Genymobile/scrcpy/blob/master/install_release.sh)):
> 	```bash
> 	git clone https://github.com/Genymobile/scrcpy.git
> 	cd scrcpy
> 	./install_release.sh
> 	```
> When a new release is out, update the repo and reinstall:
> 	```bash
> 	git pull
> 	./install_release.sh
> 	```

To uninstall:
```bash
sudo ninja -Cbuild-auto uninstall
```

# 2. Launch `scrcpy`

Once installed, run from a terminal:

```bash
scrcpy
```

Is recommended to use the flag with the value `--keyboard=uhid` to simulate a physical HID keyboard using the UHID kernel module on the device

```bash
scrcpy --keyboard=uhid
```

---
# Gemini AI

> [!human] You
> how to store the fingerprint of my android device to use scrcpy

> [!ai] Gemini
> To store the authorization fingerprint (RSA key) of your computer on your Android device for `scrcpy`, you need to permanently allow USB Debugging. This prevents the phone from asking for permission every time you connect.
> 
> # Enable Developer Options and USB Debugging
> 
> If you haven't already, enable developer options on your phone:
> 
> - Go to Settings > About phone.
> - Tap Build number seven times until you see a message saying "You are now a developer!".
> - Go back to Settings > System > Developer options.
> - Toggle USB debugging to ON
> 
> # Connect and Authorize (The "Fingerprint" Step)
> 
> - Connect your Android device to your computer using a USB cable.
> - Open your terminal/command prompt and run scrcpy (or adb devices).
> - Crucial Step: Watch your phone screen. A popup titled "Allow USB debugging?" will appear, showing your computer's RSA key fingerprint.
> - Check the box that says "Always allow from this computer".
> - Tap Allox.  
>     Once you check this box, your phone stores the computer's signature, enabling automatic connection in the future.
> 
> # Make the Authorization Persistent
> 
> To prevent authorization from being revoked if you restart your phone or disconnect it for a long time, change these settings in Developer Options:
> 
> - Disable "Revoke USB debugging authorizations": Do not click this option.
> - Enable "Disable adb authorization timeout": (If available on your device) This ensures the authorization doesn't expire.
> - Set "Default USB configuration" to "File Transfer" (MTP): Ensures a stable connection.
> 
> # Troubleshooting
> 
> If you are still prompted for authorization:
> 
> - Use the original cable: Some cables are for charging only, not data.
> - Restart ADB: In your terminal, run adb kill-server followed by adb devices.
> - Revoke and retry: Go to Developer options > Revoke USB debugging authorizations, then unplug and replug the phone to trigger the popup again.