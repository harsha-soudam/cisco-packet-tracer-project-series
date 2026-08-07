# 📡 Cisco Packet Tracer Project 06 – Enterprise Wireless Network

<p align="center">

![Cisco Packet Tracer](https://img.shields.io/badge/Cisco-Packet%20Tracer-1BA0D7?style=for-the-badge&logo=cisco&logoColor=white)
![Enterprise Networking](https://img.shields.io/badge/Enterprise-Networking-0A66C2?style=for-the-badge)
![Wireless](https://img.shields.io/badge/Wireless-WLAN-00A98F?style=for-the-badge)
![Network Security](https://img.shields.io/badge/Network-Security-red?style=for-the-badge)
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
| 📡 Corporate Wireless | ✅ |
| 🔐 WPA2-PSK & AES | ✅ |
| 🌐 VLAN Integration | ✅ |
| 📥 DHCP | ✅ |
| 👥 Guest Wireless | ✅ |
| 🧩 Guest VLAN 50 | ✅ |
| 🔄 Inter-VLAN Routing | ✅ |
| 🛡️ Guest ACL | ✅ |
| 🧪 Security Testing | ✅ |
| 📚 Documentation | ✅ |

---

# 🏢 Project Overview

This project demonstrates the implementation of an **Enterprise Wireless Network** using Cisco Packet Tracer.

The network contains a secure corporate wireless network and a separate guest wireless network. The Guest network is placed into **VLAN 50** and restricted from accessing internal corporate networks using an extended ACL.

### Main Technologies

- Cisco Wireless Access Points
- SSID Configuration
- WPA2-PSK
- AES
- VLANs
- VLAN 50 Guest Segmentation
- DHCP
- Inter-VLAN Routing
- Extended ACL
- Network Access Control
- Wireless Security

---

# 🎯 Project Objectives

- Configure a corporate wireless network.
- Configure a separate Guest wireless network.
- Repurpose **Fa0/9 and Fa0/10** for wireless access points.
- Configure **AP1** for corporate wireless access.
- Configure **AP2** for Guest wireless access.
- Create **Guest-Wireless VLAN 50**.
- Create the **VLAN 50 gateway**.
- Configure DHCP for Guest wireless clients.
- Configure an extended ACL.
- Prevent Guest users from accessing internal networks.
- Verify connectivity and security controls.

---

# 🏢 Real-World Business Scenario

TechNova Corporation wants to provide wireless access to both employees and visitors.

Employees need access to corporate resources, while visitors should only receive access to permitted services.

The network therefore separates the two wireless environments:

```text
                         TECHNOVA WIRELESS
                                │
                ┌───────────────┴───────────────┐
                │                               │
                ▼                               ▼
        📡 TechNova-Corp                📡 TechNova-Guest
                │                               │
                ▼                               ▼
        Corporate Users                    Guest Users
                │                               │
                ▼                               ▼
        Corporate Network                    VLAN 50
                                                │
                                                ▼
                                               ACL
                                                │
                                                ▼
                                      Internal Networks ❌
```

---

# 🧠 Why Wireless Segmentation Is Needed

Wireless users should not automatically receive the same level of network access.

A Guest device is considered an **untrusted endpoint**. If it becomes compromised, unrestricted access to internal systems could allow an attacker to perform reconnaissance or attempt lateral movement.

The design therefore follows:

> **Connect users, but control where they can go.**

---

# 🛡️ SOC Analyst Perspective

From a SOC Analyst perspective, the Guest network provides a security boundary.

If a Guest device starts scanning internal systems, the SOC can investigate:

- Source IP
- Destination IP
- VLAN
- Protocol
- Destination port
- DHCP assignment
- ACL activity
- Network logs

The ACL also helps limit potential lateral movement from Guest devices.

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
                                      Internal Access ❌
```

---

# 🔌 Fa0/9 and Fa0/10 Configuration

Fa0/9 and Fa0/10 were repurposed for the wireless access points.

```text
                 Switch
                   │
          ┌────────┴────────┐
          │                 │
       Fa0/9              Fa0/10
          │                 │
          ▼                 ▼
        AP1               AP2
     Corporate           Guest
      Wireless          Wireless
                            │
                            ▼
                         VLAN 50
```

### Configuration

```cisco
enable
configure terminal

interface fa0/9
 description AP1-Corporate-Wireless
 switchport mode access
 no shutdown
 exit

interface fa0/10
 description AP2-Guest-Wireless
 switchport mode access
 switchport access vlan 50
 no shutdown
 exit

end
```

---

# 📡 AP1 – Corporate Wireless Configuration

AP1 provides wireless connectivity for employees.

```text
                    AP1
                     │
                     ▼
              TechNova-Corp
                     │
                     ▼
             Corporate Users
                     │
                     ▼
             Corporate Network
```

### Wireless Configuration

```text
SSID:
TechNova-Corp

Security:
WPA2-PSK

Encryption:
AES
```

The corporate wireless client was tested successfully after authentication.

### Security Concept

Knowing the SSID does not provide access to the network.

```text
SSID Known
    │
    ▼
WPA2 Authentication Required
    │
    ▼
Correct PSK Required
    │
    ▼
Network Access
```

---

# 👥 AP2 – Guest Wireless Configuration

AP2 provides wireless connectivity for visitors.

```text
                    AP2
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

### Wireless Configuration

```text
SSID:
TechNova-Guest
```

The Guest network is treated as an untrusted network and is separated from the corporate environment.

---

# 🧩 Creating Guest-Wireless VLAN 50

A dedicated VLAN was created for Guest wireless clients.

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
                    Guest Users
```

### VLAN Configuration

```cisco
enable
configure terminal

vlan 50
 name Guest-Wireless
 exit

end
```

### Guest VLAN Details

```text
VLAN ID       : 50
VLAN Name     : Guest-Wireless
Network       : 192.168.50.0/24
Gateway       : 192.168.50.1
```

---

# 🌐 Creating VLAN 50 Gateway

The router provides the default gateway for Guest VLAN 50.

```text
                         Router
                           │
                        G0/0.50
                           │
                           ▼
                    192.168.50.1/24
                           │
                           ▼
                        VLAN 50
                           │
                           ▼
                    Guest Wireless
```

### Router Configuration

```cisco
enable
configure terminal

interface gigabitEthernet0/0.50
 description Guest-Wireless-Gateway
 encapsulation dot1Q 50
 ip address 192.168.50.1 255.255.255.0
 no shutdown
 exit

end
```

---

# 📥 Guest Wireless DHCP Pool

DHCP automatically provides Guest clients with their IP configuration.

```text
                  DHCP Pool
                      │
                      ▼
                   VLAN 50
                      │
                      ▼
              192.168.50.0/24
                      │
                      ▼
                Guest Client
```

### DHCP Configuration

```cisco
enable
configure terminal

ip dhcp excluded-address 192.168.50.1 192.168.50.49

ip dhcp pool GUEST-WIRELESS
 network 192.168.50.0 255.255.255.0
 default-router 192.168.50.1
 dns-server 8.8.8.8
 exit

end
```

### Expected Guest Address

```text
IP Address      : 192.168.50.x
Subnet Mask     : 255.255.255.0
Default Gateway : 192.168.50.1
```

---

# 🔄 Inter-VLAN Routing

VLANs are separate Layer-2 networks, so routing is required for communication between them.

```text
VLAN 10 ─────┐
VLAN 20 ─────┤
VLAN 30 ─────┼──► Router ──► Inter-VLAN Routing
VLAN 50 ─────┤
VLAN 99 ─────┘
```

For the Guest network:

```text
Guest Client
     │
     ▼
VLAN 50
     │
     ▼
192.168.50.1
     │
     ▼
Router
```

---

# 🛡️ ACL Concept

An **Access Control List (ACL)** is a set of rules used to permit or deny network traffic.

For this project, the ACL protects internal networks from Guest users.

```text
                    Guest VLAN 50
                          │
                          ▼
                         ACL
                          │
             ┌────────────┼────────────┐
             ▼            ▼            ▼
        Corporate      Management    Other
         Networks        Network     Traffic
             ❌            ❌           ✅
```

### Security Objective

```text
Guest → Required Services        ✅ ALLOW
Guest → Corporate Networks       ❌ DENY
Guest → Management Network       ❌ DENY
Guest → Other permitted traffic  ✅ ALLOW
```

---

# 📜 Guest ACL Rules

The ACL blocks Guest traffic from reaching the internal VLANs.

```cisco
enable
configure terminal

ip access-list extended GUEST-ACL

deny ip 192.168.50.0 0.0.0.255 192.168.10.0 0.0.0.255
deny ip 192.168.50.0 0.0.0.255 192.168.20.0 0.0.0.255
deny ip 192.168.50.0 0.0.0.255 192.168.30.0 0.0.0.255
deny ip 192.168.50.0 0.0.0.255 192.168.99.0 0.0.0.255

permit ip 192.168.50.0 0.0.0.255 any

exit
```

### Rule Summary

```text
Guest VLAN 50 → VLAN 10     ❌ DENY
Guest VLAN 50 → VLAN 20     ❌ DENY
Guest VLAN 50 → VLAN 30     ❌ DENY
Guest VLAN 50 → VLAN 99     ❌ DENY
Guest VLAN 50 → Other       ✅ PERMIT
```

---

# 🔐 Applying the Guest ACL

The ACL is applied inbound on the Guest VLAN interface.

```text
                 Guest Client
                      │
                      ▼
                   VLAN 50
                      │
                      ▼
                   G0/0.50
                      │
                      ▼
                     ACL
                      │
              ┌───────┴───────┐
              ▼               ▼
           Allowed           Denied
              │               │
              ▼               ▼
          Continue         Blocked
```

### Configuration

```cisco
enable
configure terminal

interface gigabitEthernet0/0.50
 ip access-group GUEST-ACL in
 exit

end
```

---

# ⚠️ ACL Rule Order

ACLs are processed from **top to bottom**.

```text
Guest Packet
     │
     ▼
Rule 1 → VLAN 10? → ❌ DENY
     │
     ▼
Rule 2 → VLAN 20? → ❌ DENY
     │
     ▼
Rule 3 → VLAN 30? → ❌ DENY
     │
     ▼
Rule 4 → VLAN 99? → ❌ DENY
     │
     ▼
Final Permit
     │
     ▼
    ✅ ALLOW
```

The specific deny statements must appear before the broad permit statement.

---

# 🧪 Verification

## Verify VLAN 50

```cisco
show vlan brief
```

Expected:

```text
50   Guest-Wireless
```

## Verify Interfaces

```cisco
show ip interface brief
```

Expected:

```text
GigabitEthernet0/0.50    192.168.50.1
```

## Verify DHCP

```cisco
show ip dhcp binding
show ip dhcp pool
```

## Verify ACL

```cisco
show access-lists
```

## Verify ACL Application

```cisco
show running-config
```

Look for:

```text
interface GigabitEthernet0/0.50
 ip access-group GUEST-ACL in
```

---

# 🔎 Connectivity Testing

### Guest Client IP

```text
ipconfig
```

Expected:

```text
IP Address      : 192.168.50.x
Subnet Mask     : 255.255.255.0
Default Gateway : 192.168.50.1
```

### Test Guest Gateway

```text
ping 192.168.50.1
```

Expected:

```text
✅ Successful
```

### Test Internal Network

```text
ping <internal-network-ip>
```

Expected:

```text
❌ Blocked
```

A blocked ping in this test is expected because the ACL is intentionally preventing Guest-to-internal communication.

---

# 🔧 Troubleshooting

## Wireless Client Cannot Connect

Check:

- SSID
- WPA2-PSK
- Wireless security settings
- AP configuration
- Switch-port configuration

Expected sequence:

```text
SSID
  ↓
Authentication
  ↓
Association
  ↓
DHCP
  ↓
Network Connectivity
```

---

## Guest Client Gets Wrong IP Address

Check:

- VLAN 50
- Fa0/10 configuration
- AP2 configuration
- DHCP pool
- Router subinterface
- Trunk configuration

The Guest client should receive:

```text
192.168.50.x
```

---

## Guest Cannot Reach Internal Network

Check:

```cisco
show access-lists
```

and:

```cisco
show running-config
```

Confirm:

```text
GUEST-ACL
     │
     ▼
G0/0.50 inbound
```

---

# 🎤 Interview Questions

### 1. Why create a separate Guest VLAN?

To isolate untrusted Guest devices from trusted corporate networks and reduce the possibility of lateral movement.

### 2. Does knowing the SSID allow an attacker to connect?

No. The client must still successfully authenticate using the configured wireless security credentials.

### 3. Why use VLAN 50 for Guest users?

It creates a dedicated Layer-2 broadcast domain that can be independently controlled and secured.

### 4. Why use an extended ACL?

An extended ACL provides more granular traffic filtering using source, destination, protocol, and port information.

### 5. Why does ACL rule order matter?

ACLs are evaluated from top to bottom. The first matching rule is applied.

### 6. What would you investigate if a Guest device scanned internal systems?

I would investigate the source IP, DHCP assignment, VLAN, destination addresses, ports, protocols, ACL activity, and other available network/security logs.

---

# 🧠 Lessons Learned

- SSID visibility does not mean network access.
- WPA2-PSK provides wireless authentication.
- AES provides wireless encryption.
- VLANs provide network segmentation.
- Guest users should be separated from corporate users.
- VLAN 50 provides a dedicated Guest security zone.
- DHCP automatically provides client network configuration.
- Inter-VLAN routing allows communication between VLANs.
- ACLs enforce network access policies.
- ACL rule order is important.
- Security controls should be tested after implementation.
- A blocked connection can indicate that a security control is working.

---

# 💼 Skills Demonstrated

### 🌐 Networking

- Cisco Packet Tracer
- Enterprise Wireless Networking
- SSID Configuration
- Access Point Configuration
- VLAN Configuration
- Guest VLAN Segmentation
- DHCP
- Inter-VLAN Routing
- IP Addressing
- Network Troubleshooting

### 🔐 Network Security

- WPA2-PSK
- AES
- Network Segmentation
- Guest Network Isolation
- Extended ACLs
- Access Control
- Least Privilege

### 🛡️ SOC Analyst Skills

- Network Security Validation
- Access Control Analysis
- Security Boundary Testing
- Basic Lateral Movement Prevention
- Network Troubleshooting
- Security-focused Incident Analysis

---

# 📁 Project Structure

```text
packet-tracer-project-06-enterprise-wireless-network/
│
├── README.md
├── packet-tracer-project-06-enterprise-wireless-network.pkt
│
└── screenshots/
    
```

---

# 🚀 Enterprise Improvements

A production environment could improve this design with:

- WPA3-Enterprise
- 802.1X authentication
- RADIUS/AAA
- Wireless LAN Controller
- Network Access Control (NAC)
- Wireless IDS/IPS
- Firewall-based Guest isolation
- SIEM integration
- EDR integration
- Centralized logging

Example:

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
```

The final security model is:

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
   Required Services      Internal Networks
          ✅                    ❌
```

---

# 🎓 Final Takeaway

This project demonstrates a core enterprise networking and security principle:

> **Provide connectivity while enforcing access control.**

Wireless access points provide connectivity, VLANs provide segmentation, DHCP provides addressing, routing provides communication between networks, and ACLs enforce the security policy.

From a SOC perspective, the Guest VLAN also creates a clear security boundary that helps reduce unauthorized access and potential lateral movement.

---

<p align="center">
<b>📡 Secure the Wireless Network • 🌐 Segment the Environment • 🛡️ Control the Access</b>
</p>

<p align="center">
Cisco Packet Tracer Enterprise Networking Project Series — Project 06
</p>
