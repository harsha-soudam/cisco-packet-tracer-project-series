# 📡 Cisco Packet Tracer Project 06 – Enterprise Wireless Network

<p align="center">

![Cisco Packet Tracer](https://img.shields.io/badge/Cisco-Packet%20Tracer-1BA0D7?style=for-the-badge&logo=cisco&logoColor=white)
![Enterprise Networking](https://img.shields.io/badge/Enterprise-Networking-0A66C2?style=for-the-badge)
![Wireless](https://img.shields.io/badge/Wireless-WLAN-00A98F?style=for-the-badge)
![Security](https://img.shields.io/badge/Network-Security-red?style=for-the-badge)
![VLAN](https://img.shields.io/badge/VLAN-Segmentation-orange?style=for-the-badge)
![ACL](https://img.shields.io/badge/ACL-Access%20Control-purple?style=for-the-badge)

</p>

<p align="center"><b>Enterprise Wireless Network with Corporate & Guest Network Segmentation</b></p>

---

## 📊 Project Progress

```text
██████████████████████████████ 100%
```

| Phase | Status |
|---|:---:|
| 📡 Corporate Wireless Network | ✅ |
| 🔐 WPA2-PSK & AES Security | ✅ |
| 🌐 VLAN Integration | ✅ |
| 📥 DHCP | ✅ |
| 👥 Guest Wireless Network | ✅ |
| 🧩 Guest VLAN 50 | ✅ |
| 🔄 Inter-VLAN Routing | ✅ |
| 🛡️ Extended ACL | ✅ |
| 🧪 Security Testing | ✅ |
| 📚 Documentation | ✅ |

---

## 🏢 Project Overview

This project demonstrates the design and implementation of an **Enterprise Wireless Network** using Cisco Packet Tracer.

The network provides:

- A secure **Corporate Wireless Network**
- A separate **Guest Wireless Network**
- VLAN-based segmentation
- DHCP addressing
- Inter-VLAN routing
- ACL-based access control
- Guest network isolation

The main security objective is:

> **Corporate users should have access to corporate resources, while Guest users should be isolated from internal networks.**

---

## 🎯 Objectives

- Configure an enterprise wireless network.
- Secure the corporate SSID using WPA2-PSK and AES.
- Repurpose switch interfaces **Fa0/9 and Fa0/10** for the wireless infrastructure.
- Configure **AP1** for the corporate wireless network.
- Configure **AP2** for the Guest wireless network.
- Create **VLAN 50** for Guest users.
- Configure DHCP pools for wireless clients.
- Configure inter-VLAN routing.
- Create and apply an extended ACL.
- Restrict Guest access to internal networks.
- Verify wireless connectivity and security controls.

---

# 🏢 Real-World Scenario

TechNova Corporation wants to provide wireless connectivity for employees and visitors.

Employees require access to the corporate network, while visitors should only receive the services intended for guests.

The network therefore uses two wireless networks:

```text
                         TECHNOVA WIRELESS
                                │
                ┌───────────────┴───────────────┐
                │                               │
                ▼                               ▼
        📡 TechNova-Corp                📡 TechNova-Guest
                │                               │
                ▼                               ▼
          Corporate Users                  Guest Users
                │                               │
                ▼                               ▼
         Corporate Network                   VLAN 50
                                                │
                                                ▼
                                               ACL
                                                │
                                                ▼
                                      Internal Access ❌
```

This provides a basic enterprise security boundary between trusted and untrusted users.

---

# 🧠 SOC Analyst Perspective

From a SOC Analyst perspective, wireless segmentation is important because a compromised Guest device should not have unrestricted access to internal systems.

For example:

```text
Guest Laptop
192.168.50.100
      │
      ▼
   VLAN 50
      │
      ▼
     ACL
      │
 ┌────┼───────────┐
 ▼    ▼           ▼
 HR  Finance      IT
 ❌    ❌          ❌
```

If the Guest device starts scanning internal IP addresses, the SOC can investigate:

- Source IP
- Destination IP
- VLAN
- Protocol
- Destination port
- ACL activity
- DHCP information

This connects the networking project directly to SOC investigation and incident-response concepts.

---

# 🗺️ Network Topology

```text
                         ┌──────────────┐
                         │    Router    │
                         │ Inter-VLAN   │
                         │   Routing    │
                         └──────┬───────┘
                                │
                              Trunk
                                │
                         ┌──────▼───────┐
                         │    Switch    │
                         └──────┬───────┘
                                │
                  ┌─────────────┴─────────────┐
                  │                           │
                Fa0/9                       Fa0/10
                  │                           │
                  ▼                           ▼
                ┌────┐                     ┌────┐
                │ AP1│                     │ AP2│
                └─┬──┘                     └─┬──┘
                  │                           │
          TechNova-Corp              TechNova-Guest
                  │                           │
                  ▼                           ▼
          Corporate Users                 VLAN 50
                                              │
                                              ▼
                                             ACL
                                              │
                                              ▼
                                      Internal Networks ❌
```

---

# 🔌 Repurposing Fa0/9 and Fa0/10

The switch ports **Fa0/9 and Fa0/10** were repurposed for the wireless access points.

```text
Fa0/9  → AP1 → Corporate Wireless
Fa0/10 → AP2 → Guest Wireless
```

This provides dedicated switch connections for the two wireless networks.

The important enterprise concept is that switch ports must be placed into the correct VLAN/trunk configuration according to the wireless design.

---

# 📡 AP1 – Corporate Wireless Configuration

**AP1** provides the corporate wireless network.

### SSID

```text
TechNova-Corp
```

### Security

```text
WPA2-PSK
AES
```

### Purpose

```text
Employees
   │
   ▼
TechNova-Corp
   │
   ▼
Corporate Network
```

The corporate wireless client was tested successfully after configuring the SSID and wireless security.

### Security concept

Knowing the SSID alone does not allow an attacker to connect.

```text
SSID Known
    ≠
Authentication Successful
```

The correct WPA2-PSK is still required.

---

# 👥 AP2 – Guest Wireless Configuration

**AP2** provides the Guest wireless network.

### SSID

```text
TechNova-Guest
```

### Purpose

```text
Visitors
   │
   ▼
TechNova-Guest
   │
   ▼
VLAN 50
   │
   ▼
Guest Network
```

The Guest network is treated as an **untrusted network**.

The purpose is to provide network connectivity while preventing Guest users from reaching protected internal networks.

---

# 🧩 Creating VLAN 50

A dedicated VLAN was created for Guest wireless clients.

```text
VLAN ID: 50
Name: Guest
Network: 192.168.50.0/24
Gateway: 192.168.50.1
```

Logical design:

```text
                Guest Wireless
                      │
                      ▼
                    VLAN 50
                      │
                      ▼
                 192.168.50.0/24
                      │
                      ▼
                 192.168.50.1
```

### Why VLAN 50?

A separate VLAN creates a separate Layer-2 broadcast domain and allows the Guest network to be controlled independently from corporate networks.

This is an important enterprise security practice:

> **Segment untrusted users from trusted users.**

---

# 📥 DHCP Pools

DHCP was configured so wireless clients could automatically receive their network information.

A DHCP pool provides:

```text
IP Address
Subnet Mask
Default Gateway
DNS Server
```

For the Guest network:

```text
Network:
192.168.50.0/24

Gateway:
192.168.50.1
```

The Guest client successfully received an address beginning with:

```text
192.168.50.x
```

This confirmed that the client was receiving an address from the Guest network rather than a corporate network.

### DHCP DORA

```text
Client                         DHCP Server
  │                                │
  │──── Discover ─────────────────►│
  │◄──── Offer ────────────────────│
  │──── Request ──────────────────►│
  │◄──── Acknowledgment ───────────│
```

---

# 🔄 Inter-VLAN Routing

Because VLANs are separate Layer-2 networks, routing is required for communication between them.

The router provides gateways for the VLANs.

```text
VLAN 10 ─────┐
VLAN 20 ─────┤
VLAN 30 ─────┼──► Router ──► Inter-VLAN Routing
VLAN 50 ─────┤
VLAN 99 ─────┘
```

The Guest VLAN therefore reaches the router through its Guest gateway:

```text
192.168.50.1
```

This creates the point where security policies can be enforced.

---

# 🛡️ ACL Concept

An **Access Control List (ACL)** is a set of rules used by a router to permit or deny traffic.

An ACL can examine information such as:

- Source IP
- Destination IP
- Protocol
- Port

For this project, an **extended ACL** was used because Guest traffic needed to be controlled based on its source and destination.

Basic concept:

```text
Guest Traffic
     │
     ▼
    ACL
     │
 ┌───┴──────────────┐
 │                  │
 ▼                  ▼
Allowed           Denied
Traffic           Traffic
  │                  │
  ▼                  ▼
Continue          Blocked
```

---

# 📜 ACL Rules

The Guest security policy was designed around the following rules:

```text
Guest → Required Services      ALLOW
Guest → Internal Networks      DENY
Guest → Corporate VLANs        DENY
Guest → Management VLAN        DENY
```

Conceptually:

```text
                 Guest VLAN 50
                       │
                       ▼
                      ACL
                       │
          ┌────────────┼────────────┐
          │            │            │
          ▼            ▼            ▼
       Required      Internal    Management
       Services      Networks      Network
          ✅             ❌            ❌
```

### ACL Rule Order

ACLs are processed from top to bottom.

The first matching rule is applied.

```text
Rule 1
  ↓
Rule 2
  ↓
Rule 3
  ↓
Rule 4
  ↓
Implicit Deny
```

Therefore, ACL rule order is important.

A broad permit placed before a specific deny could allow traffic that should have been blocked.

---

# 📍 Applying the ACL

The Guest ACL was applied to the Guest VLAN interface in the direction where traffic from Guest users enters the router.

```text
Guest Client
     │
     ▼
VLAN 50
     │
     ▼
Guest Gateway
     │
     ▼
ACL
     │
 ┌───┴───────────────┐
 ▼                   ▼
Allowed             Denied
Traffic             Internal Traffic
```

This allows the router to enforce the Guest security policy before the traffic reaches protected networks.

---

# 🔐 Guest Network Security

The final Guest security model is:

```text
Guest Client
     │
     ▼
TechNova-Guest
     │
     ▼
VLAN 50
     │
     ▼
192.168.50.1
     │
     ▼
   ACL
     │
     ├── Required Services ✅
     │
     ├── Corporate Network ❌
     ├── HR ❌
     ├── Finance ❌
     ├── IT ❌
     └── Management ❌
```

This follows the **principle of least privilege**:

> Give users only the access they actually need.

---

# 🧪 Verification

The configuration was tested in stages.

### 1. Corporate Wireless

```text
Client → TechNova-Corp
```

Result:

```text
Connected ✅
```

### 2. Guest Wireless

```text
Client → TechNova-Guest
```

Result:

```text
Connected ✅
```

### 3. Guest DHCP

The Guest client received:

```text
192.168.50.x
```

Result:

```text
DHCP Successful ✅
```

### 4. Guest Gateway

```text
ping 192.168.50.1
```

Result:

```text
Successful ✅
```

### 5. Guest to Internal Network

The Guest client attempted to reach an internal network.

Result:

```text
Destination host unreachable ❌
```

This was expected because the ACL was designed to block Guest access to internal resources.

Therefore:

> A failed ping can be a successful security test.

---

# 🔧 Troubleshooting

## Wireless Client Could Not Connect

### Symptom

The client reported:

```text
No association with access point
```

### Investigation

The SSID and WPA2-PSK configuration were checked.

### Lesson

An SSID being visible does not mean that authentication will succeed.

```text
SSID Visible
     ↓
Authentication
     ↓
Association
     ↓
DHCP
```

Each stage must work correctly.

---

## Guest Client Received the Wrong IP Range

The Guest client initially received an address from the wrong network.

The VLAN and wireless configuration were checked.

After correcting the Guest network path, the client received an address beginning with:

```text
192.168.50.x
```

### Lesson

An unexpected DHCP address can indicate:

- Incorrect VLAN
- Incorrect switch-port configuration
- Incorrect AP configuration
- Incorrect DHCP pool
- Trunk/access configuration problems

---

## Guest Could Not Reach Internal Networks

This was tested after applying the ACL.

The Guest client received:

```text
Destination host unreachable
```

The ACL was then checked.

The result was expected because the ACL was intentionally preventing Guest-to-internal communication.

---

# 💻 Useful Verification Commands

```cisco
show vlan brief
```

Verify VLANs and switch-port assignments.

```cisco
show ip interface brief
```

Verify interface status and IP addresses.

```cisco
show running-config
```

Review the active configuration.

```cisco
show access-lists
```

Review ACL rules and traffic matches.

```text
ping <destination-ip>
```

Test connectivity.

```text
nslookup <hostname>
```

Test DNS resolution.

---

# 🎤 CCNA & Security Interview Questions

### Q1. Why use a separate Guest VLAN?

> To isolate untrusted Guest devices from trusted corporate networks and reduce the risk of lateral movement.

### Q2. Does knowing an SSID allow an attacker to connect?

> No. The SSID identifies the wireless network, but the client still needs to successfully authenticate.

### Q3. Why use WPA2-PSK?

> WPA2-PSK provides wireless authentication using a shared key and protects wireless communication with encryption.

### Q4. Why create VLAN 50?

> VLAN 50 provides a separate Layer-2 network for Guest users, allowing their traffic to be controlled independently.

### Q5. Why use an extended ACL?

> Extended ACLs allow more granular filtering using source, destination, protocol, and port information.

### Q6. Why does ACL rule order matter?

> ACLs are evaluated from top to bottom and the first matching rule is applied.

### Q7. What is the purpose of a DHCP pool?

> It automatically provides clients with IP addressing information such as the IP address, subnet mask, gateway, and DNS server.

### Q8. What would you investigate if a Guest device started scanning internal systems?

> I would identify the source IP, VLAN, destination systems, ports, DHCP information, ACL activity, and other security logs to determine whether the behavior indicates reconnaissance or compromise.

---

# 🧠 Lessons Learned

- Wireless connectivity and wireless security are different concepts.
- SSID visibility does not provide network access.
- WPA2-PSK protects wireless authentication.
- AES provides wireless encryption.
- VLANs provide network segmentation.
- Guest users should be separated from corporate users.
- DHCP provides automatic network configuration.
- VLAN 50 creates a dedicated Guest security zone.
- Inter-VLAN routing enables communication between VLANs.
- ACLs control traffic between networks.
- Extended ACLs provide granular access control.
- ACL rule order matters.
- A blocked connection can indicate that a security control is working.
- Network segmentation helps reduce lateral movement.

---

# 💼 Skills Demonstrated

### 🌐 Networking

- Cisco Packet Tracer
- Enterprise Wireless Networking
- SSID Configuration
- Access Point Configuration
- VLAN Configuration
- VLAN 50 Guest Segmentation
- DHCP
- Inter-VLAN Routing
- Router-on-a-Stick
- IP Addressing

### 🔐 Network Security

- WPA2-PSK
- AES Encryption
- Guest Network Isolation
- Extended ACLs
- Access Control
- Least Privilege
- Network Segmentation

### 🛡️ SOC Analyst Skills

- Network Security Validation
- Access Control Analysis
- Security Boundary Testing
- Basic Lateral Movement Prevention
- Network Troubleshooting
- Security-focused Incident Analysis

---

# 📸 Screenshot Checklist

Save the screenshots inside:

```text
screenshots/
```

| # | Screenshot | Suggested Filename |
|---:|---|---|
| 1 | Final topology | `01-final-topology.png` |
| 2 | Fa0/9 and Fa0/10 configuration | `02-wireless-switch-ports.png` |
| 3 | AP1 corporate configuration | `03-ap1-corporate-wireless.png` |
| 4 | Corporate client connected | `04-corporate-client.png` |
| 5 | AP2 Guest configuration | `05-ap2-guest-wireless.png` |
| 6 | VLAN 50 configuration | `06-vlan-50.png` |
| 7 | Guest DHCP pool | `07-guest-dhcp-pool.png` |
| 8 | Guest client DHCP address | `08-guest-client-dhcp.png` |
| 9 | Inter-VLAN routing | `09-inter-vlan-routing.png` |
| 10 | ACL configuration | `10-guest-acl.png` |
| 11 | ACL applied to Guest interface | `11-acl-applied.png` |
| 12 | Guest gateway ping | `12-guest-gateway-ping.png` |
| 13 | Guest internal network blocked | `13-guest-access-blocked.png` |
| 14 | DNS validation | `14-dns-validation.png` |

> **Security note:** Do not expose the actual WPA2-PSK in screenshots uploaded to GitHub.

---

# 📁 Project Structure

```text
packet-tracer-project-06-enterprise-wireless-network/
│
├── README.md
│
├── packet-tracer-project-06-enterprise-wireless-network.pkt
│
└── screenshots/
    ├── 01-final-topology.png
    ├── 02-wireless-switch-ports.png
    ├── 03-ap1-corporate-wireless.png
    ├── 04-corporate-client.png
    ├── 05-ap2-guest-wireless.png
    ├── 06-vlan-50.png
    ├── 07-guest-dhcp-pool.png
    ├── 08-guest-client-dhcp.png
    ├── 09-inter-vlan-routing.png
    ├── 10-guest-acl.png
    ├── 11-acl-applied.png
    ├── 12-guest-gateway-ping.png
    ├── 13-guest-access-blocked.png
    └── 14-dns-validation.png
```

---

# 🚀 Enterprise Improvements

A production environment could improve this design further with:

- WPA3-Enterprise
- 802.1X authentication
- RADIUS/AAA
- Wireless LAN Controllers
- Network Access Control (NAC)
- Wireless IDS/IPS
- Firewall-based Guest isolation
- SIEM integration
- Endpoint Detection and Response (EDR)

A production architecture could look like:

```text
Wireless Client
      │
      ▼
   802.1X
      │
      ▼
 RADIUS / AAA
      │
      ▼
 Network Access Control
      │
      ▼
 VLAN Assignment
      │
      ▼
 Firewall / ACL
      │
      ▼
 Allowed Resources
```

---

# 🏁 Project Outcome

The project successfully implemented an enterprise wireless environment with separate corporate and Guest networks.

The final architecture combines:

```text
📡 Wireless
   +
🔐 WPA2-PSK
   +
🔑 AES
   +
🌐 VLAN Segmentation
   +
🧩 Guest VLAN 50
   +
📥 DHCP
   +
🔄 Inter-VLAN Routing
   +
🛡️ Extended ACL
   +
🚫 Guest Isolation
   +
🧪 Security Validation
```

The key security model is:

```text
              TechNova-Corp
                    │
                    ▼
             Corporate Users
                    │
                    ▼
            Corporate Network
                    │
                    │
                    │
              TechNova-Guest
                    │
                    ▼
                 VLAN 50
                    │
                    ▼
                   ACL
                    │
          ┌─────────┴─────────┐
          ▼                   ▼
      Required             Internal
      Services             Networks
         ✅                    ❌
```

---

# 🎓 Final Takeaway

This project demonstrates an important enterprise networking principle:

> **Connect users, but control where they can go.**

The wireless network provides connectivity, VLANs provide segmentation, DHCP provides addressing, routing provides communication between networks, and ACLs enforce the security policy.

From a SOC perspective, this architecture also creates clear security boundaries that can help detect and limit unauthorized access and lateral movement.

---

<p align="center">
<b>📡 Secure the Wireless Network • 🌐 Segment the Environment • 🛡️ Control the Access</b>
</p>

<p align="center">
Cisco Packet Tracer Enterprise Networking Project Series — Project 06
</p>
