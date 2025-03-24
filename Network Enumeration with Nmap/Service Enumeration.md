# **Service Enumeration**

**Service Version Detection** helps identify applications and versions for vulnerability scanning. Run a **quick port scan** first (`-p-`) to reduce traffic, then use **version detection** (`-sV`) on detected ports. Press **\[Space Bar\]** to check scan progress.

`sudo nmap 10.129.2.28 -p- -sV`

| **Scanning Options** | **Description** |
| --- | --- |
| `10.129.2.28` | Scans the specified target. |
| `-p-` | Scans all ports. |
| `-sV` | Performs service version detection on specified ports. |

* * *

### Set Intervals

Use `--stats-every=5s` to display scan progress at set intervals (seconds or minutes).

`sudo nmap 10.129.2.28 -p- -sV --stats-every=5s`

| **Scanning Options** | **Description** |
| --- | --- |
| `10.129.2.28` | Scans the specified target. |
| `-p-` | Scans all ports. |
| `-sV` | Performs service version detection on specified ports. |
| `--stats-every=5s` | Shows the progress of the scan every 5 seconds. |

* * *

### Set Verbosity Level

Increase verbosity with `-v` or `-vv` to display open ports as Nmap detects them.

`sudo nmap 10.129.2.28 -p- -sV -v`

| **Scanning Options** | **Description** |
| --- | --- |
| `10.129.2.28` | Scans the specified target. |
| `-p-` | Scans all ports. |
| `-sV` | Performs service version detection on specified ports. |
| `-v` | Increases the verbosity of the scan, which displays more detailed information. |

* * *

### Banner Grabbing

After the scan, Nmap will display all active TCP ports, along with the corresponding services and their versions on the system.

`sudo nmap 10.129.2.28 -p- -sV`

| **Scanning Options** | **Description** |
| --- | --- |
| `10.129.2.28` | Scans the specified target. |
| `-p-` | Scans all ports. |
| `-sV` | Performs service version detection on specified ports. |

* * *

### Tcpdump

`sudo tcpdump -i eth0 host 10.10.14.2 and 10.129.2.28`

* * *

### Nc

`nc -nv 10.129.2.28 25`