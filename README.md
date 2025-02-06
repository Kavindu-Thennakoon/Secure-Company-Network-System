# 📌 Design and Implementation of a Secure Company Network System

## 🏢 Company: Cytonn Innovation Ltd  
**Author:** Kavindu Thennakoon  
**Tools Used:** Cisco Packet Tracer, GNS3, Wireshark, Virtual Machines  
**Technologies:** Cisco ASA Firewall, VLANs, OSPF, HSRP, DHCP, DNS, VPN, VoIP, EtherChannel  

---

## 📜 Project Overview

Cytonn Innovation Ltd is moving to a new building and requires a **secure and scalable** network infrastructure. The goal is to design and implement a **corporate network** that ensures high performance, security, and redundancy across multiple departments.

### 🔹 Key Features:
✔ **Dual ISP Redundancy** using SEACOM & Safaricom.  
✔ **Cisco ASA Firewalls** for security zoning (Inside, Outside, DMZ).  
✔ **Hierarchical Network Design** for efficiency and scalability.  
✔ **Wireless & VoIP Integration** for seamless communication.  
✔ **Advanced Security Mechanisms** (ACLs, BPDU Guard, Port Security).  

---

## 🏗 Network Architecture

### 📌 Logical Network Design
The network is structured across three floors, with designated departments:  
- **1st Floor:** Sales & Marketing, Human Resources, Logistics  
- **2nd Floor:** Finance, Administration, Public Relations  
- **3rd Floor:** ICT Department (Software Devs, Cybersecurity, IT Support, Network Engineers)  

### 📌 IP Addressing Scheme
| Network Component | Subnet Range | VLAN ID |
|------------------|--------------|---------|
| **Management Network** | `192.168.10.0/24` | 10 |
| **LAN** | `172.16.0.0/16` | 20 |
| **WLAN** | `10.20.0.0/16` | 50 |
| **VoIP** | `172.30.0.0/16` | 70 |
| **DMZ** | `10.11.11.0/27` | 100 |
| **Public IPs (SEACOM)** | `105.100.50.0/30` | - |
| **Public IPs (Safaricom)** | `197.200.100.0/30` | - |

### 📌 VLAN Configuration
```bash
vlan 10
 name Management
vlan 20
 name LAN
vlan 50
 name WLAN
vlan 70
 name VoIP
vlan 100
 name DMZ
```

## 🔐 Security Implementations
### 🛡 Cisco ASA Firewall Configuration
```bash
interface GigabitEthernet0/0
 nameif outside
 security-level 0
 ip address 105.100.50.1 255.255.255.252

interface GigabitEthernet0/1
 nameif inside
 security-level 100
 ip address 192.168.10.1 255.255.255.0

interface GigabitEthernet0/2
 nameif dmz
 security-level 50
 ip address 10.11.11.1 255.255.255.224

access-list OUTSIDE_IN extended permit tcp any host 10.11.11.2 eq 80
access-list OUTSIDE_IN extended permit tcp any host 10.11.11.3 eq 443

global (outside) 1 interface
nat (inside, outside) dynamic interface
```

