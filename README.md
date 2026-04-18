# CCNA / Networking Labs

A collection of networking labs created while learning and practicing routing, switching, and network troubleshooting concepts using Cisco Packet Tracer.

This repository documents hands-on networking practice and provides configuration examples for common networking protocols.

## CCN LAB's

- [DHCP Configuratin](#dhcp-configuration-lab)
- [RIP Configuration](#rip-configuration-lab)
- [OSPF Confuguration](#ospf-configuration-lab)
- [Standard ACL Configuration](#standard-acl-configuration-lab)

Each folder contains:

- Packet Tracer lab file (.pkt)
- Configuration notes
- Lab instructions
- Screenshots of topology and results

# Labs

---

## DHCP Configuration Lab

### Objective
To understand and configure Dynamic Host Configuration Protocol (DHCP) in a network environment so that devices can automatically obtain IP addresses and other network configuration parameters. This lab demonstrates how DHCP simplifies network administration by dynamically assigning IP addresses to hosts instead of configuring them manually.

### How to confiugure
- Open the .pkt file
- Open the router CLI
- And in configuration mode assign dynamic IP to computers using DHCP
- Use follwoing commands to configure DCHP

### dhcp

A(config)#ip dhcp excluded-address 13.0.0.1 // Here this is the IP of interface to which switch is connected. \
A(config)#ip dhcp pool A // Creating a pool. \
A(dhcp-config)#network 13.0.0.0 255.0.0.0 // This is the network IP of the interface. \
A(dhcp-config)#default-router 13.0.0.1 // This is the default gateway for switch traffic acctually the IP of interface. \
A(dhcp-config)#dns-server 8.8.8.8 \
A(dhcp-config)#exit

### Screenshot
### Netwrok
<img width="679" height="353" alt="image" src="https://github.com/user-attachments/assets/eb0da784-f688-47b3-9e3b-a400863d9851" />
### Allowing DHCP on end devices
<img width="1362" height="472" alt="3" src="https://github.com/user-attachments/assets/e30b640a-8d0b-4da0-b122-44c65b19147a" />

## RIP Configuration Lab
### Objective
Configure and verify RIP routing between multiple routers.
### Topics Covered
- RIP Version 2
- Network advertisement
- Route verification
- Basic troubleshooting
### How to confiugure
- Open the .pkt file
- Open the router CLI
- And in configuration mode assign dynamic IP to computers using DHCP
- Use follwoing commands to configure DCHP
### Configure DHCP
First of all configure DHCP on each router to assign dynamic IP to end devices.([DHCP Configuratin](#dhcp-configuration-lab))
### RIP
A(config)#interface serial 0/3/0 \
A(config-if)#ip address 10.0.0.1 255.0.0.0 \
A(config-if)#encapsulation hdlc \
A(config-if)#clock rate 64000 \
A(config-if)#no shutdown \
%LINK-5-CHANGED: Interface Serial0/3/0, changed state to down \
A(config-if)#exit \
A(config)#interface serial 0/3/1 \
A(config-if)#ip address 11.0.0.1 255.0.0.0 \
A(config-if)#encapsulation hdlc \
A(config-if)#clock rate 64000 \
A(config-if)#no shutdown \
%LINK-5-CHANGED: Interface Serial0/3/1, changed state to down \
A(config-if)#exit \
A(config)#router rip \
A(config-router)#version 2 \
A(config-router)#no auto-summary \
A(config-router)#network 10.0.0.0 //serial network IP \
A(config-router)#network 11.0.0.0 //serail network IP \
A(config-router)#network 13.0.0.0 //Fas network IP \



### Screenshot
### Netwrok
<img width="1036" height="372" alt="2" src="https://github.com/user-attachments/assets/4b0e9d4b-003f-4344-b93b-eb050ac88531" />

---

## OSPF Configuration Lab
### Objctives
Configure a Multi area OSPF
## Network Design
### Backbone Area (Area 0)
The following networks are part of Area 0:
- 10.0.0.0/8  
- 11.0.0.0/8  
- 12.0.0.0/8  
### Other Areas
- Area 1 → Router A LAN  
- Area 2 → Router C LAN  
- Area 3 → Router D LAN

### Network
<img width="965" height="358" alt="Screenshot 2026-04-18 141336" src="https://github.com/user-attachments/assets/e0bb27d3-68d3-41ff-aeda-2b72935d0f26" />

---
## Pre-Configuration Steps
1. Assign IP addresses to all interfaces as per topology  
2. Identify DCE serial interface:
   clock rate 64000
   show controllers serial 0/0/0
3.Enable all interfaces:
   no shutdown
## Full OSPF configuration on each router:
### Router A Configuration
A(config)#router ospf 1 \
A(config-router)#network 10.0.0.0 0.255.255.255 area 0 \
A(config-router)#network 192.168.1.0 0.0.0.255 area 1 
### Router B Configuration (Backbone Router)
B(config)#router ospf 1 \
B(config-router)#network 10.0.0.0 0.255.255.255 area 0 \
B(config-router)#network 11.0.0.0 0.255.255.255 area 0 \
B(config-router)#network 192.168.2.0 0.0.0.255 area 0
### Router C Configuration
C(config)#router ospf 1 \
C(config-router)#network 11.0.0.0 0.255.255.255 area 0 \
C(config-router)#network 192.168.3.0 0.0.0.255 area 2 \
C(config-router)#network 12.0.0.0 0.255.255.255 area 0
### Router D Configuration
D(config)#router ospf 1 \
D(config-router)#network 12.0.0.0 0.255.255.255 area 0 \
D(config-router)#network 192.168.4.0 0.0.0.255 area 3
### Verification

Verify the configuration by running command: \
show ip route \
now look for any IP comming as O, Its OSPF. 

---

## Standard ACL Configuration Lab
ACL stands for access control logic. It is the protocol that allow host to block spacific traffic, it may be a single host, a network or a range of IP addresses. It is generally deployed on destination end. 
### Objective
Configure the stndard ACL by blocking a host and a network.
### Configuration
First of all configure either [RIP](#rip-configuration-lab) or [OSPF](#ospf-configuration-lab) to advertise the network over the topology.
### Network
Considet the below topology with OSPF configured.
<img width="1241" height="524" alt="image" src="https://github.com/user-attachments/assets/b0f649c4-5dbf-4743-a789-ce97a7ea6f30" />
Here we will perform following operations verfi=ying that it is a ACL
- PC1 wont communicate with network 192.168.4.0 at Router D
- Network 192.168.3.0 at Router C wont communicate with network 192.168.2.0 at Router B
### Code
As we know thatStandard ACL is configures at destinition end, thus in our example ACL will be configures at Router D and Router B. \
#### At Router B
B(config)#access-list 20 deny 192.168.3.0 0.0.0.255 \
B(config)#access-list 20 permit any \
B(config)#interface fastEthernet 0/0 \
B(config-if)#ip access-group 20 in \
B(config-if)#ip access-group 20 out 
#### AT Router D
D(config)#access-list 10 deny 192.168.1.2 \
D(config)#access-list 10 permit any \
D(config)#interface fastEthernet 0/0  \
D(config-if)#ip access-group 10 in \
D(config-if)#ip access-group 10 out 

### Verifying by pinging
Ping From PC1 to PC7
<img width="639" height="241" alt="image" src="https://github.com/user-attachments/assets/5ddac0e8-a3a3-4461-b68b-b864323925db" />
We can see saying host unreachable

---

# Tools Used

- Cisco Packet Tracer
- CLI based configuration
- Networking troubleshooting commands

---

# How to Use These Labs

1. Download the `.pkt` file from the lab folder.
2. Open it using Cisco Packet Tracer.
3. Follow the configuration steps provided in the lab README file.
4. Verify connectivity using commands like:
   - `ping`
   - `show ip route`
   - `show ip protocols`

---

# Learning Goals

The primary goal of this repository is to build practical knowledge of computer networking concepts through hands-on labs and configuration exercises.

Through these labs, the following skills and concepts are practiced:

- Understanding fundamental networking concepts and architecture
- Learning how devices communicate in a network
- Configuring routers and switches using CLI commands
- Implementing routing protocols such as RIP, OSPF, and static routing
- Creating and managing VLANs in switched networks
- Configuring IP addressing and subnetting
- Implementing Network Address Translation (NAT)
- Setting up DHCP for automatic IP address assignment
- Understanding switching concepts such as trunking and access ports
- Practicing network troubleshooting using diagnostic commands
- Interpreting routing tables and network topology
- Strengthening problem-solving skills in network configuration scenarios

These labs are intended to strengthen practical networking skills and support preparation for networking-related certifications and real-world networking environments.

---

# Contributing

Contributions are welcome.  
If you would like to improve a lab or add new networking scenarios, feel free to open a pull request.

---
## 👨‍💻 Author

**Muddasir Azam**

GitHub  
https://github.com/MuddasirAzam-065

---
Created as part of networking practice and CCNA learning journey.
