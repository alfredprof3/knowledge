#type/cheatsheet #topic/Timeshift #for/Debian 

Source: [How to Use Timeshift for System Snapshots on Ubuntu](https://oneuptime.com/blog/post/2026-01-15-use-timeshift-system-snapshots-ubuntu/view)

Timeshift is a system restore utility for Linux that takes incremental snapshots of system files. To manage timeshift via the termina, you must run all core commands with root privileges using `sudo`

- List all snapshots
```bash
sudo timeshift --list
```

- List a specific snapshot
```bash
sudo timeshift --list --snapshot-device /dev/sda1
```

- List storage devices
```bash
sudo timeshift --list-devices
```

- Create a manual snapshot
```bash
sudo timeshift --create --verbose --comments "My first snapshot"
```

- Create a snapshot specifying the storage
```bash
sudo timeshift --create --verbose --comments "My first snapshot" --snapshot-device /dev/sda1
```

- Create snapshot with specific tag
```bash
sudo timeshift --create --tags D   # Daily
sudo timeshift --create --tags W   # Weekly
sudo timeshift --create --tags M   # Monthly
sudo timeshift --create --tags O   #On-demand
```

- Restore a snapshot
```bash
sudo timeshift --restore
```

- Restore a specific snapshot from a specific device
```bash
sudo timeshift --restore --snapshot '2026-08-09_18-00-00' --target-device /dev/sda1
```

- Delete a snapshot a specific snapshot
```bash
sudo timeshift --delete --snapshot '2026-08-09_18-00-00'
```

- Delete all snapshot
```bash
sudo timeshift --delete-all
```

# Troubleshooting 1. Device: Not selected
This error occurs because Timeshift does not know which storage device or partition to use for its operations. On a clean Debian installation without a GUI, Timeshift cannot automatically guess or prompt you for a target drive until you explicitly declare it in the configuration.

You can fix this directly from you TTY by specifying your target backup device.
## 1. Identify Your Target Partition
First, you need to find the name of the partition where you want to store your snapshots (for example, `/dev/sda2` or `/dev/nvme0n1p2`). Run either of these commands to list your drives:
```bash
sudo timeshift --list-devices
lsblk
```
## 2. Force Timeshift to Select the Device
Once you know your target partition name, you must explicitly pass it to Timeshift using the `--snapshot-device` flag. Run the list command again while pointing directly to that device:
```bash
sudo timeshift --list --snapshot-device /dev/sdX1
```
_(Replace /dev/sdX1 with your actual partition name)._
## 3. Permanently Save the Device to Settings
To fix the error permanently so you don't have to type the device path every time, you need to update Timeshift's configuration file.