# FishHunter Kali-NetHunter Kernel (Pixel 4a / sunfish)

Custom NetHunter & blu_spark performance kernel for Google Pixel 4a (`sunfish`) running Android 13 (Linux 4.14).

---

## Features

### Kali NetHunter & Penetration Testing
- **mac80211 / cfg80211 Packet Injection:** Patched for raw frame transmission, frame injection, and monitor mode support.
- **External Wi-Fi USB Dongle Drivers (Loadable Kernel Modules):**
  - Realtek RTL8812AU / RTL8821AU (`88XXau.ko`)
  - Realtek RTL8814AU (`8814au.ko`)
  - Realtek RTL88x2BU (`88x2bu.ko`)
  - Realtek RTL8188EUS (`8188eu.ko`)
  - Realtek RTL8192EU (`8192eu.ko`)
- **Native Wireless Driver Patches:** In-tree injection fixes for `ath9k_htc`, `rtl8187`, and `zd1211rw`.
- **Bluetooth Pentesting:**
  - Full Bluetooth RFCOMM, TTY support, BNEP, HIDP, and HCI USB (`btusb`) / HCI UART.
- **Software-Defined Radio (SDR):**
  - Drivers enabled for RTL-SDR (`rtl2832_sdr`), HackRF, and AirSpy.
- **USB Gadget & BadUSB (ConfigFS):**
  - HID Gadget support (BadUSB keyboard/mouse injection).
  - USB Mass Storage with CD-ROM emulation support (DriveDroid compatible).
  - CDC-Ethernet, RNDIS, and ECM/EEM network tethering emulation.

### blu_spark & Performance Enhancements
- **WireGuard:** In-tree backport of WireGuard v1.0.20220627 for high-performance encrypted tunneling.
- **Direct High Brightness Mode (HBM):** Sysfs control for display sunlight readability mode.
- **USB2 Fast Charge:** Force high-current charging mode on standard USB ports.
- **Haptics Control:** Fine-tuned vibration intensity controls for `drv2624`.
- **I/O Scheduler:** Zen I/O scheduler enabled for lower latency mobile storage I/O.
- **Filesystems:** arter97 exFAT 2.2.0-3 driver backport.
- **Memory & Compression:** Fast LZ4 decompressor update and LZO-RLE backport for ZRAM swap.

---

## Defconfig Verification

The kernel configuration was validated with the Kali Linux kernel defconfig checker (`check-kernel-config`):
- **0 errors** against official Kali NetHunter kernel requirements.
- Validated against Google AOSP `check_defconfig` rules.

---

## Building

### Prerequisites
Google Clang toolchain and Android kernel build environment (AOSP `android-msm-sunfish-4.14-android13-qpr3`).

```bash
# Sync Google kernel manifest
repo init -u https://android.googlesource.com/kernel/manifest -b android-msm-sunfish-4.14-android13-qpr3
repo sync -j$(nproc)

# Build sunfish kernel image and modules
./build_sunfish.sh
```

Build outputs will be generated in `out/android-msm-pixel-4.14/dist/`:
- `Image.lz4-dtb`
- `dtbo.img`
- Realtek wireless modules: `88XXau.ko`, `8814au.ko`, `88x2bu.ko`, `8188eu.ko`, `8192eu.ko`
- Pixel 4a system modules (`wlan.ko`, `drv2624.ko`, `ftm5.ko`, etc.)

---

## Flashing

Package `Image.lz4-dtb`, `dtbo.img`, and module `.ko` files using [AnyKernel3](https://github.com/osm0sis/AnyKernel3) and flash via TWRP / custom recovery or Kernel Flasher.
