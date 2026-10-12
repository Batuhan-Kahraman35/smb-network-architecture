# ADR-0005: Wi-Fi access control through a dedicated physical segment

- **Status:** Accepted
- **Date:** 2026-10-12

## Context

The office wants to stop Wi-Fi password sharing. The only access point is a consumer device that can't tag VLANs or do per-user authentication. There is no managed switch. In AP (bridge) mode, the firewall sees wireless clients with their own MACs and can't distinguish them from wired clients. Two ISP modems also broadcast their own Wi-Fi.

## Decision

- Connect access points **only** to a dedicated firewall port (the WIFI interface). Never connect them to the office switch.
- On that interface: DHCP for known MACs only, a MAC allow-list firewall rule, a captive portal with vouchers for guests, and no access to private networks.
- **Disable Wi-Fi on every ISP modem.**

Details: [3. Wi-Fi access control](../03-wifi-access-control.md)

## Alternatives considered

| Alternative | Why not |
|---|---|
| Managed APs with PPSK / WPA2-Enterprise | Best solution, no budget yet |
| MAC allow-list on the AP | No visibility, and doesn't cover the modems' Wi-Fi |
| VLANs | Need a VLAN-aware AP and a managed switch, neither is available |
| AP in router (NAT) mode | Hides all clients behind one MAC, so per-device control is lost |

## Consequences

**Positive:** full visibility of Wi-Fi clients, per-device control, guest vouchers, the AP's occasional rogue DHCP is contained to its own segment, and Wi-Fi gets its own uplink.

**Negative:** MAC-based, so spoofable. Users must disable MAC randomization for this network. One server port is used up.

**Revisit when** VLAN-capable access points with WPA2/3-Enterprise or PPSK are available.
