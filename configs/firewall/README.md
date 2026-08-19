# Firewall Configurations

This directory contains sanitized Cisco ASA firewall configurations used at the Internet edge of the Three-Tier Enterprise Network Architecture project.

## Devices

- `ASA-FW-1.cfg`
- `ASA-FW-2.cfg`

The firewalls provide:

- Inside and outside security zones
- Dynamic PAT for internal networks
- OSPF connectivity with the Core Layer
- Default-route advertisement toward the enterprise network
- Outside ICMP filtering for connectivity and troubleshooting
- Application inspection for supported protocols

Sensitive credentials are removed or replaced with `<REDACTED>`.
