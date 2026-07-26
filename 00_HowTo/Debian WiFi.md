#type/HowTo #topic/ifupdown-wpasupplicant #for/all 

These instructions require and make use of ifupdown, iproute2, wpasupplicant (For WPA2 support), iw and wireless-tools. Ensure you have al of these installed before continuing. You also might be interested in the instructions below that only use ifupdown and wpasupplicant, along with using a more advanced configuration.

Find your wirelessinterface and bring it up: (NOTE: wlp2s0 is an example, you will need to make sure to use the correct device name for your system)

```bash
ip a
```

```bash
iw dev
```

```bash
ip link set wlp2s0 up
```

Scan for available networks and get network details (If you already know your wifi network id/ESSID, you can skip this step):

```bash
su -l
```

```bash
iwlist scan | grep ESSID
```

Now edit `/etc/network/interfaces` The required configuration is much dependent on your particular setup. The following example will work for most commonly found WPA/WPA2 networks:

```bash
# My WiFi setup
allow-hotplug wlp2s0
iface wlp2s0 inet dhcp
	wpa-ssid ESSID
	wpa-psk PASSWORD
```

Bring up your interface and verify the connection:

```bash
ifup wlp2s0
```

```bash
iw wlp2s0 link
```

```bash
ip a
```

You can manually bring your interface up and down with the `ifup` and `ifdown` commands. If you added `allow-hotplug wlp2s0` as in the example above, the interface will be brought up automatically at boot.

For further information on available configurations options, see `man interfaces` , `man iw` , `man wireless` , and `/usr/share/doc/wireless-tools/README.Debian`