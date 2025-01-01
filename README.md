# Dynamic_Routing_Using_RIP
Here’s an explanation of RIP (Routing Information Protocol), the configuration you shared, and a detailed `README` for your network configuration using RIPv2 with passive interfaces.

---

# Routing Information Protocol (RIP)

## Overview of RIP
RIP (Routing Information Protocol) is one of the simplest and oldest distance-vector routing protocols. It dynamically exchanges routing information between routers in a network. Key features of RIP include:
- **Metric:** Uses hop count as a metric (maximum of 15 hops, making it unsuitable for very large networks).
- **Versions:** Supports two versions: RIPv1 (classful, no support for VLSM) and RIPv2 (classless, supports VLSM).
- **Periodic Updates:** Sends routing table updates every 30 seconds.
- **Algorithm:** Bellman-Ford algorithm.

### Advantages of RIP:
- Easy to configure and implement.
- Suitable for small networks.

### Disadvantages:
- Limited scalability (15-hop limit).
- Slow convergence.
- High bandwidth consumption due to periodic updates.

---

# README: RIP Network Configuration

This document explains how to configure RIPv2 for your network topology and includes the use of passive interfaces to prevent routing updates on specific interfaces.

## **Steps to Configure RIPv2**

### 1. Enable RIP on Routers

For each router, configure RIP, specify the version, disable auto-summary, and advertise the networks.

#### **Router R1 Configuration**
```bash
R1>enable
R1#configure terminal
R1(config)#router rip
R1(config-router)#version 2
R1(config-router)#no auto-summary
R1(config-router)#network 1.0.0.0
R1(config-router)#network 3.0.0.0
```

#### **Router R2 Configuration**
```bash
R2>enable
R2#configure terminal
R2(config)#router rip
R2(config-router)#version 2
R2(config-router)#no auto-summary
R2(config-router)#network 1.0.0.0
R2(config-router)#network 2.0.0.0
R2(config-router)#network 4.0.0.0
```

#### **Router R3 Configuration**
```bash
R3>enable
R3#configure terminal
R3(config)#router rip
R3(config-router)#version 2
R3(config-router)#no auto-summary
R3(config-router)#network 2.0.0.0
R3(config-router)#network 10.1.0.0
R3(config-router)#network 10.2.0.0
```

#### **Router R4 Configuration**
```bash
R4>enable
R4#configure terminal
R4(config)#router rip
R4(config-router)#version 2
R4(config-router)#no auto-summary
R4(config-router)#network 3.0.0.0
R4(config-router)#network 4.0.0.0
R4(config-router)#network 5.0.0.0
R4(config-router)#network 6.0.0.0
```

#### **Router R5 Configuration**
```bash
R5>enable
R5#configure terminal
R5(config)#router rip
R5(config-router)#version 2
R5(config-router)#no auto-summary
R5(config-router)#network 5.0.0.0
```

#### **Router R6 Configuration**
```bash
R6>enable
R6#configure terminal
R6(config)#router rip
R6(config-router)#version 2
R6(config-router)#no auto-summary
R6(config-router)#network 6.0.0.0
```

---

### 2. Use Passive Interfaces

Passive interfaces prevent RIP routing updates from being sent through specific interfaces while still allowing the interface to be used for traffic forwarding.

#### Configure Passive Interfaces on R3
To prevent routing updates on R3's Gigabit Ethernet interfaces connected to switches:
```bash
R3(config)#router rip
R3(config-router)#passive-interface GigabitEthernet 0/0
R3(config-router)#passive-interface GigabitEthernet 0/1
```

This ensures that RIP updates are not broadcast to the LAN, improving security and reducing unnecessary traffic.

---

### 3. Verification Commands

#### Check Routing Table
Use `show ip route` to verify that RIP has populated the routing table:
```bash
R1#show ip route
```

#### Check RIP Configuration
Use `show run | section router rip` to verify the RIP configuration:
```bash
R1#show run | section router rip
```

#### Check RIP Updates
Use `debug ip rip` to observe RIP updates being sent and received:
```bash
R1#debug ip rip
```

#### Test Connectivity
Ping devices in the network to ensure end-to-end connectivity:
```bash
R1#ping 10.2.0.2
```

---

### 4. Important Notes
- RIP has a maximum hop count of 15, so ensure all routes are within this limit.
- Use `passive-interface` on LAN-facing interfaces to secure the network and reduce unnecessary RIP traffic.
- RIP v2 supports VLSM (Variable Length Subnet Mask), so networks can have different subnet masks.

---

### Diagram Reference

Refer to the provided diagram `Routing_using_RIP_Protocol.PNG` for the network topology and IP addressing.

---

