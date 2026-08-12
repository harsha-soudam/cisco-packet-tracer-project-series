# 🔄 Cisco Packet Tracer Project 10 — High Availability (HSRP) Lab

![Cisco Packet Tracer](https://img.shields.io/badge/Cisco-Packet%20Tracer-blue?logo=cisco)
![HSRP](https://img.shields.io/badge/Network-HSRP-orange)
![Status](https://img.shields.io/badge/Lab-Completed-success)

> **Project:** High Availability (HSRP) Lab  
> **Platform:** Cisco Packet Tracer  
> **Focus:** HSRP, gateway redundancy, Active/Standby routers, failover, and recovery

---

## 📌 Project Overview

This project demonstrates **Hot Standby Router Protocol (HSRP)**, a first-hop redundancy protocol used to provide a highly available default gateway.

Two routers, R1 and R2, share a virtual IP address. R1 operates as the **Active** router while R2 operates as the **Standby** router.

If R1 becomes unavailable, R2 automatically takes over the virtual gateway. When R1 recovers, its higher HSRP priority and preemption configuration allow it to become Active again.

---

## 🏗️ Network Architecture

```text
                         ┌──────────────┐
                         │     SW1      │
                         │    2960      │
                         └──────┬───────┘
                         ┌─────┴─────┐
                         │           │
                       R1            R2
                    Gi0/0          Gi0/0
                 192.168.40.2   192.168.40.3
                 Priority 110    Priority 100
                   ACTIVE         STANDBY
                         │           │
                         └─────┬─────┘
                               │
                       HSRP Virtual IP
                         192.168.40.1
                               │
                         ┌─────┴─────┐
                         │           │
                        PC1         PC2
                    192.168.40.10 192.168.40.11
```

---

## 🧮 IP Addressing Scheme

| Device | Interface | IP Address | Subnet Mask | Role |
|---|---|---|---|---|
| R1 | Gi0/0 | `192.168.40.2` | `255.255.255.0` | HSRP Active |
| R2 | Gi0/0 | `192.168.40.3` | `255.255.255.0` | HSRP Standby |
| HSRP | Virtual | `192.168.40.1` | `255.255.255.0` | Virtual Gateway |
| PC1 | Fa0 | `192.168.40.10` | `255.255.255.0` | LAN Host |
| PC2 | Fa0 | `192.168.40.11` | `255.255.255.0` | LAN Host |

## 💻 PC Addressing

| PC | IP Address | Subnet Mask | Default Gateway |
|---|---|---|---|
| PC1 | `192.168.40.10` | `255.255.255.0` | `192.168.40.1` |
| PC2 | `192.168.40.11` | `255.255.255.0` | `192.168.40.1` |

---

## 🧠 Key Concepts

### HSRP

**Hot Standby Router Protocol (HSRP)** provides gateway redundancy by allowing multiple routers to share a virtual IP address.

The PCs use:

```text
Default Gateway → 192.168.40.1
```

They do not directly depend on R1 or R2 as their gateway.

### Active and Standby

```text
R1 → Priority 110 → Active
R2 → Priority 100 → Standby
```

The router with the higher priority becomes Active.

### HSRP Virtual IP

```text
R1 physical IP → 192.168.40.2
R2 physical IP → 192.168.40.3
Virtual IP     → 192.168.40.1
```

### Preemption

The command:

```text
standby 1 preempt
```

allows the higher-priority R1 to reclaim the Active role after recovery.

---

## ⚙️ Configuration Commands

### R1 — LAN Interface

```text
enable
configure terminal
interface gigabitEthernet 0/0
ip address 192.168.40.2 255.255.255.0
no shutdown
exit
end
```

### R2 — LAN Interface

```text
enable
configure terminal
interface gigabitEthernet 0/0
ip address 192.168.40.3 255.255.255.0
no shutdown
exit
end
```

### R1 — HSRP Active

```text
enable
configure terminal
interface gigabitEthernet 0/0
standby 1 ip 192.168.40.1
standby 1 priority 110
standby 1 preempt
exit
end
```

### R2 — HSRP Standby

```text
enable
configure terminal
interface gigabitEthernet 0/0
standby 1 ip 192.168.40.1
standby 1 priority 100
standby 1 preempt
exit
end
```

---

## 🔍 Verification Commands

### Interface Status

```text
show ip interface brief
```

### HSRP Summary

```text
show standby brief
```

### Detailed HSRP Information

```text
show standby
```

The final verification showed:

```text
Virtual IP address is 192.168.40.1
Active router is 192.168.40.2, priority 110
Standby router is local
Priority 100
Preemption enabled
```

Final state:

| Router | IP Address | Priority | State |
|---|---|---:|---|
| R1 | `192.168.40.2` | 110 | 🟢 Active |
| R2 | `192.168.40.3` | 100 | 🟡 Standby |
| Virtual Gateway | `192.168.40.1` | — | 🔵 Virtual |

---

## 🧪 Connectivity Testing

### Router-to-Router

```text
R1# ping 192.168.40.3
R2# ping 192.168.40.2
```

**Result:** ✅ Successful

### Virtual Gateway

```text
PC1> ping 192.168.40.1
PC2> ping 192.168.40.1
```

**Result:** ✅ Successful

---

## 🔥 HSRP Failover Test

Normal state:

```text
R1 → ACTIVE
R2 → STANDBY
Virtual IP → 192.168.40.1
```

R1 was deliberately taken offline:

```text
enable
configure terminal
interface gigabitEthernet 0/0
shutdown
end
```

R2 then became Active:

```text
R1 → DOWN
R2 → ACTIVE
Virtual IP → 192.168.40.1
```

The first ping during the transition failed, while subsequent pings succeeded.

```text
First ping       ❌ Failed
Following pings  ✅ Successful
```

This demonstrates that HSRP can transfer gateway responsibility to the Standby router without changing the PCs' configured gateway.

---

## 🔄 HSRP Recovery Test

R1 was restored:

```text
enable
configure terminal
interface gigabitEthernet 0/0
no shutdown
end
```

Because R1 has priority 110 and preemption enabled:

```text
R1 → ACTIVE
R2 → STANDBY
Virtual IP → 192.168.40.1
```

The PC successfully pinged the virtual gateway after recovery.

---

## 🔁 Complete HSRP Lifecycle

```text
🟢 NORMAL

R1 → ACTIVE
R2 → STANDBY
Virtual IP → 192.168.40.1

        ↓ R1 FAILURE

🟡 FAILOVER

R1 → DOWN
R2 → ACTIVE
Virtual IP → 192.168.40.1

        ↓ R1 RECOVERY

🟢 RECOVERED

R1 → ACTIVE
R2 → STANDBY
Virtual IP → 192.168.40.1
```

---

## 🎯 Learning Objectives

- Understand HSRP and first-hop redundancy
- Configure a virtual gateway
- Configure Active and Standby routers
- Understand HSRP priority
- Configure HSRP preemption
- Test gateway failover
- Test router recovery
- Verify HSRP operation
- Understand why the PC gateway remains unchanged during failover

---

## 🧰 Technologies & Devices

- Cisco Packet Tracer
- Cisco 2911 Routers
- Cisco 2960 Switch
- PC-PT endpoints
- IPv4
- HSRP
- First-hop redundancy
- Virtual gateway

---

## 📁 Suggested Project Structure

```text
packet-tracer-project-10-high-availability-hsrp/
│
├── README.md
├── packet-tracer-project-10.pkt
│
└── screenshots/
    ├── topology.png
    ├── ip-addressing.png
    ├── hsrp-configuration.png
    ├── hsrp-active-standby.png
    ├── failover-test.png
    └── recovery-test.png
```

---

## 🏁 Project Status

**Completed — High Availability (HSRP) successfully demonstrated.**

```text
Topology              🟩🟩🟩🟩🟩🟩🟩🟩🟩🟩  100%
IP Addressing         🟩🟩🟩🟩🟩🟩🟩🟩🟩🟩  100%
Basic Connectivity    🟩🟩🟩🟩🟩🟩🟩🟩🟩🟩  100%
HSRP Configuration    🟩🟩🟩🟩🟩🟩🟩🟩🟩🟩  100%
Active/Standby        🟩🟩🟩🟩🟩🟩🟩🟩🟩🟩  100%
Virtual Gateway Test  🟩🟩🟩🟩🟩🟩🟩🟩🟩🟩  100%
Failover Testing      🟩🟩🟩🟩🟩🟩🟩🟩🟩🟩  100%
Recovery Testing      🟩🟩🟩🟩🟩🟩🟩🟩🟩🟩  100%
Documentation         🟩🟩🟩🟩🟩🟩🟩🟩🟩🟩  100%
```

---

## 💡 Key Takeaway

HSRP provides gateway redundancy by allowing multiple routers to share a virtual IP address.

In this lab:

```text
R1 → Active
R2 → Standby
Virtual IP → 192.168.40.1
```

When R1 failed, R2 automatically became Active while the PCs continued using the same gateway.

When R1 recovered, its higher priority and preemption configuration allowed it to become Active again.

This demonstrates how HSRP maintains **default-gateway availability** even when a router fails.
