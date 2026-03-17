---
title: "Virtualization"
weight: 3
---

# Virtualization

Running Vitruvian in a virtual machine is the recommended way to test during development without dedicated hardware.

## QEMU

QEMU with KVM gives the best performance on Linux hosts.

### Install QEMU

```bash
sudo apt install qemu-system-x86 qemu-kvm
```

### Boot a raw image

After [building]({{< relref "/docs/getting-started/building" >}}) or downloading a nightly image:

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

Adjust `-m` and `-smp` to match available host memory and CPU cores. Remove `-enable-kvm` if your host does not support KVM (performance will be lower).

### Boot an ISO image

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

## VirtualBox

VirtualBox works but has lower performance than QEMU/KVM on Linux hosts.

1. Create a new VM — type **Linux**, version **Linux 2.6 / 3.x / 4.x (64-bit)**
2. Assign at least 4 GB RAM
3. Attach the raw or ISO image as a storage device
4. Set display to **VMSVGA** with 3D acceleration disabled
5. Boot the VM

## Tips

- If the desktop doesn't appear, check the log output in your terminal for error messages from the servers.
- Shared folders and clipboard integration are not yet supported.
- For networking, the default user-mode NAT works for outbound connections; use port forwarding for inbound access.
