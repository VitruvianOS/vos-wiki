---
title: "Linux"
weight: 1
---

# Virtualization on Linux

## QEMU with KVM (recommended)

QEMU with KVM gives near-native performance on Linux hosts and is the primary tested environment.

### Install

```bash
sudo apt install qemu-system-x86 qemu-kvm virt-manager
```

Check that KVM is available:

```bash
kvm-ok
```

If KVM is not available, QEMU will still work but will run slower without hardware acceleration.

### Boot a raw image

```bash
qemu-system-x86_64 \
  -enable-kvm \
  -m 4096 \
  -smp 4 \
  -drive file=vitruvian.img,format=raw,if=virtio \
  -vga virtio \
  -display sdl \
  -net nic,model=virtio \
  -net user \
  -rtc base=localtime
```

Adjust `-m` (RAM in MB) and `-smp` (CPU cores) to your host. Remove `-enable-kvm` if KVM is unavailable.

### Boot an ISO

```bash
qemu-system-x86_64 \
  -enable-kvm \
  -m 4096 \
  -smp 4 \
  -cdrom vitruvian.iso \
  -boot d \
  -vga virtio \
  -display sdl \
  -net nic,model=virtio \
  -net user
```

### Useful flags

| Flag | Purpose |
|------|---------|
| `-display gtk` | Use a GTK window instead of SDL |
| `-display spice-app` | Use SPICE for better clipboard and display integration |
| `-cpu host` | Expose host CPU features to the guest (better compatibility) |
| `-snapshot` | Run without writing changes back to the image |
| `-serial stdio` | Print serial output to the terminal |

### GNOME Boxes

GNOME Boxes provides a simple GUI over QEMU/KVM. It works but offers less control over VM parameters. Useful for quick tests.

## VirtualBox

Install from [virtualbox.org](https://www.virtualbox.org) or your distribution's package manager. Create a new VM with:

- Type: **Linux**, version **Linux 2.6 / 3.x / 4.x (64-bit)**
- RAM: 4 GB or more
- Display: **VMSVGA**, 3D acceleration **off**
- Storage: attach the raw or ISO image

VirtualBox is slower than QEMU/KVM on Linux and is less well-tested with Vitruvian.
