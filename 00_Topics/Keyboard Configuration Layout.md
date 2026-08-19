#notes/Debian/Alacritty #topic/Debian/Keyboard/Configuration #for/Asus-Vivobook

> [!timeline-harr]
> # Debian Clean Installation NO GUI
> ## Installing DWM
> ### Alacritty, rofi, feh
> When I launch alacritty for the first time, the layout keyboard changed it. I run the command `sudo localectl set-keymap la-latin1`

# Permanent Change (System-Wide)
To update the layout permanently, use one of the options below based on your setup.
## Option 1: Using the Terminal (Recommended)
> [!steps]
> 1. Open your terminal
> 2. Run the configuration tool:
> 	```bash
> 	sudo dpkg-reconfigure keyboard-configuration
> 	```
> 3. Select your standard **Keyboard Model** (usually generic 105-key).
> 4. Choose **Other** in the country list.
> 5. Select **Spanish (Latin American)** from the layout list.
> 6. Keep the defaults for the remaining prompts (AltGr key, Compose key).
> 7. Restart the keyboard service to apply changes:
> 	```bash
> 	sudo systemctl restart keyboard-setup.serive
> 	```
## Option 2: Editing the Configuration File

> [!steps]
> 1. Open the configuration file:
> 	```bash
> 	sudo nvim /etc/default/keyboard
> 	```
> 2. Look for the `XKBLAYOUT` line and change it to:
> 	```bash
> 	XKBLAYOUT="latam"
> 	```
> 3. Save the file and exit `:x`
> 4. Force Debian to update its initial ramdisk so the settings apply immediately at the next boot:
> 	```bash
> 	sudo udevadm trigger --subsystem-match=input --action=change
> 	```

# ⚠️ The console uses `la-latin1` while X11 uses `latam`
