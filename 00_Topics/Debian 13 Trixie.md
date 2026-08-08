#type/topic/Debian #topic/Debian/13-Trixie #for/Debian 

According to LastDragon video of how to [Install Debian 13 Trixie for Workstation use](https://www.youtube.com/watch?v=aiI_23UEIqc) the system partition starts the following rules.
# System Partition
- EFI partition: **300 MiB**
- Boot / Kernel partition: **2.0 GiB**
- Swap area: if you have 8 GiB of RAM, this means that swap has 17 GiB. **Needs to be the double + one.**
- Root partition: at least **100 GiB**
	- Select **BTRFS** as the file system management.
- Home partition: the rest of the storage.
	- Select **XFS** as the file system management.
# Modify the `@rootfs` partition sub volume
## 1. Mount and configure the `/root` partition
> [!steps]
> 1. Verify the root partition is in your system with the following command.
> 	```bash
> 	df -Th
> 	```
> 2. You can confirm that exists in your system if displayed the following information.
> 	```bash
> 	/dev/vda4    btrfs    100G  1.1G   45G   3%  /
> 	```
> Remember the `/` means the root directory.
> 3. Now, mount the root partition executing the next command.
> 	```bash
> 	sudo mount /dev/vda4 /mnt
> 	```
> Go to the `/mnt` directory to confirm the mounted.
> 	```bash
> 	cd /mnt
> 	ls -lah
> 	```
> 4. You will see `@rootfs` that has already mounted at `/mnt` Now rename the root directory.
> 	```bash
> 	sudo mv -v @rootfs/ @
> 	```
> 5. Now, we need to fix the mount point in `/etc/fstab`
> 	```bash
> 	sudo nvim /etc/fstab
> 	```
> 6. Search for `defaults,subvol=@rootfs`and replace as we did in the previous step (4).
> 	```bash
> 	defaults, subvol=@
> 	```
> 7. Now we need to update the grub. Do it with the command below.
> 	```bash
> 	sudo update-grub
> 	```
> 8. Be sure that `grub.cfg` is also updated in the path `/boot/grub/grub.cfg`. Search for those lines where `@rootfs` → `@` Do a quick search with the next command.
> 	```vim
> 	:/@
> 	```
> 9. Reboot the system to apply all changes.
> 	```bash
> 	sudo reboot -h now
> 	```
# Gnome for a low resources machine
Source: [Debian 11 + Gnome en una maquina de bajos recursos](https://www.youtube.com/watch?v=aXgECPQYxL0)

This configuration applies to a clean installation of Debian. If you want to keep resource usage to a minimum while still using a desktop environment (GNOME), run the following command.

```bash
sudo apt install gnome-session gnome-shell gnome-background gnome-applets gnome-control-center mutter gjs gnome-terminal
```

If you want another terminal emulator, replace `gnome-terminal` for `alacritty` or `kitty` you can install them via `sudo apt install`
# DWM setup
Source: [JustAGuyLinux](https://codeberg.org/justaguylinux)

A script that helps you how to install i3w

[dwm-setup](https://codeberg.org/justaguylinux/dwm-setup)
<iframe src="https://codeberg.org/justaguylinux/dwm-setup" title="dwm-setup" width="100%" height="800px" scrolling="no" frameborder="no" allow="fullscreen"></iframe>
# i3-wm by IT'S FOSS
Source [The Ultimate Guide to i3 Customization in Linux](https://itsfoss.com/i3-customization/)

How to customize i3 in Debian.

<iframe src="https://itsfoss.com/i3-customization/" width="100%" height="800px" scrolling="no" frameborder="no" allow="fullscreen"></iframe>

# Sway setup
Source: [JustAGuyLinux](https://codeberg.org/justaguylinux)

How to setup Sway in Debian.

<iframe src="https://codeberg.org/justaguylinux/sway-setup" width="100%" height="800px" scrolling="no" frameborder="no" allow="fullscreen"></iframe>
