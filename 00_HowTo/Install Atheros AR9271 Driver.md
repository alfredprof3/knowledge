#type/HowTo #topic/alfa-aus036ach-rtl8812au #for/all 

Driver: `ath9k_htc`

In the past I used to install the driver using the `aircrack-ng` Github repository, but now days a message shows that _THESE DRIVERS IS DEPRECATED_
They suggest to use https://github.com/lwfinger/rtw88
# Installation Guide
## Prerequisites 📋
Below are prerequisites for common Linux distributions **before** installing this driver.
### Ubuntu
```shell
sudo apt update && sudo apt upgrade
```

```shell
sudo apt install linux-headers-generic build-essential git
```
### Fedora
```shell
sudo dnf update
```

```shell
sudo dnf install kernel-devel git
```
### openSUSE
```shell
sudo zypper install make gcc kernel-devel kernel-default-devel git libopenssl-devel
```
### Arch
```shell
sudo pacman -Syu
```

```shell
sudo pacman -Sy base-devel git linux-firmware
```

Remember to install the corresponding [kernel headers](https://archlinux.org/packages/?q=Headers+and+scripts+for+building+modules) which are also needed for compilation.
### Raspberry Pi OS
```shell
sudo apt update && sudo apt upgrade
```

```shell
sudo apt install -y raspberrypi-kernel-headers build-essential git
```
# Installation Using DKMS 🔄
Installing this driver via DKMS is highly recommended, especially if Secure Boot is enabled on your machine. Using DKMS (Dynamic Kernel Module Support) ensures that the `rtw88` kernel modules are automatically rebuilt and re-signed whenever the Linux kernel is updated. Without DKMS, these drivers would stop working after each kernel update, requiring manual re-compilation and re-signing. DKMS should be available through your distribution’s package manager. You can learn more about DKMS [here](https://github.com/dell/dkms).

> [!steps]
> 1. Install `dkms` and all its required dependencies using your preferred package manager.
> 	```bash
> 	sudo apt install dkms
> 	```
> 2. Clone the `rtw88` Github repository.
> 	```bash
> 	git clone https://github.com/lwfinger/rtw88.git
> 	```
> 3. Build, sign, and install the `rtw88` driver.
> 	```bash
> 	cd rtw88
> 	sudo dkms install $PWD
> 	```
> 4. Install the firmware necessary for the `rtw88` driver.
> 	```bash
> 	sudo make install_fw
> 	```
> 5. Copy the configuration file `rtw88.conf` to `/etc/modprobe.d/`
> 	```bash
> 	sudo cp -v rtw88.conf /etc/modprobe.d/
> 	```
> 6. Enroll the MOK (Machine Owner Key), this is needed **ONLY IF SECURE BOOT IS ENABLED** on your machine.
> 	```bash
> 	sudo mokutil --import /var/lib/dkms/mok.pub
> 	```
> 7. For Ubuntu-based distro user, run this command instead.
> 	```bash
> 	sudo mokutil --import /var/lib/shim-signed/mok/MOK.der
> 	```

> [!box|info] Note
> At this point, you will be requested to enter a password. Remember this password and re-enter it after rebooting your system in order to enroll your new MOK into your system's UEFI. Please see [this tutorial](https://github.com/dell/dkms?tab=readme-ov-file#secure-boot) for more details.

# Installation Using make 🛠
You will need to rebuild and reinstall the driver manually after each kernel updates if you choose this way to install the driver. This method is **NOT RECOMMENDED** for systems with Secure Boot enabled.

```shell
git clone https://github.com/lwfinger/rtw88
```

```shell
cd rtw88
```

```shell
make
```

```shell
sudo make install
```

```shell
sudo make install_fw
```

```shell
sudo cp rtw88.conf /etc/modprobe.d/
```
