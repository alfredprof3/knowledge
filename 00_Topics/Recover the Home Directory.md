#type/HowTo #topic/Debian/Recover #for/Debian 

# How can I recover my files and my system after running rm -r /home/username?
If your account was new (without any important/personal files) just re-create your home:

> [!steps]
> 1. Log out current user from GUI
> 2. Switch to text console by pressing `Ctrl+Alt+F1`
> 3. Copy fresh user base files and change its ownership:
> 	```bash
> 	sudo mkhomedir_helper $USER
> 	```
> 	Or instead
> 	```bash
> 	sudo cp -r /etc/skel $HOME
> 	sudo chown `id -u`.`id -g` $HOME
> 	```
> 4. Log user again to GUI, your additional dirs will be recreated (`Desktop`, `Pictures` etc. - according to `/etc/xdg/user-dirs*`).

---
# Thread 2
Source: [How to recreate a user's home directory after it was accidentally deleted?](https://superuser.com/questions/1919440/how-to-recreate-a-users-home-directory-after-it-was-accidentally-deleted)

There's nothing special about the default structure. After you've copied the entirety of /etc/skel – and `chown`ed to the target user (if necessary) – all of the "GUI" special folders can just be created using `mkdir`.

```bash
mkdir ~/Desktop
mkdir ~/Documents
mkdir ~/Pictures
[etc.]
```

What makes them special in the GUI file manager is not a hidden attribute but being listed in `~/.config/user-dirs.dirs`, which your GUI environment will most likely pre-create with the standard locations if missing:

```bash
$ cat ~/.config/user-dirs.dirs
# This file is written by xdg-user-dirs-update
XDG_DESKTOP_DIR="$HOME/.local/share/Desktop"
XDG_DOCUMENTS_DIR="$HOME/Dropbox"
XDG_DOWNLOAD_DIR="$HOME/Downloads"
XDG_MUSIC_DIR="$HOME/Music"
XDG_PICTURES_DIR="$HOME/Dropbox/Pictures"
XDG_PUBLICSHARE_DIR="$HOME/.local/share/Public"
XDG_TEMPLATES_DIR="$HOME/.local/share/Templates"
XDG_VIDEOS_DIR="$HOME/Videos"
```

(The paths must start with either `/` or literally `$HOME/`. In my example I've deliberately moved Desktop and Templates somewhere away in order to reduce clutter, but you can keep them directly under $HOME if you want.)

I might also pre-create a few "standard" configuration directories like `~/.config` or `~/.cache`, just to make sure they have correct permissions (0700 i.e. private), but that's not required; programs will create them as needed.

```bash
mkdir -m 0700 ~/.config
mkdir -m 0700 ~/.cache
mkdir -m 0700 ~/.local
```

---

# Thread 3
Source: [Re-create Linux Home Directory for Existing Users (2026)](https://www.fosslinux.com/109397/creating-home-directory-for-existing-users-in-linux.htm)

> [!steps]
> 1. Step 1: Create the Target.
>    
>    Use `mkdir` to create the directory as root.
>    
> 	```bash
> 	sudo mkdir /home/testuser
> 	ls -lah /home/testuser
> 	```
> 	
> 	You will see something like this:
> 	
> 	```bash
> 	drwxr-x--- 2 root root 4096 Jun 15 18:32 /home/testuser
> 	```
> 	
> 1. Step 2: Populate from Skeleton.
>    
>    The `/etc/skel` directory contains the default profile files. Copy them recursively, including hidden dot files.
>    
> 	```bash
> 	sudo cp -r /etc/skel/. /home/testuser
> 	```
> 	
> 1. Step 3: Set Correct Ownership.
>    
>    The folder is currently owned by root. You must realign the ownership to the specific user.
>    
> 	```bash
> 	sudo chown -R testuser:testuser /home/testuser
> 	```
> 	
> 1. Step 4: Harden Permissions.
>    
>    I recommend `750` or `700`. This ensures privacy so other users on the system cannot read your files.
>    
> 	```bash
> 	sudo chmod 750 /home/testuser
> 	```
