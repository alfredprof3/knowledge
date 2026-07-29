#type/HowTo/Configure #topic/Debian/TTY/Font-Size-Family #for/Debian 

Debian includes a built-in wizard to change the TTY font look and scale.
# Change TTY Font Family and Size
> [!steps]
> 1. Run the console setup utility:   
> 	```bash
> 	sudo dpkg-reconfigure console-setup
> 	```
> 2. **Encoding**: Select **UTF-8**.
>    - _Why:_ This is the universal standard. It perfectly supports English text, Spanish accents (like `ñ`, `á`), and regional currency symbols (like `$`).
> 3. **Character Set**: Select **Guess optimal character set** or **Combined - Latin; Slavic cyrillic; Hebrew; Arabic**.
>    - _Why:_ The "Combined" or standard "Latin" sets include all English characters, Spanish characters used in Mexico, and the standard dollar/peso sign (`$`).
> 4. **Font Family (The Style)**: Select your preferred look:
>     - **Terminus**: Highly recommended. Modern, sharp, and very readable at 1920x1080.
>     - **Fixed**: The standard Linux monospace console font.
>     - **VGA**: The classic, blocky DOS/BIOS style text.
> 5. **Font Size**: Choose a larger size so the text is not microscopic at 1080p:
>     - For **Terminus**, select **16x32** (large and extremely clear) or **14x28** (slightly smaller).
# Apply Changes and Reboot
The changes should apply immediately to your current TTY session. Restart your QEMU/KVM virtual machine to ensure the resolution and font load perfectly from the fresh boot:

```bash
sudo reboot -h now
```

# Crucial Note: Fixing Your Keyboard Layout
The `console-setup` tool only changes how text _looks_. Because you have a physical **Latin American keyboard**but use **English system settings**, your keys (like `@`, `-`, or `/`) might be mixed up in the TTY.

To fix your keyboard layout independently of your system language, run this separate command:

```bash
sudo dpkg-reconfigure keyboard-configuration
```

- **Keyboard model**: Choose the default **Generic 105-key PC**.
- **Keyboard layout**: Scroll down and select **Spanish**.
- **Specific variant**: Select **Spanish (Latin American)**.
- **Key to function as AltGr / Compose**: Choose the defaults (usually _Right Alt_ and _No compose key_).

After completing both menus, apply all changes immediately by running:

```bash
sudo systemctl restart console-setup
```
