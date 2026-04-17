# Week 06 Portfolio - Mirza Abrar Ahmed Baig

## Student Information
- **Name:** Mirza Abrar Ahmed Baig
- **Student ID:** 12319236
- **Unit:** COIT20261

---

## Task 1: Resolving IP Addresses to Hardware Addresses (ARP)

### Network Setup
- Used Week 2 project: Setting-IP-12319236
- 4 Linux hosts (PC1, PC2, PC3, PC4) connected via Ethernet switch
- All hosts on same subnet: 10.1.1.0/24

### IP Address Configuration

| Host | IP Address |
|------|------------|
| PC1 (Host A) | 10.1.1.1 |
| PC2 (Host B) | 10.1.1.2 |
| PC3 (Host C) | 10.1.1.3 |
| PC4 (Host D) | 10.1.1.4 |

### ARP Table Before Communication

**Command:**
```bash
ip neigh show
Initial ARP Table (PC1):

(empty - no entries)
Explanation: Initially, PC1 has no ARP entries because it hasn't communicated with any other devices.

After Ping from PC1 to PC2
Command executed on PC1:

ping 10.1.1.2

ARP Table (PC1) after ping:

10.1.1.2 dev eth0 lladdr 00:50:79:66:68:01 REACHABLE

Understanding ARP Table Entry:

Field	Value	Meaning
IP Address	10.1.1.2	IP of destination host
Device	eth0	Interface used
lladdr	00:50:79:66:68:01	Hardware (MAC) address
State	REACHABLE	Confirmed reachable
After Ping from PC3 to PC1
Command executed on PC3:

bash
ping 10.1.1.1
ARP Table (PC1) after PC3 pinged PC1:

text
10.1.1.2 dev eth0 lladdr 00:50:79:66:68:01 REACHABLE
10.1.1.3 dev eth0 lladdr 00:50:79:66:68:02 STALE
ARP Table Evolution Summary
Stage	PC1 ARP Table Contents
Initial	Empty
After ping to PC2	PC2 (10.1.1.2) - REACHABLE
After PC3 pinged PC1	PC2 (REACHABLE), PC3 (STALE)
ARP Table Screenshots
https://ARP-Basics-12319236-HostA-Table.png

Key ARP Concepts
Concept	Explanation
ARP	Address Resolution Protocol - maps IP addresses to MAC addresses
REACHABLE	Confirmed the device is responding
STALE	Entry hasn't been used recently, needs verification
lladdr	Link-layer address (MAC address)
Why ARP Matters
Ethernet communication requires MAC addresses, not IP addresses

ARP dynamically discovers MAC addresses of devices on same network

ARP table caches mappings to reduce network traffic

Task 2: Default Gateways
Network Topology
text
Subnet 1 (10.1.1.0/24)        Subnet 2 (10.1.2.0/24)
    [Switch1]                      [Switch2]
       |                              |
    PC1, PC2                        PC3, PC4
       |                              |
    Router1 ----------------------- Router2
                     10.1.3.0/24
Device Configuration

Subnet 1: 10.1.1.0/24
Device	Interface	IP Address	Gateway	Forwarding
PC1	eth0	10.1.1.2/24	10.1.1.1	Disabled (0)
PC2	eth0	10.1.1.3/24	10.1.1.1	Disabled (0)
Router1	eth0	10.1.1.1/24	N/A	Enabled (1)
Router1	eth1	10.1.3.1/24	N/A	Enabled (1)
Subnet 2: 10.1.2.0/24
Device	Interface	IP Address	Gateway	Forwarding
PC3	eth0	10.1.2.2/24	10.1.2.1	Disabled (0)
PC4	eth0	10.1.2.3/24	10.1.2.1	Disabled (0)
Router2	eth0	10.1.2.1/24	N/A	Enabled (1)
Router2	eth1	10.1.3.2/24	N/A	Enabled (1)
Configuration Commands
/etc/network/interfaces file syntax:
bash
auto eth0
iface eth0 inet static
    address <ipaddress>
    netmask <networkmask>
    gateway <routerip>
    up sysctl net.ipv4.ip_forward=<0or1>
Router1 Configuration:
bash
# eth0 (connected to Switch1)
auto eth0
iface eth0 inet static
    address 10.1.1.1
    netmask 255.255.255.0
    up sysctl net.ipv4.ip_forward=1

# eth1 (connected to Router2)
auto eth1
iface eth1 inet static
    address 10.1.3.1
    netmask 255.255.255.0
Router2 Configuration:
bash
# eth0 (connected to Switch2)
auto eth0
iface eth0 inet static
    address 10.1.2.1
    netmask 255.255.255.0
    up sysctl net.ipv4.ip_forward=1

# eth1 (connected to Router1)
auto eth1
iface eth1 inet static
    address 10.1.3.2
    netmask 255.255.255.0
PC1 Configuration (Host):
bash
auto eth0
iface eth0 inet static
    address 10.1.1.2
    netmask 255.255.255.0
    gateway 10.1.1.1
    up sysctl net.ipv4.ip_forward=0
Routing Tables
PC1 Routing Table:
text
default via 10.1.1.1 dev eth0
10.1.1.0/24 dev eth0 proto kernel scope link src 10.1.1.2
Router1 Routing Table:
text
10.1.1.0/24 dev eth0 proto kernel scope link src 10.1.1.1
10.1.3.0/24 dev eth1 proto kernel scope link src 10.1.3.1
Router2 Routing Table:
text
10.1.2.0/24 dev eth0 proto kernel scope link src 10.1.2.1
10.1.3.0/24 dev eth1 proto kernel scope link src 10.1.3.2
PC3 Routing Table:
text
default via 10.1.2.1 dev eth0
10.1.2.0/24 dev eth0 proto kernel scope link src 10.1.2.2
Manual Route Addition (if needed)
bash
# Add specific route on Router1 to reach Subnet 2
ip route add 10.1.2.0/24 via 10.1.3.2 dev eth1

# Add specific route on Router2 to reach Subnet 1
ip route add 10.1.1.0/24 via 10.1.3.1 dev eth1
Ping Test Results
From PC1 (10.1.1.2) to PC3 (10.1.2.2):

text
ping 10.1.2.2
64 bytes from 10.1.2.2: icmp_seq=1 ttl=62 time=1.45 ms
64 bytes from 10.1.2.2: icmp_seq=2 ttl=62 time=1.32 ms
64 bytes from 10.1.2.2: icmp_seq=3 ttl=62 time=1.38 ms
https://Default-Gateway-12319236-ping.png

Result:  Successful! Path: PC1 → Router1 → Router2 → PC3

TTL of 62 indicates 2 router hops (default Linux TTL is 64)

Network Topology Screenshot
https://Default-Gateway-12319236-network.png

Key Concepts Learned
ARP (Address Resolution Protocol)
Concept	Description
Purpose	Maps IP addresses to MAC addresses
How it works	Broadcast request asking "Who has this IP?"
ARP Table	Cache of IP-to-MAC mappings
REACHABLE state	Confirmed working mapping
STALE state	Entry expired, needs refresh
Default Gateway
Concept	Description
Default Gateway	Router that handles traffic for unknown destinations
When used	When destination is not on local subnet
Configuration	Set in /etc/network/interfaces with gateway line
Routing Between Subnets
Component	Function
Router	Forwards packets between different subnets
Routing Table	Tells router where to send packets
Default Route	Used when no specific route matches
Commands Summary
Command	Purpose
ip neigh show	View ARP table
ip route show	View routing table
ip route add default via X.X.X.X	Add default gateway
ip route add X.X.X.X/24 via Y.Y.Y.Y	Add specific route
sysctl net.ipv4.ip_forward	Check forwarding status
ARP Table States
State	Meaning
REACHABLE	Device confirmed reachable, entry valid
STALE	Entry expired, needs verification
DELAY	Waiting for confirmation
PROBE	Actively sending ARP requests
Reflection
This week I learned about ARP and default gateways:

ARP (Task 1)
Initial State: PC1's ARP table was empty because no communication had occurred yet.

After Ping: When PC1 pinged PC2, an ARP entry appeared for PC2 with state REACHABLE. This shows ARP automatically discovers MAC addresses.

After External Ping: When PC3 pinged PC1, PC1 added PC3 to its ARP table with state STALE. This shows ARP works bidirectionally.

ARP Importance: Without ARP, devices couldn't communicate at Layer 2 even with correct IP addresses.

Default Gateways (Task 2)
Network Design: Created 3 subnets with 2 routers. Each subnet needed its own IP range.

Gateway Configuration: Hosts need a default gateway (router IP) to reach other subnets.

Forwarding: Routers need forwarding enabled (ip_forward=1). Hosts need it disabled.

Routing Tables: Each router needs to know how to reach both subnets.

Successful Communication: PC1 could ping PC3 across two routers, proving the routing worked.

Troubleshooting Insights
Problem	Likely Cause	Solution
ARP entry missing	No communication yet	Ping the device first
Cross-subnet ping fails	Missing default gateway	Add gateway to host config
Router not forwarding	ip_forward=0	Set ip_forward=1
Can't reach far subnet	Missing route	Add specific route
Why Two Tasks Together
Task 1 (ARP) shows how devices find each other on the same subnet. Task 2 (Default Gateways) shows how devices find each other across different subnets. Together, they explain how all network communication works.



Default-Gateway-12319236-ping.png - Ping test screenshot



