# VM-Network-Architecture

This project simulates a small business network environment using VirtualBox, pfSense, and multiple VLANs to isolate infrastructure, client, and server systems.

---

## Project Summary
 - Platform: VirtualBox
 - Tools: pfSense CE acting as the virtual firewall/router
 - Objective: Segment networks using VLAN-like isolation to simulate departmental boundaries, enforce inter-VLAN access policies, and demonstrate secure architecture.

---

## Network Overview

### pfSense WAN (Untagged)
| VLAN | Subnet           | Purpose             |
|------|------------------|---------------------|
| 0    | 192.168.1.0/24   | Internal Network Firewall |

### VLANs (LAN Side)
| VLAN | Subnet           | Purpose             |
|------|------------------|---------------------|
| 1    | 192.168.10.0/24  | Infrastructure + DC |
| 2    | 192.168.20.0/24  | Clients (Sales)       |
| 3    | 192.168.30.0/24  | Clients (HR) |
| 4    | 192.168.40.0/24  | File Server (HR) |

> Each VLAN is simulated via separate VirtualBox internal networks due to lack of 802.1Q tagging support.
> pfSense acts as the default gateway and control point for all traffic between VLANs and to the internet.
> DHCP is provisioned per VLAN segment to ensure network autonomy and proper IP assignment.

### VirtualBox Adapter Configuration (pfSense VM):
| Adapter | Network Mode  | VB Network Name  | Simulated VLAN | Purpose |
|------|------------------|---------------------|-------------|---------|
| 1    | NAT  | --- | WAN | Internet access (host)
| 2    | Internal  | LabNet_Trunk  | Management | pfSense Web UI |
| 3    | Internal  | LabNet_VLAN10 | VLAN10 | Infrastructure (DC01) |
| 4    | Internal  | LabNet_VLAN20 | VLAN20 | Sales Client |

> **Note:** VLAN 30 and 40 are demonstrated by reassigning Adapter 3/4 to `LabNet_VLAN30` and `LabNet_VLAN40` as needed. This simulates dynamic VLAN testing within VirtualBox's 4-adapter limit.

---

## Security Design Principles

### Network Segmentation & Lease Privilege
 - Each department is isolated into is own VLAN to minimize lateral movement risk.
 - Inter VLAN communication is restricted explicitly based on business needs.
### Explicit Firewall Policies
 - No "allow all" rules (except temporary testing allowances). Each service port is explicitly defined.
> VLAN20_SALES only allows TCP 53, 135, 389, 445, etc., to the Domain Controller in VLAN10_INFRA.
### Zero Trust Emulation
 - VLAN boundaries act as security zones.
 - Trust is not assumed; even internal flows are controlled based on roles/policies.
### Defense in Depth
 - All VLANs are firewall isolated.
 - Services such as DNS, NTP, AD DS run only on the INFRA VLAN reducing attack surface.


## Traffic Flow & Firewall Intent

<a href="https://github.com/mstarLabs/VM-Network-Architecture/blob/main/VM%20Network%20Architecture%20Diagram.png">Ref 1: VM Network Diagram</a>

![VM Network Architecture Diagram](https://github.com/user-attachments/assets/c076820d-1fa0-4b73-bdb5-e2ce99c49035)

-  pfSense enforces all inter-VLAN rules centrally
-  All VMs route external traffic through pfSense NAT
-  All clients get DNS/GPO from DC01 (192.168.10.5)
-  Win10 (VLAN 20) **cannot access** File Server (VLAN40)
-  Win11 (VLAN 30) **can** access File Server (VLAN 40) on SMB/HTTPS

---

##  Skills Practiced

- Documentation and Diagramming
- Firewall Policy Planning
- Logical Topology Design
- Logical Network Mapping
- Network Segmentation Design
- Virtualized Network Simulation
