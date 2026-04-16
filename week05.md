# Week 05 Portfolio - Mirza Abrar Ahmed Baig

## Student Information
- **Name:** Mirza Abrar Ahmed Baig
- **Student ID:** 12319236
- **Unit:** COIT20261

---

## Task 1: Setup VLANs on Switch

### Network Topology
- 4 x Linux Hosts (PC1, PC2, PC3, PC4)
- 1 x Ethernet Switch (unmanaged - used for basic connectivity)
- All hosts on same subnet: 10.1.1.0/24

![VLAN Basics Network](Vlan-Basics-12319236-network.png)

### IP Address Configuration

| Host | IP Address | Subnet Mask |
|------|------------|-------------|
| PC1 | 10.1.1.1 | 255.255.255.0 |
| PC2 | 10.1.1.2 | 255.255.255.0 |
| PC3 | 10.1.1.3 | 255.255.255.0 |
| PC4 | 10.1.1.4 | 255.255.255.0 |

### VLAN Theory
Connectivity Test Results
Source	Destination	Same VLAN?	Result
PC1	PC2	Yes (VLAN 236)	 Successful
PC3	PC4	Yes (VLAN 237)	 Successful
PC1	PC3	No	 No connectivity


**What are VLANs?**
- VLAN (Virtual Local Area Network) logically segments a physical switch into multiple isolated broadcast domains
- Devices in different VLANs cannot communicate directly without a router

**VLAN IDs Used (based on student ID 12319236):**
- Last 3 digits: 236
- VLAN 236: Ports eth1, eth2 (PC1, PC2)
- VLAN 237: Ports eth3, eth4 (PC3, PC4)

### Switch Configuration (Open vSwitch commands for reference)

```bash
# Assign access ports to VLANs
ovs-vsctl set port eth1 tag=236
ovs-vsctl set port eth2 tag=236
ovs-vsctl set port eth3 tag=237
ovs-vsctl set port eth4 tag=237

# View configuration
ovs-vsctl showv
Connectivity Test Results
Source	Destination	Same VLAN?	Result
PC1	PC2	Yes (VLAN 236)	 Successful
PC3	PC4	Yes (VLAN 237)	Successful
PC1	PC3	No	 No connectivity


