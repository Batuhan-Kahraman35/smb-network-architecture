# ADR-0002: Proxmox VE as the hypervisor, not VMware ESXi 8

- **Status:** Accepted
- **Date:** 2026-10-12

## Context

The server will host the firewall VM and possibly a Windows Server VM. Both ESXi and Proxmox VE were on the table, and the team has prior ESXi experience.

## Decision

Use **Proxmox VE**, installed on the server's RAID array (not on a USB stick).

## Rationale

| Factor | ESXi 8 (free edition) | Proxmox VE |
|---|---|---|
| License | Free edition returned in 2025 (8.0 U3e), with restrictions and terms that have changed more than once | Free and open source, optional paid support |
| Limits | 8 vCPU per VM, no vCenter | None |
| Backups | Backup APIs not available in the free edition, so tools like Veeam can't back up VMs | Built-in vzdump, plus Proxmox Backup Server for incremental backups |
| Hardware support | HPE supports Gen9 up to **ESXi 7.0 U3**. ESXi 8 works but is unsupported | Runs well on Gen9 |
| Firewall VM networking | vSwitch per NIC: fine | Linux bridge per NIC, or PCI passthrough: fine |

The deciding factor was **backups**. A hypervisor that can't back up its own VMs for free is a poor base for the office's only router.

## Why not install Proxmox on USB

ESXi was designed to boot from USB/SD. Proxmox constantly writes logs, cluster configuration and RRD metrics to its system disk and would wear out a USB stick within weeks to months. The USB stick is only the **installation medium**.

## Consequences

- No licensing risk, built-in backups.
- The team has to learn Proxmox, a modest learning curve coming from ESXi.
