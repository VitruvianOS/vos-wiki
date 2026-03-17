---
title: "Windows"
weight: 2
---

# Virtualization on Windows

## VirtualBox (recommended for most users)

VirtualBox is the easiest option on Windows and works on all hardware.

1. Download and install from [virtualbox.org](https://www.virtualbox.org)
2. Create a new VM:
   - Type: **Linux**, version **Linux 2.6 / 3.x / 4.x (64-bit)**
   - RAM: 4 GB or more
   - Disk: skip (attach the image manually)
3. In VM settings → Storage, attach the Vitruvian raw image or ISO as a storage device
4. In VM settings → Display, set adapter to **VMSVGA**, disable 3D acceleration
5. Boot the VM

## QEMU on Windows

QEMU runs on Windows and provides better performance than VirtualBox when used with WHPX (Windows Hypervisor Platform) or HAXM acceleration.

### Install

Download the QEMU installer from [qemu.org](https://www.qemu.org/download/#windows) or install via [winget](https://winget.run):

```powershell
winget install qemu
```

### Enable Windows Hypervisor Platform

In PowerShell (as Administrator):

```powershell
Enable-WindowsOptionalFeature -Online -FeatureName HypervisorPlatform
```

Reboot after enabling.

### Boot a raw image

```powershell
qemu-system-x86_64.exe `
  -accel whpx `
  -m 4096 `
  -smp 4 `
  -drive file=vitruvian.img,format=raw,if=virtio `
  -vga virtio `
  -net nic,model=virtio `
  -net user `
  -rtc base=localtime
```

Replace `-accel whpx` with `-accel haxm` if you have HAXM installed, or remove it entirely to fall back to software emulation (much slower).

### Boot an ISO

```powershell
qemu-system-x86_64.exe `
  -accel whpx `
  -m 4096 `
  -smp 4 `
  -cdrom vitruvian.iso `
  -boot d `
  -vga virtio `
  -net nic,model=virtio `
  -net user
```

## Notes

- Hyper-V and VirtualBox cannot run simultaneously on the same host. If you use WSL2 or Docker Desktop, Hyper-V is likely already active — use QEMU with WHPX in that case.
- Vitruvian has not been tested under WSL2 and is not expected to work there.
