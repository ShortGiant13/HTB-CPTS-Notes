For large networks or low bandwidth, adjust scan speed and efficiency:

- **Timing template**: `-T0` (slow) to `-T5` (aggressive)
- **Parallelism**: `--min-parallelism <num>` (increase concurrent scans)
- **Timeouts**: `--max-rtt-timeout <time>` (adjust response wait time)
- **Packet rate**: `--min-rate <num>` (set min packets per second)
- **Retries**: `--max-retries <num>` (limit reattempts for unresponsive ports)

* * *

## Timeouts

* * *

Nmap starts with a 100ms RTT timeout (--min-rtt-timeout). Adjust for faster scans on large networks.

### Default Scan

`sudo nmap 10.129.2.0/24 -F`

* * *

### Optimized RTT

`sudo nmap 10.129.2.0/24 -F --initial-rtt-timeout 50ms --max-rtt-timeout 100ms`

| **Scanning Options** | **Description** |
| --- | --- |
| `10.129.2.0/24` | Scans the specified target network. |
| `-F` | Scans top 100 ports. |
| `--initial-rtt-timeout 50ms` | Sets the specified time value as initial RTT timeout. |
| `--max-rtt-timeout 100ms` | Sets the specified time value as maximum RTT timeout. |

**A shorter RTT timeout (--initial-rtt-timeout) speeds up scans but may miss some hosts. Balance speed and accuracy.**

* * *

## Max Retries

* * *

Reduce retry rate (--max-retries=0) to speed up scans by skipping unresponsive ports after the first attempt.

### Default Scan

`sudo nmap 10.129.2.0/24 -F | grep "/tcp" | wc -l`

* * *

### Reduced Retries

`sudo nmap 10.129.2.0/24 -F --max-retries 0 | grep "/tcp" | wc -l`

| **Scanning Options** | **Description** |
| --- | --- |
| `10.129.2.0/24` | Scans the specified target network. |
| `-F` | Scans top 100 ports. |
| `--max-retries 0` | Sets the number of retries that will be performed during the scan. |

* * *

## Rates

* * *

**Whitelisting**: Allows scanning without security restrictions.  
**Faster Scans**: Adjust packet rate based on network bandwidth.  
**Command**: --min-rate <number>sets the minimum packet sending rate.</number>

### Default Scan

`sudo nmap 10.129.2.0/24 -F -oN tnet.default`

* * *

### Optimized Scan

`sudo nmap 10.129.2.0/24 -F -oN tnet.minrate300 --min-rate 300`

| **Scanning Options** | **Description** |
| --- | --- |
| `10.129.2.0/24` | Scans the specified target network. |
| `-F` | Scans top 100 ports. |
| `-oN tnet.minrate300` | Saves the results in normal formats, starting the specified file name. |
| `--min-rate 300` | Sets the minimum number of packets to be sent per second. |

* * *

### Default Scan - Found Open Ports

`cat tnet.default | grep "/tcp" | wc -l`

* * *

### Optimized Scan - Found Open Ports

`cat tnet.minrate300 | grep "/tcp" | wc -l`

* * *

## Timing

* * *

Nmap offers six timing templates (-T &lt;0-5&gt;) to adjust scan aggressiveness. These templates balance speed and stealth, with more aggressive scans risking detection and being blocked by security systems. The default is -T 3 (normal).

**\-T 0 (paranoid):** Slow, stealthy.  
**\-T 1 (sneaky):** Slow, avoids detection.  
**\-T 2 (polite):** Balanced, reduces traffic.  
**\-T 3 (normal):** Default, good balance.  
**\-T 4 (aggressive):** Fast, more detectable.  
**\-T 5 (insane):** Very fast, high traffic, very detectable.

* * *

### Default Scan

`sudo nmap 10.129.2.0/24 -F -oN tnet.default`

* * *

### Insane Scan

`sudo nmap 10.129.2.0/24 -F -oN tnet.T5 -T 5`

| **Scanning Options** | **Description** |
| --- | --- |
| `10.129.2.0/24` | Scans the specified target network. |
| `-F` | Scans top 100 ports. |
| `-oN tnet.T5` | Saves the results in normal formats, starting the specified file name. |
| `-T 5` | Specifies the insane timing template. |

* * *

### Default Scan - Found Open Ports
`cat tnet.default | grep "/tcp" | wc -l`

* * *

### Insane Scan - Found Open Ports
`cat tnet.T5 | grep "/tcp" | wc -l`

* * *