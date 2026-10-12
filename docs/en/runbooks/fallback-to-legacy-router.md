# Runbook: Fall Back to the Legacy Router

[🇹🇷 Türkçe](../../tr/runbooks/eski-routera-donus.md) · [← README](../../../README.md)

**Use when:** the server, Proxmox or the OPNsense VM has failed and won't be fixed quickly, and the office needs internet access now.
**Target time:** 10 minutes.

## Prepared in advance

- [ ] The legacy router stays on the shelf **with its configuration intact** and its power adapter.
- [ ] A backup of its configuration is stored on the NAS.
- [ ] OPNsense's LAN address is **the same** as the legacy router's LAN address, so clients need no changes.
- [ ] A printed copy of this page hangs next to the rack.

## Steps

1. Shut down the server, or unplug the WAN and LAN cables from it.
2. Power on the legacy router.
3. Connect:
   - ISP A → router port 1 (WAN1)
   - ISP B → router port 2 (WAN2)
   - Office switch → router port 3 (LAN)
4. Temporarily connect the access point to the office switch. **Wi-Fi access control is disabled while in fallback mode.** Note it.
5. On a workstation: renew DHCP, check the gateway, confirm internet access.
6. Confirm the NAS is reachable.
7. Log the fallback in the changelog: date, time, reason.

## What doesn't work in fallback mode

- Wi-Fi allow-list and captive portal
- WireGuard VPN
- The third WAN line

## Returning to normal

Reverse the steps once the server is healthy, verify each WAN and the Wi-Fi segment, and log it.
