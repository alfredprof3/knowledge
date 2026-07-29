#type/HowTo/Configure #topic/Debian/TTY/Screen-Resolution/Virt-Manager #for/Debian 

To set the screen resolution for a GUI-less TTY console in Debian 13 running under QEMU/KVM, you must ==configure the **GRUB bootloader to pass the desired resolution to the Linux kernel frame buffer**==.
# First, update your GRUB settings to use your target resolution
> [!steps]
> 1. Open the configuration file with root privileges.   
> 	```bash
> 	sudo nano /etc/default/grub
> 	```
> 2. Update the following lines to match **1920x1080**:
> 	```bash
> 	GRUB_CMDLINE_LINUX_DEFAULT="quiet video=1024x768"
> 	GRUB_GFXMODE=1920x1080
> 	GRUB_GFXPAYLOAD_LINUX=keep
> 	```    
> 3. Save and exit (`:x`), then update GRUB:
> 	```bash
> 	sudo update-grub
> 	```
> 4. Restart your virtual machine to initialize the new frame buffer resolution:
> 	```bash
> 	sudo reboot -h now
> 	```

# Troubleshooting Virtual Display Drivers
If the resolution does not change, your `virt-manager` video device settings might be overriding it.

- Open **Virt-Manager**, click the **i (Information)** icon, and go to the **Video** hardware section.
- Ensure the model is set to **Virtio** or **QXL** (Virtio is highly recommended for modern Debian guests).