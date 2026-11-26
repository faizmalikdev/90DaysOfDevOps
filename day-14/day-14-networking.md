
# Day 14 – Networking Fundamentals & Hands-on Checks

## Objective

Learn basic networking concepts and practise essential Linux networking commands used in DevOps troubleshooting.

## 1. OSI vs TCP/IP Model

### OSI Model

| Layer | Name | Purpose |
|---|---|---|
| 7 | Application | Network services like HTTP, HTTPS and DNS |
| 6 | Presentation | Data formatting and encryption |
| 5 | Session | Manages communication sessions |
| 4 | Transport | Communication using TCP and UDP |
| 3 | Network | IP addressing and routing |
| 2 | Data Link | Local network communication using MAC addresses |
| 1 | Physical | Transfers bits through cables or wireless signals |

### TCP/IP Model

| TCP/IP Layer | Related OSI Layers | Examples |
|---|---|---|
| Application | L5, L6, L7 | HTTP, HTTPS, DNS |
| Transport | L4 | TCP, UDP |
| Internet | L3 | IP |
| Link | L1, L2 | Ethernet, Wi-Fi |

### Important Concepts

- IP works at the Internet/Network layer.
- TCP and UDP work at the Transport layer.
- HTTP, HTTPS and DNS work at the Application layer.
- DNS translates domain names into IP addresses.

**Real Example:**

`curl https://example.com`

The command sends an HTTP request over TLS, using TCP and IP for network communication. DNS resolves the domain name into an IP address.

## 2. Hands-on Networking Commands

### Task 1: Check IP Address

**Command:**

```bash
hostname -I
```

**Purpose:** Displays the IP addresses assigned to the machine.

**Additional command:**

```bash
ip addr show
```

**Observation:** These commands help identify network interfaces and their assigned IP addresses.

### Task 2: Check Network Reachability

**Command:**

```bash
ping -c 4 google.com
```

**Actual Output:**

```text
PING google.com (192.178.177.138) 56(84) bytes of data.
64 bytes from lcdelw-in-f138.1e100.net (192.178.177.138): icmp_seq=1 ttl=108 time=120 ms
64 bytes from lcdelw-in-f138.1e100.net (192.178.177.138): icmp_seq=2 ttl=108 time=83.9 ms
64 bytes from lcdelw-in-f138.1e100.net (192.178.177.138): icmp_seq=3 ttl=108 time=99.3 ms
64 bytes from lcdelw-in-f138.1e100.net (192.178.177.138): icmp_seq=4 ttl=108 time=79.4 ms

4 packets transmitted, 4 received, 0% packet loss
rtt min/avg/max/mdev = 79.433/95.556/119.632/15.737 ms
```

**Observation:**

Google was reachable from my machine. All four packets received replies, resulting in 0% packet loss.

Average latency was approximately 95.56 ms.

### Task 3: Trace Network Path

**Command:**

```bash
traceroute google.com
```

**Observation:**

- The first hop responded at `172.29.112.1`.
- The second hop responded at `172.20.10.1`.
- Later hops showed `* * *`, meaning no response was received from those hops.
- Some routers may block or ignore traceroute probes.

**Learning:** Traceroute helps identify the network path and possible points where responses stop.

### Task 4: Check Listening Ports

**Command:**

```bash
sudo ss -tulpn
```

**Purpose:** Displays listening TCP/UDP sockets and associated processes.

**Important findings:**

| Service | Protocol | Port | Purpose |
|---|---|---|---|
| systemd-resolve | TCP/UDP | 53 | DNS resolution |
| chronyd | UDP | 323 | Time synchronization |

**Observation:**

My machine has DNS services listening on port 53 and Chrony listening on UDP port 323. SSH on port 22 was not visible in the output.

### Task 5: Port Probe and Interpret

**Command:**

```bash
nc -zv 127.0.0.53 53
```

**Actual Output:**

```text
Connection to 127.0.0.53 53 port [tcp/domain] succeeded!
```

**Observation:**

The TCP connection to port 53 was successful. This confirms that the local DNS resolver was accepting TCP connections on that port.

### Task 6: DNS Name Resolution

**Command:**

```bash
nslookup google.com
```

**Actual Output:**

```text
Server:         10.255.255.254
Address:        10.255.255.254#53

Non-authoritative answer:
Name:   google.com
Address: 142.250.118.139
Name:   google.com
Address: 142.250.118.102
Name:   google.com
Address: 142.250.118.101
Name:   google.com
Address: 142.250.118.138
Name:   google.com
Address: 142.250.118.100
Name:   google.com
Address: 142.250.118.113
```

The response also included IPv6 addresses.

**Observation:**

DNS resolution worked successfully. My DNS server was `10.255.255.254`, and Google resolved to multiple IPv4 and IPv6 addresses.

### Task 7: HTTP/HTTPS Connectivity

**Command:**

```bash
curl -I https://google.com
```

**Actual Output:**

```text
HTTP/2 301
location: https://www.google.com/
content-type: text/html; charset=UTF-8
server: gws
```

**Observation:**

Google returned HTTP status `301`, indicating a permanent redirect to `https://www.google.com/`.

This confirms that the HTTPS request received a valid HTTP response.

**Additional command:**

```bash
curl -IL https://google.com
```

This follows redirects and displays the final response headers.

### Task 8: Network Connections Snapshot

**Command:**

```bash
netstat -an | head
```

**Actual Output:**

```text
Active Internet connections (servers and established)
Proto Recv-Q Send-Q Local Address           Foreign Address         State
tcp        0      0 10.255.255.254:53       0.0.0.0:*               LISTEN
tcp        0      0 127.0.0.53:53           0.0.0.0:*               LISTEN
tcp        0      0 127.0.0.54:53           0.0.0.0:*               LISTEN
udp        0      0 127.0.0.54:53           0.0.0.0:*
udp        0      0 127.0.0.53:53           0.0.0.0:*
udp        0      0 10.255.255.254:53      0.0.0.0:*
udp        0      0 127.0.0.1:323          0.0.0.0:*
```

**Observation:**

The output showed TCP services in the LISTEN state and UDP sockets. No ESTABLISHED TCP connection was visible in the displayed lines.

## 3. Reflection

**Q1. Which command gives the fastest signal when something is broken?**

`ping` is a quick first check for network reachability. For checking an HTTP application, `curl` is more useful.

**Q2. Which layer would you inspect if DNS fails or HTTP 500 appears?**

- DNS failure: Check the Application layer, DNS resolver and network connectivity.
- HTTP 500: Check the Application layer, web server logs and backend application.

**Q3. What two follow-up checks would you run during a real incident?**

1. Check service status:

```bash
systemctl status nginx
```

2. Check service logs:

```bash
journalctl -u nginx -n 50
```

## 4. Important Commands Summary

| Command | Purpose |
|---|---|
| `hostname -I` | Display IP addresses |
| `ip addr show` | Show network interfaces |
| `ping -c 4 google.com` | Check network reachability |
| `traceroute google.com` | Trace network path |
| `ss -tulpn` | Display listening ports |
| `nc -zv localhost 53` | Test TCP port connectivity |
| `nslookup google.com` | Resolve domain names |
| `curl -I https://google.com` | Check HTTP response headers |
| `netstat -an` | Display network connections |
| `ip route` | Display routing table |

## 5. What I Learned

1. Understood the OSI and TCP/IP networking models.
2. Learned how IP, TCP, UDP, DNS and HTTP work together.
3. Practised checking network connectivity using ping and traceroute.
4. Learned how to inspect listening ports using `ss`.
5. Tested TCP port connectivity using Netcat.
6. Learned DNS resolution using `nslookup`.
7. Checked HTTP responses using curl.
8. Learned how to inspect network connections using netstat.

## 6. Day 14 Status

**Status:** Practical checks completed; IP address observation and connection-state counting can be added if required.

**Main Learning:** Networking commands are essential for troubleshooting connectivity, DNS, ports and application availability in DevOps.
