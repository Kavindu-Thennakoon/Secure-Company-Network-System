# 📌 Design and Implementation of a Secure Company Network System

## 🏢 Company: Cytonn Innovation Ltd  
**Author:** Kavindu Thennakoon  
**Tools Used:** Cisco Packet Tracer 
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

## 📡 Network Diagram

![Network Diagram]([Diagrams/network-diagram.png](https://github.com/Kavindu-Thennakoon/Secure-Company-Network-System/blob/eb654d267bf2d3949abbadf6c1c1f0351246dcee/Network%20Diagram.png))


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

---

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

### 🛡 Standard ACL for SSH
```bash
access-list 10 permit 192.168.10.5
line vty 0 4
 access-class 10 in
 transport input ssh
```

### 🛡 BPDU Guard & Port Security
```bash
interface FastEthernet0/1
 switchport mode access
 switchport port-security
 switchport port-security maximum 2
 switchport port-security violation restrict
 spanning-tree bpduguard enable
```

### 🛡 OSPF Configuration
```bash
router ospf 1
 network 192.168.10.0 0.0.0.255 area 0
 network 10.20.0.0 0.0.255.255 area 1
```

---

## 🌐 Wireless Network Configuration
```bash
configure terminal
interface Dot11Radio0
 ssid CorporateWiFi
 authentication open
 authentication key-management wpa
 wpa-psk ascii "Cisco123"
```

---

## ☎ VoIP Configuration 
```bash
telephony-service
 max-ephones 50
 max-dn 100
 ip source-address 172.30.0.1 port 2000
 auto assign 1 to 100
```

---

## 🛠 Final Testing & Troubleshooting
| Issue | 	Solution |
|------------------|--------------|
| Devices not getting IPs | Check DHCP bindings & helper addresses |
| Inter-VLAN communication failing | Verify 'ip routing' & VLAN trunking |
| Firewall blocking access | 'Check access-list' & NAT rules |
| Slow network speeds | Analyze EtherChannel & STP config |

---

## 📌 Future Enhancements
🚀 **Integrate VPN** for secure remote access
🚀 **Implement Network** Monitoring using SNMP & Syslog
🚀 **Deploy Redundant Firewalls** for failover support
🚀 **Enable Multi-Factor Authentication** for added security

---

## 🔄 How to Run this Project
1️⃣ Open **Cisco Packet Tracer**
2️⃣ Load the .pkt project file
3️⃣ Configure devices using the provided scripts
4️⃣ Test connectivity & security policies
5️⃣ Deploy on a **real-world Cisco network**

---

## ✅ If you liked this project, don't forget to ⭐ star the repo!


