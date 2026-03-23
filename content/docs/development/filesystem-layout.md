---
title: "Filesystem Layout"
weight: 3
---

# Filesystem Layout

Vitruvian runs on a standard Linux filesystem hierarchy, but maps BeOS/Haiku path conventions onto it so that application source code written for BeOS or Haiku compiles and runs without path changes.

## Boot Filesystem

The reference boot filesystems are **XFS** (standard desktop installs) and **SquashFS** (live images and embedded targets). Both support Linux extended attributes, which are required for the BeOS attribute store. Most other Linux-supported filesystems with extended attribute support also work; see the [FAQ]({{< relref "/docs/getting-started/faq" >}}) for details.

## Path Mapping

BeOS and Haiku expose their filesystem tree under `/boot`. In VitruvianOS, `/boot` maps directly to the Linux root `/`, rather than to a separate `/system` subtree:

| BeOS/Haiku path | Vitruvian path |
|-----------------|-----------------|
| `/boot` | `/` |
| `/boot/home` | `/home` |
| `/boot/system` | `/system` |

The `/system/home` directory is a symlink to the actual home directory, preserving compatibility with code that constructs paths via either convention.

Standard Linux paths (`/usr`, `/lib`, `/etc`, `/proc`, `/dev`, and so on) coexist alongside the BeOS-mapped paths and are used by the Debian-derived userland and package manager in the normal way.

## Extended Attributes

BeOS relies heavily on filesystem extended attributes for file metadata, MIME types, application signatures, and the attribute database that powers live queries. Vitruvian stores these as Linux extended attributes (`xattr`). XFS is the recommended boot filesystem because it has no practical per-file xattr size limit, which matches BeOS behavior most closely.

{{< hint info >}}
More detail on individual directories and the attribute namespace is coming.
{{< /hint >}}
