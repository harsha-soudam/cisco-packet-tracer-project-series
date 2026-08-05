# 🌐 Cisco Packet Tracer Project 04 – DHCP, DNS & Network Services

<div align="center">

![Cisco](https://img.shields.io/badge/Cisco-Packet%20Tracer-1BA0D7?style=for-the-badge&logo=cisco&logoColor=white)
![DHCP](https://img.shields.io/badge/DHCP-DNS-blue?style=for-the-badge)
![CCNA](https://img.shields.io/badge/Enterprise-Networking-success?style=for-the-badge)

</div>

## 📖 Overview

This project demonstrates the implementation of enterprise network services using **DHCP** and **DNS** in Cisco Packet Tracer. Building upon the Inter-VLAN Routing configuration from Project 3, a dedicated server is configured to automatically assign IP addresses, subnet masks, default gateways, and DNS server information to multiple VLANs.

The project also implements **DHCP Relay (ip helper-address)**, allowing DHCP requests to cross VLAN boundaries, and configures DNS name resolution for easier access to network resources.

---

## 🎯 Objectives

- Configure a DHCP Server
- Create DHCP Pools for multiple VLANs
- Configure DHCP Relay (ip helper-address)
- Enable automatic IP address assignment
- Configure DNS Server
- Implement hostname resolution
- Verify DHCP and DNS functionality
- Understand enterprise network services

---

## 🏢 Real-World Scenario

TechNova Solutions has expanded significantly, making manual IP address configuration inefficient and difficult to manage. The IT department decides to deploy centralized DHCP and DNS services so that all devices automatically receive their network configuration while users can access resources using hostnames instead of IP addresses.

---

## 🖥️ Network Topology

<img width="773" height="323" alt="image" src="https://github.com/user-attachments/assets/432e874e-e5dd-400a-ab12-2daccd26b96e" />


---

## 📦 Devices Used

| Device | Quantity |
|---------|---------:|
| Cisco 2960 Switch | 1 |
| Cisco 1941 Router | 1 |
| Server-PT | 1 |
| PC-PT | 7 |

---

## 🌐 Network Design

| Department | VLAN | Network | DHCP Pool |
|------------|------|----------------|----------------|
| HR | 10 | 192.168.10.0/24 | 192.168.10.100 – 199 |
| Finance | 20 | 192.168.20.0/24 | 192.168.20.100 – 199 |
| IT | 30 | 192.168.30.0/24 | 192.168.30.100 – 199 |
| Management | 99 | 192.168.99.0/24 | 192.168.99.100 – 199 |
| Server VLAN | 1 | 192.168.1.0/24 | Static |

---

## 🛠️ Services Configured

### DHCP

- DHCP Server Enabled
- Multiple DHCP Pools
- Automatic IP Assignment
- Automatic Default Gateway Assignment
- Automatic DNS Assignment

### DNS

Hostname configured:

| Hostname | IP Address |
|-----------|------------|
| fileserver.technova.local | 192.168.1.100 |

---

## 📚 Concepts Learned

- DHCP
- DORA Process
- DHCP Relay
- ip helper-address
- DNS
- Name Resolution
- APIPA (169.254.x.x)
- Enterprise IP Address Management
- Automatic Network Configuration

---

## ✅ Verification

Network services were verified using:

- DHCP Client Configuration
- DNS Name Resolution
- Ping by Hostname
- Ping by IP Address
- Router CLI
- Server Services

---

## 📸 Verification Screenshots

### Network Topology

<img width="773" height="323" alt="image" src="https://github.com/user-attachments/assets/855c5f04-dd58-43ee-9ad2-a64953edadbf" />


---

### DHCP Pools

<img width="848" height="357" alt="image" src="https://github.com/user-attachments/assets/3a728867-6794-4b96-863c-060240e295b9" />


---

### DHCP Relay Configuration

<img width="396" height="193" alt="image" src="https://github.com/user-attachments/assets/c2373ef8-7ef2-4e50-9db0-76c2f6dff0fa" />


---

### Automatic IP Assignment

<img width="960" height="377" alt="image" src="https://github.com/user-attachments/assets/5d5a81de-89c3-4728-b07c-0e45de8a3592" />


---

### DNS Configuration

<img width="843" height="173" alt="image" src="https://github.com/user-attachments/assets/187783fb-e2be-4605-9755-b082f0c59aa7" />


---

### Successful DNS Resolution

<img width="521" height="229" alt="image" src="https://github.com/user-attachments/assets/616dbb97-11c9-4949-8b17-d33e2bfcf7ab" />


---

## 🛠️ Troubleshooting Performed

- DHCP client receiving APIPA (169.254.x.x)
- Missing DHCP Relay (ip helper-address)
- Incorrect DNS Server configuration
- Verified DHCP Pools
- Verified Router Subinterfaces
- Verified DNS Name Resolution
- Verified Inter-VLAN Connectivity

---

## 💼 Skills Demonstrated

- Cisco Packet Tracer
- DHCP Configuration
- DNS Configuration
- DHCP Relay
- Router-on-a-Stick
- Cisco IOS CLI
- Enterprise Network Services
- Network Troubleshooting
- Layer 3 Networking

---

## 📖 Lessons Learned

- DHCP broadcasts do not cross routers.
- DHCP Relay (ip helper-address) forwards DHCP requests across VLANs.
- DNS converts hostnames into IP addresses.
- APIPA (169.254.x.x) indicates a DHCP failure.
- Enterprise networks use centralized DHCP and DNS servers for easier management.

---

## 🎤 Interview Questions

### What is DHCP?

DHCP automatically assigns IP addresses and other network settings to client devices.

---

### Explain the DORA Process.

- Discover
- Offer
- Request
- Acknowledge

---

### What is APIPA?

APIPA (169.254.x.x) is a self-assigned IP address used when a client cannot contact a DHCP server.

---

### Why is ip helper-address required?

Routers do not forward DHCP broadcasts. The `ip helper-address` command converts DHCP broadcasts into unicast packets and forwards them to the DHCP server.

---

### What is DNS?

DNS translates human-readable hostnames into IP addresses.

---

### Difference Between DHCP and DNS?

| DHCP | DNS |
|------|-----|
| Assigns IP Addresses | Resolves Names |
| Dynamic Configuration | Name Resolution |
| Uses DORA Process | Uses DNS Queries |

---

## 🚀 Future Improvements

Project 5 will introduce:

- Port Security
- Secure Switch Management (SSH)
- MAC Address Security
- Disable Unused Ports
- Switch Hardening
- Basic Enterprise Network Security

---

## 👨‍💻 Author

**Harshavardhan**

Aspiring SOC Analyst | Security Analyst
