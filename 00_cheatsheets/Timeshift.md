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

