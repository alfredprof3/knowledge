#type/topic/Debian #topic/Debian/13-Trixie #for/Debian 

According to LastDragon video of how to [Install Debian 13 Trixie for Workstation use](https://www.youtube.com/watch?v=aiI_23UEIqc) the system partition starts the following rules.
# System Partition
- EFI partition: **300 MiB**
- Boot / Kernel partition: **2.0 GiB**
- Swap area: if you have 8 GiB of RAM, this means that swap has 17 GiB. **Needs to be the double + one.**
- Root partition: at least **100 GiB**
	- Select **BTRFS** as the file system management.
- Home partition: the rest of the storage.
	- Select **XFS** as the file system management.
# Modify the `/home` partition
