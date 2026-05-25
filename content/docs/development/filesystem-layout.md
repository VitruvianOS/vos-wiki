---
title: "Filesystem Layout"
weight: 3
---

# Filesystem Layout

Vitruvian runs on a standard Linux filesystem hierarchy but maps BeOS/Haiku path conventions onto it so that application source code written for BeOS or Haiku works without path changes.

## Boot filesystem

The reference boot filesystems are **XFS** (standard desktop installs) and **SquashFS** (live images, embedded targets). Both support Linux extended attributes, which are required for the BeOS attribute store. Ext4 and most other Linux filesystems with xattr support also work.

XFS is recommended because it has no practical per-file xattr size limit, matching BeOS behavior most closely.

## Path mapping

BeOS and Haiku expose their filesystem tree under `/boot`. Vitruvian maps `/boot` to the Linux root `/`:

| BeOS/Haiku path | Vitruvian path |
|---|---|
| `/boot` | `/` |
| `/boot/home` | `/home` |
| `/boot/system` | `/system` |

`/system/home` is a symlink to `/home`, so code that constructs paths via either convention works.

Standard Linux paths (`/usr`, `/lib`, `/etc`, `/proc`, `/dev`) coexist and are used by the Debian-derived userland and `apt` as usual.

## Extended attributes

BeOS relies on filesystem extended attributes for file metadata, MIME types, application signatures, and the attribute database that powers live queries. Vitruvian stores these as Linux xattrs in the `user.` namespace:

- Attribute data: `user.<name>`
- Attribute type tag (Haiku `uint32`): `user.<name>.type`

Access is via `fgetxattr`, `fsetxattr`, `fremovexattr`, `flistxattr` on the node's open fd. `BNode::ReadAttr`, `WriteAttr`, `GetAttrInfo` wrap these. Attribute iteration (`BNode::GetNextAttr`) walks `flistxattr` output and strips the `user.` prefix.

{{< hint info >}}
More detail on individual directories and the attribute namespace is coming.
{{< /hint >}}
