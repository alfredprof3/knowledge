#type/HowTo #topic/Alacritty/Keyboard/Configuration-latam-layout #for/all 

# Option 1.
I don't test it at all but I think it works. 28-07-2026, Tuesday

[Debian Language Settings](https://www.compilenrun.com/docs/os/debian/debian-customization/debian-language-settings/)
<iframe src="https://www.compilenrun.com/docs/os/debian/debian-customization/debian-language-settings/" title="Debian Language Settings" width="100%" height="800px" scrolling="no" frameborder="no" allow="fullscreen"></iframe>

---

# Option 2. Duck Duck Search Assist AI
> [!human] You
> how to set my keyboard layout to latin american in debian

> [!ai] Duck Duck Go Search Assist
> To set your keyboard layout to Latin American in Debian, install the keyboard-configuration package if needed, then run sudo dpkg-reconfigure keyboard-configuration, choose "Generic 105-key PC" (or your keyboard model), select "Spanish" and then "Latin American" when prompted, and apply with sudo service keyboard-setup restart.
> ## Setting Keyboard Layout to Latin American in Debian
> 
> To configure your keyboard layout to Latin American in Debian, follow these steps:
> 
> ### Step 1: Install Required Package
> 
> Ensure that the `keyboard-configuration` package is installed. If it is not installed, you can do so by running:
> 
> bash
> 
> ```
> sudo apt install keyboard-configuration
> ```
> 
> ### Step 2: Reconfigure Keyboard Settings
> 
> Run the following command to start the keyboard configuration process:
> 
> bash
> 
> ```
> sudo dpkg-reconfigure keyboard-configuration
> ```
> 
> ### Step 3: Select Keyboard Model
> 
> When prompted, choose your keyboard model. If unsure, select:
> 
> - Generic 105-key PC for most keyboards.
> 
> ### Step 4: Choose Language and Layout
> 
> Next, select the following options:
> 
> - Country of origin: Choose Spanish.
> - Keyboard layout: Select Latin American.
> 
> ### Step 5: Apply Changes
> 
> To apply the new keyboard settings, restart the keyboard setup service with:
> 
> bash
> 
> ```
> sudo service keyboard-setup restart
> ```
> 
> ### Additional Notes
> 
> - If you encounter issues, you can manually edit the configuration file located at `/etc/default/keyboard`.
> - After making changes, you may need to reboot your system for all settings to take effect.
> 
> By following these steps, your keyboard layout should now be set to Latin American in Debian.

