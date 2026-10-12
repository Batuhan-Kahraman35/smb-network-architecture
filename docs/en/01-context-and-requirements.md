# 1. Context and Requirements

[🇹🇷 Türkçe](../tr/01-baglam-ve-gereksinimler.md) · [← README](../../README.md)

## Starting point

A small office running on a flat network:

| Component | Model | Role |
|---|---|---|
| Edge router | TP-Link TL-R480T+ (hw v9, firmware from 2018) | Dual-WAN load balancing, single LAN |
| Access point | Tenda F9 in AP mode | Office Wi-Fi |
| NAS | Zyxel NAS542, 4 × 1 TB | File storage |
| Server | HPE ProLiant DL360 Gen9, 4 × 1 GbE | Unused, available |
| Internet | Three lines from three ISPs, one with a static IP | Two connected to the router, one standalone |

Two of the three ISP modems also broadcast **their own Wi-Fi networks**.

## Requirements

1. **Multi-WAN with per-network selection.** Use all three lines with failover, and decide which network leaves through which line.
2. **Remote access** to the office network over VPN.
3. **Wi-Fi access control.** Someone who learns the Wi-Fi password should not automatically get internet access.
4. **Safe backups** for the new virtual machines.
5. **Constraint:** no budget for new network equipment for now. Use what is already there.

## Assessment of the existing equipment

### Router (TL-R480T+)

- Supports load balancing, policy routing and 802.1Q VLANs on its switch ports.
- **But** the LAN has a single IP interface. VLANs can't get their own subnet and gateway, so "port X uses WAN1, port Y uses WAN2" isn't possible. WAN selection only works by **source IP range**.
- **No VPN server.**
- It can't tell **wired from wireless** clients. Its MAC filter applies to the whole LAN, so an allow-list would also lock out wired devices.

### Access point (Tenda F9)

- Has a MAC allow-list, but no per-user authentication (no WPA2-Enterprise, no PPSK).
- In AP mode it sometimes **starts its own DHCP server** after the upstream router restarts, which breaks the office network.
- Phones increasingly use **randomized MAC addresses**, which makes MAC-based rules fragile unless they're turned off for this network.

### ISP modems

- Any device on a modem's Wi-Fi bypasses every control on the router. **Any access control is meaningless while those networks are on.**

### NAS (Zyxel NAS542)

- **End of life since 2024**, running firmware affected by critical remote-code-execution vulnerabilities with public technical write-ups.
- CPU constantly at 96–100% while idle.
- Detailed in the [case study](case-studies/eol-nas-hardening.md).

### Server (DL360 Gen9)

- A 1U Xeon rack server, more than enough for a firewall plus a few VMs. 4 onboard gigabit ports and free PCIe slots.
- Not officially supported by VMware ESXi 8 (HPE lists Gen9 up to ESXi 7.0 U3).

## Options considered

| Option | Verdict |
|---|---|
| Keep the TL-R480T+ and work around it | ❌ No VPN, no per-network WAN, no Wi-Fi visibility |
| Buy an all-in-one appliance (e.g. TP-Link Omada ER706W) or Omada router + APs | ✅ Technically a good fit, ❌ no budget for now |
| Use the DL360 as a router with Windows Server | ❌ See [ADR-0004](adr/0004-no-windows-server-as-router.md) |
| **Use the DL360 with Proxmox VE + OPNsense** | ✅ **Chosen**. See [ADR-0001](adr/0001-reuse-existing-server-as-router.md) |

Next: [2. Architecture →](02-architecture.md)
