# Host Discovery

For internal network pen testing, first, discover active systems using Nmap host discovery options. ICMP echo requests are the most effective method for host discovery. Always save scans for comparison, documentation, and reporting, as different tools may yield varying results.

**Scan Network Range**  
`sudo nmap <Target Network Range> -sn -oA tnet | grep for | cut -d" " -f5`

| Scanning Options | Description |
| --- | --- |
| 10.129.2.0/24 | Target network range. |
| \-sn | Disables port scanning. |
| \-oA tnet | Stores the results in all formats starting with the name 'tnet'. |

* * *

**Scan IP List**  
`sudo nmap -sn -oA tnet -iL hosts.lst | grep for | cut -d" " -f5`

| Scanning Options | Description |
| --- | --- |
| \-sn | Disables port scanning. |
| \-oA tnet | Stores the results in all formats starting with the name 'tnet'. |
| \-iL | Performs defined scans against targets in provided 'hosts.lst' list. |

* * *

### Scan Multiple IPs

`sudo nmap -sn -oA tnet 10.129.2.18 10.129.2.19 10.129.2.20| grep for | cut -d" " -f5`

***If next to each other***  
`sudo nmap -sn -oA tnet 10.129.2.18-20| grep for | cut -d" " -f5`

* * *

**Scan Single IP**  
`sudo nmap 10.129.2.18 -sn -oA host`

| Scanning Options | Description |
| --- | --- |
| 10.129.2.18 | Performs defined scans against the target. |
| \-sn | Disables port scanning. |
| \-oA | host Stores the results in all formats starting with the name 'host'. |

* * *

***When disabling the port scan (-sn), Nmap sends an ICMP Echo Request (-PE) to check if the host is alive. Previous scans may have used ARP pings instead. Use the --packet-trace option to confirm this, and ensure ICMP Echo Requests are sent by explicitly defining the -PE option.***

`sudo nmap 10.129.2.18 -sn -oA host -PE --packet-trace`

| Scanning Options | Description |
| --- | --- |
| 10.129.2.18 | Performs defined scans against the target. |
| \-sn | Disables port scanning. |
| \-oA | host Stores the results in all formats starting with the name 'host'. |
| \-PE | Performs the ping scan by using 'ICMP Echo requests' against the target. |
| \--packet-trace | Shows all packets sent and received |

* * *

***Use the --reason option in Nmap to understand why a target is marked as "alive."***

`sudo nmap 10.129.2.18 -sn -oA host -PE --reason`

| Scanning Options | Description |
| --- | --- |
| 10.129.2.18 | Performs defined scans against the target. |
| \-sn | Disables port scanning. |
| \-oA host | Stores the results in all formats starting with the name 'host'. |
| \-PE | Performs the ping scan by using 'ICMP Echo requests' against the target. |
| \--reason | Displays the reason for specific result. |

* * *

***To disable ARP requests and use ICMP Echo Requests instead, use the --disable-arp-ping option in Nmap. This forces Nmap to send ICMP Echo Requests, and you can observe the packets sent and received during the scan.***

`sudo nmap 10.129.2.18 -sn -oA host -PE --packet-trace --disable-arp-ping`

* * *