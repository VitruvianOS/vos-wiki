---
title: "Building"
weight: 1
---

# Building

How to compile VitruvianOS from source on a Debian-based system.

## Prerequisites

Required software:

- cmake ≥ 3.25
- gcc ≥ 8
- libinput ≥ 1.16.4-3
- ninja

Install all build dependencies:

```bash
sudo apt install -y \
  autoconf \
  automake \
  bison \
  build-essential \
  cmake \
  debhelper \
  debootstrap \
  dh-dkms \
  dkms \
  elfutils \
  flex \
  generate-ninja \
  git \
  grub-common \
  grub-efi-amd64-bin \
  grub-pc-bin \
  libbfd-dev \
  libdrm-dev \
  libdw-dev \
  libdwarf-dev \
  libelf-dev \
  libfl-dev \
  libfreetype6-dev \
  libgif-dev \
  libicns-dev \
  libicu-dev \
  libinput-dev \
  libjpeg-dev \
  libncurses-dev \
  libopenexr-dev \
  libpng-dev \
  libtiff-dev \
  libudev-dev \
  libwebp-dev \
  linux-headers-$(uname -r) \
  mtools \
  ninja-build \
  xorriso \
  zlib1g-dev \
  squashfs-tools \
  --fix-missing
```

## Getting the Source

```bash
git clone https://github.com/VitruvianOS/Vitruvian.git
```

## Building VitruvianOS

```bash
git submodule update --init --recursive
mkdir buildtools/ && mkdir generated.x86/
cd buildtools/
cmake -DBUILDTOOLS_MODE=1 .. -GNinja
ninja
cd ../generated.x86 && ../build/scripts/setupenv.sh
../configure --chroot-build
ninja
```

`setupenv.sh` uses `debootstrap` to bootstrap a minimal Debian Trixie (amd64) environment under `generated.x86/image_tree/chroot`, installs all required `-dev` packages inside it, and writes `imagekernelversion.conf` with the chroot kernel version. The compiler and build tools are always taken from the host.

If you only need to build and run Vitruvian locally without producing an image, you can skip `setupenv.sh` and run `../configure` without `--chroot-build`, building directly against the host libraries instead.

### configure options

The `configure` script accepts the following options:

| Option | Default | Description |
|---|---|---|
| `--build-type=TYPE` | `Debug` | CMake build type: `Debug`, `Release`, or `Workflow` |
| `--arch=ARCH` | `x86_64` | Target architecture (see [Architectures](#architectures)) |
| `--chroot-build` | off | Build against the chroot sysroot instead of host libraries |
| `--chroot-path=PATH` | `<build>/image_tree/chroot` | Override the chroot path (requires `--chroot-build`) |

Example — release build with chroot:

```bash
../configure --build-type=Release --chroot-build
```

### Architectures

| Architecture | Status |
|---|---|
| `x86_64` | Supported (default) |
| `arm64` | Not yet supported |

x86_64 is selected by default. To make the selection explicit:

```bash
../configure --arch=x86_64
```


## Building the .deb Package

After a successful `ninja` build:

```bash
cd generated.x86 && cpack
```

## Creating a Live ISO Image

After building and generating the `.deb` packages:

```bash
chmod +x ./build/scripts/mkiso.sh
cd generated.x86 && ../build/scripts/mkiso.sh
```

## Creating a Live Raw Image

After building and generating the `.deb` packages:

```bash
chmod +x ./build/scripts/mkraw.sh
cd generated.x86 && ../build/scripts/mkraw.sh
```
