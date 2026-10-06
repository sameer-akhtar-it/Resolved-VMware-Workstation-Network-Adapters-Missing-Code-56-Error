# Resolved: VMware Workstation Network Adapters Missing & Code 56 Driver Error

## Problem
After repairing or reinstalling VMware Workstation, the virtual network adapters (**VMnet1** and **VMnet8**) displayed yellow exclamation marks in Windows Device Manager with **Code 56** (*"Windows is still setting up the class configuration for this device"*)[cite: 2, 3]. Additionally, SSH/PuTTY connection attempts to the Linux virtual machine resulted in a `"Network error: Connection timed out"` error.

---

## Cause
1. **Windows Registry Class Lock:** A corrupted network class configuration registry entry was blocking Windows from binding the VMware virtual network drivers properly[cite: 3].
2. **Disabled VM Interface:** The wired network connection inside the Linux VM was turned OFF, preventing the VM from receiving an IP address from VMware's DHCP server.

---

## Solution

### Step 1: Clear the Corrupted Registry Entry
1. Press `Win + R`, type `regedit`, and press **Enter** (Run as Administrator).
2. Navigate to:
   ```text
   HKEY_LOCAL_MACHINE\SOFTWARE\Classes\CLSID\{3d09c1ca-2bcc-40b7-b9bb-3f3ec143a87b}
1. Right-click the {3d09c1ca-2bcc-40b7-b9bb-3f3ec143a87b} key and select Delete.
2. Restart the physical computer.

### Step 2: Restore VMware Virtual Network Adapters
1. Search for Virtual Network Editor in the Windows Start menu, right-click it, and select Run as Administrator.
2. Click Restore Defaults in the bottom-right corner to rebuild VMnet1 and VMnet8.
3. Verify in Device Manager (devmgmt.msc) that the network adapters are functioning properly without warning icons.

### Step 3: Enable Guest VM Network & Connect via PuTTY
1. Open the Linux VM console in VMware Workstation.
2. Go to Settings > Network inside Linux and turn ON the Wired Connection.
3. Set the connection to Connect automatically.
4. Open the terminal and run ip a to find the newly assigned IP address.
5. Launch PuTTY on the host machine and connect to the VM using the updated IP address on Port 22.
