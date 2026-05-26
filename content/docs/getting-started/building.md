---
title: "Building"
weight: 1
---

# Building

How to build Vitruvian from source. The build system produces a Debian package (`.deb`) that gets installed into a chroot and packaged as a bootable image. Two architectures are supported: **amd64** and **arm64**.

Both `configure`, `bake` commands must be run from inside the build directory (`generated.<arch>/`), not from the repo root. They use `realpath ./` as their base; running them from anywhere else fails.

## Prerequisites

A Debian-based host (Trixie or newer recommended). Required toolchain:

- cmake ≥ 3.25
- gcc ≥ 8 (native) or `gcc-aarch64-linux-gnu` (arm64 cross)
- ninja
- debootstrap (for chroot builds)

### amd64 dependencies

```bash
sudo apt install -y \
  autoconf automake bison build-essential cmake \
  debhelper debootstrap dh-dkms dkms dosfstools elfutils flex \
  generate-ninja git grub-common grub-efi-amd64-bin grub-pc-bin \
  libbfd-dev libdrm-dev libdw-dev libdwarf-dev libelf-dev libfl-dev \
  libfreetype6-dev libgif-dev libicns-dev libicu-dev libinput-dev \
  libjpeg-dev libncurses-dev libopenexr-dev libpng-dev libtiff-dev \
  libudev-dev libwebp-dev linux-headers-$(uname -r) \
  mtools ninja-build ovmf parted qemu-utils rsync squashfs-tools \
  xfsprogs xorriso zlib1g-dev \
  --fix-missing
```

### arm64 cross-compile dependencies

Everything from the amd64 list, plus:

```bash
sudo apt install -y gcc-aarch64-linux-gnu qemu-user-static
```

## Getting the source

```bash
git clone https://github.com/VitruvianOS/Vitruvian.git
cd Vitruvian
git submodule update --init --recursive
```

## Build host tools

Before any build, compile the host tools (`rc`, `xres`, `resattr`) needed by the rdef→resource pipeline. This step is shared across all architectures and only needs to be done once:

```bash
mkdir -p buildtools
cd buildtools
cmake -DBUILDTOOLS_MODE=1 .. -GNinja
ninja
cd ..
```

Skipping this will cause the main build to fail with "missing rc / xres binary".

## amd64 build

### Quick build (no image)

For development. Compiles against host libraries, no chroot, no image:

```bash
mkdir -p generated.amd64
cd generated.amd64
../configure --arch=amd64
ninja
```

Run individual targets:

```bash
ninja app_server          # just app_server
ninja                     # everything
```

### Full build with ISO or raw image

This uses a debootstrap chroot so the resulting `.deb` is reproducible and installable on a target system. The `configure` command automatically creates the chroot with the debendencies bootstrapped. The chroot is not meant to be manipulated by the user in general.

```bash
mkdir -p generated.amd64
cd generated.amd64
../configure --arch=amd64 --chroot-build
../bake build --image-type=iso
```

For a raw GPT+XFS disk image instead of ISO:

```bash
../bake build --image-type=raw
```

### Boot the result

```bash
../bake boot --image-type=iso
```

This launches QEMU with the correct flags. See [Virtualization]({{< relref "/docs/getting-started/virtualization" >}}) for manual QEMU setup.

## arm64 cross-compile

arm64 is cross-compiled from an amd64 host. The toolchain file `build/cross/aarch64-linux-gnu.cmake` handles the cross-compilation flags.

```bash
mkdir -p generated.arm64
cd generated.arm64
../configure --arch=arm64 --chroot-build
../bake build --image-type=iso
```

arm64 produces `.deb` packages that include the Nexus DKMS kernel module, which auto-rebuilds on kernel update.

## configure options

| Option | Default | Description |
|---|---|---|
| `--arch=ARCH` | `amd64` | `amd64` or `arm64` |
| `--build-type=TYPE` | `Debug` | `Debug`, `Release`, or `Workflow` |
| `--chroot-build` | off | Build inside debootstrap chroot (required for reproducible images) |
| `--image-type=TYPE` | *none* | Default image type for `bake build` |
| `--buildtools=PATH` | *none* | Use pre-built buildtools instead of building locally |

Release build example:

```bash
../configure --arch=amd64 --build-type=Release --chroot-build
```

## bake commands

| Command | What it does |
|---|---|
| `bake build --image-type=TYPE` | Build everything and produce the specified image |
| `bake clean` | Clean build artifacts |
| `bake boot --image-type=TYPE` | Boot the most recent image in QEMU |

## Troubleshooting

**`CMakeLists.txt not found`**: you're running `configure` from the repo root. `cd` into `generated.<arch>/` first.

**`no chroot found at ./image_tree/chroot`**: This should not happen normally as `configure --chroot-build` automatically invokes `setupenv.sh` when needed. If you see this, try running `../configure --arch=<arch> --chroot-build` again.

**Build fails on missing `xres` or `rc`**: you skipped the buildtools step. Build it first (see above).

**arm64 build can't find cross compiler**: make sure `gcc-aarch64-linux-gnu` is installed and `build/cross/aarch64-linux-gnu.cmake` exists.
