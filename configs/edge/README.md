# Edge Router Configuration

This directory contains the sanitized Cisco IOS configuration for the simulated ISP/WAN router used in the Three-Tier Enterprise Network Architecture project.

## Device

- `ISP-Router.cfg` - Simulated Internet Service Provider (ISP) router

## Role in the Network

The ISP router represents the external WAN/Internet environment connected to the enterprise network through two Cisco ASA firewalls.

It provides:

- Two separate WAN links to the enterprise firewalls
- A simulated external Internet endpoint
- Primary and backup static routes toward the internal enterprise network
- Connectivity testing between the enterprise network and the simulated Internet

## WAN Addressing

| Interface | IP Address | Connected Device |
|---|---|---|
| GigabitEthernet0/0 | `200.0.1.1/30` | ASA FW-1 |
| GigabitEthernet0/1 | `200.0.2.1/30` | ASA FW-2 |
| Loopback0 | `8.8.8.8/32` | Simulated external Internet endpoint |

## Static Routing

The ISP router uses two static routes toward the internal enterprise address space:

```cisco
ip route 10.0.0.0 255.0.0.0 200.0.1.2
ip route 10.0.0.0 255.0.0.0 200.0.2.2 10
