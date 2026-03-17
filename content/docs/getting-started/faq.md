---
title: "FAQ"
weight: 4
---

# FAQ

---

## What is VitruvianOS?

VitruvianOS (V\OS) is an operating system based on Linux, inspired by BeOS. It runs the BeOS/Haiku desktop and API on top of a real-time patched Linux kernel, with custom kernel modules — collectively called **Nexus** — providing the missing BeOS-style system services.

See the [Nexus reference page]({{< relref "/docs/reference/nexus" >}}) for details on the kernel bridge.

---

## Is it ready to use?

The project is in active development and experimental. Core components (Deskbar, Tracker, app_server, registrar) run, and BeOS/Haiku application source code compiles and runs with minimal changes. Expect rough edges.

---

## How is it different from Haiku?

Haiku is a complete from-scratch reimplementation of BeOS that runs its own kernel. Vitruvian runs on the Linux kernel instead, which means it inherits Linux's hardware support, driver ecosystem, and security model. The goal is full BeOS/Haiku API compatibility on top of that foundation — the best of both worlds.

---

## How mature is the API compatibility?

The core kits (Application, Interface, Storage, Support) are functional enough to run meaningful BeOS/Haiku applications with little or no source changes. Some kits are further along than others. The [API Changes]({{< relref "/docs/development/api-changes" >}}) page tracks known gaps and deviations from the BeOS/Haiku API.

---

## Is there a package manager?

Yes — Vitruvian ships as a Debian-derived image, so **apt** is available for installing packages from Debian repositories alongside native Vitruvian software.

---

## What filesystems are supported for booting?

The reference boot filesystems are **XFS** and **SquashFS**, both with full extended attribute support:

- **XFS** is the reference filesystem for standard desktop installs
- **SquashFS** is the reference filesystem for live images, live CDs, and embedded targets

Vitruvian will also boot from **ext4** and most other Linux-supported filesystems that provide extended attribute support, but XFS and SquashFS are the primary tested and recommended options. Even without attributes, the system should be able to boot, albeit with limited capabilities.

---

## Will it run in a virtual machine?

Yes. Running in QEMU is the recommended way to test during development. See [Virtualization]({{< relref "/docs/getting-started/virtualization" >}}) for platform-specific setup guides.

---

## What architectures are supported?

Vitruvian targets x86-64, ARM, and RISC-V. x86-64 is the primary development platform; ARM and RISC-V support is actively being built.

---

## Can I contribute without being a kernel developer?

Absolutely. There is useful work at every level — porting or fixing BeOS/Haiku applications, improving the desktop experience, writing documentation, testing on real hardware and filing issues, or helping with the website and wiki. Jump in on [Telegram](https://t.me/vitruvian_official_chat) and ask where help is most needed.

---

## What is Nexus?

Nexus is the set of custom Linux kernel modules that bridge Linux with the BeOS/Haiku runtime. It provides node monitoring (filesystem event notifications), device and volume tracking, and routes kernel events to the BeOS/Haiku messaging subsystem in userspace. See the [Nexus]({{< relref "/docs/reference/nexus" >}}) page.

---

## How do I report a bug?

Open an issue on [GitHub](https://github.com/VitruvianOS/Vitruvian/issues). Include relevant log output and a clear description of what you were doing and what you expected to happen.

---

## Can I donate?

Yes — see the [Donate]({{< relref "/docs/reference/donate" >}}) page. Every bit helps keep the project moving.

---

## Can I send hardware to the developers?

Hardware donations for testing and development are very welcome. Particularly useful:

- **ARM single-board computers** — Raspberry Pi 4/5, Orange Pi, Rock Pi, and similar
- **RISC-V boards** — any development board running a recent Linux kernel
- **Laptops** — not-so-old x86-64 laptops (ThinkPads, Dell, HP from the last ~6 years work well) with varied GPU and wireless hardware
- **Desktops or workstations** with unusual or niche GPU/NIC combinations that are hard to test remotely

Reach out on [Telegram](https://t.me/vitruvian_official_chat) first to coordinate. We will confirm a shipping address directly.

---

## Where can I get help?

- [Telegram chat](https://t.me/vitruvian_official_chat) — best place for quick questions
- [GitHub Issues](https://github.com/VitruvianOS/Vitruvian/issues) — bugs and feature requests
- [Mailing list](https://www.freelists.org/list/vitruvian) — broader discussions
