# VM-Network-Architecture

This lab simulates a small business network environment using VirtualBox, pfSense, and multiple VLANs to isolate infrastructure, client, and server systems.

---

## Network Overview

**Virtualized Environment:**  
- All systems run in VirtualBox on a single host
- pfSense manages routing, DHCP, DNS forwarding, and inter-VLAN access

**VLANs:**
| VLAN | Subnet           | Purpose             |
|------|------------------|---------------------|
| 1    | 192.168.12.0/24  | Infrastructure + DC |
| 2    | 192.168.20.0/24  | Clients (Sales)       |
| 3    | 192.168.30.0/24  | Clients (HR) |
| 4    | 192.168.40.0/24  | File Server (HR) |

---

## Traffic Flow & Firewall Intent

![Network Diagram](../../docs/network-diagrams/homelab_network_diagram_v1.png)

-  Win11 (VLAN 3) can access File Server (VLAN 4) on SMB/HTTPS
-  Win10 (VLAN 2) **cannot access** File Server
-  All clients get DNS/GPO from DC01 (192.168.12.5)
-  All VMs route external traffic through pfSense NAT
-  pfSense enforces all inter-VLAN rules centrally

---

##  Skills Practiced

- Documentation and Diagramming
- Firewall Policy Planning
- Logical Network Mapping
- Network Segmentation Design
