#type/Notes #topic/Debian/Notes #for/Debian 
# Issue. How to Set the Time Without Getting Reset?
> [!human] You
> The context.
> I'm doing a clean install of Debian—no graphical interface, no desktop environment, no window manager, and no X server—just pure TTY. So a few days ago, everything was working fine; the time was correct and matched my time zone, so it wasn't showing the wrong time. However, today when I tried to update my repositories, the terminal displayed an error stating that the repositories could not be updated due to a security certificate validation issue. When I saw this message, I realized it was because the time had become misconfigured, showing a different time—and even a different day and month. How can I set the date (time, day, and month) so that it doesn't get reset in the future? Why is this happening?

Welcome to the Linux ecosystem! Hitting an SSL certificate validation error because of a misconfigured system clock is a classic rite of passage when building a minimal, command-line-only system.

When you install a full desktop environment, graphical background services usually handle time synchronization silently. On a pure TTY Debian installation, you are the one in the driver's seat.

## Why is this happening?

There are three common culprits for this kind of time drift or complete reset:  

1. **The Dual-Boot Conflict:** If you are dual-booting with Windows, the two operating systems fight over the motherboard's hardware clock (the RTC). Windows assumes the hardware clock is set to your local time. Linux assumes it is set to Coordinated Universal Time (UTC). Every time you reboot from one to the other, the time gets shifted.
    
2. **Missing Network Sync (NTP):** Because you are on a bare-bones TTY install and likely managing your network connections manually, the service that automatically pulls the correct time from the internet (`systemd-timesyncd`) might be disabled, or it might be trying to sync before your Wi-Fi is fully connected.
    
3. **A Dying CMOS Battery:** If the year and month were completely wrong (like jumping back to 2010), the coin-cell battery on your motherboard that keeps the hardware clock ticking while the PC is turned off might be dead.

## How to set the time permanently

To fix your repository errors right now and ensure the clock stays accurate going forward, we will use `timedatectl`, which is systemd's built-in time management utility.

Because you cannot currently use `apt` to install anything due to the SSL errors, the order of these steps is critical.

> [!steps]
> 1. Disable automatic sync and manually force the time:
>    
>    _Required to fix SSL errors immediately so repositories work._
>    
>    If your time is drastically wrong, network time servers will sometimes reject the sync, and SSL certificates will fail. We need to manually put the time in the correct ballpark first.
> 
> 	```bash
> 	sudo timedatectl set-ntp false
> 	sudo timedatectl set-time "2026-08-13 18:45:00"
> 	```
> 	
> 	_(Adjust the time slightly if needed, using the `YYYY-MM-DD HH:MM:SS` format)._
> 
> 1. Set your exact timezone:
>    
>    Tell systemd exactly where you are so it can handle daylight saving changes and UTC offsets automatically.
> 
> 	```bash
> 	sudo timedatectl set-timezone America/Cancun
> 	```
> 
> 1. Enable Network Time Protocol (NTP):
>    
>    _This tells systemd to automatically sync time from internet servers._
>    
>    Now that the system time is close enough to reality, you can turn the automatic network sync back on.
> 
> 	```bash
> 	sudo timedatectl set-ntp true
> 	```
> 
> 1. Sync the System Time to the Hardware Clock:
>    
>    _Writes the correct Linux time back to your motherboard._
>    
>    Finally, ensure that your motherboard's internal clock matches the corrected system time, so it remembers it on your next boot.
> 
> 	```bash
> 	sudo hwclock --systohc
> 	```

You can verify that everything is running correctly by simply typing `timedatectl status`. You should see `System clock synchronized: yes` and `NTP service: active`. Your `apt update` command will now work without security certificate complaints

---

> [!human] You
> I'm facing two issues.
> 
> First, I want to let you know that when I run the command `sudo hwclock --systohc`, I get a terminal message saying "command not found." I ran the command `apt search hwclock` and noticed that "fake-hwclock" appears; the repository description for it says the following: "Save/restore system clock on machines without working RTC hardware." I proceeded to install the repository by running the command `sudo apt install fake-hwclock`, intending to run the command `sudo hwclock --systohc` again, but I got the same response: "command not found."
> 
> Second, when I run the command `timedatectl status`, "no" appears under `System clock synchronized`, while "active" appears under `NTP service`.
> 
> What should I fix or install? How can I fix the "command not found" error? 

Finding the "wrong" package because it has a similar name is a very normal part of the Linux learning curve! You are doing exactly what you should be doing: searching repositories, reading descriptions, and testing solutions.

Here is the breakdown of what is happening under the hood for both issues, and how to fix them.

## Issue 1: Why `hwclock` is missing and what `fake-hwclock` actually does

In recent versions of Debian (12 and 13), the developers made the base installation even smaller by splitting up some standard tools. The `hwclock` command was moved into a package called `util-linux-extra`.  

The package you installed, `fake-hwclock`, is a clever workaround designed specifically for single-board computers (like a Raspberry Pi) that literally do not have a physical hardware clock (RTC) or a coin battery on the motherboard. Instead of writing to hardware, it writes the current time to a text file on shutdown and reads it back on boot. Because you are on a machine with a real hardware clock, this package will conflict with your system, so we need to remove it.

## Issue 2: Why NTP is "Active" but not "Synchronized"

When `timedatectl` shows `NTP service: active`, it means the background service responsible for syncing the time (`systemd-timesyncd`) is currently running. However, `System clock synchronized: no` means it hasn't successfully received a response from an internet time server.

This usually happens because the service started up and tried to check the time _before_ your command-line Wi-Fi/network was fully connected, or it simply got stuck in a loop after we forced the manual time jump earlier.

Here is the sequence to clean this up, install the correct tools, and force the network sync to trigger.

> [!steps]
> 1. Remove the fake-hwclock package:
>    
>    _Prevents conflicts with your physical hardware clock._
>    
>    First, we will completely remove `fake-hwclock` and its configuration files using the `purge` command.
>    
> 	```bash
> 	sudo apt purge fake-hwclock
> 	```
> 
> 2. Install util-linux-extra:
>    
>    _This package contains the actual hwclock command._
>    
>    Now, install the correct package that contains standard hardware clock utilities.
> 
> 	```bash
> 	sudo apt install util-linux-extra
> 	```
> 
> 3. Restart the timesyncd service:
>    
>    _Forces the service to try contacting time servers again._
>    
>    Since we know your internet is working (because `apt install` worked), restarting the time service will force it to reach out to the network immediately.
> 
> 	```bash
> 	sudo systemctl restart systemd-timesyncd
> 	```
> 
> 4. Verify the service status:
>    
>    _Checking the logs is a crucial Linux troubleshooting skill._
>    
>    Instead of just checking `timedatectl`, let's look directly at the service's logs to see exactly what it is doing.
>    
> 	```bash
> 	systemctl status systemd-timesyncd
> 	```

When you run that last command, look at the bottom few lines of the output. You should see a log message that says something like: `Initial synchronization to time server [IP address]`. If you see that, the time is officially locked in!

You can then press `q` to exit the status view, and safely run `sudo hwclock --systohc` to save your now-perfect time to the motherboard

---

Excellent troubleshooting! You just experienced one of the core realities of building a minimal Linux system from scratch: the OS assumes nothing and leaves every choice up to you. Reading the error, recognizing a missing piece, searching the repositories, and verifying the service status is the exact workflow of a seasoned system administrator.

**Why wasn't it there to begin with?**

In older versions of Debian, `systemd-timesyncd` was bundled directly into the core system. However, starting with Debian 12 and continuing into your Trixie install, the developers unbundled it into a separate package. They did this because many enterprise servers prefer more advanced time-sync tools (like `chrony` or `ntpsec`). On a pure TTY bare-bones installation, Debian simply leaves it out entirely, waiting for you to decide which tool you want for the job.

By installing it manually, you successfully patched that missing link. Your time is synced, your SSL certificates will validate properly, and `apt` is fully functional.

Now that your foundational system is stable, securely connected, and updating properly, your canvas is ready for the next layer—whether that involves locking down your command-line networking or starting to build out a lightweight, custom graphical environment with a window manager and terminal emulator.