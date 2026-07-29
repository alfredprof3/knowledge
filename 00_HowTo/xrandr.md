#type/HowTo #topic/xrandr/configuration/screen-resolution-CLI #for/all 

To configure the resolution for a monitor using `xrandr` (usually in a virtual-kvm qemu), follow the steps below.
> [!steps]
> 1. Create the file
> 	```bash
> 	sudo nvim /etc/X11/xorg.conf.d/Virtual-1.conf
> 	```
> 2. Add the syntax
> 	```bash
> 	Section "Monitor"
> 	    Identifier "VGA-1"
> 	    Modeline "1368x768-60.00" 85.25 1368 1440 1576 1784 768 771 781 798 -HSync +VSync
> 	    Option "PreferredMode" "1368x768-60.00"
> 	EndSection
> 	```
> 3. Save, exit and reload the configuration.
# Option 2.
We do so by going into terminal and typing “gtf x y r” where x is the horizontal resolution, y is the vertical resolution and r is the refresh rate (which is largely irrelevant since LCDs are the norm). So for example, mine was:

```
gtf 1024 768 85
```

Once you’ve executed the command you’ll be presented with something like this

```
abhishek@abhishek:~$ gtf 1024 768 85

  # 1024x768 @ 85.00 Hz (GTF) hsync: 68.60 kHz; pclk: 94.39 MHz
  Modeline "1024x768_85.00"  94.39  1024 1088 1200 1376  768 769 772 807  -HSync +Vsync

abhishek@abhishek:~$
```

We’re only interested in the second half, so make a note of everything from modeline onwards.

2. We need to find the display interface name

In the terminal type: xrandr This will give you something along the lines of:

```
Screen 0: minimum 320 x 200, current 1024 x 768, maximum 8192 x 8192
VGA-1 connected primary 1024x768+0+0 (normal left inverted right x axis y axis) 0mm x 0mm
   1024x768      60.00*  
   800x600       60.32    56.25  
   848x480       60.00  
   640x480       59.94  
DVI-D-1 disconnected (normal left inverted right x axis y axis)
abhishek@abhishek:~$
```

The display interface name is the bit before ‘connected’ so in this case ‘VGA-1’. Make a note of yours.

3. Creating the 10-monitor.conf

In order to create our spangly new resolution we need to create `/usr/share/X11/xorg.conf.d/10-monitor.conf` So in the terminal run:

```
sudo vi /usr/share/X11/xorg.conf.d/10-monitor.conf
```

This will open a blank text file, into which you want to paste the following:

```
Section "Monitor"
  Identifier "Monitor0"
  <INSERT MODELINE HERE>
EndSection
Section "Screen"
  Identifier "Screen0"
  Device "<INSERT DEVICE HERE>"
  Monitor "Monitor0"
  DefaultDepth 24
  SubSection "Display"
    Depth 24
    Modes "<INSERT MODENAME HERE>"
  EndSubSection
EndSection
```

The modename is the bit in quotes (so "1024x768_85" in our earlier example). You can add additional resolutions that already exist in the list xandr shows just by putting them in quotes and adding them to the end of the modes line.

So for reference, mine looks like this:

```
Section "Monitor"
  Identifier "Monitor0"
  Modeline "1024x768_85.00"  94.39  1024 1088 1200 1376  768 769 772 807  -HSync +Vsync
EndSection
Section "Screen"
  Identifier "Screen0"
  Device "VGA-1"
  Monitor "Monitor0"
  DefaultDepth 24
  SubSection "Display"
    Depth 24
    Modes "1024x768_85.00"
  EndSubSection
EndSection
```

And you’re done!

Once you’ve saved 10-monitor.conf in /usr/share/X11/xorg.conf.d/, restart your computer and you should have your brand new resolution available and set as default.

---

If you get a black screen on restarting, don’t panic, it probably means a typo or other syntax error of some description. While the computer’s on, hit ctrl+alt+F1 to go into a terminal and run:

```
sudo rm /usr/share/X11/xorg.conf.d/10-monitor.conf
```

Then restart and you’ll be back to defaults! Hope this saves someone some time and hair-pulling!