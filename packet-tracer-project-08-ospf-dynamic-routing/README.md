# 🛡️ Cisco Packet Tracer --- OSPF Dynamic Routing Lab

![Cisco Packet
Tracer](https://img.shields.io/badge/Cisco-Packet%20Tracer-blue?style=for-the-badge&logo=cisco)
![OSPF](https://img.shields.io/badge/Routing-OSPF-green?style=for-the-badge)
![Networking](https://img.shields.io/badge/Network-Dynamic%20Routing-orange?style=for-the-badge)
![Status](https://img.shields.io/badge/Status-Completed-success?style=for-the-badge)

> A hands-on Cisco Packet Tracer project demonstrating OSPF dynamic
> routing, neighbor adjacency, route learning, OSPF cost, ECMP,
> failover, convergence, and troubleshooting in a three-router
> enterprise topology.

------------------------------------------------------------------------

## 📌 Project Overview

This project demonstrates how **Open Shortest Path First (OSPF)** can
dynamically exchange routing information between routers in an
enterprise network.

Instead of manually configuring static routes, OSPF allows routers to:

-   Discover neighboring routers
-   Exchange routing information
-   Build a link-state database
-   Calculate the best available path
-   Dynamically install routes
-   Reroute traffic when a link fails
-   Restore preferred paths after recovery

The lab also includes controlled failure testing to demonstrate **OSPF
neighbor loss, route recalculation, failover, and convergence**.

------------------------------------------------------------------------

## 🏢 Enterprise Network Architecture

``` text
                              ┌──────────────────┐
                              │       R1         │
                              │                  │
                              │ Gi0/0  10.0.12.1 │
                              │ Gi0/1  10.0.13.1 │
                              │ Gi0/2 192.168.10.1│
                              └──────┬─────┬─────┘
                                     │     │
                          10.0.12.0/30     │ 10.0.13.0/30
                                     │     │
                                     │     │
                              ┌──────▼─────▼─────┐
                              │                  │
                              │       R2         │
                              │                  │
                              │ Gi0/0  10.0.12.2 │
                              │ Gi0/1  10.0.23.1 │
                              │ Gi0/2 192.168.20.1│
                              └────────┬─────────┘
                                       │
                                  10.0.23.0/30
                                       │
                              ┌────────▼─────────┐
                              │       R3         │
                              │                  │
                              │ Gi0/0  10.0.13.2 │
                              │ Gi0/1  10.0.23.2 │
                              │ Gi0/2 192.168.30.1│
                              └──────────────────┘
```

### End Devices

``` text
PC1 ─── R1 Gi0/2
PC2 ─── R2 Gi0/2
PC3 ─── R3 Gi0/2
```

The three routers form a triangle topology, providing redundant paths
between networks.

------------------------------------------------------------------------

## 🌐 IP Addressing Scheme

  Device   Interface   IP Address     Subnet Mask       Purpose
  -------- ----------- -------------- ----------------- ---------------
  R1       Gi0/0       10.0.12.1      255.255.255.252   R1 ↔ R2
  R1       Gi0/1       10.0.13.1      255.255.255.252   R1 ↔ R3
  R1       Gi0/2       192.168.10.1   255.255.255.0     LAN 1 Gateway
  R2       Gi0/0       10.0.12.2      255.255.255.252   R2 ↔ R1
  R2       Gi0/1       10.0.23.1      255.255.255.252   R2 ↔ R3
  R2       Gi0/2       192.168.20.1   255.255.255.0     LAN 2 Gateway
  R3       Gi0/0       10.0.13.2      255.255.255.252   R3 ↔ R1
  R3       Gi0/1       10.0.23.2      255.255.255.252   R3 ↔ R2
  R3       Gi0/2       192.168.30.1   255.255.255.0     LAN 3 Gateway

### PC Addressing

  Device   IP Address      Subnet Mask     Default Gateway
  -------- --------------- --------------- -----------------
  PC1      192.168.10.10   255.255.255.0   192.168.10.1
  PC2      192.168.20.10   255.255.255.0   192.168.20.1
  PC3      192.168.30.10   255.255.255.0   192.168.30.1

------------------------------------------------------------------------

## 🛠️ Technologies & Tools

-   Cisco Packet Tracer
-   Cisco IOS
-   OSPF
-   IPv4
-   Ethernet
-   ICMP
-   Dynamic Routing
-   Routing Tables
-   OSPF Neighbor Relationships
-   OSPF Cost
-   ECMP
-   Network Troubleshooting

------------------------------------------------------------------------

## 🎯 Learning Objectives

-   Understand dynamic routing and OSPF
-   Configure OSPF Area 0
-   Understand OSPF network statements and wildcard masks
-   Form and verify OSPF neighbor adjacencies
-   Understand Router IDs and DR/BDR
-   Read OSPF routing-table entries
-   Understand OSPF cost and path selection
-   Observe ECMP
-   Test routing failover
-   Investigate OSPF neighbor failure
-   Understand route recalculation and convergence
-   Connect routing behavior to practical network/SOC troubleshooting

------------------------------------------------------------------------

# 🧠 OSPF Fundamentals

## What is OSPF?

**Open Shortest Path First (OSPF)** is a link-state dynamic routing
protocol used to exchange routing information within an autonomous
system.

A simplified OSPF workflow is:

``` text
Discover Neighbors
       ↓
Exchange Link-State Information
       ↓
Build Link-State Database
       ↓
Run SPF Algorithm
       ↓
Calculate Best Paths
       ↓
Install Routes
```

------------------------------------------------------------------------

## 🌐 OSPF Area 0

All routers in this lab belong to:

``` text
Area 0
```

Area 0 is the OSPF backbone area.

``` text
R1 ───────── R2
│             │
│             │
└──── R3 ─────┘

       Area 0
```

------------------------------------------------------------------------

# ⚙️ OSPF Configuration

## R1

``` text
R1# configure terminal
R1(config)# router ospf 1
R1(config-router)# network 10.0.12.0 0.0.0.3 area 0
R1(config-router)# network 10.0.13.0 0.0.0.3 area 0
R1(config-router)# network 192.168.10.0 0.0.0.255 area 0
R1(config-router)# end
```

## R2

``` text
R2# configure terminal
R2(config)# router ospf 1
R2(config-router)# network 10.0.12.0 0.0.0.3 area 0
R2(config-router)# network 10.0.23.0 0.0.0.3 area 0
R2(config-router)# network 192.168.20.0 0.0.0.255 area 0
R2(config-router)# end
```

## R3

``` text
R3# configure terminal
R3(config)# router ospf 1
R3(config-router)# network 10.0.13.0 0.0.0.3 area 0
R3(config-router)# network 10.0.23.0 0.0.0.3 area 0
R3(config-router)# network 192.168.30.0 0.0.0.255 area 0
R3(config-router)# end
```

------------------------------------------------------------------------

# 🔍 Wildcard Masks

OSPF uses wildcard masks with the `network` command.

For example:

``` text
network 10.0.12.0 0.0.0.3 area 0
```

The wildcard mask:

``` text
0.0.0.3
```

corresponds to:

``` text
/30
255.255.255.252
```

A wildcard mask tells IOS which bits must match and which bits can vary.

Common masks used in this project:

``` text
/30 → 0.0.0.3
/24 → 0.0.0.255
```

------------------------------------------------------------------------

# 🆔 OSPF Router IDs

The routers automatically selected these Router IDs from their available
interface addresses:

  Router   Router ID
  -------- --------------
  R1       192.168.10.1
  R2       192.168.20.1
  R3       192.168.30.1

A Router ID provides a unique identity for an OSPF router.

Verify with:

``` text
show ip ospf
```

------------------------------------------------------------------------

# 🤝 OSPF Neighbor Adjacency

OSPF routers discover neighbors using Hello packets.

Simplified process:

``` text
Hello
  ↓
Neighbor Discovery
  ↓
Database Exchange
  ↓
Database Synchronization
  ↓
FULL
```

The `FULL` state indicates successful OSPF database synchronization.

Verify with:

``` text
show ip ospf neighbor
```

------------------------------------------------------------------------

# 🏆 DR / BDR

Ethernet networks use OSPF's broadcast network behavior, which allows a
**Designated Router (DR)** and **Backup Designated Router (BDR)**
election.

The lab displayed states such as:

``` text
FULL/DR
FULL/BDR
FULL/DROTHER
```

For basic adjacency verification, the important state is:

``` text
FULL
```

------------------------------------------------------------------------

# 🛣️ OSPF Routing Table

OSPF routes are identified by:

``` text
O
```

Example:

``` text
O 192.168.20.0/24 [110/2] via 10.0.12.2
```

Breakdown:

``` text
O
│
└── OSPF

192.168.20.0/24
│
└── Destination network

[110/2]
│   │
│   ├── OSPF metric
│   └── Administrative Distance
│
└── Next hop: 10.0.12.2
```

Useful commands:

``` text
show ip route ospf
show ip route 192.168.20.0
```

------------------------------------------------------------------------

# 🔄 Equal-Cost Multi-Path (ECMP)

The triangle topology provides redundant paths:

``` text
                 R1
                /  \
               /    \
              R2────R3
```

If two paths have the same total OSPF cost, OSPF can install multiple
equal-cost paths.

This behavior is called:

**Equal-Cost Multi-Path (ECMP)**

In this lab, the direct R1 → R2 path was lower cost than the alternate
R1 → R3 → R2 path, so the direct route was preferred.

------------------------------------------------------------------------

# 📊 OSPF Cost

OSPF selects paths using cumulative interface cost.

In the lab:

``` text
R1 → R2
Cost = 2
```

while:

``` text
R1 → R3 → R2
Cost = 3
```

Therefore:

``` text
R1 → R2
```

was preferred.

The important rule is:

> **OSPF chooses the path with the lowest cumulative cost, not
> necessarily the path with the fewest physical hops.**

------------------------------------------------------------------------

# 🧪 OSPF Cost Manipulation

To demonstrate path selection, R1 Gi0/0 was temporarily configured with
a higher cost:

``` text
R1# configure terminal
R1(config)# interface gigabitEthernet 0/0
R1(config-if)# ip ospf cost 10
R1(config-if)# end
```

This changed the routing preference.

The direct path became:

``` text
R1 → R2
Cost = 11
```

while the alternate path remained:

``` text
R1 → R3 → R2
Cost = 3
```

R1 therefore selected:

``` text
Next Hop: 10.0.13.2
Metric: 3
```

The original cost was then restored:

``` text
R1# configure terminal
R1(config)# interface gigabitEthernet 0/0
R1(config-if)# no ip ospf cost
R1(config-if)# end
```

------------------------------------------------------------------------

# 🧪 OSPF Failure & Failover Investigation

## Scenario

The R1--R2 link was intentionally disabled to simulate a network
failure.

``` text
R1 Gi0/0 ───────── R2 Gi0/0
             ❌
```

Command:

``` text
R1# configure terminal
R1(config)# interface gigabitEthernet 0/0
R1(config-if)# shutdown
R1(config-if)# end
```

------------------------------------------------------------------------

## 🚨 Neighbor Failure Detection

The OSPF neighbor table was checked:

``` text
show ip ospf neighbor
```

The R1--R2 neighbor disappeared while R1--R3 remained `FULL`.

This demonstrated how an OSPF adjacency change can be detected during
troubleshooting.

------------------------------------------------------------------------

## 🔎 Route Recalculation

R2's LAN was:

``` text
192.168.20.0/24
```

Before failure:

``` text
R1 → R2 → 192.168.20.0/24
```

After failure:

``` text
R1 → R3 → R2 → 192.168.20.0/24
```

The route showed:

``` text
Next Hop: 10.0.13.2
Metric: 3
```

OSPF automatically recalculated the available path.

------------------------------------------------------------------------

## 🧪 End-to-End Failover Test

PC1 tested connectivity to PC2 while the R1--R2 link remained down:

``` text
PC1> ping 192.168.20.10
```

Result:

``` text
Ping successful
```

Traffic successfully used:

``` text
PC1
 ↓
R1
 ↓
R3
 ↓
R2
 ↓
PC2
```

------------------------------------------------------------------------

# 🔄 Link Recovery

The failed interface was restored:

``` text
R1# configure terminal
R1(config)# interface gigabitEthernet 0/0
R1(config-if)# no shutdown
R1(config-if)# end
```

The R1--R2 OSPF adjacency returned to:

``` text
FULL
```

After convergence, the lower-cost direct path was restored:

``` text
R1 → R2
```

Next hop:

``` text
10.0.12.2
```

------------------------------------------------------------------------

# 🔁 OSPF Convergence

The complete failure and recovery process was:

``` text
Normal Operation
       ↓
Link Failure
       ↓
OSPF Neighbor Lost
       ↓
Topology Change
       ↓
SPF Recalculation
       ↓
Alternate Route
       ↓
Traffic Continues
       ↓
Link Recovery
       ↓
Neighbor Re-establishment
       ↓
SPF Recalculation
       ↓
Preferred Route Restored
```

This process demonstrates **OSPF convergence**.

------------------------------------------------------------------------

# 🕵️ SOC / Network Analyst Perspective

Routing information is valuable during security and infrastructure
investigations.

For example:

``` text
🚨 OSPF Neighbor Down
Router: R1
Neighbor: R2
```

An analyst should not immediately conclude that the network is
completely down.

A structured investigation can be:

``` text
1. Identify the failed neighbor
          ↓
2. Identify the affected interface
          ↓
3. Check the routing table
          ↓
4. Determine whether an alternate route exists
          ↓
5. Test end-to-end connectivity
          ↓
6. Identify the new forwarding path
          ↓
7. Monitor recovery
          ↓
8. Verify route reconvergence
```

This troubleshooting approach is useful when investigating:

-   Network outages
-   Routing anomalies
-   Unexpected traffic paths
-   Infrastructure failures
-   Security incidents
-   Lateral movement
-   Network segmentation issues

------------------------------------------------------------------------

# 🔍 Investigation Evidence

  Investigation Point     Evidence
  ----------------------- --------------------------------
  OSPF neighbor failure   R1--R2 adjacency disappeared
  Remaining adjacency     R1--R3 remained FULL
  Alternate route         Next hop changed to 10.0.13.2
  Alternate metric        OSPF metric 3
  Failover                PC1 → PC2 ping succeeded
  Recovery                R1--R2 returned to FULL
  Preferred route         Next hop returned to 10.0.12.2

------------------------------------------------------------------------

# 🧰 Configuration Command Reference

## Interface Configuration

Example R1 configuration:

``` text
interface gigabitEthernet 0/0
ip address 10.0.12.1 255.255.255.252
no shutdown

interface gigabitEthernet 0/1
ip address 10.0.13.1 255.255.255.252
no shutdown

interface gigabitEthernet 0/2
ip address 192.168.10.1 255.255.255.0
no shutdown
```

R2 and R3 were configured according to the IP addressing table.

## OSPF

``` text
router ospf 1
network <network-address> <wildcard-mask> area 0
```

## Neighbor Verification

``` text
show ip ospf neighbor
```

## OSPF Process

``` text
show ip ospf
```

## OSPF Interfaces

``` text
show ip ospf interface
```

## OSPF Routes

``` text
show ip route ospf
```

## Specific Route

``` text
show ip route 192.168.20.0
```

## Interface Status

``` text
show ip interface brief
```

## Change OSPF Cost

``` text
interface gigabitEthernet 0/0
ip ospf cost 10
```

## Restore Automatic Cost

``` text
interface gigabitEthernet 0/0
no ip ospf cost
```

## Simulate Link Failure

``` text
interface gigabitEthernet 0/0
shutdown
```

## Restore Link

``` text
interface gigabitEthernet 0/0
no shutdown
```

------------------------------------------------------------------------

# 🧪 Final Verification

The following tests were successfully completed:

``` text
✅ Router-to-router connectivity
✅ OSPF neighbor formation
✅ FULL adjacency
✅ OSPF route exchange
✅ LAN route advertisement
✅ PC-to-PC connectivity
✅ OSPF cost verification
✅ OSPF path manipulation
✅ Link failure simulation
✅ Neighbor failure detection
✅ Route recalculation
✅ Automatic failover
✅ End-to-end failover testing
✅ Link recovery
✅ OSPF reconvergence
```

------------------------------------------------------------------------

# 📚 Key Concepts Learned

  Concept               Practical Demonstration
  --------------------- ----------------------------------------
  Dynamic Routing       Routers automatically exchanged routes
  OSPF                  Dynamic link-state routing protocol
  Area 0                OSPF backbone area
  Router ID             Unique OSPF router identity
  Neighbor Discovery    OSPF Hello process
  FULL State            Database synchronization completed
  DR/BDR                OSPF Ethernet election
  Wildcard Mask         OSPF network matching
  OSPF Cost             Path-selection metric
  SPF                   Shortest Path First calculation
  ECMP                  Equal-cost paths
  Convergence           Recovery after topology changes
  Failover              Traffic moved to an alternate path
  Route Recalculation   New path selected after failure

------------------------------------------------------------------------

# 🎯 Skills Demonstrated

### Networking

-   IPv4 addressing
-   Subnetting
-   `/30` point-to-point networks
-   `/24` LAN networks
-   Router interface configuration
-   Routing table analysis
-   Network troubleshooting

### OSPF

-   OSPF configuration
-   Area 0
-   Neighbor relationships
-   Router IDs
-   DR/BDR
-   OSPF metrics
-   Interface cost
-   Dynamic route learning
-   ECMP
-   SPF path selection
-   Convergence
-   Failover

### Troubleshooting

-   Neighbor failure analysis
-   Route verification
-   Next-hop identification
-   Path comparison
-   Link failure simulation
-   Alternate path verification
-   End-to-end connectivity testing
-   Recovery validation

### SOC-Relevant Skills

-   Event-driven investigation
-   Network evidence collection
-   Alert validation
-   Route/path analysis
-   Root-cause investigation
-   Impact assessment
-   Infrastructure monitoring mindset
-   Structured troubleshooting

------------------------------------------------------------------------

# 🧠 Key Takeaways

The main lesson from this project is that **OSPF dynamically adapts to
network changes**.

A simplified process is:

``` text
Network Change
      ↓
OSPF Detects Change
      ↓
Link-State Information Updated
      ↓
SPF Recalculation
      ↓
Best Available Route Selected
      ↓
Routing Table Updated
      ↓
Traffic Forwarded
```

The failure-testing portion demonstrated that a routing protocol can
maintain connectivity even when a primary path becomes unavailable.

The OSPF cost exercise also demonstrated that **path selection depends
on cumulative cost rather than simply counting hops**.

------------------------------------------------------------------------

# 🏁 Project Outcome

This project successfully demonstrated a three-router enterprise network
using **OSPF dynamic routing**.

The lab progressed from basic connectivity to:

``` text
OSPF Configuration
        ↓
Neighbor Formation
        ↓
Dynamic Route Exchange
        ↓
Path Selection
        ↓
OSPF Cost Manipulation
        ↓
Link Failure
        ↓
Automatic Failover
        ↓
Route Convergence
        ↓
Network Recovery
```

The result is practical experience with how enterprise routers
dynamically discover networks, select paths, respond to failures, and
restore preferred routes.

------------------------------------------------------------------------

## 📁 Project Structure

``` text
packet-tracer-project-08-ospf-dynamic-routing/
│
├── README.md
│
├── topology/
│   └── ospf-enterprise-topology.pkt
│
├── screenshots/
│   ├── ospf-neighbor-r1.png
│   ├── ospf-neighbor-r2.png
│   ├── ospf-neighbor-r3.png
│   ├── ospf-routing-table.png
│   ├── ospf-failure.png
│   ├── ospf-failover.png
│   └── ospf-recovery.png
│
└── documentation/
    └── ospf-command-reference.md
```

------------------------------------------------------------------------

# 📊 Project Status

``` text
🟩🟩🟩🟩🟩🟩🟩🟩🟩🟩
       100% COMPLETE
```

**Project 08 --- OSPF Dynamic Routing** ✅

------------------------------------------------------------------------

## 🔐 Skills

`Cisco Packet Tracer` • `Cisco IOS` • `OSPF` • `Dynamic Routing` •
`IPv4` • `Routing Tables` • `OSPF Troubleshooting` • `Network Failover`
• `Route Convergence` • `ECMP` • `Network Monitoring` •
`Incident Investigation`
