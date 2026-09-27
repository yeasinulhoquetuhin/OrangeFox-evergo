<div align="center">

# OrangeFox Recovery — Evergo

### Redmi Note 11T 5G · Redmi Note 11 5G · POCO M4 Pro 5G (India)

**MediaTek Dimensity 810 (MT6833P) · 6 nm · Virtual A/B · FBE flags enabled**

[![Branch](https://img.shields.io/badge/branch-fox__12.1-0073b5?style=flat-square&logo=git&logoColor=white)](https://github.com/yeasinulhoquetuhin/OrangeFox-evergo/tree/fox_12.1)
[![License](https://img.shields.io/badge/license-Apache--2.0-0073b5?style=flat-square&logo=apache&logoColor=white)](https://www.apache.org/licenses/LICENSE-2.0)
[![SoC](https://img.shields.io/badge/SoC-MT6833P-FF6D00?style=flat-square&logo=mediatek&logoColor=white)](https://www.mediatek.com/)
[![Device](https://img.shields.io/badge/codename-evergo-EA4B4C?style=flat-square)](https://github.com/yeasinulhoquetuhin/OrangeFox-evergo)
[![Recovery](https://img.shields.io/badge/OrangeFox-R11.1-FF6D00?style=flat-square&logo=android&logoColor=white)](https://github.com/OrangeFoxRecovery)
[![Last Commit](https://img.shields.io/github/last-commit/yeasinulhoquetuhin/OrangeFox-evergo?style=flat-square&logo=github&logoColor=white)](https://github.com/yeasinulhoquetuhin/OrangeFox-evergo/commits/fox_12.1)
[![Size](https://img.shields.io/github/repo-size/yeasinulhoquetuhin/OrangeFox-evergo?style=flat-square&logo=github&logoColor=white)](https://github.com/yeasinulhoquetuhin/OrangeFox-evergo)

Device tree for the **Redmi Note 11T 5G** (`evergo`) on the MediaTek MT6833P platform, carrying
OrangeFox R11.1 variables. Virtual A/B, FBE flags enabled, AVB aware.

> [!CAUTION]
> This is a **source tree only**. No prebuilt recovery is published here, and no build of it has
> been verified. It also inherits TWRP product makefiles rather than OrangeFox ones, so it will
> not build an OrangeFox recovery as-is — see
> [What this tree really builds](#what-this-tree-really-builds).

**Maintainer:** [Yeasinul Hoque Tuhin](https://github.com/yeasinulhoquetuhin) — [tuhinbro.com](https://tuhinbro.com) · [Telegram: @TuhinBroh](https://t.me/TuhinBroh)

</div>

---

## Contents

- [Overview](#overview)
- [Supported Devices](#supported-devices)
- [Device Specifications](#device-specifications)
- [Features](#features)
- [Build Configuration](#build-configuration)
- [Repository Layout](#repository-layout)
- [Partition Layout](#partition-layout)
- [Building from Source](#building-from-source)
- [Flashing the Recovery](#flashing-the-recovery)
- [Post-Flash Setup](#post-flash-setup)
- [Build Variables Reference](#build-variables-reference)
- [Known Constraints](#known-constraints)
- [Component Provenance](#component-provenance)
- [Troubleshooting](#troubleshooting)
- [Contributing](#contributing)
- [Credits](#credits)
- [License and Disclaimer](#license-and-disclaimer)

---

## Overview

This repository is a **device tree**, not the recovery itself. It is the build configuration,
ramdisk overlay, MediaTek-specific userspace and prebuilt firmware that lets the generic
OrangeFox source compile into a bootable recovery for `evergo`.

| | |
| :--- | :--- |
| Recovery | OrangeFox R11.1, variant `S` |
| Platform | `mt6833` / MT6833P (MediaTek Dimensity 810) |
| Target OS level | Android 12 (S) — VNDK 31, shipping API level 30 |
| Update model | Virtual A/B + dynamic partitions |
| Encryption | FBE with metadata encryption, `v2+inlinecrypt_optimized` |
| Userdata filesystem | f2fs (ext4 also supported) |
| Kernel | Prebuilt stock `Image.gz` |
| Boot control | MediaTek `android.hardware.boot@1.2` HAL |
| Verified boot | AVB enabled |
| Storage type | UFS 2.2 |

> [!NOTE]
> There is deliberately no CI badge on this repository. Compiling an AOSP-based recovery needs
> roughly 16–32 GB RAM and 300–500 GB disk, which no stock GitHub-hosted runner provides. A
> green badge here would be misleading. Build locally.

---

## Supported Devices

This tree supports **`evergo` only**.

| Retail name | Region | Codename | Xiaomi model |
| :--- | :--- | :--- | :--- |
| Redmi Note 11T 5G | India | `evergo` | `21091116AI` |
| Redmi Note 11 5G | India | `evergo` (`evergo_in` firmware) | `21091116AI` |
| POCO M4 Pro 5G | India | `evergo` | `21091116AI` hardware |

All three are the same board and run the same `evergo` firmware. The India POCO M4 Pro 5G is the
same handset as the Redmi Note 11T 5G under a different badge.

### Not supported by this tree

These are **different devices** with different model numbers. Do not flash an `evergo` recovery
onto them.

| Retail name | Region | Codename | Xiaomi model |
| :--- | :--- | :--- | :--- |
| POCO M4 Pro 5G | Global | `evergreen` | `21091116AG` |
| Redmi Note 11S 5G | Global | `opal` | `22031116BG` |

The Global `21091116AG` and `22031116BG` variants ship a different 5G band set than the India
`21091116AI`, and are separate boards for firmware purposes.

> [!NOTE]
> The imported `vendorsetup.sh` still lists `evergreen` and `opal` in `OF_TARGET_DEVICES` and
> `TARGET_DEVICE_ALT`, inherited from the upstream OrangeFox tree. Those entries are **not** a
> statement of support. This project documents and ships `evergo` only.

---

## Device Specifications

Values below are for `evergo` (Redmi Note 11T 5G / `21091116AI`).

| Category | Specification |
| :--- | :--- |
| SoC | MediaTek Dimensity 810 (MT6833P), 6 nm |
| CPU | Octa-core — 2 × Cortex-A76 @ 2.4 GHz + 6 × Cortex-A55 @ 2.0 GHz |
| GPU | Arm Mali-G57 MC2 @ 1068 MHz |
| RAM | 4 / 6 / 8 GB LPDDR4X |
| Storage | 64 / 128 GB UFS 2.2, hybrid microSD up to 1 TB |
| Display | 6.6" IPS LCD, 1080 × 2400 (20:9), ~399 ppi |
| Refresh rate | Up to 90 Hz, AdaptiveSync 30 / 50 / 60 / 90 Hz |
| Touch sampling | Up to 240 Hz |
| Display extras | DCI-P3 wide gamut, sunlight mode, reading mode |
| Protection | Corning Gorilla Glass 3 (front) |
| Rear camera | 50 MP f/1.8 PDAF 26 mm + 8 MP f/2.2 ultra-wide 119° — **dual** |
| Front camera | 16 MP f/2.5, punch-hole |
| Video | 1080p @ 30 / 60 fps |
| Battery | 5000 mAh typical / 4900 mAh minimum, 33 W wired |
| Charging time | ~59–62 min to 100%, 33 W charger in box |
| SIM | Hybrid dual nano-SIM / microSD |
| Cellular | 5G SA/NSA, dual 5G standby, LTE |
| Wi-Fi | 802.11 a/b/g/n/ac dual-band (2.4 + 5 GHz) |
| Bluetooth | 5.1 |
| Wired | USB-C 2.0 with OTG |
| Extras | IR blaster, stereo speakers, 3.5 mm jack, 24-bit/192 kHz Hi-Res audio |
| Sensors | Side-mounted fingerprint, accelerometer, gyroscope, compass |
| Dimensions | 163.6 × 75.8 × 8.8 mm |
| Weight | 195 g |
| Ingress rating | IP53 |
| Stock OS | Android 11 + MIUI 12.5 |

### OS update history

| Stage | Android | Firmware |
| :--- | :--- | :--- |
| Launch | Android 11 | MIUI 12.5 |
| July 2022 | Android 12 | MIUI 13 |
| June 2023 | Android 13 | MIUI 14 |
| March 2024 | Android 13 | HyperOS 1 |

The device reached **end-of-life** — it does not receive HyperOS 2 or further security patches.
This matters for a recovery: an unpatched stock OS is a patched-attack-surface system.

---

## Features

### Recovery

- OrangeFox R11.1 UI, variant `S`, notch-aware layout (`OF_HIDE_NOTCH=1`) tuned for the
  1080 × 2400 panel with 48 px status-bar insets.
- App Manager enabled (`FOX_ENABLE_APP_MANAGER=1`).
- Toolbox built in (`TW_USE_TOOLBOX=1`).
- `nano` as the built-in file editor (`FOX_USE_NANO_EDITOR=1`).
- Bash as `ash` (`FOX_USE_BASH_SHELL=1`, `FOX_ASH_IS_BASH=1`) — a real shell with globbing,
  arrays and `[[ ]]`, not a stripped appletset.
- Panel-accurate brightness control, maximum **2047**, default 1200.
- Backup and restore across boot and internal storage.

### Correctness

- FBE flags enabled in the tree — `TW_INCLUDE_CRYPTO`, `TW_INCLUDE_CRYPTO_FBE` and
  `TW_INCLUDE_FBE_METADATA_DECRYPT` are all `true` in `BoardConfig.mk`. Note the `twrp-12.1`
  manifest states FDE decryption is **not presently supported** on that branch, so a build from it
  will not actually decrypt `/data` regardless of these flags.
- AVB aware — `vbmeta`, `vbmeta_system` and `vbmeta_vendor` are exposed for selective
  re-patching.
- Dynamic partitions sized correctly at **9 126 805 504** bytes and mirrored in
  `OF_DYNAMIC_FULL_SIZE`, so reported internal-storage capacity is accurate.
- Virtual A/B snapshots via `OF_VIRTUAL_AB_DEVICE=1`.
- Magisk-boot for every patch target (`OF_USE_MAGISKBOOT_FOR_ALL_PATCHES=1`).
- `fastbootd` and the mock fastboot HAL ship, so dynamic-partition resize and userdata
  reformat work from recovery.
- Vanilla build (`OF_VANILLA_BUILD=1`) suppresses MIUI patching warnings — correct behaviour
  on this device.
- microSD (`/sdcard1`) and USB-OTG (`/usb_otg`) both handled as removable storage.

### MediaTek platform

- Preloader A/B plumbing — `mtk_plpath_utils` plus the symlink farm in
  `init.recovery.mt6833.rc` recreate `preloader_a` / `preloader_b`, the `*_emmc_*` and `*_ufs_*`
  aliases, and the `preloader_raw_*` device-mapper links, so slot-aware OTA bookkeeping works.
- MediaTek Boot Control HAL 1.2 built from source in `bootctrl/` rather than dropped in as a
  blob, so A/B slot selection is genuinely functional. UFS only; the eMMC path was removed
  upstream.
- TEE / microtrust — `teei_daemon` service definition, the full
  `init.recovery.microtrust.rc` layout routine, and the proprietary trusted applications under
  `vendor/thh/ta/`, so Keymaster and Gatekeeper can answer during decryption.
- Haptics and touch firmware — the AW8697 haptic waveform bank and Novatek touchscreen firmware
  are bundled in `vendor/firmware/`.
- USB — MediaTek `musb-hdrc` configfs gadget supporting `adb`, `sideload`, `mtp` and `mtp,adb`.

---

## Build Configuration

| Setting | Value | Source |
| :--- | :--- | :--- |
| Recovery version | `R11.1_0` | `vendorsetup.sh` → `FOX_VERSION` |
| Variant | `S` | `vendorsetup.sh` → `FOX_VARIANT` |
| Theme | `portrait_hdpi` | `BoardConfig.mk` → `TW_THEME` |
| Shipping API | `30` | `device.mk` → `PRODUCT_SHIPPING_API_LEVEL` |
| VNDK | `31` | `device.mk` → `PRODUCT_TARGET_VNDK_VERSION` |
| Platform version | `99.87.36` | `BoardConfig.mk` → `PLATFORM_VERSION` |
| Security patch | `2127-12-31` (sentinel, not a real date) | `BoardConfig.mk` → `PLATFORM_SECURITY_PATCH` |
| Screen height | `2400` | `vendorsetup.sh` → `OF_SCREEN_H` |
| Status bar height | `100` | `vendorsetup.sh` → `OF_STATUS_H` |
| Status bar insets | `48` left, `48` right | `vendorsetup.sh` |
| Max brightness | `2047` | `BoardConfig.mk` → `TW_MAX_BRIGHTNESS` |
| Default brightness | `1200` | `BoardConfig.mk` → `TW_DEFAULT_BRIGHTNESS` |
| Super partition size | `9126805504` | `BoardConfig.mk` / `OF_DYNAMIC_FULL_SIZE` |
| Kernel cmdline | `bootopt=64S3,32N2,64N2` | `BoardConfig.mk` → `BOARD_KERNEL_CMDLINE` |
| Boot image header | `v2`, 2048-byte pages | `BoardConfig.mk` |
| Kernel base | `0x40078000` | `BoardConfig.mk` |
| Ramdisk offset | `0x11088000` | `BoardConfig.mk` |
| Tags offset | `0x07c08000` | `BoardConfig.mk` |
| Pixel format | `RGBX_8888` | `BoardConfig.mk` |

---

## Repository Layout

```
OrangeFox-evergo/
├── Android.bp                          # Soong namespace root
├── Android.mk                          # Legacy makefile entry (TARGET_DEVICE gate)
├── AndroidProducts.mk                  # Lunch registration  ->  twrp_evergo-eng
├── BoardConfig.mk                      # Platform, kernel, partitions, TW_/OF_ flags
├── device.mk                           # Packages, HALs, update-engine, A/B plumbing
├── twrp_evergo.mk                      # Product definition  (PRODUCT_NAME=twrp_evergo)
├── system.prop                         # Crypto, gatekeeper, keymaster, TEE, USB props
├── vendorsetup.sh                      # OrangeFox build variables (all OF_* / FOX_*)
│
├── prebuilt/                           # Stock firmware blobs (proprietary)
│   ├── Image.gz                        #   Kernel, ~15 MB, gzip-compressed
│   ├── dtb.img                         #   Device Tree Blob
│   └── dtbo.img                        #   Device Tree Overlay image
│
├── bootctrl/                           # MediaTek Boot Control HAL 1.2 (Apache-2.0)
│   ├── Android.bp                      #   android.hardware.boot@1.2-mtkimpl
│   ├── BootControl.cpp                 #   Slot / boot-slot control
│   ├── BootControl.h
│   ├── boot_control_definition.h
│   ├── boot_region_control.cpp         #   UFS boot-region control
│   ├── boot_region_control_private.h
│   ├── ufs-mtk-ioctl.h                 #   UFS ioctl definitions
│   ├── ufs-mtk-ioctl-private.h
│   └── NOTICE                          #   AOSP license attribution
│
├── mtk_plpath_utils/                   # Preloader A/B path utilities
│   ├── Android.bp                      #   mtk_plpath_utils (cc_binary)
│   └── mtk_plpath_utils.cpp            #   Recreates preloader symlinks and device nodes
│
└── recovery/root/                      # Ramdisk overlay
    ├── init.recovery.mt6833.rc         #   MTK A/B symlinks, boot HAL, keystore, TEE stop rules
    ├── init.recovery.microtrust.rc     #   teei_daemon, RPMB, /data/vendor/thh layout
    ├── init.recovery.usb.rc            #   musb-hdrc configfs gadget (adb / sideload / mtp)
    ├── ueventd.mt6833.rc               #   /dev node permissions and ownership
    │
    ├── first_stage_ramdisk/
    │   ├── fstab.mt6833                #   First-stage mount table
    │   └── fstab.emmc                  #   Legacy eMMC mount table
    │
    ├── system/
    │   ├── bin/
    │   │   ├── android.hardware.gatekeeper@1.0-service
    │   │   ├── android.hardware.keymaster@4.1-service.beanpod
    │   │   └── teei_daemon
    │   ├── etc/
    │   │   ├── recovery.fstab          #   Recovery mount table
    │   │   ├── twrp.flags              #   Partition list for the Wipe/Flash GUI
    │   │   └── vintf/manifest.xml      #   Framework VINTF manifest
    │   └── flashlight/
    │       ├── brightness              #   -> symlink to torch_brightness
    │       └── max_brightness
    │
    └── vendor/
        ├── etc/vintf/manifest.xml      #   Device VINTF manifest (MTK + Xiaomi HALs)
        ├── firmware/                   #   15 blobs: AW8697 haptics, Novatek touch, MT6631/35 FM
        ├── lib64/
        │   ├── libTEECommon.so
        │   ├── libteei_daemon_vfs.so
        │   └── hw/                     #   keymaster + gatekeeper impls, libimsg_log
        └── thh/
            └── ta/                     #   24 proprietary trusted applications + isee_model.json
```

---

## Partition Layout

### Boot and verified boot

| Partition | Type | Slot-aware | Notes |
| :--- | :--- | :---: | :--- |
| `boot` | emmc | yes | Recovery-as-boot, header v2, backupable |
| `recovery` | emmc | yes | Recovery image target |
| `dtbo` | emmc | — | Prebuilt `dtbo.img`, flashable |
| `vbmeta` | emmc | — | AVB top level |
| `vbmeta_system` | emmc | yes | AVB, `avb=vbmeta` |
| `vbmeta_vendor` | emmc | yes | AVB |
| `boot_para` | emmc | — | MediaTek boot parameters |

### Dynamic partitions

| Partition | Type | Notes |
| :--- | :--- | :--- |
| `super` | — | 9 126 805 504 B total, groups `xiaomi_dynamic_partitions` |
| `system` | ext4 | logical, `first_stage_mount`, AVB via `vbmeta_system` |
| `vendor` | ext4 | logical, `first_stage_mount`, AVB |
| `product` | ext4 | logical, `first_stage_mount`, AVB |
| `*_a` / `*_b` | — | Virtual A/B slot suffixes |

The partition list is `system vendor product`
(`BOARD_XIAOMI_DYNAMIC_PARTITIONS_PARTITION_LIST`), with 9 122 611 200 B reserved for them
(`BOARD_XIAOMI_DYNAMIC_PARTITIONS_SIZE`) inside the 9 126 805 504 B super
(`BOARD_SUPER_PARTITION_SIZE`). Individual logical sizes are computed at build time by
`build_super_image`, not hard-coded.

### Data and metadata

| Partition | Mount | Filesystem | Notes |
| :--- | :--- | :--- | :--- |
| `userdata` | `/data` | f2fs | `fileencryption=aes-256-xts:aes-256-cts:v2+inlinecrypt_optimized`, `fsverity`, `reservedsize=128m` |
| `metadata` | `/metadata` | ext4 | `keydirectory=/metadata/vold/metadata_encryption` |
| `rescue` | `/cache` | ext4 | OrangeFox cache |
| `misc` | `/misc` | emmc | Boot-control state |

### MediaTek firmware and secure partitions

All entries below are from the ramdisk `fstab.mt6833`.

| Group | Partitions |
| :--- | :--- |
| Bootloader | `lk`, `lk2`, `para`, `boot_para` |
| Security / TEE | `seccfg`, `expdb`, `tee1`, `tee2`, `scp1`, `scp2`, `sspm_1`, `sspm_2`, `otp`, `efuse` |
| Persistence | `nvram`, `proinfo`, `frp` (as `/persistent`) |
| AVB | `vbmeta`, `vbmeta_system`, `vbmeta_vendor` |
| DSP / firmware | `md1img`, `md1dsp`, `md1arm7`, `md3img`, `gz1`, `gz2`, `spmfw` |
| Regional / data | `persist`, `protect1` (as `/mnt/vendor/protect_f`), `protect2` (as `/mnt/vendor/protect_s`), `nvdata`, `nvcfg` |
| Misc | `logo`, `pi_img`, `odmdtbo`, `dtbo` |
| PFMU (PMIC) | `dpm_1`, `dpm_2`, `mcupm_1`, `mcupm_2` |

Declared only in `ueventd.mt6833.rc` — permission only, not mounted:
`misc2`, `secro`, `md1img_a`, `md1img_b`.

Present only in `twrp.flags`, which drives the Wipe/Flash GUI rather than the mount table:
`cust`, `cust_image`, `persist_image`, `audio_dsp`, `protect_f`, `protect_s`.

> [!NOTE]
> The `*_image` entries in `twrp.flags` (`persist_image`, `cust_image`) exist so you can back up
> and re-flash the underlying blocks as images. They point at the *same* device node as the plain
> entry — pick one or the other in the GUI, not both.

---

## What this tree really builds

This tree is a hybrid, and the naming does not describe it accurately. Reading the makefiles:

| File | What it says |
| :--- | :--- |
| `vendorsetup.sh` | Sets ~40 `OF_*` / `FOX_*` variables, `FOX_VERSION="R11.1_0"`, `FOX_VARIANT="S"`, `OF_MAINTAINER="Sushrut1101"` |
| `twrp_evergo.mk` | Inherits `product/core_64_bit.mk`, `product/aosp_base.mk` and **`vendor/twrp/config/common.mk`** |
| `AndroidProducts.mk` | Registers `twrp_evergo-eng` |
| `PRODUCT_MODEL` | `Redmi Note 11T 5G` |

So the tree carries **OrangeFox branding and variables**, but its product makefiles inherit
**TWRP**, not OrangeFox's recovery products. The `OF_*` and `FOX_*` variables in `vendorsetup.sh`
are only read by OrangeFox's own build scripts — and those scripts are not published for Android
12 (see the note in [Building from Source](#building-from-source)).

Practically this means:

- Building with the `twrp-12.1` manifest produces a **TWRP** recovery for `evergo`. The OrangeFox
  variables are inert.
- You cannot reproduce an OrangeFox R11.1 build from this tree alone. Doing so needs OrangeFox's
  recovery source and build tooling, which do not exist publicly for 12.1.
- The device configuration itself — partitions, fstab, BoardConfig, screen geometry, the
  `twrp.flags` partition list — is complete and usable regardless.

> [!NOTE]
> **No binary release.** This repository contains no prebuilt recovery, and no build of this tree
> has been verified. Treat it as a source tree to be compiled by whoever wants to try it.

---

## Building from Source

> [!IMPORTANT]
> **There is no official OrangeFox 12.1 build.** The OrangeFox project publishes a manifest only
> for R6.0 — [`fox-6.0_manifest`](https://github.com/OrangeFoxRecovery/fox-6.0_manifest) on branch
> `fox_6.0`, last touched December 2021, and it builds against OmniROM's recovery tree. There is
> no `fox-12.1` manifest, no OrangeFox `build.sh`, and no published `buildspec` for Android 12.
>
> The commands below therefore build this tree against the **TWRP `twrp-12.1` minimal manifest**,
> which is the manifest this tree is actually written for. See
> [What this tree really builds](#what-this-tree-really-builds) for what that means in practice.

### 1. Host requirements

Recovery-only build (`mka recoveryimage`), not a full system build.

| Resource | Minimum | Recommended |
| :--- | :--- | :--- |
| RAM | 16 GB | 32 GB (add 16 GB swap) |
| Disk | 100 GB | 200 GB, SSD |
| CPU | 8 threads | 16+ threads |
| OS | Ubuntu 22.04 LTS | Ubuntu 22.04 or 24.04 LTS |

> [!TIP]
> Build on an SSD. If RAM-bound, put swap on the same SSD:
> ```bash
> sudo fallocate -l 16G /swapfile && sudo chmod 600 /swapfile
> sudo mkswap /swapfile && sudo swapon /swapfile
> echo '/swapfile none swap sw 0 0' | sudo tee -a /etc/fstab
> ```

### 2. Host dependencies

```bash
sudo apt update && sudo apt upgrade -y

sudo apt install -y \
  bc bison build-essential ccache curl flex g++-multilib gcc-multilib git git-lfs \
  gnupg gperf imagemagick lib32readline-dev lib32z1-dev libelf-dev liblz4-tool \
  libncurses-dev libpng-dev libssl-dev libxml2-dev libxml2-utils \
  lzop m4 make python3 rsync squashfs-tools syslinux-utils \
  unzip xz-utils zlib1g-dev cpio lz4

git config --global user.name  "Your Name"
git config --global user.email "you@example.com"
git config --global core.longpaths true
```

> [!NOTE]
> `libncurses5-dev`, `pngcrush` and `schedtool` are dropped from the usual Android list. The
> first does not exist on Ubuntu 22.04 or 24.04 (`libncurses-dev` covers it); the other two are
> pre-Android-10 leftovers that TWRP 12.1 does not use.

### 3. Sync the sources

```bash
mkdir -p ~/android/twrp && cd ~/android/twrp

repo init -u https://github.com/minimal-manifest-twrp/platform_manifest_twrp_aosp.git \
          -b twrp-12.1 --depth=1

repo sync -c -j8 --no-clone-bundle --current-branch
```

### 4. Place this tree

The build expects the tree at `device/xiaomi/evergo`.

```bash
cd ~/android/twrp
git clone -b fox_12.1 \
  https://github.com/yeasinulhoquetuhin/OrangeFox-evergo.git \
  device/xiaomi/evergo
```

`AndroidProducts.mk` registers exactly one lunch target, so this is the only product to select:

```
COMMON_LUNCH_CHOICES := twrp_evergo-eng
```

### 5. Build

```bash
cd ~/android/twrp
export ALLOW_MISSING_DEPENDENCIES=true
source build/envsetup.sh
lunch twrp_evergo-eng

mka recoveryimage
```

`mka recoveryimage` is the target for a device with a dedicated `recovery` partition. For a
device whose stock recovery lives in the boot ramdisk the target would be `mka bootimage`, and
`mka vendorbootimage` for vendor_boot — this one is the first case.

<details>
<summary>Other useful invocations</summary>

```bash
# Clean, then rebuild
mka clean
mka recoveryimage

# Build only this module, much faster for iteration
mmm device/xiaomi/evergo/recovery/

# A full `mka twrp_evergo-eng` builds the whole image tree, not just recovery
mka twrp_evergo-eng
```

</details>

### 6. Collect the output

```bash
ls -lh out/target/product/evergo/
```

| Artefact | Path |
| :--- | :--- |
| Recovery image | `out/target/product/evergo/recovery.img` |
| Packaged ZIP | `out/target/product/evergo/*.zip` |
| Build log | `out/target/product/evergo/*.log` |
| Boot image | `out/target/product/evergo/boot.img` |
| Build log | `out/target/product/evergo/*.log` |

The output directory is keyed off `PRODUCT_DEVICE` (`evergo`) in `twrp_evergo.mk`, not
`PRODUCT_NAME` (`twrp_evergo`). The `_twrp_` prefix in the lunch target is the TWRP naming
convention that the `twrp-12.1` manifest requires — see the note above.

`mka recoveryimage` produces `recovery.img` and a packaged ZIP. Copy the ZIP off the build
machine — that is the file you flash.

---

## Flashing the Recovery

> [!CAUTION]
> Read this before proceeding. Flashing the wrong partition, or with the wrong fastboot binary,
> can hard-brick a MediaTek device. Back up `/data` first. This is a **recovery-as-boot** device
> — the image goes to the **`recovery`** partition, **not** `boot`.

### Pre-flight

1. Battery above 50%, on a known-good cable.
2. Back up `/data`.
3. Get the **MediaTek fastboot binary** for your host OS. This is a MediaTek Download Station
   tool; the Android SDK `fastboot` does not reliably speak the MTK protocol on this SoC.
4. Boot to **stock recovery**, then issue fastboot from there.
5. Never interrupt the hardware preloader.

### From stock recovery

```bash
adb devices
adb reboot bootloader
```

At the fastboot prompt:

```
fastboot flash recovery <recovery>.zip
fastboot reboot recovery
```

### From Linux, with MTK fastboot

```bash
./fastboot flash recovery <recovery>.zip
./fastboot reboot recovery
```

### From OrangeFox already booted

Use **Advanced → Flash Current Slot**, or the on-device **Terminal**:

```
fastboot flash recovery /path/to/recovery.img
```

> [!IMPORTANT]
> If TWRP was previously installed, do **not** run `fastboot --disable-verity flash` and do
> not wipe `vbmeta` on this device. AVB is properly wired here — `BOARD_AVB_ENABLE := true`, and
> `vbmeta`, `vbmeta_system`, `vbmeta_vendor` are all slotted. Disabling it unnecessarily causes
> boot loops on some Android 13 payloads.

---

## Post-Flash Setup

1. Boot the recovery: hold **Power + Volume Up** until the OrangeFox logo appears.
2. **Format Data.** This is mandatory for FBE with v2 metadata encryption; a stock-encrypted
   `/data` will not mount otherwise. `Advanced → Wipe Format → Data`.
3. **Reboot → System** and let it settle through first boot.
4. If the screen is dim after a wipe, brightness control is panel-accurate: path
   `/sys/class/leds/lcd-backlight/brightness`, maximum **2047**.

---

## Build Variables Reference

Every value OrangeFox reads comes from `vendorsetup.sh`. The load-bearing ones:

| Variable | Value | Purpose |
| :--- | :--- | :--- |
| `FDEVICE` | `evergo` | The device this tree claims |
| `FOX_VERSION` | `R11.1_0` | Version string baked into the build |
| `FOX_VARIANT` | `S` | OrangeFox variant |
| `OF_MAINTAINER` | `Sushrut1101` | Maintainer string in the tree |
| `OF_TARGET_DEVICES` | `evergo,evergreen,opal` | Accepted build targets (inherited, not a support claim) |
| `TARGET_DEVICE_ALT` | `evergreen,opal` | Alternative codenames (inherited) |
| `FOX_ENABLE_APP_MANAGER` | `1` | App Manager in the GUI |
| `OF_USE_MAGISKBOOT_FOR_ALL_PATCHES` | `1` | Magisk-boot for every patch target |
| `OF_VIRTUAL_AB_DEVICE` | `1` | Virtual A/B handling |
| `OF_DYNAMIC_FULL_SIZE` | `9126805504` | Super size — must match `BoardConfig.mk` |
| `OF_DONT_PATCH_ENCRYPTED_DEVICE` | `1` | Skip patching while `/data` is encrypted |
| `OF_IGNORE_LOGICAL_MOUNT_ERRORS` | `1` | Tolerate logical-mount failures |
| `OF_NO_TREBLE_COMPATIBILITY_CHECK` | `1` | Skip the Treble check |
| `OF_VANILLA_BUILD` | `1` | Suppress MIUI patching warnings |
| `OF_NO_MIUI_PATCH_WARNING` | `1` | Suppress the MIUI warning banner |
| `OF_ENABLE_LPTOOLS` | `1` | Ship `lpunpack` / `lpdump` |
| `OF_HIDE_NOTCH` | `1` | Notch-aware layout |
| `OF_CLOCK_POS` | `1` | Clock placement |
| `OF_FL_PATH1` | `/system/flashlight` | Flashlight node path |
| `OF_UNBIND_SDCARD_F2FS` | `1` | Unbind `/sdcard` before f2fs operations |
| `OF_DELETE_AROMAFM` | `1` | Do not ship the AromaFM installer |
| `FOX_RECOVERY_SYSTEM_PARTITION` | `/dev/block/mapper/system` | Correct dynamic-partition node |
| `FOX_RECOVERY_VENDOR_PARTITION` | `/dev/block/mapper/vendor` | Correct dynamic-partition node |
| `FOX_RECOVERY_BOOT_PARTITION` | `…/by-name/boot` | Boot partition path |
| `OF_USE_GREEN_LED` | `0` | Use the white/other LED |
| `FOX_USE_SPECIFIC_MAGISK_ZIP` | `~/Magisk/Magisk-v25.2.zip` | Magisk to embed |

> [!CAUTION]
> `OF_DYNAMIC_FULL_SIZE` **must** stay in sync with `BOARD_SUPER_PARTITION_SIZE` and
> `BOARD_XIAOMI_DYNAMIC_PARTITIONS_SIZE` in `BoardConfig.mk` (`9126805504`). If they drift,
> OrangeFox reports an incorrect internal-storage size and repartitioning refuses to run.

---

## Known Constraints

> [!WARNING]
> **Do not remove `TW_NO_FASTBOOT_BOOT := true`.**
> This device cannot boot a recovery image via `fastboot boot` — the MediaTek preloader does not
> hand off that way. The variable stops the build scripts from generating a `fastboot boot` step.
> Removing it yields a recovery that flashes successfully and then fails to boot.

| Constraint | Detail |
| :--- | :--- |
| SELinux | The kernel cmdline forces `androidboot.selinux=permissive`. Intentional — enforcing policy breaks the TEE/keymaster decryption path. |
| Prebuilt kernel | The kernel is a stock `Image.gz`, repackaged. This tree cannot build a kernel. |
| Recovery-as-boot | `BOARD_USES_RECOVERY_AS_BOOT := true` — flash to `recovery`, not `boot`. |
| UFS only | The MediaTek boot-control HAL has eMMC support removed. There is no eMMC variant of this device. |
| `product` in the fstab | `/product` is a real separate logical partition here and is declared `first_stage_mount`. |
| Licence boundary | `*.mk`, `*.bp` and `*.cpp` are Apache-2.0. Everything under `prebuilt/`, `recovery/root/system/bin/` and `recovery/root/vendor/` is proprietary — see [License and Disclaimer](#license-and-disclaimer). |
| Touch input | `hbtp_vm` is in `TW_INPUT_BLACKLIST`; that virtual device must not receive events. |
| `PLATFORM_SECURITY_PATCH` | `2127-12-31` is a sentinel inherited from the platform tree, not a real patch date. |
| No APEX support | `TW_EXCLUDE_APEX := true` — this device has no APEX partitions. |
| No TWRP app | `TW_EXCLUDE_TWRPAPP := true`; use the built-in App Manager. |
| End-of-life OS | Stock OS stopped at HyperOS 1 (Android 13). No further security patches. |

---

## Component Provenance

| Component | Source | Licence |
| :--- | :--- | :--- |
| Boot Control HAL | AOSP `hardware/interfaces/boot/1.2`, MediaTek implementation | Apache-2.0, see `bootctrl/NOTICE` |
| `mtk_plpath_utils` | MediaTek public preloader utilities | Apache-2.0 |
| `init.recovery.usb.rc` | AOSP `system/core` (`garden` branch, per commit history) | Apache-2.0 |
| `ueventd.mt6833.rc` | MediaTek MT6833 reference `ueventd` | MediaTek terms |
| `init.recovery.microtrust.rc` | MediaTek microtrust TEE reference | MediaTek terms |
| `vendor/thh/ta/*.ta` | MediaTek trusted applications | Proprietary |
| `libTEECommon.so`, `libteei_daemon_vfs.so` | MediaTek | Proprietary |
| `*keymaster*.so`, `*gatekeeper*.so` | MediaTek | Proprietary |
| `vendor/firmware/aw869x_*` | Awinic haptic waveforms | Proprietary |
| `vendor/firmware/novatek_ts_*` | Novatek touchscreen firmware | Proprietary |
| `vendor/firmware/mt6631_*`, `mt6635_*` | MediaTek FM tuner firmware | Proprietary |
| `prebuilt/Image.gz` | Xiaomi stock kernel | Proprietary |
| `prebuilt/dtb.img`, `dtbo.img` | Xiaomi stock device tree | Proprietary |
| Device tree, build system, ramdisk overlay | This repository | Apache-2.0 |

---

## Troubleshooting

| Symptom | Likely cause | Fix |
| :--- | :--- | :--- |
| Bootloops back to fastboot | `vbmeta` mismatch after patching | Flash stock `vbmeta*`, or skip the patch step for those partitions |
| `/data` will not mount, "decryption failed" | The base tree does not implement FDE | Expected on a `twrp-12.1` build — that manifest states FDE decryption is not presently supported. A wipe will be required |
| Black screen, recovery is running | Brightness at 0 after wipe | `echo 1200 > /sys/class/leds/lcd-backlight/brightness` |
| Wrong internal-storage size | `OF_DYNAMIC_FULL_SIZE` desynced | Re-sync with `BoardConfig.mk` |
| `fastboot boot` fails silently | Expected | `TW_NO_FASTBOOT_BOOT` is correct. Flash to `recovery` instead |
| No preloader A/B detection | `mtk_plpath_utils` did not run | Check `/dev/block/mapper/pl_a` exists |
| Touch dead | `hbtp_vm` receiving events | Confirm `TW_INPUT_BLACKLIST := "hbtp_vm"` is set |
| Build fails on Soong namespace | Tree in the wrong path | Must live at `device/xiaomi/evergo` |
| Black or washed-out colours | Recovery assumes RGBX_8888 | Expected — the panel is IPS LCD, not AMOLED |

On-device diagnostics, from OrangeFox **Terminal**:

```bash
getprop | grep -E 'ro.(build|product|boot|recovery|crypto|vendor)'
dmesg | grep -iE 'tee|keymaster|gatekeeper|decrypt'
ls -l /dev/block/mapper/
cat /proc/cmdline
ls /sys/class/leds/
```

Build-time diagnostics — `vendorsetup.sh` dumps the tree's own variables into this file when it is
set, so it works with either build system:

```bash
export FOX_BUILD_LOG_FILE=~/fox-build.log
mka recoveryimage
grep -E '^(FOX|OF_|TARGET_|TW_)' ~/fox-build.log
```

---

## Contributing

1. **Open an issue first** for anything non-trivial. Many MediaTek fixes look device-specific but
   turn out to be board-generic.
2. **One logical change per commit**, message style matching the existing history:
   `evergo: <what you did>`.
3. **Never** commit:
   - personal data, logs, or anything from `/data`
   - a different Magisk binary than the one referenced, without saying so
   - changes to `prebuilt/` unless the blob is genuinely being upgraded
4. If you change partition sizing, change it in **both** places and say so explicitly.
5. If you touch the boot-control HAL, state which storage type you tested on. UFS only.

---

## Credits

- **[OrangeFox Recovery Project](https://github.com/OrangeFoxRecovery)** — the recovery
  framework, build system and UI this tree targets, and the original `device_xiaomi_evergo`.
- **[Sushrut1101](https://github.com/Sushrut1101)** — author and maintainer of the original
  `evergo` tree. Every commit in the imported history is theirs.
- **[AOSP](https://source.android.com)** — boot-control HAL interfaces, USB gadget scripts,
  `ueventd`, build system.
- **[TWRP](https://github.com/teamwin)** — device-tree conventions, the `twrp.flags` partition
  format, and the `TW_*` configuration vocabulary this tree inherits.
- **[MediaTek](https://www.mediatek.com)** — preloader path utilities, TEE/microtrust
  integration, boot-control implementation, trusted applications.
- **[LineageOS](https://www.lineageos.org)** — MT6833 platform reference for the ramdisk overlay
  and `PRODUCT_VNDK_VERSION` conventions.
- **The wider Xiaomi device-tree community** — dynamic-partition and Treble conventions.
- **Every contributor** in the [commit history](https://github.com/yeasinulhoquetuhin/OrangeFox-evergo/commits/fox_12.1)
  of the imported tree.
- **[Wikipedia — Redmi Note 11](https://en.wikipedia.org/wiki/Redmi_Note_11)** — the codename
  table that distinguishes `evergo` from `evergreen` and `opal`.
- **[GSMArena](https://www.gsmarena.com/)** and the Xiaomi official spec pages — hardware
  specifications.

---

## License and Disclaimer

### Source licence

The build configuration, Makefiles, Blueprints, ramdisk `rc` / `fstab` / `ueventd` overlay,
`bootctrl/` and `mtk_plpath_utils/` sources are licensed under the
[Apache License, Version 2.0](http://www.apache.org/licenses/LICENSE-2.0).

### Proprietary components — not Apache-2.0

Redistributed as-is from Xiaomi and MediaTek, under their own terms:

- `prebuilt/Image.gz`, `prebuilt/dtb.img`, `prebuilt/dtbo.img`
- `recovery/root/system/bin/*`
- `recovery/root/vendor/lib64/**`
- `recovery/root/vendor/firmware/**`
- `recovery/root/vendor/thh/**`
- `recovery/root/system/etc/vintf/manifest.xml`, `recovery/root/vendor/etc/vintf/manifest.xml`
- `recovery/root/ueventd.mt6833.rc`, `recovery/root/init.recovery.microtrust.rc`

Redistributing a build of this tree means redistributing proprietary firmware. Compliance with
the applicable licences is your responsibility.

### Disclaimer

> [!CAUTION]
> **Not affiliated with, endorsed by, or sponsored by Xiaomi, Redmi, POCO, MediaTek, or the
> OrangeFox Recovery Project.**
>
> Custom recoveries modify system partitions. A failed flash, a bad patch, or a
> manufacturer-side bootloader update can leave a device **permanently unbootable** — including
> devices with an unlockable bootloader, because MediaTek's AVB chain and boot-region handling
> are unforgiving.
>
> You do all of this **at your own risk**. The maintainers accept no liability for any damage
> arising from the use of these files. Back up your data before you start. If you are not
> comfortable recovering a device from a hard brick, do not start.

---

<div align="center">

**Yeasinul Hoque Tuhin** — [tuhinbro.com](https://tuhinbro.com) · [Telegram @TuhinBroh](https://t.me/TuhinBroh)

<sub>
[Report a bug](https://github.com/yeasinulhoquetuhin/OrangeFox-evergo/issues) ·
[Commit history](https://github.com/yeasinulhoquetuhin/OrangeFox-evergo/commits/fox_12.1)
</sub>

</div>
