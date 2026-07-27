#type/HowTo #topic/gpu-pass-through-virt-manager #for/all 

To pass a dedicated GPU through to a virtual machine (VFIO/PCI passthrough), you must isolate the GPU from Debian host so the `vfio-pci` driver can claim it at boot.

Before starting, ensure you have a secondary GPU (like integrated Interl/AMD graphics or a second PCIe card) becuase the host will lose access to the paased-through GPU.

Here is the step-by-step configuration.
# 1. Enable IOMMU in the Bootloader
You must tell the Linux kernel to enable the IOMMU memory management unit.
> [!steps]
> 1. Open the GRUB configuration file:
> 	```bash
> 	sudo nvim /etc/default/grub
> 	```
> 2. Find the line starting with `GRUB_CMDLINE_LINUX_DEFAULT`
> 3. Append the correct parameter based on your CPU:
>    
>    - **Intel**: `intel_iommu=on iommu=pt`
>    - **AMD**: `amd_iommu=on iommu=pt`
>      
> 	_Example for Intel_
> 	
> 	`GRUB_CMDLINE_LINUX_DEFAULT="quiet splash intel_iommu=on iommu=pt"`
> 
> 1. Save and exit (`:x`)
> 2. Update GRUB to apply the changes:
> 	```bash
> 	sudo update-grub
> 	```

# 2. Identify GPU Hardware IDs
You need the vendor and device IDs of your dedicated graphics card and its bundled audio controller.
1. Run this command to list your GPU devices:
   
```bash
lspci -nn | grep -i vga
```
_Look for your dedicated GPU. It will output something like: `01:00.0 VGA compatible controller [...]: NVIDIA Corporation [10de:2484]`_
2. Run this command to find its matching audio controller (usually the next address):
   
```bash
lspci -nn | grep -i audio
```
_Look for the audio entry on the same bus: `01:00.1 Audio device [...]: NVIDIA Corporation [10de:228b]`_
3. Write down the ID numbers inside the brackets. In this example, they are `10de:2484` and `10de:228b`
# 3. Isolate the GPU via VFIO
Force Debian to bind these hardware IDs to the VFIO driver instead of your normal graphics driver.
> [!steps]
> 1. Create or edit the VFIO configuration file:
> 	```bash
> 	sudo nvim /etc/modprobe.d/vfio.conf
> 	```
> 2. Add your hardware IDs (separated by commas) to the file:
> 	```bash
> 	options vfio-pci ids=10de:2484,10de:228b
> 	```
> 3. Save and exit (`:x`)
> 4. Ensure VFIO modules load before your graphics drivers by editing the modules file:
> 	```bash
> 	sudo nvim /etc/initramfs-tools/modules
> 	```
> 5. Add these lines to the bottom:
> 	```bash
> 	vfio
> 	vfio_iommu_type1
> 	vfio_pci
> 	vfio_virqfd
> 	```
> 6. Save, exit and update your initramfs:
> 	```bash
> 	sudo update-iniramfs -u -k all
> 	```
> 7. Reboot your computer.
# 4. Verify the isolation
After rebooting, verify that the `vfio-pci` driver has successfully claimed the GPU.

```bash
lspci -nnk -d 10de:
```
_(Replace `10de:` with the first 4 characters of your specific hardware ID Vendor)_

Look at the line reading **"Kernel driver in use"**. It must say `vfio-pci`. If it still says `nvidia` or `amdgpu` the isolation failed.
# 5. Attach the GPU in Virt-Manager
Once isolated, adding the GPU to your VM takes just a few clicks.
> [!steps]
> 1. Open **Virt-Manager** and open your VM's settings.
> 2. Ensure the VM firmware is set to **UEFI** (OVMF), as modern GPUs require UEFI to pass through correctly.
> 3. Click **Add Hardware** at the bottom left.
> 4. Select **PCI Host Device**.
> 5. Find you GPU's VGA address (e.g., `0000:01:00.0`) and click **Finish**.
> 6. Click **Add Hardware** again, select **PCI Host Device**, and add the matching GPU Audio device (e.g., `0000:01:00.1`)
