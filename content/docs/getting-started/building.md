---
title: "Building"
weight: 1
---

# Building

How to build Vitruvian from source. The build system produces a Debian package (`.deb`) that gets installed into a chroot and packaged as a bootable image. Two architectures are supported: **amd64** and **arm64**.

All commands below must be run from inside the build directory (`generated.<arch>/`), not from the repo root. `configure`, `bake`, and `setupenv.sh` all use `realpath ./` as their base — running them from anywhere else fails.

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
  debhelper debootstrap dh-dkms dkms elfutils flex \
  generate-ninja git grub-common grub-efi-amd64-bin grub-pc-bin \
  libbfd-dev libdrm-dev libdw-dev libdwarf-dev libelf-dev libfl-dev \
  libfreetype6-dev libgif-dev libicns-dev libicu-dev libinput-dev \
  libjpeg-dev libncurses-dev libopenexr-dev libpng-dev libtiff-dev \
  libudev-dev libwebp-dev linux-headers-$(uname -r) \
  mtools ninja-build xorriso zlib1g-dev squashfs-tools \
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

## amd64 build

### Quick build (no image)

For development — compiles against host libraries, no chroot, no image:

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

This uses a debootstrap chroot so the resulting `.deb` is reproducible and installable on a target system. `setupenv.sh` bootstraps a minimal Debian Trixie environment under `generated.amd64/image_tree/chroot` and installs all `-dev` packages inside it.

```bash
mkdir -p generated.amd64
cd generated.amd64
../build/scripts/setupenv.sh --chroot-build --arch=amd64
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
../build/scripts/setupenv.sh --chroot-build --arch=arm64
../configure --arch=arm64 --chroot-build
../bake build --image-type=raspberry
```

### Board image types

| Image type | Target |
|---|---|
| `raspberry` | Raspberry Pi 4 / 5 |
| `rockchip` | Rockchip-based boards (Rock Pi, etc.) |
| `allwinner` | Allwinner-based boards |
| `allwinner-h3` | Allwinner H3 specifically |
| `beagle` | BeagleBone |
| `visionfive2` | StarFive VisionFive 2 (RISC-V) |
| `licheerv` | LicheeRV (RISC-V) |

Each board type bundles the appropriate u-boot, device tree, and firmware. The board-specific logic lives in `build/scripts/lib/boards.sh`.

arm64 produces `.deb` packages that include the Nexus DKMS kernel module, which auto-rebuilds on kernel update.

## configure options

| Option | Default | Description |
|---|---|---|
| `--arch=ARCH` | `amd64` | `amd64` or `arm64` |
| `--build-type=TYPE` | `Debug` | `Debug`, `Release`, or `Workflow` |
| `--chroot-build` | off | Build inside debootstrap chroot (required for reproducible images) |
| `--image-type=TYPE` | — | Default image type for `bake build` |
| `--buildtools=PATH` | — | Use pre-built buildtools instead of building locally |

Release build example:

```bash
../configure --arch=amd64 --build-type=Release --chroot-build
```

## bake commands

`bake` is a symlink at the repo root pointing to `build/scripts/bake.sh`.

| Command | What it does |
|---|---|
| `bake build --image-type=TYPE` | Build everything and produce the specified image |
| `bake clean` | Clean build artifacts |
| `bake boot --image-type=TYPE` | Boot the most recent image in QEMU |

`bake build` handles the full pipeline: `ninja` → `cpack` → image creation. Don't call `cpack`, `mkiso.sh`, or `mkraw.sh` directly — those are internal.

## Troubleshooting

**`CMakeLists.txt not found`** — you're running `configure` from the repo root. `cd` into `generated.<arch>/` first.

**`no chroot found at ./image_tree/chroot`** — you passed `--chroot-build` without running `setupenv.sh` first.

**Build fails on missing `xres` or `rc`** — these are built as part of `configure` but a partial build state can leave them missing. Clean and re-run `configure`.

**arm64 build can't find cross compiler** — make sure `gcc-aarch64-linux-gnu` is installed and `build/cross/aarch64-linux-gnu.cmake` exists.
