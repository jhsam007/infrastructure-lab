# Network Engineering Lab — Linux Network Administration and Troubleshooting

## 1. Project Overview

This project demonstrates practical networking and troubleshooting skills using Ubuntu 24.04 running under Windows Subsystem for Linux 2 (WSL2).

The lab covers network interface inspection, IPv4 addressing, subnetting, routing, DNS resolution, connectivity testing, TCP/UDP socket inspection, and Nginx virtual hosting.

**Environment:**
- Operating System: Ubuntu 24.04 LTS
- Platform: Windows Subsystem for Linux 2
- Network Interface: eth0
- Web Server: Nginx 1.24.0
- Tools: iproute2, ping, curl, getent, ss, resolvectl

## 2. Network Interface Inspection

Commands used:

```bash
ip -br addr
ip -br link
```

Identified interfaces:

| Interface | Address | Purpose |
|---|---|---|
| lo | 127.0.0.1/8 | Loopback |
| eth0 | 172.21.222.26/20 | Main WSL2 network interface |

The eth0 interface was operational and had an IPv4 address assigned.

## 3. IPv4 Addressing and Subnetting

Network configuration:

| Property | Value |
|---|---|
| IP Address | 172.21.222.26 |
| CIDR | /20 |
| Subnet Mask | 255.255.240.0 |
| Network Address | 172.21.208.0 |
| Broadcast Address | 172.21.223.255 |
| Usable Host Range | 172.21.208.1 – 172.21.223.254 |
| Default Gateway | 172.21.208.1 |

A /20 subnet contains 4,096 total IPv4 addresses, including the network and broadcast addresses.

## 4. Routing Inspection

Command:

```bash
ip route
```

Observed default route:

```text
default via 172.21.208.1 dev eth0
```

The routing table showed that traffic destined for external networks was forwarded through the default gateway.

Additional route inspection:

```bash
ip route get 20.27.177.113
```

The system selected:
- Gateway: 172.21.208.1
- Interface: eth0
- Source IP: 172.21.222.26

## 5. DNS Resolution

Commands:

```bash
resolvectl status
getent hosts github.com
```

The configured DNS server was 10.255.255.254.

The hostname github.com successfully resolved to an IPv4 address.

This demonstrated successful DNS name resolution.

## 6. Network Connectivity Testing

### Gateway connectivity

```bash
ping -c 4 172.21.208.1
```

Result: 100% packet loss.

The gateway did not respond to ICMP echo requests.

### External IP connectivity

```bash
ping -c 4 1.1.1.1
```

Result:
- Packets transmitted: 4
- Packets received: 4
- Packet loss: 0%
- Average round-trip time: 40.316 ms

### HTTPS connectivity

```bash
curl -I --max-time 10 https://github.com
```

Result: HTTP/2 200.

This confirmed successful outbound HTTPS connectivity.

### Troubleshooting conclusion

Although the gateway did not respond to ICMP echo requests, external ICMP connectivity, DNS resolution, and HTTPS connectivity were successful.

The gateway ping result alone was therefore insufficient to conclude that the network connection was unavailable.

## 7. TCP and UDP Socket Inspection

Commands:

```bash
ss -tuna
sudo ss -tulpn
```

Identified services included:

| Port | Protocol | Service |
|---|---|---|
| 80 | TCP | Nginx |
| 53 | TCP/UDP | DNS-related listeners |

Nginx was listening on:
- 0.0.0.0:80 (IPv4)
- [::]:80 (IPv6)

Process inspection confirmed that Nginx owned these listening sockets.

## 8. Firewall Inspection

Command:

```bash
sudo ufw status verbose
```

Result:

```text
Status: inactive
```

UFW was inactive during this lab.

No firewall configuration changes were made.

## 9. Nginx Virtual Hosting

Two websites were served by the same Nginx installation on port 80.

### Default website

```bash
curl -I http://localhost
```

Observed:
- HTTP status: 200 OK
- Document root: /var/www/html
- HTML title: Welcome to nginx!

### Custom website

```bash
curl -I http://infrastructure-lab.local
```

Observed:
- HTTP status: 200 OK
- Document root: /var/www/infrastructure-lab
- HTML title: Infrastructure Lab

The custom website was selected through the HTTP Host header and its corresponding Nginx server block.

This demonstrated basic Nginx name-based virtual hosting.

## 10. Troubleshooting Tools Practiced

| Tool | Purpose |
|---|---|
| ip | Inspect interfaces, addresses, and routing |
| ping | Test ICMP connectivity |
| getent | Resolve hostnames |
| resolvectl | Inspect DNS configuration |
| curl | Test HTTP and HTTPS services |
| ss | Inspect TCP and UDP sockets |
| ufw | Inspect host firewall status |

## 11. Skills Demonstrated

- Linux network interface inspection
- IPv4 addressing and subnet calculations
- Default gateway and routing-table analysis
- DNS troubleshooting
- ICMP and HTTPS connectivity testing
- TCP and UDP socket inspection
- Process-to-port identification
- Firewall status inspection
- Nginx virtual hosting
- Technical documentation

## 12. Environment Limitations

This lab was performed in a local WSL2 environment rather than on a physical enterprise network.

The network configuration, gateway, and DNS infrastructure are managed by the WSL2 virtual networking environment.

The results demonstrate practical foundational knowledge and hands-on exercises, not production network administration experience.

## Author

Jahid Hasan

GitHub: https://github.com/jhsam007