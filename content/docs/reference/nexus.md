---
title: "Nexus"
weight: 2
---

# Nexus

Nexus is the VitruvianOS kernel subsystem that bridges the Linux kernel with the BeOS/Haiku runtime. It is implemented as a set of custom Linux kernel modules loaded at boot.

## Purpose

BeOS and Haiku expose a set of kernel APIs that have no direct equivalent in Linux. Nexus implements these APIs as thin kernel modules, allowing the Vitruvian userspace — including the BeOS/Haiku API compatibility layer — to run on an otherwise standard Linux kernel.

## Components

### Node Monitor

The node monitor is the core of Nexus. It provides filesystem event notifications equivalent to the BeOS `node_monitor` API:

- `B_ENTRY_CREATED` — a new file or directory was created
- `B_ENTRY_REMOVED` — a file or directory was removed
- `B_ENTRY_MOVED` — a file or directory was renamed or moved
- `B_STAT_CHANGED` — file metadata (size, modification time, etc.) changed
- `B_ATTR_CHANGED` — an extended attribute was added, modified, or removed
- `B_DEVICE_MOUNTED` / `B_DEVICE_UNMOUNTED` — a volume was mounted or unmounted

Internally, Nexus hooks into Linux's `fsnotify` subsystem and translates events into BeOS-compatible messages delivered to registered listeners in userspace.

### Device and Volume Tracking

Nexus monitors mount and unmount events via the Linux kernel's filesystem notification infrastructure and exposes volume information through an API compatible with the BeOS `fs_info` / `device` calls. This allows Tracker, the Deskbar, and applications to enumerate mounted volumes and respond to changes in real time.

### Messaging Bridge

Events generated inside the kernel are routed to the VitruvianOS messaging subsystem in userspace. This allows standard `BLooper`/`BHandler` code to receive node monitor notifications exactly as it would on Haiku, with no API changes required.

## Source

Nexus lives under `src/system/kernel/nexus/` in the [VitruvianOS repository](https://github.com/VitruvianOS/Vitruvian).

## Further Reading

- [Node Monitoring — Haiku Book](https://www.haiku-os.org/docs/api/group__node__monitor.html)
- [Building VitruvianOS](../getting-started/building/)
