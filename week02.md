# Week 02 Portfolio - Mirza Abrar Ahmed Baig

## Student Information
- **Name:** Mirza Abrar Ahmed Baig
- **Student ID:** 12319236
- **Unit:** COIT20261
- **Date:** April 15, 2026

---

## Task 1: Setting Static IP Addresses

### Network Topology
- 4 x VPCS Linux Hosts (PC1, PC2, PC3, PC4)
- 1 x Ethernet Switch
- Network: 10.1.1.0/24

![Network Topology](Setting-IP-12319236-network.png)

### IP Configuration Results

| Host | IP Address | Method Used | Status |
|------|------------|-------------|--------|
| PC1 | 10.1.1.1/24 | VPCS `ip` command | ✅ |
| PC2 | 10.1.1.2/24 | VPCS `ip` command | ✅ |
| PC3 | 10.1.1.3/24 | VPCS `ip` command | ✅ |
| PC4 | 10.1.1.4/24 | VPCS `ip` command | ✅ |

### Verification Screenshots

#### PC1 (10.1.1.1)
![PC1 IP](Setting-IP-12319236-host1.png)

#### PC2 (10.1.1.2)
![PC2 IP](Setting-IP-12319236-host2.png)

#### PC3 (10.1.1.3)
![PC3 IP](Setting-IP-12319236-host3.png)

#### PC4 (10.1.1.4)
![PC4 IP](Setting-IP-12319236-host4.png)

### Commands Used

| Command | Purpose |
|---------|---------|
| `ip 10.1.1.x 255.255.255.0` | Set static IP address on VPCS node |
| `show ip` | Display current IP configuration |

### Configuration Commands Executed

**PC1:**
