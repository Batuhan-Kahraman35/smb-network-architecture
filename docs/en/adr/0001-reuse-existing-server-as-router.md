# ADR-0001: Reuse an existing rack server as the edge router

- **Status:** Accepted
- **Date:** 2026-10-12

## Context

The office needs multi-WAN with per-network line selection, a VPN server and Wi-Fi access control. The existing load-balance router provides none of these (see [Context](../01-context-and-requirements.md)). There's no budget for new network gear right now. An HPE ProLiant DL360 Gen9 with four gigabit ports sits unused.

## Decision

Run the edge router as a **virtual machine (OPNsense) on the DL360 under Proxmox VE**. Add a used 4-port Intel NIC to get enough ports for three WANs, the LAN and a dedicated Wi-Fi segment.

## Alternatives considered

| Alternative | Why not (now) |
|---|---|
| All-in-one appliance (e.g. TP-Link Omada ER706W) | Good fit: multi-WAN, VPN, built-in Wi-Fi 6, centrally managed. Rejected only on budget |
| Omada router + managed APs + controller | The best long-term Wi-Fi story (PPSK, per-SSID VLANs). Same budget issue |
| OPNsense directly on the server (bare metal) | Simpler failure modes, but wastes the server and leaves no room for Windows Server or other VMs |
| Keep the existing router | Can't meet the requirements |

## Consequences

**Positive**

- Meets every requirement with near-zero spend (one used NIC).
- Enterprise-grade firewall features: gateway groups, captive portal, WireGuard, an API.
- VM snapshots and backups make firewall changes reversible.

**Negative**

- **Single point of failure.** If the server goes down, the internet goes down. Mitigated by a [fallback runbook](../runbooks/fallback-to-legacy-router.md) and the old router kept configured on the shelf.
- Higher power use, noise and heat than an appliance.
- Hypervisor maintenance takes the network down. Host updates happen in maintenance windows.

**Revisit when** budget allows managed access points or a dedicated appliance.
