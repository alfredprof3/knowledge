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
# Modify the `@rootfs` partition sub volume
## 1. Root partition
1. Verify the root partition is in your system with the following command.
```bash
df -Th
```
2. You can confirm that exists in your system if displayed the following information.
```bash
/dev/vda4    btrfs    100G  1.1G   45G   3%  /
```
Remember the `/` means the root directory.
3. Now, mount the root partition executing the next command.
```bash
sudo mount /dev/vda4 /mnt
```
Go to the `/mnt` directory to confirm the mounted.

```bash
cd /mnt
ls -lah
```
4. You will see `@rootfs` that has already mounted at `/mnt` Now rename the root directory.

```bash
sudo mv -v @rootfs/ @
```

Now, we need to fix the mount point in `/etc/fstab`

```bash
sudo nvim /etc/fstab
```
Search for `defaults,subvol=@rootfs`and replace as we did in the previous step (4).

```bash
defaults, subvol=@
```
Now we need to update the grub. Do it with the command below.

```bash
sudo update-grub
```
Be sure that the `grub.cfg` also has updated. Review for those lines where `@rootfs` → `@` Do a quick search with the next command.

```vim
:/@
```
Reboot the system to apply all changes.

```bash
sudo reboot -h now
```
