# VLAN and IP Addressing Plan

## Overview

This document defines the VLAN segmentation and IPv4 addressing plan for the Three-Tier Enterprise Network project.

The design separates departmental users, servers, wireless networks, cameras, printers, administrative systems, and network-management traffic into dedicated VLANs.

### Key Addressing Information

- Central DHCP/DNS Server: `10.1.60.10`
- DHCP/DNS Server Gateway: `10.1.60.1`
- Application Server: `10.1.65.10`
- Application Server Gateway: `10.1.65.1`
- Most client VLANs use DHCP address ranges from `.100` to `.200`
- VLAN 120 is reserved for network-management traffic and uses static addressing

---

## 1. Primary VLAN Reference

| VLAN | Name | Purpose | Default Gateway | IP Address / DHCP Pool |
|---:|---|---|---|---|
| 10 | Administration | Administration PCs | `10.1.10.1` | `10.1.10.100 - 10.1.10.200` |
| 15 | Training-Users | Training user PCs | `10.2.15.1` | `10.2.15.100 - 10.2.15.200` |
| 20 | HR | HR PCs | `10.1.20.1` | `10.1.20.100 - 10.1.20.200` |
| 25 | Reception | Reception PCs | `10.1.25.1` | `10.1.25.100 - 10.1.25.200` |
| 30 | Finance | Finance PCs | `10.1.30.1` | `10.1.30.100 - 10.1.30.200` |
| 35 | General-Office-Users | General office PCs | `10.1.35.1` | `10.1.35.100 - 10.1.35.200` |
| 40 | Sales | Sales PCs | `10.1.40.1` | `10.1.40.100 - 10.1.40.200` |
| 45 | Technical-Users | Technical user PCs | `10.2.45.1` | `10.2.45.100 - 10.2.45.200` |
| 50 | Operations | Operations PCs | `10.1.50.1` | `10.1.50.100 - 10.1.50.200` |
| 55 | IT-Support | IT Support PCs | `10.2.55.1` | `10.2.55.100 - 10.2.55.200` |
| 60 | DHCP-DNS-Server | Central DHCP/DNS services | `10.1.60.1` | Server: `10.1.60.10` |
| 65 | Application-Server | Application services | `10.1.65.1` | Server: `10.1.65.10` |
| 80 | Cameras | Camera connections | `10.1.80.1` | `10.1.80.100 - 10.1.80.200` |
| 85 | Network-Device-Admin | Network device admin PCs | `10.1.85.1` | `10.1.85.100 - 10.1.85.200` |
| 90 | Guest-WiFi | Guest wireless clients | `10.1.90.1` | `10.1.90.100 - 10.1.90.200` |
| 95 | Camera-PCs | Camera monitoring PCs | `10.1.95.1` | `10.1.95.100 - 10.1.95.200` |
| 100 | Staff-WiFi | Staff wireless clients | `10.1.100.1` | `10.1.100.100 - 10.1.100.200` |
| 105 | Backup-PCs | Backup systems | `10.1.105.1` | `10.1.105.100 - 10.1.105.200` |
| 110 | Printers | Network printers | `10.1.110.1` | `10.1.110.100 - 10.1.110.200` |
| 120 | Network-Management | Network infrastructure management | `10.1.120.1` | Static addressing |

### DHCP and DNS

For VLANs that use dynamic addressing:

- DHCP Server: `10.1.60.10`
- DNS Server: `10.1.60.10`

---

## 2. Building A1

Building A1 contains HR, Reception, Finance, and Administration user networks.

| VLAN | Name | Subnet | Default Gateway | DHCP Pool | Access Switch | SVI Owner |
|---:|---|---|---|---|---|---|
| 20 | HR | `10.1.20.0/24` | `10.1.20.1` | `10.1.20.100 - 10.1.20.200` | `ACC-A1-01` | `DIST-A1` |
| 25 | Reception | `10.1.25.0/24` | `10.1.25.1` | `10.1.25.100 - 10.1.25.200` | `ACC-A1-01` | `DIST-A1` |
| 30 | Finance | `10.1.30.0/24` | `10.1.30.1` | `10.1.30.100 - 10.1.30.200` | `ACC-A1-02` | `DIST-A1` |
| 10 | Administration | `10.1.10.0/24` | `10.1.10.1` | `10.1.10.100 - 10.1.10.200` | `ACC-A1-03` | `DIST-A1` |

### A1 Access-Switch Mapping

- `ACC-A1-01` -> VLAN 20 HR and VLAN 25 Reception
- `ACC-A1-02` -> VLAN 30 Finance
- `ACC-A1-03` -> VLAN 10 Administration

---

## 3. Building B1

| VLAN | Name | Subnet | Default Gateway | DHCP Pool | Access Switch | SVI Owner |
|---:|---|---|---|---|---|---|
| 40 | Sales | `10.1.40.0/24` | `10.1.40.1` | `10.1.40.100 - 10.1.40.200` | `ACC-B1-01` | `DIST-B1` |
| 50 | Operations | `10.1.50.0/24` | `10.1.50.1` | `10.1.50.100 - 10.1.50.200` | `ACC-B1-02` | `DIST-B1` |
| 35 | General-Staff-Users | `10.1.35.0/24` | `10.1.35.1` | `10.1.35.100 - 10.1.35.200` | `ACC-B1-03` | `DIST-B1` |

---

## 4. Building A2

| VLAN | Name | Subnet | Default Gateway | DHCP Pool | Access Switch | SVI Owner |
|---:|---|---|---|---|---|---|
| 55 | IT-Support | `10.2.55.0/24` | `10.2.55.1` | `10.2.55.100 - 10.2.55.200` | `ACC-A2-01` | `DIST-A2` |
| 45 | Technical-Users | `10.2.45.0/24` | `10.2.45.1` | `10.2.45.100 - 10.2.45.200` | `ACC-A2-02` | `DIST-A2` |
| 15 | Training-Users | `10.2.15.0/24` | `10.2.15.1` | `10.2.15.100 - 10.2.15.200` | `ACC-A2-03` | `DIST-A2` |

---

## 5. Building B2

| VLAN | Name | Subnet | Default Gateway | DHCP Pool | Access Switch | SVI Owner |
|---:|---|---|---|---|---|---|
| 35 | General-Staff-Overflow | `10.2.35.0/24` | `10.2.35.1` | `10.2.35.100 - 10.2.35.200` | `ACC-B2-01` | `DIST-B2` |
| 40 | Sales-Overflow | `10.2.40.0/24` | `10.2.40.1` | `10.2.40.100 - 10.2.40.200` | `ACC-B2-02` | `DIST-B2` |
| 10 | Administration | `10.2.10.0/24` | `10.2.10.1` | `10.2.10.100 - 10.2.10.200` | `ACC-B2-03` | `DIST-B2` |

---

## 6. Building A3

| VLAN | Name | Subnet | Default Gateway | DHCP Pool | Access Switch | SVI Owner |
|---:|---|---|---|---|---|---|
| 50 | Operations-Overflow | `10.3.50.0/24` | `10.3.50.1` | `10.3.50.100 - 10.3.50.200` | `ACC-A3-01` | `DIST-A3` |
| 55 | IT-Support | `10.3.55.0/24` | `10.3.55.1` | `10.3.55.100 - 10.3.55.200` | `ACC-A3-02` | `DIST-A3` |
| 35 | General-Staff-Users-Overflow | `10.3.35.0/24` | `10.3.35.1` | `10.3.35.100 - 10.3.35.200` | `ACC-A3-03` | `DIST-A3` |

---

## 7. Building B3 - Server and Administration Area

| VLAN | Name | IP Address / DHCP Pool | Default Gateway | Access Switch | SVI Owner |
|---:|---|---|---|---|---|
| 60 | DHCP-DNS-Server | `10.1.60.10` | `10.1.60.1` | `ACC-B3-01` | `DIST-B3` |
| 65 | Application-Server | `10.1.65.10` | `10.1.65.1` | `ACC-B3-01` | `DIST-B3` |
| 85 | Network-Device-Admins | `10.1.85.100 - 10.1.85.200` | `10.1.85.1` | `ACC-B3-02` | `DIST-B3` |
| 95 | Camera-PCs | `10.1.95.100 - 10.1.95.200` | `10.1.95.1` | `ACC-B3-03` | `DIST-B3` |
| 105 | Backup-PCs | `10.1.105.100 - 10.1.105.200` | `10.1.105.1` | `ACC-B3-04` | `DIST-B3` |

---

## 8. Shared Infrastructure VLANs

The following VLAN IDs are used for shared infrastructure services across the three main addressing groups.

| VLAN | Name | A1/B1 Subnet | A2/B2 Subnet | A3/B3 Subnet |
|---:|---|---|---|---|
| 80 | Cameras | `10.1.80.0/24` | `10.2.80.0/24` | `10.3.80.0/24` |
| 90 | Guest-WiFi | `10.1.90.0/24` | `10.2.90.0/24` | `10.3.90.0/24` |
| 100 | Staff-WiFi | `10.1.100.0/24` | `10.2.100.0/24` | `10.3.100.0/24` |
| 110 | Printers | `10.1.110.0/24` | `10.2.110.0/24` | `10.3.110.0/24` |
| 120 | Network-Management | `10.1.120.0/24` | `10.2.120.0/24` | `10.3.120.0/24` |

### Network Management

VLAN 120 is used for management of network infrastructure devices and uses static IP addressing.

---

## 9. Server Reference

### DHCP/DNS Server

| Setting | Value |
|---|---|
| VLAN | `60` |
| IP Address | `10.1.60.10` |
| Default Gateway | `10.1.60.1` |
| Access Switch | `ACC-B3-01` |
| SVI Owner | `DIST-B3` |

### Application Server

| Setting | Value |
|---|---|
| VLAN | `65` |
| IP Address | `10.1.65.10` |
| Default Gateway | `10.1.65.1` |
| Access Switch | `ACC-B3-01` |
| SVI Owner | `DIST-B3` |

---

## 10. Distribution-Layer Summary

| Building | Distribution Switch | Local VLANs |
|---|---|---|
| A1 | `DIST-A1` | VLAN 20, 25, 30, 10 |
| B1 | `DIST-B1` | VLAN 40, 50, 35 |
| A2 | `DIST-A2` | VLAN 55, 45, 15 |
| B2 | `DIST-B2` | VLAN 35, 40, 10 |
| A3 | `DIST-A3` | VLAN 50, 55, 35 |
| B3 | `DIST-B3` | VLAN 60, 65, 85, 95, 105 |

---

## 11. Future Expansion

Access Switch Number 5 in each distribution-switch block is reserved for future use.

This provides room for future access-layer expansion without changing the existing device-numbering structure.

---

## 12. Design Summary

The VLAN and IP addressing design provides:

- Logical separation between departments and infrastructure services
- Dedicated VLANs for servers, wireless users, cameras, printers, and management devices
- Centralized DHCP and DNS services
- Layer 3 SVI gateways at the distribution layer
- Building-based addressing using the `10.1.x.x`, `10.2.x.x`, and `10.3.x.x` private address ranges
- Reserved access-layer capacity for future network growth

This document serves as the addressing reference for VLAN creation, SVI configuration, DHCP configuration, access-switch assignments, endpoint addressing, and network verification throughout the project.
