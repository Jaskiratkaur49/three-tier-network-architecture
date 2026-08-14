# Three-Tier Enterprise Network Architecture

## Project Overview

This project demonstrates a **three-tier enterprise network architecture** designed and simulated using **Cisco Packet Tracer**.

The network follows a hierarchical design consisting of:

- Core Layer
- Distribution Layer
- Access Layer

The purpose of this project is to demonstrate a structured, scalable, and manageable enterprise network design with VLAN segmentation, IP addressing, centralized network services, wired and wireless connectivity, and hierarchical network infrastructure.

---

## Network Topology

[![Three-Tier Network Topology](images/topology-overview.png)](images/topology-overview.png)

*Click the image to view the full-size topology.*

---

## Architecture

### Core Layer

The Core Layer provides high-speed connectivity between the major sections of the network and forms the backbone of the three-tier architecture.

### Distribution Layer

The Distribution Layer connects the Access Layer to the Core Layer.

It is responsible for functions such as:

- Inter-VLAN routing
- SVI/default-gateway placement
- Network segmentation
- Traffic forwarding between access and core layers
- Policy and routing functions

### Access Layer

The Access Layer provides network connectivity for end-user and infrastructure devices such as:

- Desktop computers
- Laptops
- Wireless access points
- Printers
- Cameras
- Servers
- Administrative devices

---

## VLAN and IP Addressing

The network uses VLAN-based segmentation to separate departments, infrastructure services, wireless users, cameras, printers, servers, and network-management traffic.

The addressing design uses private IPv4 address ranges across the network, including:

- `10.1.x.x`
- `10.2.x.x`
- `10.3.x.x`

Most client VLANs use `/24` networks with DHCP address pools generally ranging from `.100` to `.200`.

### Centralized Network Services

| Service | VLAN | IP Address | Default Gateway |
|---|---:|---|---|
| DHCP/DNS Server | 60 | `10.1.60.10` | `10.1.60.1` |
| Application Server | 65 | `10.1.65.10` | `10.1.65.1` |

### Example VLANs

| VLAN | Name | Example Network |
|---:|---|---|
| 10 | Administration | `10.1.10.0/24` |
| 20 | HR | `10.1.20.0/24` |
| 25 | Reception | `10.1.25.0/24` |
| 30 | Finance | `10.1.30.0/24` |
| 35 | General Staff | `10.1.35.0/24` |
| 40 | Sales | `10.1.40.0/24` |
| 50 | Operations | `10.1.50.0/24` |
| 60 | DHCP/DNS Server | `10.1.60.0/24` |
| 65 | Application Server | `10.1.65.0/24` |
| 80 | Cameras | `10.1.80.0/24` |
| 90 | Guest Wi-Fi | `10.1.90.0/24` |
| 100 | Staff Wi-Fi | `10.1.100.0/24` |
| 110 | Printers | `10.1.110.0/24` |
| 120 | Network Management | `10.1.120.0/24` |

For the complete building-by-building VLAN plan, DHCP pools, default gateways, access-switch assignments, SVI ownership, server addressing, and shared infrastructure VLANs, see:

### [View Complete VLAN and IP Addressing Plan](docs/vlan-ip-addressing.md)

---

## Project Features

- Three-tier hierarchical network architecture
- Core, Distribution, and Access Layer design
- Department-based VLAN segmentation
- Structured IPv4 addressing
- Layer 3 SVI gateways at the Distribution Layer
- Centralized DHCP and DNS services
- Application server connectivity
- Wired and wireless connectivity
- Guest and Staff Wi-Fi networks
- Camera and security-related VLANs
- Printer VLANs
- Dedicated Network Management VLAN
- Redundant network connections
- Cisco Packet Tracer simulation
- Reserved access-layer capacity for future expansion

---

## Repository Structure

```text
three-tier-network-architecture/
│
├── README.md
│
├── docs/
│   ├── README.md
│   └── vlan-ip-addressing.md
│
├── images/
│   └── topology-overview.png
│
├── packet-tracer/
│   └── [Cisco Packet Tracer project file]
│
└── .gitattributes
```

### Directory Description

| Directory | Purpose |
|---|---|
| `docs/` | Technical network documentation |
| `images/` | Network topology diagrams and screenshots |
| `packet-tracer/` | Cisco Packet Tracer project files |

Additional configuration and verification directories will be added as the project develops.

---

## How to Open the Project

1. Install **Cisco Packet Tracer**.
2. Open the `packet-tracer/` directory.
3. Download the `.pkt` project file.
4. Open the file using Cisco Packet Tracer.
5. Examine the network topology and device configurations.

---

## Documentation

Current technical documentation includes:

- [VLAN and IP Addressing Plan](docs/vlan-ip-addressing.md)
- [ACL Security Policy](docs/security-acl-policy.md)

Additional documentation will be added as the implementation and testing phases progress.

---

## Future Improvements

Planned improvements include:

- Add sanitized switch and router configurations
- Document inter-VLAN routing configuration
- Document routing protocol configuration
- Add DHCP configuration and verification
- Add connectivity and ping-test screenshots
- Add separate screenshots for each network layer
- Document redundancy and failover testing
- Add troubleshooting and verification commands
- Document security policies and access-control implementation

---

## Author

**Jaskirat Kaur**
