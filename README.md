# Custom Yocto Linux for Raspberry Pi 4

A custom embedded Linux distribution for the Raspberry Pi 4B built with the
Yocto Project, including my own BSP layer (`meta-clark`), image and application
recipes, and an out-of-tree kernel driver with a device tree overlay for an
SPI ADC (MCP3008).

> **Status:** In progress — started November 2026.

## Goals
- [x] Boot a minimal Yocto image on the Pi 4B over a serial console
- [ ] Custom layer, image recipe, and userspace application recipe
- [ ] Out-of-tree kernel driver + device tree overlay for an MCP3008 SPI ADC
- [ ] CI for the layer, driver, and application
- [ ] Stretch: U-Boot with verified (signed FIT) boot

## Hardware
- Raspberry Pi 4 Model B
- 3.3 V USB-to-TTL serial adapter (console on GPIO14/15, 115200 8N1)
- MCP3008 10-bit SPI ADC on SPI0 / CE0

## Build host
- Windows 11 + WSL2, Ubuntu 24.04
- Yocto Project [Scarthgap 5.0 / Wrynose 6.0] — layers pinned with kas

## Repository layout
    kas/          kas build configuration (layer versions, machine, local.conf)
    meta-clark/   custom layer: recipes, bbappends, kernel module, DT overlay

## Building
**Host:** Ubuntu 24.04 (WSL2 on Windows 11), Ryzen 7 5700X3D, 8 BitBake threads.
**Release:** Yocto Project 6.0 "Wrynose" (LTS), layers pinned in `kas/rpi4.lock.yml`.

### Prerequisites
- [Yocto host packages](https://docs.yoctoproject.org/ref-manual/system-requirements.html)
- kas: `pipx install kas`
- Ubuntu 24.04 only: allow unprivileged user namespaces for BitBake
  (`kernel.apparmor_restrict_unprivileged_userns = 0` in `/etc/sysctl.d/60-yocto.conf`)

### Build
```bash
export KAS_WORK_DIR=~/yocto/work       # layer checkouts + build/ live outside the repo
export DL_DIR=~/yocto/downloads        # shared source cache
export SSTATE_DIR=~/yocto/sstate-cache # shared build cache
kas build kas/rpi4.yml
```
Output: `$KAS_WORK_DIR/build/tmp/deploy/images/raspberrypi4-64/core-image-minimal-raspberrypi4-64.rootfs.wic.bz2`

### Flash and boot
1. Flash the `.wic.bz2` with balenaEtcher (or `bmaptool copy` on Linux).
2. Serial console: 3.3 V USB-TTL adapter, GND→pin 6, RX→pin 8 (GPIO14), TX→pin 10 (GPIO15);
   115200 8N1, no flow control. Console is `ttyS0` (mini-UART).
3. Log in as `root` with no password (dev image only — see `EXTRA_IMAGE_FEATURES` in the kas file).

## Results
| Metric | Value |
|---|---|
| Image size (Compressed bz2) | 29.37 MiB (30,794,167 B) |
| First build time | 71m 45.076s |

## Bring-up notes
_Problems encountered and how they were debugged._

## Next steps
