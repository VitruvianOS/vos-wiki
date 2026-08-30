---
title: "Linux"
weight: 1
---

# Virtualization on Linux

## QEMU with KVM

QEMU with KVM gives near-native performance on Linux hosts and is the primary tested environment.

### Install

```bash
sudo apt install qemu-system-x86 qemu-kvm
```

Check that KVM is available:

```bash
kvm-ok
```

If KVM is not available, QEMU will still work but will run slower without hardware acceleration.

### Boot a raw image

```bash
qemu-system-x86_64 \
  -cdrom vitruvian.img -boot menu=on \
  -m 8G -cpu host -smp sockets=1,cores=2,threads=2 --enable-kvm \
  -netdev user,id=mynet,hostfwd=tcp::2222-:22 \
  -device virtio-net-pci,netdev=mynet \
  -audiodev id=snd0,driver=pa -device ich9-intel-hda  \
  -device hda-output,audiodev=snd0
```

Adjust `-m` (RAM in MB) and `-smp` (CPU cores) to your host. Remove `-enable-kvm` if KVM is unavailable.

### Boot an ISO

```bash
qemu-system-x86_64 \
  -cdrom vitruvian.iso -boot menu=on \
  -m 8G -cpu host -smp sockets=1,cores=2,threads=2 --enable-kvm \
  -netdev user,id=mynet,hostfwd=tcp::2222-:22 \
  -device virtio-net-pci,netdev=mynet \
  -audiodev id=snd0,driver=pa -device ich9-intel-hda  \
  -device hda-output,audiodev=snd0
```

### Useful flags

| Flag | Purpose |
|------|---------|
| `-display gtk` | Use a GTK window instead of SDL |
| `-display spice-app` | Use SPICE for better clipboard and display integration |
| `-cpu host` | Expose host CPU features to the guest (better compatibility) |
| `-snapshot` | Run without writing changes back to the image |
| `-serial stdio` | Print serial output to the terminal |
