Access Switch Configurations

This directory contains sanitized Cisco IOS configuration files for the access layer of the three-tier network architecture.

Access switches provide endpoint connectivity for users, IP phones, printers, wireless access points, CCTV devices, and other network resources. Each access switch connects redundantly to the distribution layer through two trunk uplinks.

Directory Structure

All access-switch configuration files are stored together in this directory:

access-switches/
├── README.md
├── ACCESS-SWITCH-A1-01.cfg
├── ACCESS-SWITCH-A1-02.cfg
├── ACCESS-SWITCH-A1-03.cfg
├── ACCESS-SWITCH-A1-04.cfg
└── ACCESS-SWITCH-A1-05.cfg

Additional access-switch configurations can be added directly to this folder as the project expands. The filename and switch hostname identify the corresponding network block and device number, so separate distribution-switch subfolders are not required.

Naming Convention

Access-switch configuration files use this format:

ACCESS-SWITCH-<BLOCK>-<NUMBER>.cfg

Example:

ACCESS-SWITCH-A1-03.cfg

The corresponding switch hostname is:

ACC-A1-03

Access-Layer Design

The access layer provides:

Access VLAN assignment for endpoint devices

Voice VLAN assignment for IP phones

Connectivity for users, printers, wireless devices, and CCTV equipment

Redundant trunk uplinks to the distribution-switch pair

Management connectivity through a dedicated management VLAN

Layer 2 loop prevention with Rapid PVST

Edge-port protection with PortFast and BPDU Guard

Administrative shutdown of unused ports

Inter-VLAN routing and default-gateway services are provided by the distribution layer rather than the access switches.

Common Uplink Layout

Access switches use two redundant uplinks where supported by the design:

Access-switch port

Connection

FastEthernet0/23

Primary distribution switch

FastEthernet0/24

Secondary distribution switch

Trunk allowed-VLAN lists are restricted to the VLANs required by each individual access switch. Rapid PVST prevents Layer 2 loops and maintains a redundant path.

Common Security Features

The access-switch configurations include the following controls where applicable:

SSH version 2 for remote administration

Local administrative authentication

SSH-only access on VTY lines

PortFast on end-device access ports

BPDU Guard on end-device access ports

Static trunk configuration

DTP disabled on established trunks where configured

VLAN 1 management interface disabled

Unused ports administratively shut down

Parking VLANs for unused ports where required

Dedicated management VLAN

Sanitization Notice

Authentication secrets are removed before the configuration files are published. Sanitized entries are represented as comments:

! SECURITY NOTICE: Authentication secret removed
! username admin privilege 15 secret <REDACTED>

The published files are intended for documentation, portfolio review, and lab reference. Replace all redacted authentication information securely before deploying a configuration.

Verification Commands

Useful Cisco IOS verification commands include:

show vlan brief
show interfaces trunk
show spanning-tree
show ip interface brief
show ip ssh
show running-config

Related Documentation

Refer to the main repository documentation for:

Network topology

VLAN and IP-addressing plan

Distribution-switch configurations

Inter-VLAN routing

Routing protocols

DHCP services

Redundancy and failover testing

Packet Tracer project files
