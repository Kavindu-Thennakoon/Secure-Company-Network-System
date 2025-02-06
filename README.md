# Secure Company Network System for Cytonn Innovation Ltd.

![Network Diagram](https://via.placeholder.com/800x400.png?text=Network+Diagram)  
*Figure 1: High-level network architecture for Cytonn Innovation Ltd.*

## 🌟 Project Overview
Cytonn Innovation Ltd is a fast-growing company specializing in cutting-edge cloud solutions. With the move to a new three-floor building housing 600 employees, this project outlines the design and implementation of a secure, scalable, and high-availability network infrastructure. The system ensures robust security, redundancy, and future-proof scalability to support the company's growth.

---

## 🏢 Building Structure and Departmental Layout
The new building accommodates the following departments across three floors:
- **Sales and Marketing**
- **Human Resources and Logistics**
- **Finance and Accounts**
- **Administration and Public Relations**
- **ICT Department** (Software Developers, Cloud Engineers, Cybersecurity Engineers, Network Engineers, System Administrators, IT Support Specialists, Business Analysts, and Project Managers)
- **Server Room** (Hosting critical IT infrastructure)

---

## 🔒 Network Security and Architecture
### Multi-Layered Security Approach
To safeguard the network from internal and external threats, the following security measures are implemented:
- **Cisco ASA Firewalls (5500-X series)**: Dual firewalls for redundancy and failover.
- **Security Zones**:
  - **Inside Zone**: AD servers (DHCP, DNS, Radius) for internal management.
  - **DMZ**: FTP, Web, Email, Application, and NAS storage servers.
  - **Outside Zone**: Two ISPs (SEACOM & Safaricom) for external connectivity.

---

## 🛠 Networking Components
### Internet Service Providers (ISPs)
- **SEACOM**: Public IP range `105.100.50.0/30`
- **Safaricom**: Public IP range `197.200.100.0/30`

### Routing and Switching
- **Core Switches**: Catalyst 3850 48-Port switches for centralized connectivity.
- **Access Switches**: Catalyst 2960 48-Port switches for end-user devices.
- **Routing Protocol**: OSPF for internal routing with static and dynamic routing on firewalls and switches.

### Wireless Infrastructure
- **Cisco Wireless LAN Controller (WLC)**: Manages Lightweight Access Points (LAPs) across floors.
- **Wireless Networks**: Separate SSIDs for employees, guests, and auditors.

### VoIP Services
- **Cisco Voice Gateway**: Manages IP phones and assigns dial numbers in the `4xxx` format.

---

## 📡 IP Addressing and VLANs
The network is segmented into VLANs for security and structure:
| VLAN ID | Name          | IP Range           | Purpose                     |
|---------|---------------|--------------------|-----------------------------|
| 10      | Management    | 192.168.10.0/24    | Network management          |
| 20      | LAN           | 172.16.0.0/16      | Internal LAN                |
| 50      | WLAN          | 10.20.0.0/16       | Wireless LAN                |
| 70      | VoIP          | 172.30.0.0/16      | Voice over IP               |
| 100     | DMZ           | 10.11.11.0/27      | Demilitarized Zone          |
| 199     | Blackhole     | Reserved           | Unused ports                |

---

## 🚀 Technical Requirements
- **Cisco Packet Tracer**: Used for design and simulation.
- **Hierarchical Network Model**: Ensures scalability and redundancy.
- **HSRP**: High availability routing for failover.
- **EtherChannel (LACP)**: Enhances link aggregation efficiency.
- **Inter-VLAN Routing**: Configured on multilayer switches.
- **DHCP Server**: For dynamic IP allocation.
- **Static Addressing**: For DMZ and critical infrastructure.
- **ACLs for SSH**: Restricts access to core devices.
- **BPDUguard & STP PortFast**: Prevents network loops and speeds up convergence.
- **Firewall Policies**: Defines security zones and ACLs.

---

## ⚙ Configuration Steps
### Initial Setup
```cisco
hostname CoreSwitch1
no ip domain-lookup
service password-encryption
enable secret CytonnSecure
banner motd #Unauthorized access is prohibited#
line console 0
 password CytonnConsole
 login
exit
