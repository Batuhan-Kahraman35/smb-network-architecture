# ADR-0004: Windows Server is not the edge router

- **Status:** Accepted
- **Date:** 2026-10-12

## Context

Windows Server 2022 was suggested as the server's operating system, with RRAS providing routing and VPN.

## Decision

Windows Server **does not** act as the router or firewall. If it's needed (Active Directory, file shares, NPS/RADIUS), it runs as a **VM next to** OPNsense.

## Rationale

| Requirement | Windows Server (RRAS) |
|---|---|
| Multi-WAN load balancing / failover | No real load balancing. Failover only through route metrics |
| Per-network WAN selection | No practical source-based policy routing |
| VPN | SSTP / IKEv2 / L2TP available, no built-in WireGuard |
| Internet-facing attack surface | Much larger than a purpose-built firewall distribution |
| Patching | Monthly cumulative updates usually require a reboot, which means **an office-wide internet outage every month** |
| Cost | Server license plus CALs, versus free |

Windows Server *is* the better tool for one part of the problem: **NPS (RADIUS)** for WPA2-Enterprise with per-user credentials from Active Directory. That's why it stays in the design as an optional VM.

## Consequences

- Routing and security stay on a dedicated, minimal system.
- Windows updates no longer affect connectivity.
