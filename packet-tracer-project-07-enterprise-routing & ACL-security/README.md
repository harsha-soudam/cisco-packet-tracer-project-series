# 🔐 Cisco Packet Tracer Project 07 — Enterprise Routing & ACL Security

![Cisco](https://img.shields.io/badge/Cisco-Packet%20Tracer-blue?logo=cisco)
![Networking](https://img.shields.io/badge/Networking-Enterprise-green)
![Security](https://img.shields.io/badge/Security-ACL-orange)
![Status](https://img.shields.io/badge/Status-Completed-success)

> **Enterprise Routing, Static Routes & Extended ACL Security**

---

## 📊 Project Progress

```text
██████████████████████████████ 100%
```

| Project | Focus |
|---|---|
| 🏢 Project 07 | Enterprise Routing & ACL Security |
| 🌐 Routing | Static Routing |
| 🔐 Security | Extended Named ACL |
| 🧪 Testing | Ping, Traceroute & ACL Counters |

---

## 🎯 Project Objective

Build a small enterprise network connecting two LANs through two routers and implement an Extended ACL to restrict specific communication between the networks.

### Security Requirement

```text
PC1 (192.168.10.10)  ──X──>  PC2 (192.168.20.10)
                         DENY
```

Other permitted traffic between the two LAN networks should remain available.

---

# 🗺️ Complete Network Architecture

```text
                         ENTERPRISE NETWORK
┌─────────────────────────────────────────────────────────────────────────────┐
│                                                                             │
│   LAN 1                         WAN / TRANSIT                         LAN 2  │
│ 192.168.10.0/24                10.0.0.0/30                    192.168.20.0/24│
│                                                                             │
│   ┌───────┐     Fa0/1      Gi0/0                 Gi0/1      ┌───────┐      │
│   │  PC1  │───────────────[ R1 ]================[ R2 ]──────────────│ PC2 │      │
│   │.10.10 │                │    │ 10.0.0.1 10.0.0.2 │             │.20.10│      │
│   └───────┘                │    │                    │             └───────┘      │
│      │                    Gi0/0                     Gi0/0             │      │
│      │                 192.168.10.1              192.168.20.1       │      │
│      │                       │                       │               │      │
│    ┌─┴─┐                   ┌─┴─┐                   ┌─┴─┐           ┌─┴─┐    │
│    │SW1│                   │R1 │                   │R2 │           │SW2│    │
│    └───┘                   └───┘                   └───┘           └───┘    │
│                                                                             │
└─────────────────────────────────────────────────────────────────────────────┘
```

### 🔌 Interface & IP Addressing

| Device | Interface | Connected To | IP Address | Mask |
|---|---|---|---|---|
| PC1 | NIC | SW1 | `192.168.10.10` | `/24` |
| R1 | Gi0/0 | LAN 1 | `192.168.10.1` | `/24` |
| R1 | Gi0/1 | R2 Gi0/1 | `10.0.0.1` | `/30` |
| R2 | Gi0/1 | R1 Gi0/1 | `10.0.0.2` | `/30` |
| R2 | Gi0/0 | LAN 2 | `192.168.20.1` | `/24` |
| PC2 | NIC | SW2 | `192.168.20.10` | `/24` |

### Default Gateways

```text
PC1 → 192.168.10.1
PC2 → 192.168.20.1
```

---


## 💻 PC IP Address Configuration

PC IP addresses are configured through **Packet Tracer → PC → Desktop → IP Configuration**.

### PC1 — LAN 1

```text
IP Address:      192.168.10.10
Subnet Mask:     255.255.255.0
Default Gateway: 192.168.10.1
```

### PC2 — LAN 2

```text
IP Address:      192.168.20.10
Subnet Mask:     255.255.255.0
Default Gateway: 192.168.20.1
```

> **Note:** PCs in Packet Tracer do not use Cisco IOS `ip address` commands for normal host configuration. Their IPv4 address, subnet mask, and default gateway are entered through the **Desktop → IP Configuration** window.

### Verification from PC Command Prompt

PC1:

```text
ipconfig
```

PC2:

```text
ipconfig
```

Connectivity test from PC1:

```text
ping 192.168.10.1
ping 10.0.0.2
ping 192.168.20.10
```

# 🧭 Static Routing

The routers automatically know their directly connected networks, but each router needs a route to the remote LAN.

### R1

```text
192.168.20.0/24 → 10.0.0.2
```

### R2

```text
192.168.10.0/24 → 10.0.0.1
```

Static routes manually tell the router which **next hop** should be used to reach a remote network.

---

## ⚙️ R1 Static Route

```cisco
ip route 192.168.20.0 255.255.255.0 10.0.0.2
```

## ⚙️ R2 Static Route

```cisco
ip route 192.168.10.0 255.255.255.0 10.0.0.1
```

### Verify

```cisco
show ip route
```

---

# 🔐 Extended ACL

An Extended ACL allows more specific traffic filtering using information such as:

- Source IP
- Destination IP
- Protocol
- Ports

For this project:

```text
DENY
192.168.10.10 → 192.168.20.10

PERMIT
192.168.10.0/24 → 192.168.20.0/24
```

The **specific deny rule must be placed before the broader permit rule** because ACLs are processed from top to bottom using first-match logic.

---

## 🛡️ ACL Policy

```text
              LAN 1
                │
                ▼
        ┌─────────────────┐
        │ LAN1_TO_LAN2 ACL│
        ├─────────────────┤
        │ PC1 → PC2 ❌    │
        │ LAN1 → LAN2 ✅  │
        └────────┬────────┘
                 │
                 ▼
                R2
                 │
                LAN 2
```

---

# ⚙️ Configuration Commands

## R1 — Interface Configuration

```cisco
enable
configure terminal

interface gigabitEthernet 0/0
ip address 192.168.10.1 255.255.255.0
no shutdown
exit

interface gigabitEthernet 0/1
ip address 10.0.0.1 255.255.255.252
no shutdown
exit
```

## R1 — Static Route

```cisco
ip route 192.168.20.0 255.255.255.0 10.0.0.2
```

## R1 — Extended Named ACL

```cisco
ip access-list extended LAN1_TO_LAN2
deny ip host 192.168.10.10 host 192.168.20.10
permit ip 192.168.10.0 0.0.0.255 192.168.20.0 0.0.0.255
exit
```

## R1 — Apply ACL

```cisco
interface gigabitEthernet 0/0
ip access-group LAN1_TO_LAN2 in
exit

end
```

---

## R2 — Interface Configuration

```cisco
enable
configure terminal

interface gigabitEthernet 0/0
ip address 192.168.20.1 255.255.255.0
no shutdown
exit

interface gigabitEthernet 0/1
ip address 10.0.0.2 255.255.255.252
no shutdown
exit
```

## R2 — Static Route

```cisco
ip route 192.168.10.0 255.255.255.0 10.0.0.1

end
```

---

# 🔎 Verification Commands

### Check Interfaces

```cisco
show ip interface brief
```

### Check Routing Table

```cisco
show ip route
```

### Check ACL

```cisco
show access-lists LAN1_TO_LAN2
```

### Check ACL Applied to Interface

```cisco
show ip interface gigabitEthernet 0/0
```

### Check Complete Configuration

```cisco
show running-config
```

---

# 🧪 Connectivity Testing

### Before ACL Testing

From PC1:

```text
ping 192.168.20.10
```

The initial connectivity should be successful before applying the security restriction.

### Traceroute

```text
tracert 192.168.20.10
```

Expected path:

```text
192.168.10.1
10.0.0.2
192.168.20.10
```

### After ACL

PC1 → PC2:

```text
ping 192.168.20.10
```

Expected:

```text
❌ Blocked
```

PC1 → R2 LAN interface:

```text
ping 192.168.20.1
```

Expected:

```text
✅ Successful
```

This confirms that the ACL blocks the **specific PC1-to-PC2 communication** rather than simply blocking PC1 completely.

---

# 🧠 ACL Concepts Used

### Wildcard Mask

For a `/24` network:

```text
Subnet Mask:  255.255.255.0
Wildcard:       0.0.0.255
```

For a single host:

```text
host 192.168.10.10
```

is equivalent to:

```text
192.168.10.10 0.0.0.0
```

### First-Match Processing

```text
Rule 10 → DENY PC1 → PC2
Rule 20 → PERMIT LAN1 → LAN2
```

The router checks Rule 10 first. If it matches, processing stops.

### Implicit Deny

Traffic that does not match an explicit ACL rule is denied by the implicit deny at the end of the ACL.

---

# 🔍 ACL Verification & Counters

Use:

```cisco
show access-lists LAN1_TO_LAN2
```

Example:

```text
Extended IP access list LAN1_TO_LAN2
    10 deny ip host 192.168.10.10 host 192.168.20.10 (4 match(es))
    20 permit ip 192.168.10.0 0.0.0.255 192.168.20.0 0.0.0.255 (4 match(es))
```

The match counters help verify which ACL rule processed the traffic.

---

# 🕵️ SOC Analyst Connection

ACLs are also useful from a security monitoring perspective.

A blocked connection such as:

```text
192.168.10.10 → 192.168.20.10
```

can be investigated by asking:

```text
Who is the source?
What is the destination?
Was the traffic allowed or denied?
Which rule blocked it?
How many attempts occurred?
Was the communication expected?
```

In a real SOC, this information could be correlated with firewall logs, SIEM events, endpoint telemetry, DNS logs, and network-flow data.

---

# 🛠️ Troubleshooting Checklist

```text
1. Check physical connections
2. Check interface status
3. Verify IP addresses
4. Verify default gateways
5. Check routing tables
6. Check static routes
7. Verify ACL configuration
8. Verify ACL interface
9. Verify ACL direction
10. Check ACL rule order
11. Check ACL counters
12. Test connectivity again
```

---

# 🎓 Skills Demonstrated

| Category | Skills |
|---|---|
| 🌐 Networking | IPv4, Routing, Static Routes |
| 🔀 Routing | Next-Hop Routing, Routing Tables |
| 🔐 Security | Extended ACL, Named ACL |
| 🛡️ Access Control | Permit/Deny Policies |
| 🧪 Testing | Ping, Traceroute, ACL Counters |
| 🔎 Troubleshooting | Routing & ACL Verification |
| 🕵️ SOC | Network Traffic & Security Policy Analysis |

---

# 📁 Project Files

```text
packet-tracer-project-07-enterprise-routing-acl-security/
│
├── README.md
├── packet-tracer/
│   └── project-07.pkt
├── screenshots/
└── configuration/
    └── acl-configuration.txt
```

---

# 🏁 Project Status

```text
╔══════════════════════════════════════════╗
║        PROJECT 07 — COMPLETED            ║
║                                          ║
║        Enterprise Routing & ACL          ║
║                                          ║
║        ████████████████████████ 100%     ║
╚══════════════════════════════════════════╝
```

> **Built and tested in Cisco Packet Tracer as part of the Enterprise Networking & Security Project Series.**
