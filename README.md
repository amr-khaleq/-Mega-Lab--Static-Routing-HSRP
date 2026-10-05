
**🚀 Enterprise Routing Lab | Static Routing + HSRP + Redundancy**
A practical Cisco Packet Tracer lab designed to demonstrate:

- Static Routing
- Default Routing
- Floating Static Routes
- HSRP Gateway Redundancy
- Layer 3 Switching
- Inter-VLAN Routing
- VLANs
- Trunking
- Redundant Network Paths
- Network Failure & Recovery Testing

---

## 🏗️ Topology

The lab consists of:

- 2 × Layer 3 Switches
- 2 × Edge Routers
- 1 × ISP Router
- 2 × Access Switches
- 4 × PCs

### Network Architecture

```text
                         ISP
                      203.0.113.1
                     /           \
                    /             \
              R1 - EDGE          EDGE - R2
             203.0.113.2        203.0.113.6
                 |                   |
             10.0.13.2          10.0.23.2
                 |                   |
             L3-SW1 ============== L3-SW2
             ACTIVE                 STANDBY
              |                         |
             SW1                       SW2
           /    \                    /    \
         PC1    PC2                 PC3    PC4
````

---

## 🌐 VLAN & IP Addressing

| VLAN | Department | Network         | HSRP Virtual Gateway |
| ---- | ---------- | --------------- | -------------------- |
| 10   | SALES      | 192.168.10.0/24 | 192.168.10.254       |
| 20   | HR         | 192.168.20.0/24 | 192.168.20.254       |
| 30   | IT         | 192.168.30.0/24 | 192.168.30.254       |

### L3-SW1

| VLAN    | IP Address   | HSRP Priority |
| ------- | ------------ | ------------- |
| VLAN 10 | 192.168.10.1 | 150           |
| VLAN 20 | 192.168.20.1 | 150           |
| VLAN 30 | 192.168.30.1 | 150           |

**Role: HSRP Active**

### L3-SW2

| VLAN    | IP Address   | HSRP Priority |
| ------- | ------------ | ------------- |
| VLAN 10 | 192.168.10.2 | 100           |
| VLAN 20 | 192.168.20.2 | 100           |
| VLAN 30 | 192.168.30.2 | 100           |

**Role: HSRP Standby**

---

## 🔗 Point-to-Point Links

### L3-SW1 ↔ R1

```text
Network: 10.0.13.0/30

L3-SW1: 10.0.13.1
R1:     10.0.13.2
```

### L3-SW2 ↔ R2

```text
Network: 10.0.23.0/30

L3-SW2: 10.0.23.1
R2:     10.0.23.2
```

### L3-SW1 ↔ L3-SW2

```text
Network: 10.0.12.0/30

L3-SW1: 10.0.12.1
L3-SW2: 10.0.12.2
```

### R1 ↔ ISP

```text
Network: 203.0.113.0/30

R1:  203.0.113.2
ISP:  203.0.113.1
```

### R2 ↔ ISP

```text
Network: 203.0.113.4/30

R2:  203.0.113.6
ISP:  203.0.113.5
```

---

## ⚡ HSRP Configuration

HSRP provides a virtual default gateway for the end devices.

```text
VLAN 10 → 192.168.10.254
VLAN 20 → 192.168.20.254
VLAN 30 → 192.168.30.254
```

L3-SW1 has a higher priority:

```text
Priority: 150
Role: Active
```

L3-SW2:

```text
Priority: 100
Role: Standby
```

The PCs use the HSRP virtual IP as their default gateway.

---

## 🛣️ Static Routing

The lab uses static routing to provide connectivity between the internal networks and the edge routers.

### Default Route — L3-SW1

```cisco
ip route 0.0.0.0 0.0.0.0 10.0.13.2
```

### Default Route — L3-SW2

```cisco
ip route 0.0.0.0 0.0.0.0 10.0.23.2
```

### R1 Internal Routes

```cisco
ip route 192.168.10.0 255.255.255.0 10.0.13.1
ip route 192.168.20.0 255.255.255.0 10.0.13.1
ip route 192.168.30.0 255.255.255.0 10.0.13.1
```

### R2 Internal Routes

```cisco
ip route 192.168.10.0 255.255.255.0 10.0.23.1
ip route 192.168.20.0 255.255.255.0 10.0.23.1
ip route 192.168.30.0 255.255.255.0 10.0.23.1
```

---

## 🔄 Floating Static Route

A Floating Static Route is configured with a higher Administrative Distance and acts as a backup path.

Example:

```cisco
ip route 0.0.0.0 0.0.0.0 10.0.12.2 10
```

Primary route:

```text
Next Hop: 10.0.13.2
AD: 1
```

Backup route:

```text
Next Hop: 10.0.12.2
AD: 10
```

The backup route is installed only when the primary route is unavailable.

---

## 🧪 HSRP Failure Test

To simulate a failure:

```cisco
L3-SW1(config)# interface vlan 10
L3-SW1(config-if)# shutdown
```

Then verify:

```cisco
show standby brief
```

Expected result:

```text
L3-SW1 → Down
L3-SW2 → Active
```

The PCs continue using:

```text
192.168.10.254
```

No default gateway change is required on the end devices.

---

## 🔍 Verification Commands

```cisco
show ip interface brief
show vlan brief
show interfaces trunk
show ip route
show ip route static
show standby brief
show standby
```

Connectivity testing:

```cisco
ping 192.168.20.10
ping 192.168.30.10
ping 203.0.113.1
traceroute 203.0.113.1
```

---

## 🎯 Key Learning Outcomes

This lab demonstrates how to:

1. Configure Layer 3 Switches.
2. Convert switch interfaces into routed ports.
3. Configure VLANs and SVIs.
4. Configure HSRP.
5. Implement Static Routing.
6. Configure Default Routes.
7. Configure Floating Static Routes.
8. Build redundant gateway paths.
9. Simulate network failures.
10. Verify automatic gateway failover.

---

## 🛠️ Technologies

* Cisco Packet Tracer
* Cisco IOS
* VLAN
* SVI
* Layer 3 Switching
* Static Routing
* HSRP
* Default Routing
* Floating Static Routing
* 802.1Q Trunking
* Network Redundancy
* High Availability

---

## 👨‍💻 Author

**Amr Abdel-Khaleq Ibrahim**

Aspiring Network Engineer

Cisco Networking | Routing & Switching | Network Infrastructure

#Cisco #CCNA #CCNP #Networking #HSRP #StaticRouting



