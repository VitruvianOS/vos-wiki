---
title: "Windows"
weight: 2
---

# Virtualization on Windows

Welcome to the official guide to launching Vitruvian on Windows operating systems.

# System Requirements

Before proceeding, please ensure that your PC meets the following minimum requirements:  
**Operating System:** Windows 10 or Windows 11 (64-bit).  
**Processor:** Intel i5 / AMD Ryzen 5 or higher.  
**RAM:** At least 8 GB (16 GB recommended).  
**Additional Software:** QEMU (Link to download: https://qemu.weilnetz.de/w64/qemu-w64-setup-20260811.exe).  

# Post-Installation Steps
**1. Add QEMU to Windows Environment Variables (PATH)**  
### To allow Windows and Vitruvian to run QEMU commands from any terminal window, you must add it to your system PATH:  
- Press the **Windows Key**, type `env`, and select **Edit the system environment variables**.  
- Click on the **Environment Variables...** button at the bottom right.  
- In the **System variables** section (bottom list), find and select the variable named **Path**, then click **Edit...**.
- Click New and paste the default installation path for QEMU: `C:\Program Files\qemu`
- Click **OK** on all windows to save and apply the changes.

**2. Verify the Installation**  
Open a new **Command Prompt** (**cmd**) or **PowerShell** window and type the following command to verify that QEMU is correctly recognized:  
  
```powershell
qemu-img.exe --version
```  
  
Expected result:  
  
```powershell
qemu-img version 11.1.0 (v11.1.0-12130-ge470268ff4`  
Copyright (c) 2003-2026 Fabrice Bellard and the QEMU Project developers
```  

# 3. Launching Vitruvian  
  
### Boot a raw image  
  
```powershell
qemu-system-x86_64.exe `
-cdrom vitruvian.img -boot menu=on `
-m 8G -cpu host -smp sockets=1,cores=2,threads=2 `
-netdev user,id=mynet,hostfwd=tcp::2222-:22 `
-device virtio-net-pci,netdev=mynet
```

### Boot an ISO  
  
```powershell
qemu-system-x86_64.exe `
-cdrom vitruvian.iso -boot menu=on `
-m 8G -cpu host -smp sockets=1,cores=2,threads=2 `
-netdev user,id=mynet,hostfwd=tcp::2222-:22 `
-device virtio-net-pci,netdev=mynet
```
