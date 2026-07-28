#type/HowTo #topic/ath9k_htc_awus036nha-rtl8812au_awus036ach #for/all 

To pass through your Alfa network cards, the method depends entirely on ho wthe connect to your motherboard.

Both the **Atheros AR9271** and the **Realtek RTL8812AU** chips are **USB-based chipsets**. This makes the process much simpler than a GPU passthrough, as you do not need to edit GRUB boot arguments or isolate IOMMU groups. You can pass them directly as USB host devices.

Here is how to pass them through using Virt-Manager.
# Step 1. Connect the Card to your Host
> [!steps]
> 1. Plug your Alfa USB Wi-Fi card into a physical USB port on your Debian machine.
> 2. Open a terminal and run the following command to verify the system sees it:
> 	```bash
> 	lsusb
> 	```
> 3. Look for your card in the list.
>    - The **Atheros AR9271** will typically show up as `Atheros Communications Inc. AR9271 802.11n`.
>    - The **Realtek 8812AU** will typically show up as `Realtek Semiconductor Corp. RTL8812AU 801.11ac Wireless Network Adapter`.

# Step 2. Attach the Card in Virt-Manager
Because these are USB devices, you can attarch and detach them while the Virtual Machine is running (Hot-plugging).
> [!steps]
> 1. Open **Virt-Manager** and start your Virtual Machine.
> 2. Open the VM details windows (click the **lightbulb icon** 💡 or go to _View > Details_).
> 3. Click the **Add Hardware** button at the bottom left.
> 4. Select **USB Host Device** from the left panel.
> 5. Look through the list of active USB devices, select your Alfa card (Atheros or Realtek), and click **Finish**.

The guest operating system will immediatly detect the USB devices as if it were plugged directly into a physical machine.
## ⚠️ Important Guest OS Considerations
Depending on what you plan to do inside the virtual machine, keep these chipset quirks in mind:
### 1. Atheros AR9271 (Alfa AWUS036NHA)
- **Drivers**: This chip uses the open-source `ath9k_htc` driver. It is natively supported in the Linux kernel.
- **Best Use**: If your guest VM is Kali Linux or another penetration testing distro, it will work instantly out of the box for **monitor mode** and **packet injection**. No driver installation is needed.
### 2. Realtek RTL8812AU (Alfa AWUS036ACH / AC1900)
- **Drivers**: This chip is **not** natively supported by the Linux kernel.
- **Action Required**: If your guest VM is Linux, you will need to manually install the `rtl8812au` DKMS drivers inside the virtual machine before the OS can see any Wi-Fi networks.
- **USB 3.0 controller**: Becuase this is a high-speed 802.11ac card, ensure your VM has a **USB 3.0 (xHCI)** controller added to its virtual hardware layout in Virt-Manager, otherwise, your speeds will be severely throttled.

_Note: If you boot the VM while the Alfa card is unplugged, the VM will still start normally, but it will print a warning in the logs. Once you plug the card back into the host, you will have to manually attarch it for that session via the **Virtual Machine > Redirect USB device** menu._
# 🛠️ Critical Optimizations for Wireless Auditing (Pentesting)
Since you are setting up a wireless hacking lab, you need to configure two specific settings in Virt-Manager to avoid packets drops or driver crashes during injection.
## 1. Upgrade the Virtual USB Controller to USB 3.0
By default, Virt-Manager might assign an older USB 2.0 controller to the VM. While the Atheros card will handle this fine, the **Realtek 8812AU** will bottleneck, drop connections or fail to switch into monitor mode.
- In the VM hardware details 💡 click on **Controller USB**.
- Change the **Model** drop-down menu to **USB 3.0 (eXtensible Host Controller Interface - xHCI)**.
- Click **Apply**.
## 2. Blacklist the Alfa Drivers on the Debian Host (Highly Recommended)
If your Debian host operating system tries to claim the Alfa card at the exact time the VM tries to boot, a race condition occurs. This can cause the VM boot to freeze, or cause the card to fail when switching to monitor mode (`airmon-ng start`).
To prevent the host from touching the cards, blacklist their drivers on you Debian host:
> [!steps]
> 1. Open a terminal on your **Debian Host** (not the VM).
> 2. Create a wireless lab blacklist file:
> 	```bash
> 	sudo nvim /etc/modprobe.d/wireless-lab-blacklist.conf
> 	```
> 3. Add the kernel modules for both chips to the file:
> 	```bash
> 	# Blacklist Atheros AR9271 on host
> 	blacklist ath9k_htc
> 	
> 	# Blacklist Realtek RTL8812AU on host
> 	blakclist rtl8812au
> 	blacklist 8812au
> 	```
> 4. Save and exit (`:x`)

Now, your Debian host will completely ignore the cards when plugged in, leaving them perfectly clean and isolated for QEMU to claim immediately upon boot.