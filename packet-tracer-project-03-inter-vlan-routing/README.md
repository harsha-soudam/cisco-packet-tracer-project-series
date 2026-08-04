# 🌐 Cisco Packet Tracer Project 03 – Inter-VLAN Routing

<div align="center">

![Cisco](https://img.shields.io/badge/Cisco-Packet%20Tracer-1BA0D7?style=for-the-badge&logo=cisco&logoColor=white)
![Routing](https://img.shields.io/badge/Inter--VLAN-Routing-blue?style=for-the-badge)
![CCNA](https://img.shields.io/badge/Router--on--a--Stick-success?style=for-the-badge)

</div>

## 📖 Overview

This project demonstrates Inter-VLAN Routing using the Router-on-a-Stick (ROAS) technique. Building upon the VLAN configuration from Project 2, a Cisco 1941 router is introduced to enable communication between multiple VLANs over a single trunk link.

The project covers trunk port configuration, IEEE 802.1Q encapsulation, router subinterfaces, default gateway configuration, and verification of communication between different VLANs.

---

## 🎯 Objectives

- Understand the need for Inter-VLAN Routing
- Configure Router-on-a-Stick (ROAS)
- Configure 802.1Q trunking
- Create router subinterfaces
- Configure default gateways
- Enable communication between different VLANs
- Verify routing functionality
- Analyze packet flow between VLANs

---

## 🏢 Real-World Scenario

TechNova Solutions has successfully segmented its Human Resources, Finance, IT, and Management departments using VLANs. However, employees are now unable to access shared resources located in other VLANs.

To solve this issue while maintaining network segmentation, a Cisco 1941 router is deployed using the Router-on-a-Stick architecture, allowing secure communication between VLANs through a single trunk connection.

---

## 🖥️ Network Topology

<img width="760" height="323" alt="image" src="https://github.com/user-attachments/assets/93f6b61f-cd61-48e4-9e31-2bed552ae29b" />


---

## 📦 Devices Used

| Device | Quantity |
|---------|---------:|
| Cisco 2960 Switch | 1 |
| Cisco 1941 Router | 1 |
| PC-PT | 7 |
| Server-PT | 1 |

---

## 🏢 Network Design

| Department | VLAN | Network | Gateway |
|------------|------|----------------|---------------|
| HR | 10 | 192.168.10.0/24 | 192.168.10.1 |
| Finance | 20 | 192.168.20.0/24 | 192.168.20.1 |
| IT | 30 | 192.168.30.0/24 | 192.168.30.1 |
| Management | 99 | 192.168.99.0/24 | 192.168.99.1 |
| File Server | 1 | 192.168.1.0/24 | N/A |

---

## 🔌 Physical Connections

| Device | Interface | Connected To |
|----------|-----------|--------------|
| Router | Gig0/0 | Switch Gig0/1 |
| HR1 | Fa0 | Switch Fa0/1 |
| HR2 | Fa0 | Switch Fa0/2 |
| FIN1 | Fa0 | Switch Fa0/3 |
| FIN2 | Fa0 | Switch Fa0/4 |
| IT1 | Fa0 | Switch Fa0/5 |
| IT2 | Fa0 | Switch Fa0/6 |
| MANAGER | Fa0 | Switch Fa0/7 |
| FILE SERVER | Fa0 | Switch Fa0/8 |

---

## 🌐 Router Subinterfaces

| Subinterface | VLAN | IP Address |
|--------------|------|----------------|
| Gig0/0.10 | 10 | 192.168.10.1 |
| Gig0/0.20 | 20 | 192.168.20.1 |
| Gig0/0.30 | 30 | 192.168.30.1 |
| Gig0/0.99 | 99 | 192.168.99.1 |

---

## 📚 Concepts Learned

- Inter-VLAN Routing
- Router-on-a-Stick
- IEEE 802.1Q
- Trunk Ports
- Router Subinterfaces
- Default Gateway
- Layer 3 Routing
- Packet Routing
- Network Segmentation

---

## ✅ Verification

The network configuration was verified using:

- `show interfaces trunk`
- `show ip interface brief`
- Successful Inter-VLAN Ping
- Router CLI
- Switch CLI

---

## 📸 Verification Screenshots

### Network Topology

<img width="760" height="323" alt="image" src="https://github.com/user-attachments/assets/cbe4e838-ca2c-4b5e-8c5d-6ebc73e3fe2a" />


---


## 🛠️ Troubleshooting Performed

- Verified trunk port configuration
- Verified router subinterfaces
- Verified IEEE 802.1Q encapsulation
- Verified default gateway configuration
- Verified Inter-VLAN communication
- Tested connectivity using ICMP Ping

---

## 💼 Skills Demonstrated

- Cisco Packet Tracer
- Router-on-a-Stick
- VLAN Routing
- Router Configuration
- Layer 3 Networking
- Cisco CLI
- Network Troubleshooting
- Enterprise Network Design

---

## 📖 Lessons Learned

- A Layer 2 switch cannot route traffic between VLANs.
- Router-on-a-Stick enables multiple VLANs to communicate using a single physical router interface.
- IEEE 802.1Q tagging allows multiple VLANs to traverse a single trunk link.
- Every VLAN requires its own default gateway.
- Router subinterfaces provide logical interfaces for each VLAN.

---



## 🚀 Future Improvements

Project 4 will introduce:

- DHCP Configuration
- DNS Configuration
- Automatic IP Address Assignment
- Centralized Network Services
- DHCP Relay

---

## 👨‍💻 Author

**Harshavardhan**

Aspiring SOC Analyst | Security Analyst
