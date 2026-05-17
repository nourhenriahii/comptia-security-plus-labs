# 🔍 Network Reconnaissance & Security Tools

> A practical guide covering essential network discovery, scanning, and analysis tools used in cybersecurity.

---

## 📋 Table of Contents

- [5.1 - ipconfig, ping, and arp](#51---ipconfig-ping-and-arp)
- [5.2 - route and traceroute](#52---route-and-traceroute)
- [5.3 - IP Scanners and Nmap](#53---ip-scanners-and-nmap)
- [5.4 - Service Discovery and Nmap](#54---service-discovery-and-nmap)
- [5.5 - netstat and nslookup](#55---netstat-and-nslookup)
- [5.6 - Packet Capture with Tcpdump and Wireshark](#56---packet-capture-with-tcpdump-and-wireshark)
- [5.7 - Packet Injection and Replay](#57---packet-injection-and-replay)

---

## 5.1 - ipconfig, ping, and arp

Network reconnaissance involves scanning for active hosts, IP ranges, and routing paths to understand the structure of a target network. This helps build an asset inventory and detect unauthorized devices.

### Tools Overview

| Tool | Platform | Description |
|------|----------|-------------|
| `ipconfig` | Windows | Displays IP config, MAC address, gateway, DHCP info |
| `ifconfig` | macOS / Linux | Displays network interface configurations |
| `ping` | All | Tests host reachability via ICMP Echo Requests |
| `arp` | All | Shows ARP cache mapping IPs to MAC addresses |

### Ping Sweep (macOS / Linux)

```bash
for i in $(seq 1 255); do ping -c 1 -W 1 10.1.0.$i | grep "64 bytes" & done
```

> ⚠️ **Note:** Not all devices respond to ICMP — a ping sweep may miss some active hosts.

### Network Interface Info (macOS)

```bash
ifconfig          # Show all interfaces
ifconfig en0      # Show specific interface (en0 = Wi-Fi on Mac)
```

### ARP Cache

```bash
arp -a
```

> 💡 If the MAC address of your default gateway differs from the expected router MAC, it could indicate a **MITM attack**.

---

## 5.2 - route and traceroute

### route

Displays and manages the local routing table.

```bash
netstat -rn          # macOS
ip route show        # Linux (modern)
```

A typical routing table on an endpoint has:
- A **default route** (`0.0.0.0/0`) pointing to the gateway
- A **local subnet route** for internal traffic

> ⚠️ Unexpected entries in the routing table may indicate suspicious activity.

### traceroute

Performs path discovery and shows hop-by-hop RTT.

```bash
traceroute google.com        # macOS / Linux
```

| Tool | Platform | Protocol |
|------|----------|----------|
| `tracert` | Windows | ICMP |
| `traceroute` | macOS / Linux | UDP (default) |
| `mtr` | macOS / Linux | Real-time analysis (`brew install mtr`) |

> 🔒 Unexpected delays at the default gateway may indicate a **MITM attack**.

---

## 5.3 - IP Scanners and Nmap

[Nmap](https://nmap.org) is one of the most powerful open-source network scanners, available for macOS, Linux, and Windows.

### Install on macOS

```bash
brew install nmap
```

### Basic Scan

```bash
nmap 192.168.1.0/24
```

By default, Nmap:
- Sends ICMP Echo + TCP ACK probes to ports 80 and 443
- Performs ARP sweeps on local networks
- Scans the top 1,000 TCP ports

### Host Discovery Only (no port scan)

```bash
nmap -sn 192.168.1.0/24
```

---

## 5.4 - Service Discovery and Nmap

### Scan Types

| Switch | Scan Type | Description |
|--------|-----------|-------------|
| `-sS` | TCP SYN (Half-open) | Fast and stealthy — no full handshake |
| `-sU` | UDP Scan | Slower; scans UDP ports |
| `-p 1-65535` | Full port range | Scan all ports |
| `-O` | OS Detection | Identify the operating system |
| `-A` | Aggressive | OS + version + scripts + traceroute |

### Examples

```bash
# Scan all ports
nmap -p 1-65535 192.168.1.10

# Full aggressive scan
nmap -A 192.168.1.10
```

---

## 5.5 - netstat and nslookup

### netstat – Network Statistics

```bash
netstat -an                        # macOS — show all connections
netstat -an | grep "192.168"       # Filter by IP
netstat -rn                        # Show routing table on macOS
```

Use `netstat` to:
- Detect unauthorized services
- Identify suspicious connections
- Find which process is listening on each port

### nslookup / dig – DNS Query Tools

```bash
nslookup example.com                        # macOS / Linux
dig example.com                             # macOS / Linux
dig AXFR @ns1.example.com example.com       # Test for zone transfer vulnerability
```

> ⚠️ A misconfigured DNS server allowing zone transfers exposes all hostnames and IPs in the domain.

---

## 5.6 - Packet Capture with Tcpdump and Wireshark

### Capture Methods

| Method | Description |
|--------|-------------|
| SPAN / Mirror Port | Switch forwards copies of traffic to a designated port |
| TAP (Test Access Port) | Hardware device inserted into cabling to mirror traffic |

---

### Tcpdump (macOS / Linux)

```bash
# Capture on interface (en0 = Wi-Fi on Mac)
sudo tcpdump -i en0

# Save to file
sudo tcpdump -i en0 -w capture.pcap

# Read from file
tcpdump -r capture.pcap

# Filter: src IP and port 53 or 80
sudo tcpdump -i en0 "src host 10.1.0.100 and (dst port 53 or dst port 80)"
```

#### Filter Reference

| Filter | Example |
|--------|---------|
| By host | `host 192.168.1.1` |
| By source | `src host 10.0.0.1` |
| By port | `port 80` |
| By protocol | `tcp`, `udp`, `icmp` |
| Combine | `and`, `or`, `not` |

---

### Wireshark (GUI)

Download from [wireshark.org](https://www.wireshark.org) — available for macOS.

#### Display Filter Examples

```
http                          # All HTTP traffic
dns                           # DNS queries
ip.src == 192.168.1.100       # From specific IP
ip.dst == 192.168.1.1         # To specific IP
ssh                           # SSH connections
```

#### Saving Captures

- **Full capture:** `File > Save As > .pcap / .pcapng`
- **TCP Stream:** Right-click → Follow → TCP Stream → Save As

---

## 5.7 - Packet Injection and Replay

### hping3

Custom packet crafting tool for firewall and IDS testing.

```bash
# Install on macOS
brew install hping

# SYN flood simulation
sudo hping3 -S --flood -V -p 80 192.168.1.100
```

Use cases: host/port discovery, firewall testing, DoS simulations, traceroute with custom protocols.

---

### tcpreplay

Replays captured `.pcap` files through a network interface.

```bash
# Install on macOS
brew install tcpreplay

tcpreplay --intf=en0 capture.pcap
```

---

### Netcat (nc)

Built-in on macOS — no installation needed.

```bash
# Banner grabbing
echo "HEAD / HTTP/1.0" | nc 10.1.0.1 80

# Listen for incoming connection
nc -l 6666

# Connect to a listener
nc 10.1.0.1 6666

# File transfer — send
nc 10.1.0.192 6666 < accounts.sql

# File transfer — receive
nc -l 6666 > accounts.sql
```

---

## 🛠️ Attack Examples (Lab Use Only)

> ⚠️ **These examples are for educational purposes in controlled lab environments only.**

### ARP Spoofing with Ettercap

```bash
# Install on macOS
brew install ettercap

sudo ettercap -G
```

1. Select target IP and gateway
2. Launch ARP spoofing attack
3. Monitor with Wireshark to capture intercepted traffic

---

## 📝 Notes

- On macOS, the default network interface is usually `en0` (Wi-Fi) or `en1` (Ethernet) — use `ifconfig` to check yours.
- Most tools can be installed via [Homebrew](https://brew.sh): `brew install <tool>`
- Wireshark uses **PCAPNG** format by default; fully compatible with tcpdump.
- On modern Linux, `ifconfig`, `arp`, and `route` are deprecated — replaced by `ip` and `ss` from the `iproute2` suite.