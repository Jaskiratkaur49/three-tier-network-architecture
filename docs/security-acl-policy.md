# ACL Security Policy

## Overview

Access Control Lists (ACLs) are used throughout the Three-Tier Enterprise Network to enforce traffic-control and security policies between VLANs.

The ACL design follows two main principles:

- **Source-based filtering (IN):** controls where traffic originating from a VLAN is allowed to go.
- **Destination protection (OUT):** controls which networks or devices are allowed to reach a protected VLAN.

The project uses extended ACLs to provide more granular control based on source network, destination network, protocol, and service port.

---

## ACL Summary

| ACL | VLAN | Protected System / Network | Direction | Purpose |
|---:|---:|---|:---:|---|
| 100 | 100 | Staff Wi-Fi | IN | Restrict Staff Wi-Fi from sensitive infrastructure while permitting required business services |
| 105 | 105 | Backup PCs | OUT | Protect backup systems from unauthorized access |
| 160 | 60 | DHCP/DNS Server | OUT | Protect the centralized DHCP/DNS server while permitting DHCP and DNS services |
| 165 | 65 | Application Server | OUT | Protect the internal Application Server and restrict access to approved services |
| 180 | 80 | Cameras | OUT | Control which systems are allowed to access the Camera VLAN |
| 181 | 80 | Cameras | IN | Control where cameras are allowed to send traffic |
| 185 | 120 | Network Management | OUT | Restrict access to network-management devices |
| 190 | 90 | Guest Wi-Fi | IN | Isolate Guest Wi-Fi from internal enterprise resources |

---

# 1. ACL 100 - Staff Wi-Fi Security

**VLAN:** 100  
**Direction:** IN  
**Applied on:** VLAN 100 SVI

ACL 100 controls traffic originating from Staff Wi-Fi.

Staff Wi-Fi users are allowed to use required network services and normal business resources, while access to sensitive infrastructure is restricted.

### Allowed

- DHCP services
- DNS services
- Application Server
- Approved corporate resources

### Blocked

- Camera VLANs
- Camera PCs
- Network Device Admin PCs
- Backup PCs
- Network Management VLANs

### Direction Logic

Because Staff Wi-Fi devices are the **source** of the traffic:

```text
Staff Wi-Fi
     |
     | IN
     v
Distribution Switch
```

the ACL is applied **INBOUND** on VLAN 100.

---

# 2. ACL 105 - Backup PCs Protection

**VLAN:** 105  
**Network:** `10.1.105.0/24`  
**Direction:** OUT  
**Applied on:** VLAN 105 SVI

ACL 105 protects the Backup-PC VLAN from unauthorized access.

### Approved Access

- Network Device Admin PCs
- IT Support systems
- Camera networks when video backup is required
- Application Server when application backup is required
- Required DHCP/DNS traffic

### Blocked

All other unauthorized systems are denied access to the Backup-PC VLAN.

### Direction Logic

The Backup-PC VLAN is a protected **destination**:

```text
Authorized Systems
        |
        v
Distribution Switch
        |
        | OUT
        v
Backup PCs
VLAN 105
```

---

# 3. ACL 160 - DHCP/DNS Server Protection

**VLAN:** 60  
**Server:** `10.1.60.10`  
**Direction:** OUT  
**Applied on:** VLAN 60 SVI

ACL 160 protects the centralized DHCP/DNS server.

The server must remain reachable for required infrastructure services without allowing unrestricted access from normal client networks.

### Allowed Services

- DHCP
- DNS over UDP port 53
- DNS over TCP port 53
- Administrative access from Network Device Admin PCs

### Blocked

Other unauthorized traffic attempting to access `10.1.60.10` is denied.

### Security Objective

```text
Clients
  |
  +---- DHCP --------------------> ALLOW
  |
  +---- DNS ---------------------> ALLOW
  |
Network Admins ------------------> ALLOW
  |
Other unauthorized traffic -----> DENY
```

---

# 4. ACL 165 - Application Server Protection

**VLAN:** 65  
**Server:** `10.1.65.10`  
**Direction:** OUT  
**Applied on:** VLAN 65 SVI

The Application Server functions as an internal corporate web/application server.

### Approved Services

Normal authorized employee networks are permitted to access:

- HTTP - TCP port 80
- HTTPS - TCP port 443

Network Device Admin PCs receive broader administrative access.

Backup PCs may access the server where backup operations require communication with it.

### Blocked

- Guest Wi-Fi
- Cameras
- Unauthorized networks
- Unauthorized application-server services

### Example Security Policy

```text
Employees
    |
    +---- HTTP/HTTPS ----------> Application Server   ALLOW

Network Admins
    |
    +---- Administrative -----> Application Server   ALLOW

Guest Wi-Fi
    |
    +--------------------------> Application Server   DENY
```

---

# 5. ACL 180 - Camera VLAN Protection

**VLAN:** 80  
**Direction:** OUT  
**Purpose:** Control who is allowed to access Cameras

Camera networks:

- `10.1.80.0/24`
- `10.2.80.0/24`
- `10.3.80.0/24`

ACL 180 is applied OUTBOUND on the VLAN 80 SVI.

### Allowed Sources

- Camera PCs - VLAN 95
- Network Device Admin PCs - VLAN 85
- Backup PCs - VLAN 105
- Required DHCP replies
- Required DNS replies

### Blocked

All other systems are denied access to Camera VLANs.

### Direction Logic

```text
Camera PCs --------\
Network Admins -----\
Backup PCs ----------> Distribution Switch
                              |
                              | ACL 180 OUT
                              v
                          Cameras
                          VLAN 80
```

ACL 180 therefore answers:

> **Who is allowed to access the cameras?**

---

# 6. ACL 181 - Camera VLAN Outbound-Source Control

**VLAN:** 80  
**Direction:** IN  
**Purpose:** Control where Cameras are allowed to communicate

ACL 181 controls traffic originating from Cameras.

### Allowed Destinations

- DHCP/DNS Server
- Camera PCs
- Network Device Admin PCs
- Backup PCs

### Blocked Destinations

Cameras are prevented from initiating unnecessary communication with:

- HR
- Finance
- Administration
- Reception
- Sales
- Operations
- General Staff
- Training users
- Guest Wi-Fi
- Staff Wi-Fi
- Other unauthorized networks

### Direction Logic

```text
Cameras
VLAN 80
    |
    | ACL 181 IN
    v
Distribution Switch
    |
    +---- DHCP/DNS ----------> ALLOW
    +---- Camera PCs --------> ALLOW
    +---- Network Admins ----> ALLOW
    +---- Backup PCs --------> ALLOW
    +---- Other Networks ----> DENY
```

ACL 181 therefore answers:

> **Where are Cameras allowed to send traffic?**

---

# 7. ACL 185 - Network Management Protection

**VLAN:** 120  
**Direction:** OUT  
**Purpose:** Protect network-management infrastructure

The Network Management VLAN is reserved for management of switches, routers, and other network infrastructure devices.

Only authorized Network Device Admin PCs are permitted to access management devices.

### Policy

```text
Network Admin PCs
VLAN 85
      |
      | ALLOW
      v
Network Management
VLAN 120


Normal Users --------X
Guest Wi-Fi ----------X
Staff Wi-Fi ----------X
Cameras --------------X
```

ACL 185 answers:

> **Who is allowed to access Network Management?**

---

# 8. ACL 190 - Guest Wi-Fi Isolation

**VLAN:** 90  
**Direction:** IN  
**Purpose:** Prevent Guest Wi-Fi users from accessing internal enterprise resources

Guest Wi-Fi is treated as an untrusted network.

The ACL is applied INBOUND because Guest Wi-Fi clients are the source of the traffic being controlled.

### Guest Restrictions

Guest users must not access sensitive internal resources including:

- Corporate user VLANs
- Server VLANs
- Cameras
- Camera PCs
- Backup PCs
- Network Device Admin PCs
- Network Management infrastructure

Required DHCP/DNS services remain available where needed.

### Direction Logic

```text
Guest Wi-Fi
VLAN 90
    |
    | ACL 190 IN
    v
Distribution Switch
    |
    X---- Internal Networks
```

---

# IN vs OUT ACL Logic

The ACL direction is always considered from the perspective of the Layer-3 interface.

```text
FROM VLAN ---> Layer-3 Switch = IN

Layer-3 Switch ---> TO VLAN   = OUT
```

For example:

```text
Staff Wi-Fi ---> Switch
                 ACL 100 IN
```

controls traffic originating from Staff Wi-Fi.

Whereas:

```text
Switch ---> Cameras
           ACL 180 OUT
```

controls traffic trying to reach Cameras.

---

# ACL End-Rule Logic

Cisco ACLs are processed from top to bottom.

The **first matching rule wins**.

Every ACL also contains an implicit rule at the bottom:

```text
deny ip any any
```

even if it is not manually entered.

## Whitelist-Style ACL

A restrictive VLAN such as the Camera VLAN uses a whitelist approach:

```text
PERMIT required service
PERMIT approved system
PERMIT approved system
DENY everything else
```

This is appropriate when only specifically authorized communication should be allowed.

## Restriction-Style ACL

A network such as Staff Wi-Fi may use:

```text
DENY sensitive destination
DENY sensitive destination
PERMIT remaining approved Staff traffic
```

This is appropriate when users should generally have network access except for certain protected systems.

---

# Verification

ACL operation can be verified using Cisco IOS commands.

```text
show access-lists
```

To inspect a specific ACL:

```text
show access-lists 100
show access-lists 105
show access-lists 160
show access-lists 165
show access-lists 180
show access-lists 181
show access-lists 185
show access-lists 190
```

To verify ACL placement on an SVI:

```text
show ip interface vlan 80
show ip interface vlan 100
show ip interface vlan 120
```

ACL match counters should increase when permitted or denied traffic is tested.

---

# Security Testing

The following traffic tests are used to verify ACL behavior:

| Test | Expected Result |
|---|:---:|
| Staff Wi-Fi -> Application Server | ALLOW |
| Staff Wi-Fi -> Network Management | DENY |
| Guest Wi-Fi -> Internal Networks | DENY |
| Camera PCs -> Cameras | ALLOW |
| Network Admin PCs -> Cameras | ALLOW |
| Backup PCs -> Cameras | ALLOW |
| Normal User -> Cameras | DENY |
| Cameras -> DHCP/DNS | ALLOW |
| Cameras -> Backup PCs | ALLOW |
| Cameras -> Normal User VLANs | DENY |
| Network Admin PCs -> Network Management | ALLOW |
| Normal Users -> Network Management | DENY |
| Employee -> Application Server HTTP/HTTPS | ALLOW |
| Guest Wi-Fi -> Application Server | DENY |

---

## Security Design Summary

The ACL implementation provides segmentation and controlled communication between user, server, wireless, security, backup, and management networks.

The design follows the principle of least privilege by allowing only traffic required for legitimate network operation while restricting unnecessary access to sensitive infrastructure.
