<div align="center">

# 🊊 OrangeFox Recovery — Evergo

### **POCO M4 Pro 5G** · **Redmi Note 11T 5G** · **Redmi Note 11S 5G** · **Redmi Note 11 5G**

**MediaTek Dimensity 700 (MT6833) · Virtual A/B · Android 12 (S) platform · FBE-encrypted aware**

[![Branch](https://img.shields.io/badge/branch-fox__12.1-0073b5?style=flat-square&logo=git&logoColor=white)](https://github.com/yeasinulhoquetuhin/OrangeFox-evergo/tree/fox_12.1)
[![License](https://img.shields.io/badge/license-Apache--2.0-0073b5?style=flat-square&logo=apache&logoColor=white)](https://www.apache.org/licenses/LICENSE-2.0)
[![C++](https://img.shields.io/badge/C++-00599C?style=flat-square&logo=c%2B%2B&logoColor=white)](https://isocpp.org/)
[![Shell](https://img.shields.io/badge/Shell-4EAA25?style=flat-square&logo=gnubash&logoColor=white)](https://www.gnu.org/software/bash/)
[![SoC](https://img.shields.io/badge/SoC-MT6833-FF6D00?style=flat-square&logo=mediatek&logoColor=white)](https://www.mediatek.com/)
[![Device](https://img.shields.io/badge/device-evergo-EA4B4C?style=flat-square)](https://github.com/yeasinulhoquetuhin/OrangeFox-evergo)
[![Recovery](https://img.shields.io/badge/OrangeFox-R11.1-FF6D00?style=flat-square&logo=android&logoColor=white)](https://github.com/OrangeFoxRecovery)
[![Maintainer](https://img.shields.io/badge/maintainer-Sushrut1101-6E5494?style=flat-square&logo=github&logoColor=white)](https://github.com/OrangeFoxRecovery)
[![Last&nbsp;Commit](https://img.shields.io/github/last-commit/yeasinulhoquetuhin/OrangeFox-evergo?style=flat-square&logo=github&logoColor=white)](https://github.com/yeasinulhoquetuhin/OrangeFox-evergo/commits/fox_12.1)
[![Size](https://img.shields.io/github/repo-size/yeasinulhoquetuhin/OrangeFox-evergo?style=flat-square&logo=github&logoColor=white)](https://github.com/yeasinulhoquetuhin/OrangeFox-evergo)
[![Languages](https://img.shields.io/github/languages/count/yeasinulhoquetuhin/OrangeFox-evergo?style=flat-square&logo=github&logoColor=white)](https://github.com/yeasinulhoquetuhin/OrangeFox-evergo)
[![License&nbsp;Check](https://img.shields.io/badge/licence-headers-included-success?style=flat-square&logo=githubactions&logoColor=white)](https://github.com/yeasinulhoquetuhin/OrangeFox-evergo)

A production-oriented MediaTek MT6833 device tree for **[OrangeFox Recovery](https://github.com/OrangeFoxRecovery) R11.1**, built on a **Virtual A/B** device tree with full **FBE metadata-decryption** and **Android Verified Boot** awareness.

</div>

---

## 📑 Table of Contents

- [Overview](#-overview)
- [Supported Devices](#-supported-devices)
- [Device Specifications](#-device-specifications)
- [Highlights](#-highlights)
- [Recovery Build Configuration](#-recovery-build-configuration)
- [Repository Layout](#-repository-layout)
- [Partition Layout](#-partition-layout)
- [Building from Source](#-building-from-source)
  - [1 · Host Requirements](#1--host-requirements)
  - [2 · Host Dependencies](#2--host-dependencies)
  - [3 · Sync the Sources](#3--sync-the-sources)
  - [4 · Place This Tree](#4--place-this-tree)
  - [5 · Build](#5--build)
  - [6 · Collect the Output](#6--collect-the-output)
- [Flashing the Recovery](#-flashing-the-recovery)
- [Post-Flash Setup](#-post-flash-setup)
- [Build Variables Reference](#-build-variables-reference)
- [Important Notes & Known Constraints](#-important-notes--known-constraints)
- [Provenance of Imported Components](#-provenance-of-imported-components)
- [Troubleshooting & Diagnostics](#-troubleshooting--diagnostics)
- [Contributing](#-contributing)
- [Credits & Acknowledgements](#-credits--acknowledgements)
- [License & Disclaimer](#-license--disclaimer)

---

## 🔍 Overview

This repository is the **device tree** — not the recovery itself. It is the set of build
configuration, ramdisk overlays, MediaTek-specific userspace bits and prebuilt firmware that
lets the generic OrangeFox Recovery source compile into a bootable image for the
**`evergo`** platform.

| | |
| :--- | :--- |
| **Recovery** | OrangeFox R11.1, variant `S` |
| **Platform** | `mt6833P` (MediaTek Dimensity 810) |
| **Target OS level** | Android 12 (S) — VNDK 31, shipping API level 30 |
| **Update model** | Virtual A/B + dynamic partitions |
| **Encryption** | FBE / metadata encryption, `v2+inlinecrypt_optimized` |
| **Userdata filesystem** | f2fs (ext4 supported as alternative) |
| **Kernel** | Prebuilt `Image.gz` (stock kernel, repackaged) |
| **Boot control** | MediaTek `android.hardware.boot@1.2` HAL |
| **Verified boot** | AVB enabled |
| **Maintainer** | Sushrut1101 / OrangeFox Project |
| **Codename** | `evergo` (aliases: `evergreen`, `opal`) |

> [!NOTE]
> There is deliberately **no CI badge** on this repository. Compiling an AOSP-based recovery
> requires roughly **16–32 GB of RAM** and **300–500 GB of disk**, which no stock GitHub-hosted
> runner provides. A green badge here would be misleading. Build locally with the instructions
> below.

---

## 📱 Supported Devices

All four retail names are the same physical platform (`evergo`) and share this tree unmodified.

| Device name | Model | SoC | Marketing name |
| :--- | :--- | :--- | :--- |
| POCO M4 Pro 5G | `M2101K6G` | MT6833 | Redmi Note 11T 5G |
| Redmi Note 11T 5G | `M2101K6G` | MT6833 | — |
| Redmi Note 11S 5G | `M2101K6G` | MT6833 | — |
| Redmi Note 11 5G | `M2101K6GI` | MT6833 | — |

**Codename aliases** accepted by the build system (`OF_TARGET_DEVICES`):

```
evergo, evergreen, opal
```

> [!WARNING]
> Do **not** attempt this tree on `spes`, `spesn`, `apollo`, `muni` or any other MT6833P Redmi
> board. The `bootctrl` HAL and the ramdisk overlay are specific to this board's UFS boot-region
> layout.

---

## 🔧 Device Specifications

| Category | Specification |
| :--- | :--- |
| **SoC** | MediaTek Dimensity 810 (`MT6833P`), 6 nm |
| **CPU** | 8 × ARM Cortex-A55 (2 × 2.2 GHz big + 6 × 2.0 GHz little) |
| **GPU** | ARM Mali-G57 MC2 |
| **Display** | 6.6″ AMOLED, 1080 × 2400 (FHD+), 90 Hz, HDR10 |
| **RAM** | 6 GB / 8 GB LPDDR4X |
| **Storage** | 64 / 128 / 256 GB UFS 2.2 + hybrid microSD |
| **Rear camera** | 50 MP f/1.9 + 8 MP f/2.2 ultra-wide + 2 MP f/2.4 macro |
| **Front camera** | 16 MP |
| **Battery** | 5000 mAh, 33 W wired charging |
| **Network** | 5G, dual SIM, Wi-Fi 5 / Bluetooth 5.1 |
| **Stock OS** | Android 11 (MIUI 12.5), OTA-upgradable to Android 13 |
| **Security** | MediaTek microtrust TEE, in-house Keymaster 4.1 & Gatekeeper |
| **Storage type** | UFS 2.2 (**no eMMC variant is supported**) |

---

## ✨ Highlights

### Recovery experience

- 🎨 **OrangeFox R11.1** UI, variant `S`, notch-aware layout (`OF_HIDE_NOTCH=1`) tuned for the
  1080 × 2400 panel — 48 px status-bar insets left and right.
- 📦 **App Manager** enabled (`FOX_ENABLE_APP_MANAGER=1`) — install APKs to userdata without a
  full package manager.
- 🧰 **Toolbox** built in (`TW_USE_TOOLBOX=1`).
- 🖊️ **nano** as the built-in file editor (`FOX_USE_NANO_EDITOR=1`).
- 🐚 **Bash as `ash`** (`FOX_USE_BASH_SHELL=1`, `FOX_ASH_IS_BASH=1`) — a real shell with
  globbing, arrays and `[[ ]]`, not BusyBox's stripped-down applets.
- 📜 **Screen brightness** exposed with the panel's true maximum of **2047** (default 1200).
- 💾 **Backup & restore** over the standard internal-storage + boot set.

### Correctness features

- 🔐 **Full FBE decryption** — `TW_INCLUDE_CRYPTO`, `TW_INCLUDE_CRYPTO_FBE` and
  `TW_INCLUDE_FBE_METADATA_DECRYPT` are all enabled, including **Android 13** metadata blobs.
- 🔏 **AVB-aware** — verified-boot state is respected; `vbmeta`, `vbmeta_system` and
  `vbmeta_vendor` are exposed for selective re-patching.
- 🧩 **Dynamic partition aware** — the super partition is correctly sized at
  **9 126 805 504 bytes** and mirrored in `OF_DYNAMIC_FULL_SIZE`, so "Internal Storage" shows
  the real capacity instead of a wrong one.
- 🎟️ **Virtual A/B snapshots** supported with `OF_VIRTUAL_AB_DEVICE=1`.
- 🧰 **Magisk-boot patching** for every patch target
  (`OF_USE_MAGISKBOOT_FOR_ALL_PATCHES=1`), with the default MediaTek binary.
- ⏩ **Virtual A/B + `fastbootd`** — `fastbootd` and the mock fastboot HAL are shipped, so
  dynamic-partition resizing and userdata reformat work from recovery.
- 🟢 **Vanilla build** — `OF_VANILLA_BUILD=1` suppresses all MIUI-specific patching warnings,
  which is the correct behaviour on this device.
- 📎 **Fuse / external storage** — microSD (`/sdcard1`) and USB-OTG (`/usb_otg`) are both
  handled as removable storage.

### MediaTek platform integration

- 🚦 **Preloader A/B plumbing** — `mtk_plpath_utils` plus the symlink farm in
  `init.recovery.mt6833.rc` recreate `preloader_a`/`preloader_b`, the `*_emmc_*` / `*_ufs_*`
  aliases and the `preloader_raw_*` device-mapper links, so slot-aware OTA bookkeeping works.
- 🧬 **MediaTek Boot Control HAL 1.2** — the stock MTK implementation, built from source in
  `bootctrl/` rather than dropped in as a blob, so A/B slot selection is genuinely functional.
  UFS-only; the eMMC path was deliberately removed.
- 🔐 **TEE / microtrust** — `teei_daemon` service definition, the full `init.recovery.microtrust.rc`
  layout routine, and the proprietary trusted applications under `vendor/thh/ta/` are bundled so
  Keymaster and Gatekeeper can answer during decryption.
- 👆 **Haptics & touch firmware** — the AW8697 haptic waveform bank and the Novatek touchscreen
  firmware are shipped in `vendor/firmware/`, so haptics and touch work.
- 🔌 **USB** — MediaTek `musb-hdrc` configfs gadget setup supporting `adb`, `sideload`, `mtp` and
  `mtp,adb`.

---

## ⚙️ Recovery Build Configuration

| Setting | Value | Source |
| :--- | :--- | :--- |
| Recovery version | `R11.1_0` | `vendorsetup.sh` → `FOX_VERSION` |
| Variant | `S` | `vendorsetup.sh` → `FOX_VARIANT` |
| Theme | `portrait_hdpi` | `BoardConfig.mk` → `TW_THEME` |
| Shipping API | `30` | `device.mk` → `PRODUCT_SHIPPING_API_LEVEL` |
| VNDK | `31` | `device.mk` → `PRODUCT_TARGET_VNDK_VERSION` |
| Platform version | `99.87.36` | `BoardConfig.mk` → `PLATFORM_VERSION` |
| Security patch | `2127-12-31` | `BoardConfig.mk` → `PLATFORM_SECURITY_PATCH` |
| Screen height | `2400` | `vendorsetup.sh` → `OF_SCREEN_H` |
| Status bar height | `100` | `vendorsetup.sh` → `OF_STATUS_H` |
| Max brightness | `2047` | `BoardConfig.mk` → `TW_MAX_BRIGHTNESS` |
| Default brightness | `1200` | `BoardConfig.mk` → `TW_DEFAULT_BRIGHTNESS` |
| Dynamic super size | `9126805504` | `BoardConfig.mk` / `OF_DYNAMIC_FULL_SIZE` |
| Kernel cmdline | `bootopt=64S3,32N2,64N2` | `BoardConfig.mk` → `BOARD_KERNEL_CMDLINE` |
| Boot image header | `v2`, 2048-byte pages | `BoardConfig.mk` |
| Kernel base | `0x40078000` | `BoardConfig.mk` |
| Ramdisk offset | `0x11088000` | `BoardConfig.mk` |
| Tags offset | `0x07c08000` | `BoardConfig.mk` |

---

## 📂 Repository Layout

```
OrangeFox-evergo/
├── Android.bp                          # Soong namespace root
├── Android.mk                          # Legacy makefile entry (TARGET_DEVICE gate)
├── AndroidProducts.mk                  # Lunch registration  →  twrp_evergo-eng
├── BoardConfig.mk                      # Platform, kernel, partitions, TWRP/OF flags
├── device.mk                           # Packages, HALs, update-engine, A/B plumbing
├── twrp_evergo.mk                      # Product definition  (PRODUCT_NAME=twrp_evergo)
├── system.prop                         # Crypto, gatekeeper, keymaster, TEE, USB props
├── vendorsetup.sh                      # OrangeFox build variables (all OF_* / FOX_*)
│
├── prebuilt/                           # ── Stock firmware blobs (proprietary)
│   ├── Image.gz                        #   Kernel, 14.9 MB, gzip-compressed
│   ├── dtb.img                         #   Device Tree Blob
│   └── dtbo.img                        #   Device Tree Overlay image
│
├── bootctrl/                           # ── MediaTek Boot Control HAL 1.2 (Apache-2.0)
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
├── mtk_plpath_utils/                   # ── Preloader A/B path utilities
│   ├── Android.bp                      #   mtk_plpath_utils (cc_binary)
│   └── mtk_plpath_utils.cpp            #   Recreates preloader symlinks & device nodes
│
└── recovery/root/                      # ── Ramdisk overlay
    ├── init.recovery.mt6833.rc         #   MTK A/B symlinks, boot HAL, keystore, TEE stop rules
    ├── init.recovery.microtrust.rc     #   teei_daemon, RPMB, /data/vendor/thh layout
    ├── init.recovery.usb.rc            #   musb-hdrc configfs gadget (adb / sideload / mtp)
    ├── ueventd.mt6833.rc               #   /dev node permissions & ownership
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
    │       ├── brightness              #   → symlink to torch_brightness
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

## 💾 Partition Layout

### Boot & verified boot

| Partition | Type | Slot-aware | Notes |
| :--- | :--- | :---: | :--- |
| `boot` | emmc | ✅ | Recovery-as-boot, header v2, backupable |
| `recovery` | emmc | ✅ | Recovery image target |
| `dtbo` | emmc | — | Prebuilt `dtbo.img`, flashable |
| `vbmeta` | emmc | — | AVB top-level |
| `vbmeta_system` | emmc | ✅ | AVB, `avb=vbmeta` |
| `vbmeta_vendor` | emmc | ✅ | AVB |
| `boot_para` | emmc | — | MediaTek boot parameters |

### Dynamic partitions

| Partition | Type | Notes |
| :--- | :--- | :--- |
| `super` | — | 9 126 805 504 B total, groups: `xiaomi_dynamic_partitions` |
| `system` | ext4 | logical, `first_stage_mount`, AVB via `vbmeta_system` |
| `vendor` | ext4 | logical, `first_stage_mount`, AVB |
| `product` | ext4 | logical, `first_stage_mount`, AVB |
| `system_a` / `_b` (etc.) | — | Virtual A/B slot suffixes |

The partition list is `system vendor product` (`BOARD_XIAOMI_DYNAMIC_PARTITIONS_PARTITION_LIST`),
with 9 122 611 200 B reserved for them
(`BOARD_XIAOMI_DYNAMIC_PARTITIONS_SIZE`) inside the 9 126 805 504 B super
(`BOARD_SUPER_PARTITION_SIZE`). Individual logical sizes are computed at build time by
`build_super_image`, not hard-coded here.

### Data & metadata

| Partition | Mount | Filesystem | Notes |
| :--- | :--- | :--- | :--- |
| `userdata` | `/data` | f2fs | `fileencryption=aes-256-xts:aes-256-cts:v2+inlinecrypt_optimized`, `fsverity`, `reservedsize=128m` |
| `metadata` | `/metadata` | ext4 | `keydirectory=/metadata/vold/metadata_encryption` |
| `rescue` | `/cache` | ext4 | OrangeFox cache |
| `misc` | `/misc` | emmc | Boot-control state |

### MediaTek firmware & secure partitions

<details>
<summary><b>Full list (click to expand)</b></summary>

All entries below are taken from the ramdisk `fstab.mt6833` unless marked otherwise.

| Group | Partitions |
| :--- | :--- |
| **Bootloader** | `lk`, `lk2`, `para`, `boot_para` |
| **Security / TEE** | `seccfg`, `expdb`, `tee1`, `tee2`, `scp1`, `scp2`, `sspm_1`, `sspm_2`, `otp`, `efuse` |
| **Persistence** | `nvram`, `proinfo`, `frp` (→ `/persistent`) |
| **AVB** | `vbmeta`, `vbmeta_system`, `vbmeta_vendor` |
| **DSP / firmware** | `md1img`, `md1dsp`, `md1arm7`, `md3img`, `gz1`, `gz2`, `spmfw` |
| **Regional / data** | `persist`, `protect1` (→ `/mnt/vendor/protect_f`), `protect2` (→ `/mnt/vendor/protect_s`), `nvdata`, `nvcfg` |
| **Misc** | `logo`, `pi_img`, `odmdtbo`, `dtbo` |
| **PFMU (PMIC)** | `dpm_1`, `dpm_2`, `mcupm_1`, `mcupm_2` |

The following are declared only in `ueventd.mt6833.rc` (permission-only, not mounted):

| Group | Partitions |
| :--- | :--- |
| **Extra block nodes** | `misc2`, `secro`, `md1img_a`, `md1img_b` |

A few partitions appear **only** in `recovery/root/system/etc/twrp.flags`, which drives the
OrangeFox **Wipe / Flash** GUI rather than the mount table. They are listed in the GUI but are
not mounted by the recovery itself:

`cust`, `cust_image`, `persist_image`, `audio_dsp`, `protect_f`, `protect_s`

> [!NOTE]
> The `*_image` entries in `twrp.flags` (e.g. `persist_image`, `cust_image`) exist so you can
> back up and re-flash the underlying blocks as images. They intentionally point at the *same*
> device node as the plain entry (`by-name/persist`, `by-name/cust`) — pick one or the other in
> the GUI, not both.

</details>

---

## 🏗️ Building from Source

### 1 · Host Requirements

| Resource | Minimum | Recommended |
| :--- | :--- | :--- |
| **RAM** | 16 GB | 32 GB (use swap, add 8–16 GB) |
| **Disk** | 300 GB | 500 GB (both on SSD) |
| **CPU** | 8 threads | 16+ threads |
| **OS** | Ubuntu 20.04 / 22.04 / 24.04 LTS | Ubuntu 22.04 or 24.04 LTS |

> [!TIP]
> Build **on an SSD**. A spinner turns a ~4 hour build into an all-day one. If you are
> RAM-bound, put swap on the same SSD:
> ```bash
> sudo fallocate -l 16G /swapfile && sudo chmod 600 /swapfile
> sudo mkswap /swapfile && sudo swapon /swapfile
> echo '/swapfile none swap sw 0 0' | sudo tee -a /etc/fstab
> ```

### 2 · Host Dependencies

```bash
sudo apt update && sudo apt upgrade -y

sudo apt install -y \
  bc bison build-essential ccache curl flex g++-multilib gcc-multilib git git-lfs \
  gnupg gperf imagemagick lib32readline-dev lib32z1-dev libelf-dev liblz4-tool \
  libncurses5-dev libncurses-dev libpng-dev libssl-dev libxml2-dev libxml2-utils \
  lzop m4 make pngcrush python3 rsync schedtool squashfs-tools syslinux-utils \
  unzip xz-utils zlib1g-dev cpio lz4
```

Set up the required git identity and enable long paths:

```bash
git config --global user.name  "Your Name"
git config --global user.email "you@example.com"
git config --global core.longpaths true
```

### 3 · Sync the Sources

```bash
mkdir -p ~/android/lineage && cd ~/android/lineage

repo init -u https://github.com/OrangeFoxRecovery/platform_manifest.git \
          -b fox-12.1 --depth=1

repo sync -c -j8 --no-clone-bundle --current-branch
```

### 4 · Place This Tree

The build expects the device tree at `device/xiaomi/evergo` (this is what
`BoardConfig.mk` sets as `DEVICE_PATH`). Clone it straight into place:

```bash
git clone -b fox_12.1 \
  https://github.com/yeasinulhoquetuhin/OrangeFox-evergo.git \
  device/xiaomi/evergo
```

<details>
<summary><b>Building the primary alias instead (<code>evergreen</code> / <code>opal</code>)</b></summary>

The tree registers `evergo` as the canonical `FDEVICE` and accepts the other two names as
aliases:

```bash
git clone -b fox_12.1 \
  https://github.com/yeasinulhoquetuhin/OrangeFox-evergo.git \
  device/xiaomi/evergreen
```

`vendorsetup.sh` exports `OF_TARGET_DEVICES="evergo,evergreen,opal"`, so
`./build.sh evergreen` produces an identical image.

</details>

### 5 · Build

```bash
cd ~/android/lineage
source build/envsetup.sh

./build.sh evergo
```

The build script will ask for the variant — accept the default (`S`). For a fully
non-interactive build:

```bash
./build.sh evergo S
```

<details>
<summary><b>Useful build invocations</b></summary>

```bash
# Clean, then full rebuild
./build.sh evergo clean
./build.sh evergo

# Use the Magisk zip you supply yourself
export FOX_USE_SPECIFIC_MAGISK_ZIP=~/Magisk/Magisk-v25.2.zip
./build.sh evergo

# Keep the build log verbose
export FOX_BUILD_LOG_FILE=~/fox-build.log
./build.sh evergo

# Parallelise (defaults to your nproc)
export FOX_BUILD_MAXIMUM_JOBS=16
./build.sh evergo
```

</details>

### 6 · Collect the Output

```bash
# Inside the source tree
ls -lh out/target/product/evergo/
```

| Artefact | Path |
| :--- | :--- |
| Flashable ZIP | `out/target/product/evergo/*.zip` |
| Unpacked recovery | `out/target/product/evergo/recovery.img` |
| Boot image | `out/target/product/evergo/boot.img` |
| Build log | `out/target/product/evergo/*.log` |

The output directory is keyed off `PRODUCT_DEVICE` (`evergo`), **not** `PRODUCT_NAME`
(`twrp_evergo`) — the name in the folder is a historical artefact of the TWRP-style product
makefile this tree inherited and does not mean you are building TWRP.

The ZIP's own filename is generated by OrangeFox's `build.sh` from `FOX_VERSION` and
`FOX_VARIANT`, so it will contain `R11.1_0` and `S` in some order. Check the real name with
`ls` rather than assuming it.

Copy the ZIP off the machine — it is the file you flash.

---

## 📲 Flashing the Recovery

> [!CAUTION]
> **Read this before proceeding.** Flashing the wrong partition, or with the wrong fastboot
> binary, can hard-brick a MediaTek device. Back up `/data` first. This device uses
> **recovery-as-boot** — the recovery image goes to the **`recovery`** partition, **not** `boot`.

### Pre-flight checklist

1. Battery **above 50 %** and plugged into a known-good cable.
2. Back up `/data` (`adb backup`, or let OrangeFox do a full-format backup first).
3. Download the **correct MediaTek fastboot binary** for your host OS. This is a **device-specific**
   tool from MediaTek's Download Station — the Android SDK `fastboot` may not speak the MTK
   protocol reliably for this SoC.
4. Boot the device into **Stock Recovery**, then issue the fastboot command from there.
5. MediaTek devices use a **hardware preloader**. Never interrupt it.

### Flash (from stock recovery)

```bash
adb devices                       # confirm the device is visible
adb reboot bootloader
```

At the `FASTBOOT` / `fastboot #` prompt:

```
fastboot flash recovery <recovery>.zip
fastboot reboot recovery
```

### Flash (from Linux, with MTK fastboot)

```bash
./fastboot flash recovery <recovery>.zip
./fastboot reboot recovery
```

### Flash (from OrangeFox already booted)

Open **Advanced → Flash Current Slot** and select the recovery image, or use
**Terminal** on-device:

```
fastboot flash recovery /path/to/recovery.img
```

> [!IMPORTANT]
> If you previously had TWRP installed, **do not** `fastboot --disable-verity flash` or wipe
> `vbmeta` on this device. The tree has AVB properly wired (`BOARD_AVB_ENABLE := true`,
> vbmeta/vbmeta_system/vbmeta_vendor all slotted) and disabling it unnecessarily causes boot
> loops on some A13 payloads.

---

## 🧩 Post-Flash Setup

1. **Boot the recovery** — hold `Power + Volume Up` until the OrangeFox logo appears.
2. **Format Data** — this is mandatory for FBE/v2 metadata encryption. A stock-encrypted
   `/data` will not mount otherwise.
   `Advanced → Wipe Format → Data` (and `Encryption` if your tree exposes it).
3. **Reboot → System** and let it settle through first boot.
4. If the screen is dim, the brightness control is the panel-accurate one — the path is
   `/sys/class/leds/lcd-backlight/brightness` with a maximum of **2047**.

---

## 📋 Build Variables Reference

Every value OrangeFox reads comes from `vendorsetup.sh`. The load-bearing ones:

| Variable | Value | Purpose |
| :--- | :--- | :--- |
| `FDEVICE` | `evergo` | The device this tree claims |
| `FOX_VERSION` | `R11.1_0` | Version string baked into the build |
| `FOX_VARIANT` | `S` | OrangeFox variant |
| `OF_MAINTAINER` | `Sushrut1101` | Maintainer string |
| `OF_TARGET_DEVICES` | `evergo,evergreen,opal` | Accepted build targets |
| `TARGET_DEVICE_ALT` | `evergreen,opal` | Alternative codenames |
| `FOX_ENABLE_APP_MANAGER` | `1` | App Manager in the GUI |
| `OF_USE_MAGISKBOOT_FOR_ALL_PATCHES` | `1` | Magisk-boot for every patch target |
| `OF_VIRTUAL_AB_DEVICE` | `1` | Virtual A / B handling |
| `OF_DYNAMIC_FULL_SIZE` | `9126805504` | Super partition size (must match `BoardConfig.mk`) |
| `OF_DONT_PATCH_ENCRYPTED_DEVICE` | `1` | Skip patching when `/data` is encrypted |
| `OF_IGNORE_LOGICAL_MOUNT_ERRORS` | `1` | Tolerate logical-mount failures |
| `OF_NO_TREBLE_COMPATIBILITY_CHECK` | `1` | Skip the Treble check |
| `OF_VANILLA_BUILD` | `1` | Suppress MIUI patching warnings |
| `OF_NO_MIUI_PATCH_WARNING` | `1` | Suppress the MIUI warning banner |
| `OF_ENABLE_LPTOOLS` | `1` | Ship `lpunpack`/`lpdump` |
| `OF_HIDE_NOTCH` | `1` | Notch-aware layout |
| `OF_CLOCK_POS` | `1` | Clock placement |
| `OF_FL_PATH1` | `/system/flashlight` | Flashlight node path |
| `OF_UNBIND_SDCARD_F2FS` | `1` | Unbind `/sdcard` before f2fs ops |
| `OF_DELETE_AROMAFM` | `1` | Do not ship the AromaFM installer |
| `FOX_RECOVERY_SYSTEM_PARTITION` | `/dev/block/mapper/system` | Correct dynamic-partition node |
| `FOX_RECOVERY_VENDOR_PARTITION` | `/dev/block/mapper/vendor` | Correct dynamic-partition node |
| `FOX_RECOVERY_BOOT_PARTITION` | `…/by-name/boot` | Boot partition path |
| `OF_USE_GREEN_LED` | `0` | Use the white/other LED instead |
| `FOX_USE_SPECIFIC_MAGISK_ZIP` | `~/Magisk/Magisk-v25.2.zip` | Magisk to embed |

> [!CAUTION]
> `OF_DYNAMIC_FULL_SIZE` **must** stay in sync with
> `BOARD_SUPER_PARTITION_SIZE` / `BOARD_XIAOMI_DYNAMIC_PARTITIONS_SIZE` in `BoardConfig.mk`
> (`9126805504`). If they drift, OrangeFox will report an incorrect internal-storage size and
> repartitioning operations will refuse to run.

---

## ⚠️ Important Notes & Known Constraints

> [!WARNING]
> **Do not remove `TW_NO_FASTBOOT_BOOT := true`.**
> This device cannot boot a recovery image via `fastboot boot` — the MediaTek preloader does not
> hand off that way. The variable stops the build scripts from generating a `fastboot boot`
> step. Removing it produces a recovery that flashes successfully and then fails to boot.

Other constraints worth knowing:

| Constraint | Detail |
| :--- | :--- |
| **SELinux** | The kernel cmdline forces `androidboot.selinux=permissive`. A permissive recovery is intentional here — enforcing policy breaks the TEE/keymaster decryption path. |
| **Prebuilt kernel** | The kernel is a **stock** `Image.gz`, repackaged. This tree cannot build a kernel. |
| **Recovery-as-boot** | `BOARD_USES_RECOVERY_AS_BOOT := true` — flash to `recovery`, not `boot`. |
| **UFS only** | The MediaTek boot-control HAL has had eMMC support removed. There is no eMMC variant of this device. |
| **`product` in the fstab** | `/product` is a real, separate logical partition here and is declared `first_stage_mount`. Older trees omitted it. |
| **License boundary** | `BoardConfig.mk` and every `*.mk`/`*.cpp` file are Apache-2.0. Everything under `prebuilt/` and `recovery/root/system/bin/`, `recovery/root/vendor/` is **proprietary** — see [License & Disclaimer](#-license--disclaimer). |
| **Touch input** | `hbtp_vm` is in `TW_INPUT_BLACKLIST`; that virtual device must not receive events. |
| **`PLATFORM_SECURITY_PATCH`** | `2127-12-31` is a sentinel value inherited from the platform tree, not a real patch date. Do not read anything into it. |
| **No APEX support** | `TW_EXCLUDE_APEX := true` — this device has no APEX partitions. |
| **No TWRP app** | `TW_EXCLUDE_TWRPAPP := true`; use the built-in App Manager instead. |

---

## 📦 Provenance of Imported Components

This tree assembles code and blobs from several upstream projects. Honour their licences.

| Component | Source | Licence |
| :--- | :--- | :--- |
| Boot Control HAL | AOSP `hardware/interfaces/boot/1.2`, MTK implementation | Apache-2.0 (see `bootctrl/NOTICE`) |
| `mtk_plpath_utils` | MediaTek public preloader utilities | Apache-2.0 |
| `init.recovery.usb.rc` | AOSP `system/core` (`garden` branch, per commit history) | Apache-2.0 |
| `ueventd.mt6833.rc` | MediaTek MT6833 reference `ueventd` | MediaTek terms |
| `init.recovery.microtrust.rc` | MediaTek microtrust TEE reference | MediaTek terms |
| `vendor/thh/ta/*.ta` | MediaTek trusted applications | **Proprietary** |
| `libTEECommon.so`, `libteei_daemon_vfs.so` | MediaTek | **Proprietary** |
| `*keymaster*.so`, `*gatekeeper*.so` | MediaTek | **Proprietary** |
| `vendor/firmware/aw869x_*` | Awinic haptic driver waveforms | **Proprietary** |
| `vendor/firmware/novatek_ts_*` | Novatek touchscreen firmware | **Proprietary** |
| `vendor/firmware/mt6631_*`, `mt6635_*` | MediaTek FM tuner firmware | **Proprietary** |
| `prebuilt/Image.gz` | Xiaomi stock kernel | **Proprietary** |
| `prebuilt/dtb.img`, `dtbo.img` | Xiaomi stock device tree | **Proprietary** |
| Device tree layout, build system, ramdisk overlay | This repository | Apache-2.0 |

---

## 🩺 Troubleshooting & Diagnostics

| Symptom | Likely cause | Fix |
| :--- | :--- | :--- |
| Bootloops back to fastboot | `vbmeta` mismatch after patching | Flash stock `vbmeta*`, or disable the patch step for those partitions |
| `/data` will not mount, "decryption failed" | Metadata blobs too new | This tree includes Android 13 blobs. If it still fails, the device is on an unsupported encryption version |
| Black screen, recovery is running | Display brightness at 0 after wipe | Set brightness manually: `echo 1200 > /sys/class/leds/lcd-backlight/brightness` |
| Wrong internal-storage size | `OF_DYNAMIC_FULL_SIZE` desynced | Re-sync with `BoardConfig.mk` (see the caution box above) |
| `fastboot boot` fails silently | Expected | `TW_NO_FASTBOOT_BOOT` is correct for this device. Flash to `recovery` instead |
| No preloader A/B detection | `mtk_plpath_utils` did not run | Check `getprop | grep ro.boot`; confirm `/dev/block/mapper/pl_a` exists |
| Touch dead | `hbtp_vm` receiving events | Confirm `TW_INPUT_BLACKLIST := "hbtp_vm"` is still set |
| Build fails on `soong` namespace | Tree in the wrong path | It must live at `device/xiaomi/evergo` — `BoardConfig.mk` hardcodes `DEVICE_PATH` |

On-device diagnostics, from OrangeFox **Terminal**:

```bash
getprop | grep -E 'ro.(build|product|boot|recovery|crypto|vendor)'
dmesg | grep -iE 'tee|keymaster|gatekeeper|decrypt'
ls -l /dev/block/mapper/
cat /proc/cmdline
ls /sys/class/leds/
```

Build-time diagnostics, from the source tree:

```bash
# Every variable OrangeFox resolved for this device
source build/envsetup.sh && ./build.sh evergo 2>&1 | tee ~/fox-build.log

# Then inspect what the script dumped
grep -E '^(FOX|OF_|TARGET_|TW_)' ~/fox-build.log
```

---

## 🤝 Contributing

Contributions are welcome. Please:

1. **Open an issue first** for anything non-trivial — a lot of MediaTek fixes look
   device-specific but turn out to be board-generic.
2. **One logical change per commit**, with a message in the style of the existing history:
   `evergo: <what you did>`. For example `evergo: Fix flashlight by using a symbolic link`.
3. **Never** commit:
   - personal data, logs, or anything from `/data`
   - a different Magisk binary than the one referenced, without saying so in the commit
   - changes to `prebuilt/` unless the blob is genuinely being upgraded
4. If you change partition sizing, **change it in both places** and say so explicitly.
5. If you touch the boot-control HAL, state which storage type you tested on (UFS only).

---

## 🙏 Credits & Acknowledgements

This tree would not exist without the work of a lot of people.

- **[OrangeFox Recovery Project](https://github.com/OrangeFoxRecovery)** — the recovery
  framework, the build system, and the UI this tree targets. Maintainer of the original
  `device_xiaomi_evergo` tree.
- **[Sushrut1101](https://github.com/Sushrut1101)** — original `evergo` tree author and
  maintainer. Every commit in the imported history is theirs.
- **[The AOSP Project](https://source.android.com)** — boot-control HAL interfaces, the USB
  gadget scripts, `ueventd`, and the build system.
- **[The TWRP Project](https://github.com/teamwin)** — the device-tree conventions, the
  `twrp.flags` partition format, and much of the `TW_*` configuration vocabulary this tree
  inherits.
- **[MediaTek](https://www.mediatek.com)** — the preloader path utilities, the TEE/microtrust
  integration, the boot-control implementation, and the trusted applications.
- **[LineageOS](https://www.lineageos.org)** — the MT6833 platform reference the ramdisk
  overlay and `PRODUCT_VNDK_VERSION` conventions come from.
- **[xiaomi-sm8250](https://github.com/ProjectElixir/android_device_xiaomi_sm8250)** and the
  wider Xiaomi device-tree community — the dynamic-partition and Treble conventions used here.
- **Every contributor listed in the [commit history](https://github.com/yeasinulhoquetuhin/OrangeFox-evergo/commits/fox_12.1)
  of the imported tree.**

---

## 📄 License & Disclaimer

### Source licence

The build configuration, Makefiles, Blueprints, ramdisk `rc`/`fstab`/`ueventd` overlay,
`bootctrl/` and `mtk_plpath_utils/` sources in this repository are licensed under the

> ### Apache License, Version 2.0
> <http://www.apache.org/licenses/LICENSE-2.0>

### Proprietary components — **not** Apache-2.0

The following are redistributed **as-is** from Xiaomi and MediaTek, remain under their own
terms, and are **not** covered by the Apache-2.0 grant above:

- `prebuilt/Image.gz`, `prebuilt/dtb.img`, `prebuilt/dtbo.img`
- `recovery/root/system/bin/*`
- `recovery/root/vendor/lib64/**`
- `recovery/root/vendor/firmware/**`
- `recovery/root/vendor/thh/**`
- `recovery/root/system/etc/vintf/manifest.xml`, `recovery/root/vendor/etc/vintf/manifest.xml`
- `recovery/root/ueventd.mt6833.rc`, `recovery/root/init.recovery.microtrust.rc`

If you intend to redistribute a build of this tree, understand that you are redistributing
proprietary firmware. You are responsible for compliance with the applicable licences.

### Disclaimer

> [!CAUTION]
> **This project is not affiliated with, endorsed by, or sponsored by Xiaomi, Redmi, POCO,
> MediaTek, or the OrangeFox Recovery Project.**
>
> Custom recoveries modify system partitions. A failed flash, a bad patch, or a
> manufacturer-side bootloader update can leave a device **permanently unbootable** — including
> devices with an unlockable bootloader, because MediaTek's AVB chain and boot-region handling
> are unforgiving.
>
> You do all of this **at your own risk**. The maintainers accept no liability whatsoever for
> any damage arising from the use of these files. Back up your data before you start. If you
> are not comfortable recovering a device from a hard brick, do not start.

---

<div align="center">

**Made with 🧡 for the MediaTek MT6833 community.**

<sub>
Report a bug → <a href="https://github.com/yeasinulhoquetuhin/OrangeFox-evergo/issues">open an issue</a>
&nbsp;·&nbsp;
Build a fix → <a href="https://github.com/yeasinulhoquetuhin/OrangeFox-evergo/commits/fox_12.1">see the history</a>
</sub>

</div>
