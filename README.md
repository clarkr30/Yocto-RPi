# Custom Yocto Linux for Raspberry Pi 4

A custom embedded Linux distribution for the Raspberry Pi 4B built with the
Yocto Project, including my own BSP layer (`meta-clark`), image and application
recipes, and an out-of-tree kernel driver with a device tree overlay for an
SPI ADC (MCP3008).

> **Status:** 🚧 In progress — started November 2026.

## Goals
- [ ] Boot a minimal Yocto image on the Pi 4B over a serial console
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
_To be filled in after the first successful build._

## Results
| Metric | Value |
|---|---|
| Image size | TBD |
| Boot time (`systemd-analyze`) | TBD |
| First build time | TBD |
| Incremental build time | TBD |

## Bring-up notes
_Problems encountered and how they were debugged._

## Next steps
