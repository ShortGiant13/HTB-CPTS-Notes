# Host and Port Scanning

Understanding how scanning tools work is crucial for interpreting results. After confirming a target is alive, we aim to gather details like **open ports**, **services**, **service versions**, and **the operating system**.

**Ports can have 6 possible states:**

| State | Description |
| --- | --- |
| open | This indicates that the connection to the scanned port has been established. These connections can be TCP connections, UDP datagrams as well as SCTP associations. |
| closed | When the port is shown as closed, the TCP protocol indicates that the packet we received back contains an RST flag. This scanning method can also be used to determine if our target is alive or not. |
| filtered | Nmap cannot correctly identify whether the scanned port is open or closed because either no response is returned from the target for the port or we get an error code from the target. |
| unfiltered | This state of a port only occurs during the TCP-ACK scan and means that the port is accessible, but it cannot be determined whether it is open or closed. |
| open\|filtered | If we do not get a response for a specific port, Nmap will set it to that state. This indicates that a firewall or packet filter may protect the port. |
| closed\|filtered | This state only occurs in the IP ID idle scans and indicates that it was impossible to determine if the scanned port is closed or filtered by a firewall. |

* * *

## Discovering Open TCP Ports

* * *

By default, Nmap scans the top 1000 TCP ports using the SYN scan (-sS), which requires root privileges to send raw TCP packets. If not run as root, the TCP scan (-sT) is used instead. You can specify ports manually (-p 22,25,80,139,445), by range (-p 22-445), top ports (--top-ports=10), all ports (-p-), or perform a fast scan of the top 100 ports (-F).

**Scanning Top 10 TCP Ports**  
`sudo nmap 10.129.2.28 --top-ports=10`

| Scanning Options | Description |
| --- | --- |
| 10.129.2.28 | Scans the specified target. |
| \--top-ports=10 | Scans the specified top ports that have been defined as most frequent. |

* * *

**Nmap - Trace the Packets**  
`sudo nmap 10.129.2.28 -p 21 --packet-trace -Pn -n --disable-arp-ping`

| Scanning Options | Description |
| --- | --- |
| 10.129.2.28 | Scans the specified target. |
| \-p 21 | Scans only the specified port. |
| \--packet-trace | Shows all packets sent and received. |
| \-n | Disables DNS resolution. |
| \--disable-arp-ping | Disables ARP ping. |

* * *

## Connect Scan

* * *

The Nmap TCP Connect Scan (-sT) completes the full TCP three-way handshake to determine if a port is open (SYN-ACK response) or closed (RST response). While highly accurate, it is not stealthy, as it creates logs and is easily detected by IDS/IPS systems. It can be useful for accurate network mapping, especially when the target's firewall drops incoming but allows outgoing packets. However, it's slower than other scans because it waits for a response from the target after each packet. For stealthier scans, the SYN Scan (-sS) is preferred, as it doesn't complete the handshake, making it less likely to trigger logs.

**Connect Scan on TCP Port 443**  
`sudo nmap 10.129.2.28 -p 443 --packet-trace --disable-arp-ping -Pn -n --reason -sT`

* * *

**Filtered Ports**  
When a port is marked as filtered, it usually means a firewall is dropping or rejecting packets. If a packet is dropped, Nmap receives no response and will retry up to 10 times by default (--max-retries=10). For example, scanning TCP port 139, which is filtered, while disabling ICMP echo requests (-Pn), DNS resolution (-n), and ARP ping scan (--disable-arp-ping) helps track how the packets are handled by the firewall.

`sudo nmap 10.129.2.28 -p 139 --packet-trace -n --disable-arp-ping -Pn`

| Scanning Options | Description |
| --- | --- |
| 10.129.2.28 | Scans the specified target. |
| \-p 139 | Scans only the specified port. |
| \--packet-trace | Shows all packets sent and received. |
| \-n | Disables DNS resolution. |
| \--disable-arp-ping | Disables ARP ping. |
| \-Pn | Disables ICMP Echo requests. |

* * *

In the last scan, Nmap sent two TCP packets with the SYN flag, and the scan duration (2.06s) was significantly longer than previous ones (~0.05s), indicating a delay likely caused by dropped packets. If the firewall rejects packets instead, as seen with TCP port 445, the response would be quicker, but the packets would be actively rejected by the firewall rather than simply dropped.  
`sudo nmap 10.129.2.28 -p 445 --packet-trace -n --disable-arp-ping -Pn`

| Scanning Options | Description |
| --- | --- |
| 10.129.2.28 | Scans the specified target. |
| \-p 445 | Scans only the specified port. |
| \--packet-trace | Shows all packets sent and received. |
| \-n | Disables DNS resolution. |
| \--disable-arp-ping | Disables ARP ping. |
| \-Pn | Disables ICMP Echo requests. |

* * *

## Discovering Open UDP Ports

* * *

Some administrators forget to filter UDP ports, leaving them exposed. Since UDP is stateless and lacks a three-way handshake, it doesn't send acknowledgments, leading to longer timeouts and making UDP scans (-sU) much slower than TCP scans (-sS).

* * *

### UDP Port Scan

`sudo nmap 10.129.2.28 -F -sU`

| **Scanning Options** | **Description** |
| --- | --- |
| `10.129.2.28` | Scans the specified target. |
| `-F` | Scans top 100 ports. |
| `-sU` | Performs a UDP scan. |

A major drawback of **UDP scanning (`-sU`)** is the lack of responses. Nmap sends **empty datagrams**, but if the **UDP port is open**, it only responds if the application is configured to do so. This makes it hard to confirm if the packet reached the target.

`sudo nmap 10.129.2.28 -sU -Pn -n --disable-arp-ping --packet-trace -p 137 --reason`

| **Scanning Options** | **Description** |
| --- | --- |
| `10.129.2.28` | Scans the specified target. |
| `-sU` | Performs a UDP scan. |
| `-Pn` | Disables ICMP Echo requests. |
| `-n` | Disables DNS resolution. |
| `--disable-arp-ping` | Disables ARP ping. |
| `--packet-trace` | Shows all packets sent and received. |
| `-p 137` | Scans only the specified port. |
| `--reason` | Displays the reason a port is in a particular state. |

* * *

#### Version Scan

`sudo nmap 10.129.2.28 -Pn -n --disable-arp-ping --packet-trace -p 445 --reason -sV`

| **Scanning Options** | **Description** |
| --- | --- |
| `10.129.2.28` | Scans the specified target. |
| `-Pn` | Disables ICMP Echo requests. |
| `-n` | Disables DNS resolution. |
| `--disable-arp-ping` | Disables ARP ping. |
| `--packet-trace` | Shows all packets sent and received. |
| `-p 445` | Scans only the specified port. |
| `--reason` | Displays the reason a port is in a particular state. |
| `-sV` | Performs a service scan. |

&nbsp;

&nbsp;