---
title: "macOS"
weight: 3
---

# Virtualization on macOS

## UTM (recommended)

[UTM](https://mac.getutm.app) is a native macOS virtualization frontend built on QEMU. It works on both Intel and Apple Silicon Macs and is the easiest way to run Vitruvian on macOS.

1. Download UTM from [mac.getutm.app](https://mac.getutm.app) or the Mac App Store
2. Click **+** to create a new VM
3. Select **Emulate** → **Linux**
4. Follow the setup wizard — assign at least 4 GB RAM and 2+ CPU cores
5. In the drive settings, attach the Vitruvian ISO or raw image
6. Set the display to **VirtIO GPU**
7. Start the VM

### Apple Silicon note

On Apple Silicon (M1, M2, M3…), Vitruvian runs under x86-64 emulation since there is no KVM-equivalent hardware acceleration for foreign architectures. Expect reduced performance compared to a native x86-64 host. ARM support in Vitruvian will change this in the future.

## QEMU via Homebrew

For more control over QEMU flags, install QEMU directly:

```bash
brew install qemu
```

### Boot a raw image (Intel Mac)

```bash
qemu-system-x86_64 \
  -accel hvf \
  -m 4096 \
  -smp 4 \
  -drive file=vitruvian.img,format=raw,if=virtio \
  -vga virtio \
  -display cocoa \
  -net nic,model=virtio \
  -net user \
  -rtc base=localtime
```

`-accel hvf` uses the macOS Hypervisor.framework for hardware acceleration on Intel. On Apple Silicon, replace with `-accel tcg,tb-size=1024` (software emulation).

### Boot an ISO

```bash
qemu-system-x86_64 \
  -accel hvf \
  -m 4096 \
  -smp 4 \
  -cdrom vitruvian.iso \
  -boot d \
  -vga virtio \
  -display cocoa \
  -net nic,model=virtio \
  -net user
```

## VirtualBox

VirtualBox supports macOS on Intel. It is not supported on Apple Silicon.

1. Download from [virtualbox.org](https://www.virtualbox.org)
2. Create a VM with type **Linux**, version **Linux 2.6 / 3.x / 4.x (64-bit)**
3. Assign 4 GB RAM, attach the image, set display to **VMSVGA** with 3D off
4. Boot

## Notes

- macOS may show a security warning the first time you run UTM or QEMU — allow it in System Settings → Privacy & Security.
- Shared folders and clipboard integration are not yet supported in Vitruvian guests.
