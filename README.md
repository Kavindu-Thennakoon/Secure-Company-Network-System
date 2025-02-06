# Design and Implementation of a Secure Company Network System for Cytonn Innovation Ltd.

## Project Overview
Cytonn Innovation Ltd is a rapidly expanding company offering cutting-edge cloud solutions to clients worldwide. The company is moving into a new three-floor building to accommodate 600 employees, and this document outlines the network design and implementation strategy for their IT infrastructure. The proposed network design ensures high availability, redundancy, robust security, and scalability, ready to support future growth.

## Table of Contents
- [Project Overview](#project-overview)
- [Building Structure and Departmental Layout](#building-structure-and-departmental-layout)
- [Network Security and Architecture](#network-security-and-architecture)
- [Networking Components](#networking-components)
- [IP Addressing and VLANs](#ip-addressing-and-vlans)
- [Technical Requirements](#technical-requirements)
- [Configuration Steps](#configuration-steps)
- [Testing and Validation](#testing-and-validation)
- [Conclusion](#conclusion)
- [License](#license)

## Building Structure and Departmental Layout
The new building houses several departments across three floors:
- **Sales and Marketing**
- **Human Resources and Logistics**
- **Finance and Accounts**
- **Administration and Public Relations**
- **ICT Department** (including Software Developers, Cloud Engineers, Cybersecurity Engineers, Network Engineers, System Administrators, IT Support Specialists, Business Analysts, and Project Managers)
- **Server Room** (Hosting critical IT infrastructure)

## Network Security and Architecture
### Security Measures
A multi-layered security approach has been implemented to protect the network from internal and external threats:
- **Cisco ASA Firewalls (5500-X series)**: Two firewalls provide security, redundancy, and failover.
- **Security Zones**:
  - **Inside Zone**: AD servers (DHCP, DNS, Radius) for internal management.
  - **DMZ**: FTP, Web, Email, Application, and NAS storage servers.
  - **Outside Zone**: Two ISPs (SEACOM & Safaricom) for external communications.

## Networking Components
### Internet Services Provider (ISP)
- **SEACOM**: Public IP range 105.100.50.0/30
- **Safaricom**: Public IP range 197.200.100.0/30

### Routing and Switching
- **Core Switches**: Catalyst 3850 48-Port switches for centralized connectivity.
- **Access Switches**: Catalyst 2960 48-Port switches for end-user devices.
- **Routing**: OSPF for internal routing with static and dynamic routing configured on firewalls and switches.

### Wireless Infrastructure
- **Cisco Wireless LAN Controller (WLC)**: Manages Lightweight Access Points (LAPs) across floors.
- **Wireless Networks**: Separate SSIDs for employees, guests, and auditors.

### VoIP Services
- **Cisco Voice Gateway**: Manages IP phones and assigns dial numbers in the 4xxx format.

## IP Addressing and VLANs
The network is segmented into the following VLANs to ensure security and structure:
- **Management VLAN (ID 10)**: 192.168.10.0/24
- **LAN VLAN (ID 20)**: 172.16.0.0/16
- **WLAN VLAN (ID 50)**: 10.20.0.0/16
- **VoIP VLAN (ID 70)**: 172.30.0.0/16
- **DMZ VLAN (ID 100)**: 10.11.11.0/27
- **Blackhole VLAN (ID 199)**: Reserved for unused ports

## Technical Requirements
- **Cisco Packet Tracer**: Used for design and simulation.
- **Hierarchical Network Model**: Implemented for scalability and redundancy.
- **HSRP**: High availability routing for failover.
- **EtherChannel (LACP)**: Enhances link aggregation efficiency.
- **Inter-VLAN Routing**: Configured on multilayer switches.
- **DHCP Server**: For dynamic IP allocation.
- **Static Addressing**: For DMZ and critical infrastructure.
- **ACLs for SSH**: Only authorized PCs can access core devices.
- **BPDUguard & STP PortFast**: Prevents network loops and speeds up convergence.
- **Firewall Policies**: Defined security zones and ACLs.

## Configuration Steps
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
### VLAN and Network Configuration
```cisco
vlan 10
 name Management
vlan 20
 name LAN
vlan 50
 name WLAN
vlan 70
 name VoIP
vlan 199
 name Blackhole
exit
### VLAN Interface Assignment
```cisco
interface GigabitEthernet1/0/1
 switchport mode access
 switchport access vlan 10
 spanning-tree portfast
 spanning-tree bpduguard enable
exit


### EtherChannel Configuration (LACP)
```cisco
interface Port-channel1
 switchport mode trunk
 switchport trunk allowed vlan 10,20,50,70
exit
interface GigabitEthernet1/0/1
 channel-group 1 mode active
interface GigabitEthernet1/0/2
 channel-group 1 mode active
exit


### OSPF Configuration
```cisco
router ospf 1
 network 192.168.10.0 0.0.0.255 area 0
 network 172.16.0.0 0.0.255.255 area 0
 network 172.30.0.0 0.0.255.255 area 0
 network 10.20.0.0 0.0.255.255 area 0
exit


### DHCP Server Configuration
```cisco
ip dhcp excluded-address 192.168.10.1 192.168.10.10
ip dhcp pool MANAGEMENT
 network 192.168.10.0 255.255.255.0
 default-router 192.168.10.1
 dns-server 8.8.8.8
exit


### HSRP Configuration for Redundancy
```cisco
interface GigabitEthernet1/0/1
 standby 1 ip 192.168.10.254
 standby 1 priority 110
 standby 1 preempt
exit


### Standard ACL for SSH
```cisco
access-list 10 permit 192.168.10.100
line vty 0 4
 access-class 10 in
 transport input ssh
 login local
exit


### Cisco ASA Firewall Configuration
```cisco
interface GigabitEthernet0/0
 nameif OUTSIDE
 security-level 0
 ip address 105.100.50.1 255.255.255.252
exit

interface GigabitEthernet0/1
 nameif INSIDE
 security-level 100
 ip address 192.168.10.1 255.255.255.0
exit

interface GigabitEthernet0/2
 nameif DMZ
 security-level 50
 ip address 10.11.11.1 255.255.255.224
exit

route OUTSIDE 0.0.0.0 0.0.0.0 105.100.50.2

access-list OUTSIDE_IN extended permit tcp any host 105.100.50.1 eq https
access-list OUTSIDE_IN extended permit tcp any host 105.100.50.1 eq ssh

nat (INSIDE,OUTSIDE) dynamic interface


### VoIP Configuration
```cisco
voice service voip
 allow-connections h323 to sip
 allow-connections sip to h323
 allow-connections sip to sip
 exit

interface GigabitEthernet0/3
 description Voice Gateway
 ip address 172.30.0.1 255.255.0.0
exit

telephony-service
 max-ephones 50
 max-dn 100
 ip source-address 172.30.0.1 port 2000
 exit


### Wireless LAN Controller Configuration
```cisco
interface vlan 50
 ip address 10.20.0.1 255.255.0.0
 ip helper-address 192.168.10.1
exit

wireless mobility group name CYTONN_WLAN


## Technical Requirements
- **Connectivity** Ensure all devices are connected and reachable across VLANs.
- **DHCP** Test that clients are correctly receiving IP addresses.
- **Firewall Security** Validate firewall policies and access control.
- **VoIP** Test VoIP functionality and ensure clear communication.
- **Wireless Networks** Ensure SSID segmentation and connectivity.


## Technical Requirements
This network design is robust, scalable, and secure, ensuring Cytonn Innovation Ltd. is ready to meet its current and future needs. The implementation will ensure business continuity and provide a high-performance infrastructure for seamless operations across departments.


