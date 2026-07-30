#type/HowTo #topic/Debian/Screenshot/Scrot #for/Debian 

# Taking Screenshots in Debian 13 with dwm
To take a screenshot in a Debian 13 clean install with only dwm installed, you can use the command-line tool scrot. First, install it by running `sudo apt install scrot`, then take a screenshot by executing `scrot` in the terminal.
# Installation Steps
> [!steps]
> 1. Open Terminal: Launch your terminal in dwm.
> 2. Install scrot: Run the following command to install scrot:
> 	```
> 	sudo apt install scrot
> 	```
# Taking a Screenshot
To take a full-screen screenshot, simply run the following command in the terminal:

```
scrot
```

Take a screenshot with a delay of `n` seconds. For example, three seconds

```bash
scrot -d 3
```

To take a screenshot of a specific window, use the following command:

```bash
scrot -u
```

To take a screenshot of a selected area, use the command:

```bash
scrot -s
```

To save on a specific image format and custom name:

```bash
scrot test_image.png
```

Take a screenshot and save it into a specific directory:

```bash
scrot -e 'mv $f ~/Pictures/'
```
# References
[Debian Linux Screenshots: A Comprehensive Guide](https://linuxvox.com/blog/debian-linux-screenshots/)
[Mastering Screenshots in the Linux Operating System](https://linuxvox.com/blog/linux-operating-system-screenshots/)
