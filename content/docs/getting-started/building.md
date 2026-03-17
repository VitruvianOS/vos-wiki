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

After cloning, enter the source directory and run:

```bash
git submodule update --init --recursive
mkdir buildtools/ && mkdir generated.x86/
cd buildtools/
cmake -DBUILDTOOLS_MODE=1 .. -GNinja
ninja
cd ../generated.x86 && ../build/scripts/setupenv.sh
../configure
ninja
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
