# Week 03 Portfolio - Mirza Abrar Ahmed Baig

## Student Information
- **Name:** Mirza Abrar Ahmed Baig
- **Student ID:** 12319236
- **Unit:** COIT20261


---

## Task 1: Simple Application Communications with Netcat

### What is Netcat?
Netcat (nc) is a networking tool used to read and write data across network connections using TCP or UDP.

### Commands Used

**Server (Host A):**
nc -l -p 12346
- `-l` : Listen mode (act as server)
- `-p 12346` : Use port 12346 (not 12345 as required)

**Client (Host B):**
nc 10.1.1.1 12346
- Connects to server at IP 10.1.1.1 on port 12346

### Messages Exchanged
| Direction | Message |
|-----------|---------|
| Client → Server | Mirza Abrar Ahmed Baig |
| Server → Client | 12319236 |

### Screenshot
![Netcat Client-Server](Netcat-Basics-12319236-client-server.png)

### Key Learning
Netcat allows bidirectional communication between devices. Unlike ping (which only tests connectivity), netcat can send actual application data.

---

## Task 2: Capturing Packets

### Steps Performed
1. Right-clicked link between Host A and Switch
2. Selected "Start capture"
3. Generated traffic:
   - Ping from Host A to Host B: `ping -c 3 10.1.1.2`
   - Netcat message: Sent name from Host A to Host C
4. Stopped capture
5. Transferred .pcap file to host computer

### Capture File
[Download Capture File](Capture-Basics-12319236-ping-netcat.pcap)

### Key Learning
Packet capture records network traffic for analysis. Captured files (.pcap) can be opened in Wireshark for detailed inspection.

---

## Reflection

This week I learned:

1. **Netcat (nc):** A versatile tool for testing application-layer communication. Can act as client or server.

2. **Packet Capture:** Essential for network troubleshooting and security analysis. GNS3 makes it easy to capture traffic on any link.

3. **Difference from Ping:** Ping tests ICMP reachability. Netcat tests actual TCP/UDP application communication.

### Commands Summary
| Command | Purpose |
|---------|---------|
| `nc -l -p [port]` | Start netcat as server |
| `nc [IP] [port]` | Start netcat as client |
| `ping -c 3 [IP]` | Send 3 ping requests |

---
