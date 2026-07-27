#type/HowTo #topic/libvirt-kvm-qemu #for/all 

To install QEMU using Virt Manager in a clean installation of Debian use the following commands.
# 1. Verify Hardware Virtualization
Before installing anything, verify that your computer's CPU supports hardware virtualization and that it is enabled in your BIOS/UEFI

```bash
egrep -c '(vmx|svm)' /proc/cpuinfo
```

If the output is 1 or greater, hardware virtualization is supported.
If it returns 0, you must restart your machine, enter your BIOS/UEFI settings, and enable Intel VT-x or AMD-V / SVM Mode.
# 2. Update the system
Log in to your terminal and sync your local repository index with the upstream servers to get the latest package pointers.

```bash
sudo apt update && sudo apt upgrade -y
```
# 3. Install QEMU, Libvirt and Virt-Manager
Run the following consolidated command to install the emulator, management daemons, network bridging structures and the graphical manager GUI.

```bash
sudo apt install bridge-utils qemu-kvm qemu-system qemy-system-x86 qemu-utils libvirt-clients libvirt-daemon-system virtinst virt-manager
```

- `qemu-system-x86` & `qemu-utils` The core emulation engine and disk imaging utilities.
- `libvirt-daemons-system` & `libvirt-clients` The background service and command-line interfaces (virsh) used to managed VMs.
- `bridge-utils` Helps construct virtual switches so you VMs can talk to the router network.
- `virt-manager` The user-friendly graphical interface.
- `ovmf` Enables modern UEFI firmware support inside your virtual machines.
# 4. Configure Services and Virtual Network
Ensure virtualization daemon is running and configure it to boot automatically when Debian starts

```bash
sudo systemctl enable --now libvirtd
```

```bash
sudo systemctl start libvirtd
```

To prevent networking issues or DHCP delivery errors inside your guest operating systems, activate the default NAT network and set it to autostart.

```bash
sudo virsh net-start default
```

```bash
sudo virsh net-autostart default
```
# 5. Assign User Permissions
By default, only the `root` user can directly control hypervisor environments. Grant your normal user account seamless access without requiring `sudo` prompts every time you launch the GUI.

```bash
sudo usermod -aG libvirt $USER
```

```bash
sudo usermod -aG kvm $USER
```

To apply these newly configured security privileges, completely log out of your Debian desktop environment and log back in, or run a full system reboot.

```bash
sudo reboot -h now
```
# 6. Launch Virt-Manager
Once the system boots back up, you can start the application via your desktop environment's application launcher by searching for Virtual Machine Manager, or by running the following command in your terminal.

```bash
virt-manager &
```
# Create ISO and VM directories
To store and stay things organized, create the following directories. `ISO`folder will store all your iso file for the Operation System (OS) like Windows, Debian, Ubuntu, etc.

```bash
mkdir -p /home/$USER/ISO
```

To store your Virtual Machines (VMs) create the `VM` folder.

```bash
mkdir -p /home/$USER/VM
```
# Download the ISO file
To download via command line in the terminal, you can use `wget` or `curl`

- Download ISO using `wget`
```bash
wget https://cdimage.debian.org/debian-cd/current/amd64/iso-cd/debian-13.6.0-amd64-netinst.iso
```

- Downloads ISO using `curl`
```bash
curl https://cdimage.debian.org/debian-cd/current/amd64/iso-cd/debian-13.6.0-amd64-netinst.iso
```
