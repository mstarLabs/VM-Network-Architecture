# VM-Network-Architecture

This project simulates a small business network environment using VirtualBox, pfSense, and multiple VLANs to isolate infrastructure, client, and server systems.

---

## Network Overview

**Virtualized Environment:**  
- All systems run in VirtualBox on a single host
- pfSense manages routing, DHCP, DNS forwarding, and inter-VLAN access

---

## Firewall & VLAN Design

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

---

## VirtualBox Adapter Configuration (pfSense VM):
| Adapter | Network Mode  | VB Network Name  | Simulated VLAN | Purpose |
|------|------------------|---------------------|-------------|---------|
| 1    | NAT  | --- | WAN | Internet access (host)
| 2    | Internal  | LabNet_Trunk  | Management | pfSense Web UI |
| 3    | Internal  | LabNet_VLAN10 | VLAN10 | Infrastructure (DC01) |
| 4    | Internal  | LabNet_VLAN20 | VLAN20 | Sales Client |

> **Note:** VLAN 30 and 40 are demonstrated by reassigning Adapter 3/4 to `LabNet_VLAN30` and `LabNet_VLAN40` as needed. This simulates dynamic VLAN testing within VirtualBox's 4-adapter limit.

---

## Traffic Flow & Firewall Intent

<a href="https://github.com/mstarLabs/VM-Network-Architecture/blob/main/VM%20Network%20Architecture.png">Ref 1: VM Network Diagram</a>
![VM Network Architecture](https://github.com/user-attachments/assets/8672ad2b-16bf-401e-a606-a360b705fb09)



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
