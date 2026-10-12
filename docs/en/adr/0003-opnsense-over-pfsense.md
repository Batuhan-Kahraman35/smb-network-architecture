# ADR-0003: OPNsense as the firewall, not pfSense

- **Status:** Accepted
- **Date:** 2026-10-12

## Context

Both OPNsense and pfSense meet the functional needs: multi-WAN gateway groups, per-interface policy routing, WireGuard, captive portal with vouchers, VLANs and a FreeRADIUS package. They share a common ancestry (m0n0wall), so concepts transfer between them.

## Decision

Use **OPNsense**.

## Rationale

| Factor | OPNsense | pfSense |
|---|---|---|
| Licensing for business use | Fully open source, free for commercial use | CE is free. Plus is the commercial edition, and its no-cost use has been limited to home/lab. Terms have changed over time |
| Release cadence | Two major releases a year with frequent minor/security updates | CE releases have become less frequent. New features land in Plus first |
| WireGuard | In core | Package |
| REST API | Built in, covers most features | More limited in CE |
| Community and tutorials | Large | Larger |

The deciding factors were **predictable licensing for a business network** and the **built-in API**, which is planned for automating Wi-Fi device onboarding.

## Consequences

- No license review needed for commercial use.
- Somewhat fewer third-party tutorials than pfSense, but the official documentation is good.
