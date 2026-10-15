# Case Study: Turning a Second-Hand Rack Server into a Hypervisor

[🇹🇷 Türkçe](../../tr/case-studies/ikinci-el-sunucu.md) · [← README](../../../README.md)

**Hardware:** HPE ProLiant DL360 Gen9 (2 × Xeon E5-2620 v4, 64 GB DDR4, Smart Array P440ar, iLO 4 Advanced) · **Date:** 2026-10-12 → 10-13

## Starting point

The server had been idle for a long time and was bought second-hand. Remote inspection through iLO showed:

| Finding | Risk |
|---|---|
| Boot loop: *"No bootable devices were detected"* | **All drives had been removed** by the previous owner |
| System ROM from 2017, iLO firmware from 2020 | Missing CPU microcode fixes (Spectre/Meltdown era) and iLO security fixes |
| Leftovers from the previous owner: hostname, a full-admin monitoring account, dynamic DNS registration | Unknown accounts with full rights on the management controller |
| RTC reset to 01/01/1970 after power loss | Weak CMOS battery — wrong time breaks TLS, VPN and logs |

## What was done

1. **Clean-up of the management controller.** Removed the leftover admin account and DDNS registration, changed the local administrator password.
2. **Firmware.** iLO 4 2.77 → **2.82**, System ROM P89 v2.42 → **v3.40 (2024)**. Packages downloaded only from the vendor and verified by **SHA-256** before upload. The ROM update left the backup ROM bank on the old version as a rollback path.
3. **Disks.** Only two 2.5" 10K SAS drives were available, recovered from another server. The chassis has 3.5" caddies; one drive was seated, the second needs a proper 2.5"→3.5" adapter that carries the connector (a plain bracket does not).
4. **Controller in HBA mode.** The P440ar was switched from RAID to **HBA mode** so the hypervisor sees raw disks. Verified during installation: the installer listed the drive by model name instead of *"LOGICAL VOLUME"*.
5. **Proxmox VE 9** installed from the ISO **mounted over iLO virtual media by URL** — no USB stick, no one at the rack. ZFS on a single disk now; the second disk will be attached as a mirror later (`zpool attach`), no reinstall needed.
6. **Post-install:** community repository, full upgrade, NTP check, ZFS/SMART check, auto power-on after outage, NAS added as backup storage.

## Problems and how they were solved

| Problem | Cause | Fix |
|---|---|---|
| Disk invisible in iLO after insertion | iLO only refreshes the inventory at POST; and the 2.5" disk did not reach the backplane in a 3.5" caddy | Reseated the disk, rebooted; adapter ordered for the second disk |
| Second PSU "input voltage lost" | Power cable pulled while working on the front bays | Reseated; redundancy restored |
| Root password not accepted after install | The HTML5 remote console translated the keyboard layout differently during setup | Reset via boot loader (`init=/bin/bash`), then set a strong password from the web UI. **Lesson:** passwords typed into a remote console should be letters and digits only |
| ISO download from the vendor mirror kept dropping from the office line | The office IP was rate-limited by the mirror after a multi-connection download manager | Downloaded on another server, verified against the **official SHA-256 list**, served temporarily to the office IP only |

## Result

| | |
|---|---|
| Hypervisor | Proxmox VE 9.2, kernel 7.0, ZFS |
| Disk health | SMART OK, 0 grown defects — but **~61,000 power-on hours**, so the mirror and nightly backups are a priority |
| Management | iLO on the LAN only, current firmware, no unknown accounts |
| Known limits | LAN port runs at 100 Mb/s because of the office switch; one disk until the adapter arrives |

Related: [ADR-0001 Reuse the existing server as the router](../adr/0001-reuse-existing-server-as-router.md) · [ADR-0002 Proxmox over ESXi](../adr/0002-proxmox-over-esxi.md)
