# University Campus Network
### Enterprise Multi-Campus Network Infrastructure

<p align="center">
  <img src="https://img.shields.io/badge/CISCO-Packet%20Tracer-049FD9?style=for-the-badge&logo=cisco&logoColor=white" />
  <img src="https://img.shields.io/badge/NETWORK-ENGINEERING-212121?style=for-the-badge" />
  <img src="https://img.shields.io/badge/OSPF-AREA%200-2F6DB5?style=for-the-badge" />
 
</p>

---

## 01. Project Overview

This project implements an enterprise-grade university campus network infrastructure connecting two geographically separated campuses through a routed WAN architecture.

The network is designed to support multiple academic departments, administrative services, centralized servers, and dedicated student environments.

The architecture uses hierarchical network segmentation, Layer 3 switching, point-to-point WAN connectivity, and dynamic routing through OSPF.

**Primary Objectives**

- Build a scalable multi-campus network infrastructure.
- Implement departmental segmentation using VLANs.
- Enable inter-VLAN communication through multilayer switching.
- Establish routed connectivity between campuses.
- Integrate a centralized server network.
- Implement dynamic routing using OSPF Area 0.
- Prepare the infrastructure for advanced enterprise security controls.

## 02. Network Architecture

The infrastructure is organized into three logical network zones:

| Zone | Description | Main Components |
|---|---|---|
| Campus 1 | Main University Campus | R1, MLS1, 8 Access Switches |
| Campus 2 | Secondary Campus | R3, MLS2, 2 Access Switches |
| Server Network | Centralized Services | R2, Server LAN |

### Logical Network Topology

```mermaid
flowchart TB

    subgraph SERVER["CENTRALIZED SERVER NETWORK"]
        SRV["Server Farm<br/>192.168.1.0/24"]
        R2["R2 - Server Gateway"]
        SRV --- R2
    end

    subgraph WAN["WAN BACKBONE"]
        R1["R1 - Core Router"]
        R3["R3 - Campus 2 Router"]
    end

    subgraph CAMPUS1["CAMPUS 1 - MAIN UNIVERSITY"]
        MLS1["MLS1 - Layer 3 Core"]

        subgraph B1["BUILDING A"]
            SW1["Access SW1<br/>VLAN 10 - ADMIN"]
            SW2["Access SW2<br/>VLAN 20 - FINANCE"]
            SW3["Access SW3<br/>VLAN 30 - HR"]
        end

        subgraph B2["BUILDING B"]
            SW4["Access SW4<br/>VLAN 40 - COMPUTING"]
            SW5["Access SW5<br/>VLAN 50 - ART"]
            SW6["Access SW6<br/>VLAN 60 - LAB"]
        end

        subgraph B3["BUILDING C"]
            SW7["Access SW7<br/>VLAN 70 - IT"]
            SW8["Access SW8<br/>VLAN 80 - STAFF"]
        end

        MLS1 --> SW1
        MLS1 --> SW2
        MLS1 --> SW3
        MLS1 --> SW4
        MLS1 --> SW5
        MLS1 --> SW6
        MLS1 --> SW7
        MLS1 --> SW8
    end

    subgraph CAMPUS2["CAMPUS 2 - SECONDARY UNIVERSITY"]
        MLS2["MLS2 - Layer 3 Core"]
        SW9["Access SW9<br/>VLAN 90 - BUSINESS"]
        SW10["Access SW10<br/>VLAN 100 - STUDENT LAB"]

        MLS2 --> SW9
        MLS2 --> SW10
    end

    R2 ---|"10.0.0.0/30"| R1
    R1 ---|"10.0.0.4/30"| R3
    R1 ---|"10.0.0.12/30"| MLS1
    R3 ---|"10.0.0.8/30"| MLS2

    style SERVER fill:#172B3A,stroke:#049FD9,color:#FFFFFF
    style WAN fill:#172B3A,stroke:#049FD9,color:#FFFFFF
    style CAMPUS1 fill:#172B3A,stroke:#049FD9,color:#FFFFFF
    style CAMPUS2 fill:#172B3A,stroke:#049FD9,color:#FFFFFF
```

## 03. VLAN Architecture

Departmental segmentation is implemented to isolate traffic and organize network resources according to university departments.

### Campus 1

| VLAN ID | Department | Network Role |
|---|---|---|
| 10 | Administration | Administrative Services |
| 20 | Finance | Financial Operations |
| 30 | Human Resources | HR Services |
| 40 | Computing | Computing Department |
| 50 | Art | Academic Department |
| 60 | Lab | Laboratory Environment |
| 70 | IT | IT Infrastructure |
| 80 | Staff | University Staff |

### Campus 2

| VLAN ID | Department | Network Role |
|---|---|---|
| 90 | Business | Business Department |
| 100 | Student Lab | Student Computing Environment |

Each VLAN is associated with a dedicated Layer 3 gateway configured on its respective multilayer switch.

## 04. IP Addressing & WAN Design

The WAN backbone uses point-to-point IPv4 addressing with /30 subnets.

| Connection | Network | Prefix |
|---|---|---|
| R2 ↔ R1 | 10.0.0.0 | /30 |
| R1 ↔ R3 | 10.0.0.4 | /30 |
| R3 ↔ MLS2 | 10.0.0.8 | /30 |
| R1 ↔ MLS1 | 10.0.0.12 | /30 |

### Server Network

| Network | Address |
|---|---|
| Server LAN | 192.168.1.0/24 |

### Campus 2 Gateways

| VLAN | Gateway |
|---|---|
| VLAN 90 | 172.16.90.1 |
| VLAN 100 | 172.16.100.1 |

*Campus 1 VLAN subnet assignments will be documented in the detailed IP addressing table.*

## 05. Routing Architecture

**Open Shortest Path First (OSPF)**

OSPF is deployed as the dynamic routing protocol across the network infrastructure.

- OSPF Process: Area 0
- Routing Type: Link-State
- Backbone Area: Area 0
- Participating Devices: R1, R2, R3, MLS1 and MLS2
- Advertised Networks: WAN links, server network and campus VLAN networks.

The objective is to provide dynamic route exchange between both campuses and the centralized server network.

## 06. Infrastructure Components

| Device | Role |
|---|---|
| R1 | Main Campus Core Router |
| R2 | Server Network Router |
| R3 | Secondary Campus Router |
| MLS1 | Campus 1 Layer 3 Core Switch |
| MLS2 | Campus 2 Layer 3 Core Switch |
| 10 Access Switches | Departmental Access Layer |
| End Devices | PCs, Printers and Servers |

## 07. Technologies & Protocols

<p align="center">
  <img src="https://img.shields.io/badge/Cisco-IOS-049FD9?style=flat-square&logo=cisco&logoColor=white" />
  <img src="https://img.shields.io/badge/IPv4-Networking-00599C?style=flat-square" />
  <img src="https://img.shields.io/badge/VLAN-802.1Q-00599C?style=flat-square" />
  <img src="https://img.shields.io/badge/OSPF-Dynamic%20Routing-2F6DB5?style=flat-square" />
  <img src="https://img.shields.io/badge/Layer%203-Switching-049FD9?style=flat-square" />
  <img src="https://img.shields.io/badge/WAN-Point--to--Point-212121?style=flat-square" />
</p>

## 08. Security Roadmap

The following security enhancements are planned for the infrastructure:

- Access Control Lists (ACLs)
- SSH-based device administration
- Switch Port Security
- DHCP Snooping
- Dynamic ARP Inspection
- VLAN isolation and traffic restrictions
- Secure management access

## 09. Repository Structure

```text
University-Campus-Network/
│
├── README.md
│
├── topology/
│   ├── campus-network.pkt
│   └── architecture.png
│
├── configurations/
│   ├── routers/
│   │   ├── R1.txt
│   │   ├── R2.txt
│   │   └── R3.txt
│   │
│   ├── multilayer-switches/
│   │   ├── MLS1.txt
│   │   └── MLS2.txt
│   │
│   └── access-switches/
│
├── documentation/
│   ├── ip-addressing.md
│   ├── vlan-design.md
│   ├── routing-design.md
│   └── security-design.md
│
└── tests/
    └── connectivity-tests.md
```

## 10. Project Status

| Component | Status |
|---|---|
| Network Topology | Implemented |
| WAN Connectivity | Configured |
| VLAN Architecture | Configured |
| Layer 3 Switching | Configured |
| OSPF Routing | Configured |
| Connectivity Validation | Pending Documentation |
| Enterprise Security | Planned |

## 11. Learning Outcomes

This project demonstrates practical networking skills in:

- Enterprise network architecture and design.
- Multi-campus infrastructure deployment.
- VLAN planning and network segmentation.
- Layer 2 and Layer 3 switching.
- Dynamic routing and OSPF.
- WAN addressing and point-to-point links.
- Cisco IOS configuration and troubleshooting.
- Network security planning.

---

<p align="center">
  <img src="https://img.shields.io/badge/Designed%20With-Cisco%20Packet%20Tracer-049FD9?style=for-the-badge&logo=cisco&logoColor=white" />
</p>

<p align="center">
  <strong>University Campus Network Infrastructure</strong><br/>
  Enterprise Networking | Network Engineering | Infrastructure Design
</p>
