---
title: "How To Install"
weight: 2
---

# How To Install

## From a Nightly Image

Nightly builds are available via [GitHub Actions](https://github.com/VitruvianOS/Vitruvian/actions). Download the latest successful build artifact.

### Write to a USB drive

Replace `/dev/sdX` with your actual device (check with `lsblk`):

```bash
sudo dd if=vitruvian.img of=/dev/sdX bs=4M status=progress oflag=sync
```

{{< hint warning >}}
**Double-check the target device.** `dd` will overwrite it completely.
{{< /hint >}}

Boot from the USB drive. Most systems require selecting the boot device from the firmware menu (usually F12, F11, or Escape at startup).

### Write to a USB drive on macOS

```bash
diskutil list           # find your disk, e.g. /dev/disk2
diskutil unmountDisk /dev/disk2
sudo dd if=vitruvian.img of=/dev/rdisk2 bs=4m
```

### Write to a USB drive on Windows

Use [Rufus](https://rufus.ie) in **DD image** mode.

## From a Live ISO

If a live ISO is provided, boot it directly from USB using the same method above, or load it in a [virtual machine]({{< relref "/docs/getting-started/virtualization" >}}).

## Build from Source

If you prefer to build from source rather than using a nightly image, see the [Building]({{< relref "/docs/getting-started/building" >}}) page.

## First Boot

On first boot the Vitruvian desktop will load automatically. If it does not, check for error output in the console — the system is experimental and some hardware configurations may require manual troubleshooting.

For help, join the [Telegram chat](https://t.me/vitruvian_official_chat) or open an issue on [GitHub](https://github.com/VitruvianOS/Vitruvian/issues).
