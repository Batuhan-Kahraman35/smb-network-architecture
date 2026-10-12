# ADR-0006: Keep the end-of-life NAS as an isolated backup target

- **Status:** Accepted
- **Date:** 2026-10-12

## Context

The only NAS (Zyxel NAS542) reached end of life in 2024. It was running firmware affected by critical remote-code-execution vulnerabilities, and its CPU was pegged at 96–100% while idle. The new design needs a backup target for the VMs, and there's no budget for a new NAS.

## Decision

Keep the NAS, but only after a full clean-up, and only as an **isolated backup target**:

1. Update to the vendor's final firmware, which contains the 2024 security patch.
2. Wipe the volume and the configuration and rebuild from scratch.
3. Disable every service not needed for SMB backups.
4. Use a dedicated least-privilege user and a hidden share.
5. Block the NAS from the internet at the firewall, and block it from the Wi-Fi segment.
6. Keep an off-site copy of critical data.

Full write-up: [Case study: hardening an end-of-life NAS](../case-studies/eol-nas-hardening.md)

## Consequences

- Backups have a home now, at no cost.
- **Residual risk:** future vulnerabilities won't be patched. Isolation reduces the exposure, but a replacement (a supported NAS or a TrueNAS box) stays on the roadmap.
- The disks are desktop-class and one is an unbranded model. S.M.A.R.T. is checked monthly.
