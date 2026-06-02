---
title: "FAQ"
weight: 4
---

# FAQ

## What is Vitruvian?

VitruvianOS (V\\OS) is a Linux-based operating system inspired by BeOS. It runs the BeOS/Haiku desktop and API on a Linux kernel, with custom kernel modules (collectively called **Nexus**) filling in the BeOS system services that Linux doesn't provide natively.

See the [Nexus reference page]({{< relref "/docs/reference/nexus" >}}) for details.

## Is it ready to use?

Not yet. The project is in alpha. Core components (Deskbar, Tracker, app_server, registrar) work, and BeOS/Haiku application source code compiles and runs with minimal changes. Expect rough edges.

## What's coming next?

- **Filesystem indexing and live queries**: one of the defining features of BeOS. Query the filesystem like a database, find files by attribute in real time, without any indexing daemon.
- **Multiuser support with graphical login**: per-user home directories, session management, a dedicated login screen.

## How is it different from Haiku?

Haiku is a from-scratch reimplementation of BeOS with its own kernel. Vitruvian runs on the Linux kernel instead, so it picks up Linux's hardware support, driver ecosystem, and security model for free. The goal is full BeOS/Haiku API compatibility on top of that, without writing a kernel from scratch.

## How mature is the API compatibility?

The core kits (Application, Interface, Storage, Support) work well enough to run real BeOS/Haiku applications with little or no source changes. Some kits are further along than others. The [API Changes]({{< relref "/docs/development/api-changes" >}}) page tracks known gaps.

## Is there a package manager?

Yes. But not a graphical one for now. Vitruvian ships as a Debian-derived image, so **apt** is the package manager. The full Debian archive (120,000+ packages) is available alongside native Vitruvian software.

## Does Vitruvian run on this kernel or this platform?

Vitruvian is based on a standard Linux kernel with the addition of the Nexus DKMS modules. Any not-too-ancient kernel should be able to run Vitruvian.

## Will I be able to run this program or that other program on Vitruvian?

Vitruvian can run BeOS/Haiku and CLI applications for now. Keep in mind the base system is a normal Linux kernel and applications with no UI will run just as in a normal Debian distribution (for example Node.js, binutils, parted, etc.). GTK and Qt ports are available but have not been integrated in the system yet.

## Is Vitruvian running on Xorg or Wayland?

Vitruvian uses its own display server. The stack derives from the Haiku OS source code, although heavily modified to interface with modern Linux display and hardware accelerated APIs.

## What filesystems are supported for booting?

The reference boot filesystems are **XFS** and **SquashFS**, both with full extended attribute support:

- **XFS** for standard desktop installs
- **SquashFS** for live images and embedded targets

Vitruvian also boots from **ext4** and most other Linux filesystems with extended attribute support. XFS and SquashFS are the tested and recommended options. The system can boot without xattr support, but with limited capabilities.

## Will it run in a virtual machine?

Yes. QEMU with KVM is the recommended setup for testing. See [Virtualization]({{< relref "/docs/getting-started/virtualization" >}}) for guides.

## What architectures are supported?

- **amd64** (x86-64): primary development platform, fully supported
- **arm64**: cross-compilation from an amd64 host, images available for various boards
- **RISC-V**: early work in progress

## Can I contribute without being a kernel developer?

Yes. There's useful work at every level: porting BeOS/Haiku applications, fixing desktop bugs, writing docs, testing on real hardware, filing issues, helping with the website. Come say hello on [Telegram](https://t.me/vitruvian_official_chat) and ask where help is needed.

## What is Nexus?

Nexus is a set of Linux kernel modules that bridge the Linux kernel with the BeOS/Haiku runtime. It implements ports, semaphores, shared memory areas, thread control, virtual references, and node monitoring, all the kernel APIs that Haiku provides natively but Linux doesn't. See the [Nexus]({{< relref "/docs/reference/nexus" >}}) page.

## How do I report a bug?

Open an issue on [GitHub](https://github.com/VitruvianOS/Vitruvian/issues). Include relevant log output, what you were doing, and what you expected to happen.

## Can I donate?

Yes, see the [Donate]({{< relref "/docs/reference/donate" >}}) page.

## Can I send hardware to the developers?

Hardware donations for testing and development are welcome. Reach out on [Telegram](https://t.me/vitruvian_official_chat) to coordinate.

## Where can I get help?

- [Telegram chat](https://t.me/vitruvian_official_chat): quickest way to get an answer
- [GitHub Issues](https://github.com/VitruvianOS/Vitruvian/issues): bugs and feature requests
- [Mailing list](https://www.freelists.org/list/vitruvian): longer-form discussion
