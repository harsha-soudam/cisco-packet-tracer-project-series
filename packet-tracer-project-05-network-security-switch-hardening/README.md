# 🛡️ Cisco Packet Tracer Project 05 – Network Security & Switch Hardening

<div align="center">

![Cisco](https://img.shields.io/badge/Cisco-Packet%20Tracer-1BA0D7?style=for-the-badge&logo=cisco&logoColor=white)
![Security](https://img.shields.io/badge/Network-Security-red?style=for-the-badge)
![Switch Hardening](https://img.shields.io/badge/Switch-Hardening-success?style=for-the-badge)

</div>

## 📖 Overview

This project focuses on securing an enterprise access switch by implementing Cisco Layer 2 security best practices. The switch is hardened using Port Security, Sticky MAC Address learning, violation modes, secure management through SSH, and by disabling unused switch ports.

The objective is to prevent unauthorized devices from accessing the corporate network while ensuring administrators can securely manage the switch remotely.

---

## 🎯 Objectives

- Configure Port Security
- Configure Sticky MAC Address Learning
- Configure Port Security Violation Modes
- Secure Access Ports
- Create an Unused VLAN
- Disable Unused Switch Ports
- Configure SSH Remote Management
- Verify Switch Security
- Perform Attack Simulation
- Troubleshoot Security Violations

---

## 🏢 Real-World Scenario

TechNova Solutions has experienced multiple incidents where unknown devices were connected to unused network ports inside the office.

To strengthen network security, the IT team implements Cisco switch hardening by enabling Port Security, restricting unauthorized devices, disabling unused ports, and replacing Telnet with encrypted SSH remote management.

---

## 🖥️ Network Topology

<img width="771" height="327" alt="image" src="https://github.com/user-attachments/assets/83db6e62-ea81-440a-9fc1-fff411d9aecd" />


---

## 📦 Devices Used

| Device | Quantity |
|---------|---------:|
| Cisco 2960 Switch | 1 |
| Cisco 1941 Router | 1 |
| Server-PT | 1 |
| PC-PT | 7 |

---

## 🛡️ Security Features Implemented

### Port Security

- Enabled Port Security
- Maximum MAC Address = 1
- Sticky MAC Learning
- Restrict Violation Mode

---

### Switch Hardening

- Disabled Unused Ports
- Created VLAN 999 for Unused Ports
- Assigned Unused Ports to VLAN 999
- Configured Interface Descriptions

---

### Secure Remote Management

- Configured Hostname
- Configured Domain Name
- Generated RSA Keys
- Enabled SSH Version 2
- Created Local Administrator Account
- Disabled Telnet
- Enabled SSH-only Remote Access

---

## 📚 Concepts Learned

- Port Security
- Sticky MAC Address
- Protect Mode
- Restrict Mode
- Shutdown Mode
- Switch Hardening
- SSH
- VLAN 999
- Secure Remote Management
- Management VLAN
- Switch Virtual Interface (SVI)

---

## ✅ Verification

Security configuration was verified using:

- show port-security
- show port-security interface
- show port-security address
- show vlan brief
- show interfaces status
- show ip ssh
- show ip interface brief

---

## 📸 Verification Screenshots

### Network Topology

<img width="771" height="327" alt="image" src="https://github.com/user-attachments/assets/8d92101c-1873-4729-b510-cc76e3af6038" />


---

### Port Security Configuration

<img width="615" height="452" alt="Port-security port 1-7 - violation-restrict" src="https://github.com/user-attachments/assets/d2783388-ce9f-4970-816b-66a800d68d60" />


---

### Sticky MAC Address

<img width="295" height="139" alt="Screenshot 2026-08-06 125247" src="https://github.com/user-attachments/assets/9a82f542-a18a-479f-acc9-0d423a8cea23" />


---

### Port Security Violation

<img width="520" height="449" alt="violation" src="https://github.com/user-attachments/assets/dd88fbd8-ddce-4019-896b-4f76bd05496b" />


---

### VLAN 999 Configuration  

<img width="581" height="450" alt="fa0-9-24 port disabled" src="https://github.com/user-attachments/assets/57bc1aa3-79ed-4e58-97e6-0c53cdb84920" />


---

### Disabled Unused Ports

<img width="526" height="373" alt="vlan-brief" src="https://github.com/user-attachments/assets/b966e913-e7ee-49ba-b4f6-ae75b9d25194" />


---

### SSH Configuration

<img width="555" height="451" alt="ssh-enabled" src="https://github.com/user-attachments/assets/0bab339a-ce6a-4685-a215-7becc8ced29b" />

---

## 🛠️ Troubleshooting Performed

- Verified Sticky MAC Learning
- Tested Port Security Violation
- Recovered Err-disabled Interface
- Changed Violation Mode to Restrict
- Verified SSH Configuration
- Verified Management VLAN
- Verified Secure Remote Access

---

## 💼 Skills Demonstrated

- Cisco Packet Tracer
- Cisco IOS CLI
- Layer 2 Security
- Port Security
- Sticky MAC Learning
- SSH Configuration
- Switch Hardening
- Enterprise Security
- Network Troubleshooting

---

## 📖 Lessons Learned

- Port Security prevents unauthorized devices from connecting to switch ports.
- Sticky MAC automatically learns and stores trusted device MAC addresses.
- Restrict mode blocks unauthorized traffic while keeping the port operational.
- Unused switch ports should be placed into an unused VLAN and administratively shut down.
- SSH provides encrypted remote management and is preferred over Telnet.
- VLAN 999 is commonly used to isolate unused access ports from production traffic.

---

## 🎤 Interview Questions

### What is Port Security?

Port Security restricts access to a switch port by allowing only authorized MAC addresses to communicate.

---

### What is Sticky MAC?

Sticky MAC automatically learns the first MAC address connected to a switch port and saves it as a secure MAC address.

---

### Explain Port Security Violation Modes.

| Mode | Description |
|------|-------------|
| Protect | Drops unauthorized traffic without logging or shutting down the port. |
| Restrict | Drops unauthorized traffic, logs the violation, and increments the violation counter while keeping the port active. |
| Shutdown | Places the interface into the err-disabled state when a violation occurs. |

---

### Why disable unused switch ports?

Unused ports can be exploited by unauthorized users. Disabling them reduces the attack surface.

---

### Why move unused ports to VLAN 999?

VLAN 999 is an isolated VLAN with no production devices, reducing the risk of unauthorized access if a port is accidentally enabled.

---

### Why is SSH preferred over Telnet?

SSH encrypts authentication credentials and management traffic, while Telnet sends data in plain text, making it vulnerable to interception.

---

### What is an SVI?

A Switch Virtual Interface (SVI) is a logical interface that provides an IP address for managing a Layer 2 switch remotely.

---

## 🚀 Future Improvements

Project 6 will introduce:

- Wireless Access Point Configuration
- Secure Wi-Fi (WPA2/WPA3)
- Guest Wireless Network
- Wireless VLAN Integration
- Enterprise Wireless Security

---

## 👨‍💻 Author

**Harshavardhan**

Aspiring SOC Analyst | Security Analyst
