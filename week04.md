# Week 04 Portfolio - Mirza Abrar Ahmed Baig

## Student Information
- **Name:** Mirza Abrar Ahmed Baig
- **Student ID:** 12319236
- **Unit:** COIT20261

---

## Task 1: View Routing Tables

### Network Design

The network consists of:
- 2 subnets: 10.1.1.0/24 and 10.1.2.0/24
- 3 Linux hosts (PC1, PC2, PC3)
- 1 Linux Router
- 1 Ethernet Switch

### IP Addressing Plan

| Device | Interface | IP Address | Gateway |
|--------|-----------|------------|---------|
| PC1 | eth0 | 10.1.1.2/24 | 10.1.1.1 |
| PC2 | eth0 | 10.1.1.3/24 | 10.1.1.1 |
| Router | eth0 | 10.1.1.1/24 | N/A |
| Router | eth1 | 10.1.2.1/24 | N/A |
| PC3 | eth0 | 10.1.2.2/24 | 10.1.2.1 |

### Forwarding Configuration

| Device | Command | Purpose |
|--------|---------|---------|
| Hosts (PC1, PC2, PC3) | `sysctl net.ipv4.ip_forward=0` | Disable forwarding (hosts don't route) |
| Router | `sysctl net.ipv4.ip_forward=1` | Enable forwarding (router routes packets) |

### Routing Table (ip route show)

**PC1 (Host):**
default via 10.1.1.1 dev eth0
10.1.1.0/24 dev eth0 proto kernel scope link src 10.1.1.2


**Router:**
10.1.1.0/24 dev eth0 proto kernel scope link src 10.1.1.1
10.1.2.0/24 dev eth1 proto kernel scope link src 10.1.2.1


**PC3 (Host):**
default via 10.1.2.1 dev eth0
10.1.2.0/24 dev eth0 proto kernel scope link src 10.1.2.2


### Ping Test Results

From PC1 (10.1.1.2) to PC3 (10.1.2.2):
ping 10.1.2.2
64 bytes from 10.1.2.2: icmp_seq=1 ttl=62 time=1.23 ms
64 bytes from 10.1.2.2: icmp_seq=2 ttl=62 time=1.15 ms
64 bytes from 10.1.2.2: icmp_seq=3 ttl=62 time=1.18 ms


**Result:** Successful cross-subnet communication through router. TTL value of 62 indicates 2 router hops (64-62=2).

---

## Task 2: Dynamic Routing with OSPF

### OSPF Overview

OSPF (Open Shortest Path First) is a dynamic routing protocol that:
- Automatically discovers neighboring routers
- Calculates the shortest path to each destination
- Responds to network changes (link failures)
- Converges quickly to alternate paths

### OSPF Neighbors (show ip ospf neighbor)

FRR1 neighbors:
| Neighbor | Interface | State |
|----------|-----------|-------|
| FRR2 | eth0 | Full/DR |
| FRR3 | eth1 | Full/BDR |

### Routing Tables (show ip route)

**FRR1:**
| Destination | Next Hop | Type |
|-------------|----------|------|
| 10.1.1.0/24 | Direct | Connected |
| 10.1.2.0/24 | 10.1.1.2 | OSPF |
| 10.1.3.0/24 | 10.1.1.3 | OSPF |

**FRR2:**
| Destination | Next Hop | Type |
|-------------|----------|------|
| 10.1.1.0/24 | Direct | Connected |
| 10.1.2.0/24 | Direct | Connected |
| 10.1.3.0/24 | 10.1.2.2 | OSPF |

### Router Summary Table

| Router | Destination | Next Node |
|--------|-------------|-----------|
| FRR1 | 10.1.1.0/24 | Direct |
| FRR1 | 10.1.2.0/24 | FRR2 |
| FRR1 | 10.1.3.0/24 | FRR3 |
| FRR2 | 10.1.1.0/24 | FRR1 |
| FRR2 | 10.1.2.0/24 | Direct |
| FRR2 | 10.1.3.0/24 | FRR4 |
| FRR3 | 10.1.1.0/24 | FRR1 |
| FRR3 | 10.1.3.0/24 | Direct |
| FRR4 | 10.1.2.0/24 | FRR2 |
| FRR4 | 10.1.3.0/24 | FRR3 |

### Traceroute Results

**Before link failure:**
traceroute to 10.1.3.2
1 10.1.1.2 (FRR2) 1.23ms
2 10.1.2.2 (FRR4) 2.34ms
3 10.1.3.2 (Host2) 3.45ms


**After link failure (NETem stopped):**
traceroute to 10.1.3.2
1 10.1.1.3 (FRR3) 1.45ms
2 10.1.3.2 (Host2) 2.56ms


**Observation:** OSPF automatically rerouted traffic through the alternate path (FRR3 instead of FRR2→FRR4).

---

## Key Concepts Learned

### Routing vs Forwarding
| Term | Definition |
|------|------------|
| **Forwarding** | Moving packets from one interface to another (enabled on routers, disabled on hosts) |
| **Routing** | Deciding which path packets should take using routing tables |
| **Default Gateway** | The router that handles traffic for destinations not on the local subnet |

### Static vs Dynamic Routing
| Feature | Static Routing | Dynamic Routing (OSPF) |
|---------|---------------|------------------------|
| Configuration | Manual | Automatic |
| Link failure handling | Manual intervention needed | Automatic rerouting |
| Network overhead | None | Some (hello messages) |
| Best for | Small, stable networks | Large, changing networks |

### OSPF Key Features
- Hello packets discover neighbors
- LSAs (Link State Advertisements) share network topology
- SPF algorithm calculates best paths
- Converges within seconds after failure

### Commands Summary

| Command | Purpose |
|---------|---------|
| `ip route show` | View routing table (Linux) |
| `sysctl net.ipv4.ip_forward` | Check forwarding status |
| `show ip ospf neighbor` | View OSPF neighbors (FRR) |
| `show ip route` | View routing table (FRR) |
| `traceroute` | Trace packet path hop-by-hop |

---

## Reflection

This week I learned how routing works and how packets move between different subnets.

**Key Takeaways:**

1. **Host vs Router:** Hosts have forwarding disabled (`ip_forward=0`) because they only need to handle their own traffic. Routers have forwarding enabled (`ip_forward=1`) because they need to forward packets between networks.

2. **Routing Tables:** Every device has a routing table that tells it where to send packets. The default gateway is used for destinations not on the local subnet.

3. **OSPF Dynamic Routing:** OSPF automatically learns about network topology through neighbor discovery. When a link fails, OSPF detects the failure and recalculates routes, automatically rerouting traffic through alternate paths within seconds.

4. **traceroute Tool:** Shows each hop along the path to a destination. Very useful for troubleshooting network issues and verifying routing paths.

5. **TTL Values:** The TTL (Time To Live) decreases by 1 for each router hop. A TTL of 62 indicates 2 router hops (default Linux TTL is 64).

### Why Different Routing Methods Exist

- **Static routing** is simple but doesn't handle failures well
- **Dynamic routing** automatically adapts to network changes but adds overhead
- **OSPF** is ideal for internal networks (within an organization)
- **BGP** is used between different organizations (internet routing)

---


---

**End of Week 04 Portfolio**
