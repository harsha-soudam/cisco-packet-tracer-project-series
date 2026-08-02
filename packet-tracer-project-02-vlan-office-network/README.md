# 🌐 Cisco Packet Tracer Project 02 – VLAN Office Network

<div align="center">

![Cisco](https://img.shields.io/badge/Cisco-Packet%20Tracer-1BA0D7?style=for-the-badge&logo=cisco&logoColor=white)
![VLAN](https://img.shields.io/badge/VLAN-Network-blue?style=for-the-badge)
![Layer2](https://img.shields.io/badge/Layer--2-Switching-success?style=for-the-badge)

</div>

## 📖 Overview

This project demonstrates how Virtual Local Area Networks (VLANs) can logically separate departments within a single physical Cisco 2960 switch.

The network consists of Human Resources (HR), Finance, IT, Management, and a shared File Server. Each department is assigned to its own VLAN to reduce broadcast traffic, improve network organization, and provide logical segmentation.

This project focuses on VLAN creation, switch port assignment, VLAN verification, communication testing, and understanding why devices in different VLANs cannot communicate without Inter-VLAN Routing.

---

## 🎯 Objectives

- Design a VLAN-based office network
- Create VLANs on a Cisco switch
- Configure switch access ports
- Configure static IP addressing
- Verify VLAN configuration
- Test communication within the same VLAN
- Analyze communication failure between different VLANs

---

## 🏢 Real-World Scenario

TechNova Solutions has expanded into multiple departments:

- Human Resources (HR)
- Finance
- Information Technology (IT)
- Management

Although every department shares the same physical Cisco switch, they require logical separation for better security, improved network management, and reduced broadcast traffic.

To achieve this, VLANs are implemented while keeping the existing network infrastructure.

---

## 🖥️ Network Topology

(Add your topology screenshot here)

---

## 📦 Devices Used

| Device | Quantity |
|---------|---------:|
| Cisco 2960 Switch | 1 |
| PC-PT | 7 |
| Server-PT | 1 |

---

## 🏢 Network Design

| Department | VLAN | Network |
|------------|------|----------------|
| HR | 10 | 192.168.10.0/24 |
| Finance | 20 | 192.168.20.0/24 |
| IT | 30 | 192.168.30.0/24 |
| Management | 99 | 192.168.99.0/24 |
| File Server | 1 | 192.168.1.0/24 |

---

## 🏷️ VLAN Configuration

| VLAN ID | Name |
|---------|------|
| 10 | HR |
| 20 | FINANCE |
| 30 | IT |
| 99 | MANAGEMENT |
| 1 | DEFAULT |

---

## 🔌 Switch Port Assignment

| Switch Port | Device | VLAN |
|-------------|--------|------|
| Fa0/1 | HR1 | 10 |
| Fa0/2 | HR2 | 10 |
| Fa0/3 | FIN1 | 20 |
| Fa0/4 | FIN2 | 20 |
| Fa0/5 | IT1 | 30 |
| Fa0/6 | IT2 | 30 |
| Fa0/7 | MANAGER | 99 |
| Fa0/8 | FILE-SERVER | 1 |

---

## 🌐 IP Addressing

| Device | IP Address | VLAN |
|---------|------------|------|
| HR1 | 192.168.10.10 | 10 |
| HR2 | 192.168.10.11 | 10 |
| FIN1 | 192.168.20.10 | 20 |
| FIN2 | 192.168.20.11 | 20 |
| IT1 | 192.168.30.10 | 30 |
| IT2 | 192.168.30.11 | 30 |
| MANAGER | 192.168.99.10 | 99 |
| FILE-SERVER | 192.168.1.100 | 1 |

**Subnet Mask**

```
255.255.255.0
```

---

## 📚 Concepts Learned

- Virtual Local Area Network (VLAN)
- Broadcast Domains
- Layer 2 Switching
- Access Ports
- VLAN IDs
- Static IP Addressing
- Network Segmentation
- ICMP
- VLAN Verification

---

## ✅ Verification

Connectivity and VLAN configuration were verified using:

- `show vlan brief`
- Successful ping between devices in the same VLAN
- Failed ping between devices in different VLANs
- Cisco Switch CLI

---

## 📸 Verification Screenshots

### Network Topology

<img width="1503" height="625" alt="image" src="https://github.com/user-attachments/assets/82f0d4de-6853-4c3a-b0c3-91a0d9259d62" />


---




## 🛠️ Troubleshooting Performed

- Verified VLAN creation
- Verified switch port assignments
- Identified communication failure between different VLANs
- Verified VLAN membership using `show vlan brief`
- Tested connectivity using ICMP Ping

---

## 💼 Skills Demonstrated

- Cisco Packet Tracer
- VLAN Configuration
- Layer 2 Switching
- Network Segmentation
- Static IP Configuration
- Network Troubleshooting
- Network Documentation

---

## 📖 Lessons Learned

- VLANs create logical networks within a single physical switch.
- Devices communicate only within the same VLAN unless Inter-VLAN Routing is configured.
- VLAN membership is determined by switch port configuration.
- Proper VLAN planning improves scalability and simplifies network management.
- Network segmentation enhances both performance and security.

---



## 🚀 Future Improvements

Project 3 will introduce:

- Inter-VLAN Routing
- Router-on-a-Stick
- Trunk Ports
- 802.1Q Encapsulation
- Default Gateway Configuration

---

## 👨‍💻 Author

**Harshavardhan**

Aspiring SOC Analyst | Security Analyst
