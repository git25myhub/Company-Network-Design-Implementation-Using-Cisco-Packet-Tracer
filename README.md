# 🌐 Enterprise Network Infrastructure & OSPF Routing Lab

![Cisco Networking](https://img.shields.io/badge/Cisco-Packet%20Tracer-1BA0D7?style=for-the-badge&logo=cisco)
![Routing](https://img.shields.io/badge/Routing-OSPF-success?style=for-the-badge)
![Switching](https://img.shields.io/badge/Switching-VLAN%20%7C%20Trunking-orange?style=for-the-badge)
![Lab Status](https://img.shields.io/badge/Status-Completed-brightgreen?style=for-the-badge)

## 📌 Overview

This lab demonstrates the design and configuration of a multi-router enterprise network using **Cisco Packet Tracer**.

The topology combines an enterprise LAN environment with ISP-facing routers and demonstrates how multiple network segments can communicate through **OSPF Area 0**.

The lab focuses on practical **Network Engineering, Routing, Switching, VLAN, Infrastructure Security, and NOC troubleshooting concepts**.

The network includes:

- Multiple Cisco routers
- Multilayer switches
- ISP routers
- Departmental VLANs
- Inter-VLAN routing
- DHCP relay
- OSPF dynamic routing
- Trunk links
- Access ports
- Port security
- SSH management
- Network segmentation
- Server-room connectivity
- Enterprise departmental networks

---

## 🖥️ Network Topology

The topology is designed around a routed core connected to two ISP routers and multiple departmental/access networks.

### Topology Diagram

![Enterprise Network Topology](Screenshots/topology.png)

> **Note:** Place the topology screenshot inside the repository's `Screenshots` folder and name it `topology.png`. If your filename is different, update the image path above.

---

# 🏗️ Network Architecture

The network consists of several logical layers:

```text
                         ┌───────────────┐
                         │    ISP-1      │
                         │  OSPF R-ID    │
                         │    1.1.1.3    │
                         └───────┬───────┘
                                 │
                                 │ OSPF
                                 │
                         ┌───────┴───────┐
                         │      R1       │
                         │  R-ID 1.1.1.1 │
                         └───────┬───────┘
                                 │
                           Enterprise Core
                                 │
                         ┌───────┴───────┐
                         │   Core SW-1   │
                         │  R-ID 1.1.1.5 │
                         └───────┬───────┘
                                 │
               ┌─────────────────┼─────────────────┐
               │                 │                 │
             VLAN 10           VLAN 20           VLAN 30
             Sales             HR/Logistics       Finance
               │                 │                 │
             Access            Access            Access
             Switch             Switch            Switch

                         ┌───────────────┐
                         │      R2       │
                         │  R-ID 1.1.1.2 │
                         └───────┬───────┘
                                 │
                         ┌───────┴───────┐
                         │    ISP-2      │
                         │  R-ID 1.1.1.4 │
                         └───────────────┘
```

The actual Packet Tracer topology should be used as the authoritative physical diagram.

---

# 🎯 Lab Objectives

The primary objectives of this lab are to demonstrate the ability to:

1. Configure Cisco routers.
2. Configure Layer 3 interfaces.
3. Configure point-to-point serial links.
4. Configure Ethernet routed interfaces.
5. Implement OSPF dynamic routing.
6. Configure unique OSPF router IDs.
7. Establish OSPF neighbor relationships.
8. Configure multilayer switches.
9. Implement VLAN segmentation.
10. Configure trunk interfaces.
11. Configure Switch Virtual Interfaces (SVIs).
12. Implement inter-VLAN routing.
13. Configure DHCP relay using `ip helper-address`.
14. Implement access-port security.
15. Configure SSH access on switches.
16. Apply network management best practices.
17. Verify routing and connectivity.
18. Troubleshoot OSPF and Layer 3 connectivity.

---

# 🧩 Devices Used

| Device | Role |
|---|---|
| R1 | Enterprise/Core Router |
| R2 | Enterprise/Core Router |
| ISP-1 | ISP Router |
| ISP-2 | ISP Router |
| Multilayer Switch | Layer 3 Core/Distribution |
| SERVER-ROOM-SW | Server Room Access Switch |
| ICT-ENGINEER-SW | ICT Department Access Switch |
| INTER-RELATION-SW | Inter-Relations Department Switch |
| FINANCE-ACCOUNTING-SW | Finance & Accounting Switch |
| HR-LOGISTICS-SW | HR & Logistics Switch |
| SALES-MARKETING-SW | Sales & Marketing Switch |
| End Devices | PCs/servers and other hosts |

---

# 🌐 IP Addressing Plan

## Routed Links

| Connection/Network | Network | Device Address |
|---|---|---|
| R1 ↔ R2 | `195.136.17.20/30` | R1 `195.136.17.21` / R2 `195.136.17.22` |
| R1 ↔ ISP-1 | `195.136.17.36/30` | R1 `195.136.17.37` / ISP-1 `195.136.17.38` |
| R2 ↔ ISP-2 | `195.136.17.8/30` | R2 `195.136.17.9` / ISP-2 `195.136.17.10` |
| R1 ↔ Core SW | `195.136.17.0/30` | R1 `195.136.17.2` / SW `195.136.17.1` |
| R2 ↔ Core SW | `195.136.17.4/30` | R2 `195.136.17.6` / SW `195.136.17.5` |
| ISP-1 ↔ ISP-2 | `195.136.17.40/30` | ISP-1 `195.136.17.41` / ISP-2 `195.136.17.42` |

---

# 🏢 VLAN & Department Design

The enterprise LAN is divided into logical VLANs.

| VLAN | Network | Gateway | Purpose |
|---:|---|---|---|
| 10 | `192.168.10.0/26` | `192.168.10.1` | Sales & Marketing |
| 20 | `192.168.10.64/26` | `192.168.10.65` | HR & Logistics |
| 30 | `192.168.10.128/26` | `192.168.10.129` | Finance & Accounting |
| 40 | `192.168.10.192/26` | `192.168.10.193` | Inter-Relations |
| 50 | `192.168.11.0/26` | `192.168.11.1` | ICT Engineering |
| 60 | `192.168.12.192/26` | `192.168.12.193` | Server Room |

The multilayer switches provide Layer 3 gateways for the VLANs.

---

# 🔀 Inter-VLAN Routing

The multilayer switches are configured for Layer 3 routing using:

```text
ip routing
```

SVIs are configured for each departmental VLAN.

Example:

```cisco
interface Vlan10
 ip address 192.168.10.1 255.255.255.192
 ip helper-address 192.168.12.196
```

Similar SVI configurations are implemented for VLANs 20, 30, 40, 50 and 60.

This allows hosts in different VLANs to communicate through the Layer 3 switching infrastructure.

---

# 🔄 DHCP Relay

The VLAN interfaces use:

```cisco
ip helper-address 192.168.12.196
```

This forwards DHCP requests from the departmental VLANs toward the DHCP server.

Configured VLANs include:

```text
VLAN 10 → DHCP Relay
VLAN 20 → DHCP Relay
VLAN 30 → DHCP Relay
VLAN 40 → DHCP Relay
VLAN 50 → DHCP Relay
VLAN 60 → DHCP Relay
```

This demonstrates centralized DHCP services across multiple routed VLANs.

---

# 🚦 OSPF Configuration

The lab uses **OSPF Process ID 10** with **Area 0** as the routing domain.

## OSPF Router IDs

| Device | OSPF Router ID |
|---|---|
| R1 | `1.1.1.1` |
| R2 | `1.1.1.2` |
| ISP-1 | `1.1.1.3` |
| ISP-2 | `1.1.1.4` |
| Core Switch 1 | `1.1.1.5` |
| Core Switch 2 | `1.1.1.6` |

---

## R1 OSPF

```cisco
router ospf 10
 router-id 1.1.1.1
 network 195.136.17.0 0.0.0.3 area 0
 network 195.136.17.20 0.0.0.3 area 0
 network 195.136.17.36 0.0.0.3 area 0
```

---

## R2 OSPF

```cisco
router ospf 10
 router-id 1.1.1.2
 network 195.136.17.4 0.0.0.3 area 0
 network 195.136.17.20 0.0.0.3 area 0
 network 195.136.17.8 0.0.0.3 area 0
```

---

## ISP-1 OSPF

```cisco
router ospf 10
 router-id 1.1.1.3
 network 195.136.17.40 0.0.0.3 area 0
 network 195.136.17.36 0.0.0.3 area 0
```

---

## ISP-2 OSPF

```cisco
router ospf 10
 router-id 1.1.1.4
 network 195.136.17.8 0.0.0.3 area 0
 network 195.136.17.40 0.0.0.3 area 0
```

---

# 🔗 OSPF Neighbor Formation

The configuration successfully demonstrates OSPF neighbor relationships.

Examples observed during configuration include:

```text
Nbr 1.1.1.1 on Serial0/3/0 from LOADING to FULL
Nbr 1.1.1.3 on Serial0/3/0 from LOADING to FULL
Nbr 1.1.1.4 on Serial0/3/1 from LOADING to FULL
Nbr 1.1.1.5 on GigabitEthernet0/0 from LOADING to FULL
Nbr 1.1.1.6 on GigabitEthernet0/0 from LOADING to FULL
```

The `FULL` state indicates successful OSPF adjacency formation.

---

# 🔀 Trunking

The multilayer switches use trunk interfaces to transport multiple VLANs.

Example:

```cisco
interface GigabitEthernet1/0/2
 switchport mode trunk
```

Multiple uplink interfaces are configured as trunks.

The access switches also use trunk links toward the network infrastructure:

```cisco
interface FastEthernet0/1
 switchport mode trunk

interface FastEthernet0/2
 switchport mode trunk
```

This enables multiple VLANs to traverse the uplinks.

---

# 🖥️ Departmental Access Switches

## Sales & Marketing

Device:

```text
SALES-MARKETING-SW
```

Access ports are assigned to:

```text
VLAN 10
```

Example:

```cisco
interface FastEthernet0/3
 switchport access vlan 10
 switchport mode access
```

---

## HR & Logistics

Device:

```text
HR-LOGISTICS-SW
```

Access ports are assigned to:

```text
VLAN 20
```

---

## Finance & Accounting

Device:

```text
FINANCE-ACCOUNTING-SW
```

Access ports are assigned to:

```text
VLAN 30
```

This switch also demonstrates port-security configuration.

---

## Inter-Relations

Device:

```text
INTER-RELATION-SW
```

Access ports are assigned to:

```text
VLAN 40
```

---

## ICT Engineering

Device:

```text
ICT-ENGINEER-SW
```

Access ports are assigned to:

```text
VLAN 50
```

---

## Server Room

Device:

```text
SERVER-ROOM-SW
```

Access ports are assigned to:

```text
VLAN 60
```

---

# 🔐 Switch Security

The access switches demonstrate basic management security.

Configured features include:

- Local administrator account
- Encrypted passwords
- SSH-only VTY access
- Domain name configuration
- Console authentication
- Login banners
- Port security

Example:

```cisco
username admin privilege 1 password 7 <encrypted-password>

ip domain-name karis.com
```

SSH is enabled on the VTY lines:

```cisco
line vty 0 4
 login local
 transport input ssh

line vty 5 15
 login local
 transport input ssh
```

This prevents Telnet access and provides a more secure remote management method.

---

# 🛡️ Port Security

The Finance & Accounting switch demonstrates sticky MAC-based port security.

Example:

```cisco
interface FastEthernet0/3
 switchport access vlan 30
 switchport mode access
 switchport port-security
 switchport port-security mac-address sticky
```

Sticky MAC addresses allow the switch to dynamically learn and retain the connected device's MAC address.

This helps prevent unauthorized devices from being connected to protected access ports.

---

# 💻 Important Network Commands

## Check Interface Status

```cisco
show ip interface brief
```

## View Running Configuration

```cisco
show running-config
```

## Check OSPF Neighbors

```cisco
show ip ospf neighbor
```

## Check OSPF Configuration

```cisco
show ip protocols
```

## View OSPF Routes

```cisco
show ip route ospf
```

## View Routing Table

```cisco
show ip route
```

## Check VLANs

```cisco
show vlan brief
```

## Check Trunks

```cisco
show interfaces trunk
```

## Check Port Security

```cisco
show port-security
```

## Check Port-Security Interfaces

```cisco
show port-security interface
```

## Test Connectivity

```cisco
ping <destination-ip>
```

## Trace the Path

```cisco
traceroute <destination-ip>
```

---

# 🧪 Verification & Troubleshooting

A NOC/network-engineering approach should verify the network in layers.

### 1. Physical Layer

Check:

```cisco
show ip interface brief
```

Verify that interfaces are:

```text
up / up
```

---

### 2. Layer 2

Check VLAN and trunk configuration:

```cisco
show vlan brief
show interfaces trunk
```

Confirm that the correct access ports belong to the correct VLANs.

---

### 3. Layer 3

Verify SVI addresses:

```cisco
show ip interface brief
```

Check the routing table:

```cisco
show ip route
```

---

### 4. OSPF

Check neighbor relationships:

```cisco
show ip ospf neighbor
```

Expected neighbor state:

```text
FULL
```

---

### 5. End-to-End Connectivity

Use:

```cisco
ping
```

and:

```cisco
traceroute
```

to verify communication between different network segments.

---

# 📊 Key Networking Concepts Demonstrated

| Concept | Demonstrated |
|---|---:|
| IPv4 Addressing | ✅ |
| Subnetting | ✅ |
| /30 Point-to-Point Networks | ✅ |
| VLANs | ✅ |
| Access Ports | ✅ |
| Trunk Ports | ✅ |
| Layer 3 Switching | ✅ |
| SVI | ✅ |
| Inter-VLAN Routing | ✅ |
| DHCP Relay | ✅ |
| OSPF | ✅ |
| OSPF Area 0 | ✅ |
| OSPF Router IDs | ✅ |
| OSPF Neighbor Formation | ✅ |
| Port Security | ✅ |
| SSH Management | ✅ |
| Network Troubleshooting | ✅ |
| ISP Connectivity | ✅ |
| Enterprise Network Design | ✅ |

---

# 🧠 Skills Developed

This lab provides practical experience with:

### Routing

- OSPF configuration
- Dynamic route exchange
- Router IDs
- OSPF neighbor troubleshooting
- Point-to-point routing

### Switching

- VLAN segmentation
- Trunking
- Access ports
- Layer 3 switching
- SVI configuration

### Network Services

- DHCP relay
- Centralized DHCP architecture
- Server VLAN connectivity

### Network Security

- SSH
- Local authentication
- Password protection
- Port security
- Sticky MAC addresses

### NOC Operations

The lab also provides a foundation for NOC workflows such as:

- Interface monitoring
- Routing troubleshooting
- Neighbor-state verification
- Connectivity testing
- VLAN troubleshooting
- Incident isolation
- Layer-by-layer fault diagnosis

---

# 🧭 Troubleshooting Workflow

When a host cannot communicate across the network, use the following workflow:

```text
             ┌──────────────────────┐
             │   User Reports Issue │
             └──────────┬───────────┘
                        ↓
             ┌──────────────────────┐
             │ Check Physical Layer │
             └──────────┬───────────┘
                        ↓
             ┌──────────────────────┐
             │ Check Interface State│
             └──────────┬───────────┘
                        ↓
             ┌──────────────────────┐
             │ Check VLAN Assignment│
             └──────────┬───────────┘
                        ↓
             ┌──────────────────────┐
             │ Check Trunk Links    │
             └──────────┬───────────┘
                        ↓
             ┌──────────────────────┐
             │ Check SVI / Gateway  │
             └──────────┬───────────┘
                        ↓
             ┌──────────────────────┐
             │ Check Routing Table  │
             └──────────┬───────────┘
                        ↓
             ┌──────────────────────┐
             │ Check OSPF Neighbors │
             └──────────┬───────────┘
                        ↓
             ┌──────────────────────┐
             │ Ping / Traceroute    │
             └──────────┬───────────┘
                        ↓
             ┌──────────────────────┐
             │ Isolate & Resolve    │
             └──────────────────────┘
```

This workflow reflects the structured troubleshooting approach used in enterprise and ISP/NOC environments.

---

# 📁 Recommended Repository Structure

```text
enterprise-ospf-network-lab/
│
├── README.md
│
├── Screenshots/
│   └── topology.png
│
├── Packet-Tracer/
│   └── enterprise-ospf-network.pkt
│
├── Configurations/
│   ├── R1.txt
│   ├── R2.txt
│   ├── ISP-1.txt
│   ├── ISP-2.txt
│   ├── Core-SW-1.txt
│   ├── Core-SW-2.txt
│   ├── SERVER-ROOM-SW.txt
│   ├── ICT-ENGINEER-SW.txt
│   ├── INTER-RELATION-SW.txt
│   ├── FINANCE-ACCOUNTING-SW.txt
│   ├── HR-LOGISTICS-SW.txt
│   └── SALES-MARKETING-SW.txt
│
└── Documentation/
    └── IP-Addressing-Plan.md
```

---

# 📸 Evidence

The repository should include screenshots demonstrating:

- Complete network topology
- OSPF neighbor relationships
- Routing tables
- VLAN configuration
- Trunk configuration
- Interface status
- Successful ping tests
- Port-security configuration
- SSH configuration

The main topology screenshot is displayed above.

---

# 🚀 Learning Outcomes

After completing this lab, I strengthened practical skills in:

- Cisco IOS configuration
- Enterprise network architecture
- IPv4 subnetting
- Dynamic routing
- OSPF
- VLAN architecture
- Inter-VLAN routing
- Layer 3 switching
- DHCP relay
- Network security
- SSH administration
- Port security
- Network troubleshooting

The lab also demonstrates the ability to move beyond basic CCNA configuration into a more **enterprise/NOC-oriented troubleshooting and infrastructure design mindset**.

---

# 👨‍💻 Author

**Stephen Kariuki**

**Full-Stack, IoT & Networking Engineer**

- 🌐 Networking & Infrastructure
- 🔌 Cisco Networking
- 📡 ISP / GPON Networking
- 💻 Full-Stack Development
- 🤖 IoT Systems
- 🛠️ Network Automation

---

## ⭐ Project Purpose

This project is part of my practical networking portfolio, demonstrating hands-on experience with **Cisco routing, switching, OSPF, VLANs, network segmentation, infrastructure security, and NOC-style troubleshooting**.

> **Built to demonstrate practical Network Engineering skills through hands-on Cisco Packet Tracer implementation.**