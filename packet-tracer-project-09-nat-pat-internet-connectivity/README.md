# 🌐 Cisco Packet Tracer Project 09 — NAT, PAT & Internet Connectivity

![Cisco Packet Tracer](https://img.shields.io/badge/Cisco-Packet%20Tracer-blue?logo=cisco)
![NAT](https://img.shields.io/badge/Network-NAT%20%2F%20PAT-orange)
![Status](https://img.shields.io/badge/Lab-Completed-success)

> **Project:** NAT, PAT & Internet Connectivity  
> **Platform:** Cisco Packet Tracer  
> **Focus:** NAT, PAT (NAT Overload), routing, external network connectivity, and verification

---

## 📌 Project Overview

This project demonstrates how a private enterprise network can communicate with an external network using **Network Address Translation (NAT)** and **Port Address Translation (PAT)**.

The lab starts with a private LAN containing three PCs. The enterprise router connects the private network to an ISP router, which then connects to an external router and server.

The main objective is to understand how private IP addresses are translated when traffic leaves the enterprise network and how **PAT allows multiple internal hosts to share a single public IP address**.

---

## 🏗️ Network Architecture

```text
                         ENTERPRISE
┌──────────┐
│   PC1    │ 192.168.10.10
└────┬─────┘
     │
┌────┴─────┐
│   SW1    │
└────┬─────┘
     │
┌────┴──────────────────────┐
│                           │
│ R1                        │
│ Gi0/0: 192.168.10.1       │
│ Gi0/1: 203.0.113.1        │
└────────────┬───────────────┘
             │
             │ 203.0.113.0/24
             │
        ┌────┴─────┐
        │   ISP    │
        │ Gi0/0    │
        │203.0.113.2
        │ Gi0/1    │
        │198.51.100.1
        └────┬─────┘
             │
             │ 198.51.100.0/24
             │
        ┌────┴──────┐
        │  EXT-R1   │
        │ Gi0/0     │
        │198.51.100.2
        │ Gi0/1     │
        │192.0.2.1  │
        └────┬──────┘
             │
             │ 192.0.2.0/24
             │
        ┌────┴─────────┐
        │External      │
        │Server        │
        │192.0.2.10    │
        └──────────────┘

PC2: 192.168.10.11
PC3: 192.168.10.12
```

### 🔄 Traffic Flow

```text
Private Host
192.168.10.x
      │
      ▼
     R1
      │
      │ PAT
      ▼
203.0.113.1
      │
      ▼
     ISP
      │
      ▼
   EXT-R1
      │
      ▼
192.0.2.10
External Server
```

---

## 🧮 IP Addressing Scheme

| Device | Interface | IP Address | Subnet Mask | Purpose |
|---|---|---|---|---|
| R1 | Gi0/0 | `192.168.10.1` | `255.255.255.0` | Enterprise LAN Gateway |
| R1 | Gi0/1 | `203.0.113.1` | `255.255.255.0` | ISP-facing interface |
| ISP | Gi0/0 | `203.0.113.2` | `255.255.255.0` | Enterprise-facing interface |
| ISP | Gi0/1 | `198.51.100.1` | `255.255.255.0` | External network link |
| EXT-R1 | Gi0/0 | `198.51.100.2` | `255.255.255.0` | ISP-facing interface |
| EXT-R1 | Gi0/1 | `192.0.2.1` | `255.255.255.0` | External LAN Gateway |
| External Server | Fa0 | `192.0.2.10` | `255.255.255.0` | External Server |

---

## 💻 PC Addressing

| PC | IP Address | Subnet Mask | Default Gateway |
|---|---|---|---|
| PC1 | `192.168.10.10` | `255.255.255.0` | `192.168.10.1` |
| PC2 | `192.168.10.11` | `255.255.255.0` | `192.168.10.1` |
| PC3 | `192.168.10.12` | `255.255.255.0` | `192.168.10.1` |

---

## 🧠 Key Concepts

### 1. NAT

**Network Address Translation (NAT)** changes an IP address as traffic crosses a router.

In this project, private enterprise addresses such as:

```text
192.168.10.10
```

are translated before reaching the external network.

NAT allows organizations to use private addressing internally while using publicly routable addressing externally.

---

### 2. Dynamic NAT

Dynamic NAT maps private addresses to addresses from a configured public address pool.

The initial configuration used:

```text
PUBLIC_POOL
203.0.113.10 - 203.0.113.20
```

with ACL 1 identifying the enterprise LAN.

```text
192.168.10.0/24
        ↓
     ACL 1
        ↓
Dynamic NAT
        ↓
203.0.113.10 - 203.0.113.20
```

This configuration was tested before moving to PAT.

---

### 3. PAT / NAT Overload

**Port Address Translation (PAT)** allows multiple private hosts to share one public IP address.

The final configuration uses R1's outside interface address:

```text
203.0.113.1
```

Multiple internal hosts can therefore appear externally as the same public IP.

```text
192.168.10.10 ─┐
192.168.10.11 ─┼──► 203.0.113.1
192.168.10.12 ─┘
```

PAT differentiates simultaneous connections using identifiers such as TCP/UDP port information and, for ICMP, the ICMP identifier.

---

### 4. NAT Inside and Outside

R1 was configured with:

```text
Gi0/0 → ip nat inside
Gi0/1 → ip nat outside
```

This establishes the NAT boundary.

```text
Private LAN                 External Network
192.168.10.0/24             203.0.113.0/24
       │                           │
       ▼                           ▼
    Gi0/0                         Gi0/1
   NAT INSIDE                  NAT OUTSIDE
          \                     /
                 R1
```

---

### 5. Default Route

R1 uses a default route to send unknown destinations toward the ISP:

```text
ip route 0.0.0.0 0.0.0.0 203.0.113.2
```

This means:

> If R1 does not have a more specific route for the destination, forward the packet to the ISP.

---

### 6. Return Routing

NAT requires a working return path.

EXT-R1 was configured with:

```text
ip route 203.0.113.0 255.255.255.0 198.51.100.1
```

This allows replies destined for the translated address to return through the ISP.

---

## ⚙️ Configuration Commands

### R1 — Enterprise LAN Interface

```text
enable
configure terminal

interface gigabitEthernet 0/0
ip address 192.168.10.1 255.255.255.0
no shutdown
exit
```

### R1 — ISP-Facing Interface

```text
interface gigabitEthernet 0/1
ip address 203.0.113.1 255.255.255.0
no shutdown
exit
```

### ISP — Enterprise-Facing Interface

```text
enable
configure terminal

interface gigabitEthernet 0/0
ip address 203.0.113.2 255.255.255.0
no shutdown
exit
```

### ISP — External-Facing Interface

```text
interface gigabitEthernet 0/1
ip address 198.51.100.1 255.255.255.0
no shutdown
exit
```

### EXT-R1 — ISP-Facing Interface

```text
enable
configure terminal

interface gigabitEthernet 0/0
ip address 198.51.100.2 255.255.255.0
no shutdown
exit
```

### EXT-R1 — External LAN Interface

```text
interface gigabitEthernet 0/1
ip address 192.0.2.1 255.255.255.0
no shutdown
exit
```

---

## 🛣️ Routing Configuration

### ISP — Route to External Network

```text
ip route 192.0.2.0 255.255.255.0 198.51.100.2
```

### R1 — Default Route

```text
ip route 0.0.0.0 0.0.0.0 203.0.113.2
```

### EXT-R1 — Return Route

```text
ip route 203.0.113.0 255.255.255.0 198.51.100.1
```

---

## 🔐 NAT Configuration

### NAT ACL

```text
access-list 1 permit 192.168.10.0 0.0.0.255
```

This identifies the enterprise LAN as the traffic eligible for translation.

### NAT Inside / Outside

```text
interface gigabitEthernet 0/0
ip nat inside
exit

interface gigabitEthernet 0/1
ip nat outside
exit
```

### Dynamic NAT Pool

The initial Dynamic NAT configuration was:

```text
ip nat pool PUBLIC_POOL 203.0.113.10 203.0.113.20 netmask 255.255.255.0
ip nat inside source list 1 pool PUBLIC_POOL
```

This was used to demonstrate Dynamic NAT before implementing PAT.

### Final PAT Configuration

```text
ip nat inside source list 1 interface gigabitEthernet 0/1 overload
```

The final design translates the private LAN to R1's outside interface address:

```text
203.0.113.1
```

---

## 🔍 Verification Commands

### Check Interfaces

```text
show ip interface brief
```

### Check Routing Table

```text
show ip route
```

### Check Specific Route

```text
show ip route 192.0.2.0
```

### Check NAT Configuration

```text
show running-config | include ip nat
```

### Check NAT Translations

```text
show ip nat translations
```

### Check NAT Statistics

```text
show ip nat statistics
```

### Check ACL

```text
show access-lists
```

---

## 🧪 NAT/PAT Testing

The final test was performed from all three enterprise PCs.

```text
PC1> ping 192.0.2.10
PC2> ping 192.0.2.10
PC3> ping 192.0.2.10
```

### Result

```text
PC1 → External Server     ✅
PC2 → External Server     ✅
PC3 → External Server     ✅
```

The NAT translation table showed multiple internal hosts using the same inside-global address:

```text
Inside Global        Inside Local
203.0.113.1          192.168.10.10
203.0.113.1          192.168.10.11
203.0.113.1          192.168.10.12
```

This confirms that **PAT/NAT Overload was functioning successfully**.

---

## 📊 NAT Verification Evidence

Example PAT translation:

```text
Protocol   Inside Global       Inside Local       Outside Global
ICMP       203.0.113.1:1024    192.168.10.12:5    192.0.2.10:1024
ICMP       203.0.113.1:1025    192.168.10.12:6    192.0.2.10:1025
ICMP       203.0.113.1:21      192.168.10.10:21   192.0.2.10:21
ICMP       203.0.113.1:5       192.168.10.11:5    192.0.2.10:5
```

The important observation is that different private hosts can share:

```text
203.0.113.1
```

while PAT maintains separate translations.

---

## 📈 NAT Statistics

Final verification included:

```text
Inside Interfaces:
GigabitEthernet0/0

Outside Interfaces:
GigabitEthernet0/1

Hits:
12

Misses:
38

Expired translations:
28
```

`Total translations: 0` at the exact moment of checking does not indicate failure. PAT translations are temporary and can expire after traffic stops.

The successful PC-to-server tests and observed PAT translations are the primary evidence that the configuration is functioning.

---

## 🧩 Troubleshooting Performed

During the lab, the first Dynamic NAT test failed even though translations were being created.

The investigation showed:

1. NAT translations were successfully created.
2. R1 could reach the external server.
3. ISP could reach the external server.
4. EXT-R1 could reach the external server.
5. The return route was added on EXT-R1.
6. The lab was then converted to PAT using R1's outside interface.
7. All three PCs successfully reached the external server.

This demonstrated an important networking principle:

> **NAT translation and end-to-end connectivity are separate problems.**

A successful translation does not automatically guarantee a successful return path.

---

## 🎯 Learning Objectives

By completing this project, the following concepts were practiced:

- Private IPv4 addressing
- Public/external addressing
- NAT inside and outside interfaces
- Standard ACLs for NAT
- Dynamic NAT
- PAT / NAT Overload
- Public IP sharing
- Default routes
- Static routes
- Return-path routing
- NAT translation tables
- NAT statistics
- Troubleshooting connectivity
- Verifying end-to-end communication

---

## 🧰 Technologies & Devices

- Cisco Packet Tracer
- Cisco 2911 Routers
- Cisco 2960 Switch
- PC-PT endpoints
- External Server
- IPv4
- NAT
- PAT
- Static Routing
- ACL

---

## 📁 Suggested Project Structure

```text
packet-tracer-project-09-nat-pat-internet-connectivity/
│
├── README.md
├── packet-tracer-project-09.pkt
│
└── screenshots/
    ├── topology.png
    ├── ip-addressing.png
    ├── nat-configuration.png
    ├── pat-translations.png
    └── connectivity-test.png
```

---

## 🏁 Project Status

**Completed — NAT, PAT & Internet Connectivity successfully demonstrated.**

```text
Topology                🟩🟩🟩🟩🟩🟩🟩🟩🟩🟩  100%
IP Addressing           🟩🟩🟩🟩🟩🟩🟩🟩🟩🟩  100%
Routing                 🟩🟩🟩🟩🟩🟩🟩🟩🟩🟩  100%
Dynamic NAT             🟩🟩🟩🟩🟩🟩🟩🟩🟩🟩  100%
PAT                     🟩🟩🟩🟩🟩🟩🟩🟩🟩🟩  100%
External Connectivity   🟩🟩🟩🟩🟩🟩🟩🟩🟩🟩  100%
Verification            🟩🟩🟩🟩🟩🟩🟩🟩🟩🟩  100%
Documentation           🟩🟩🟩🟩🟩🟩🟩🟩🟩🟩  100%
```

## 💡 Key Takeaway

The most important concept from this project is the difference between **NAT and PAT**:

```text
NAT
Private IP → Public IP
One-to-one translation

PAT
Multiple Private IPs
       ↓
One Public IP
       ↓
Different connection identifiers
```

PAT is widely used in enterprise networks because it allows many internal devices to access external networks while conserving public IPv4 addresses.
