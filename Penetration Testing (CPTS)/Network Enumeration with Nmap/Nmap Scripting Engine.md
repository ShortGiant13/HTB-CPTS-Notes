The **Nmap Scripting Engine (NSE)** allows interaction with services using **Lua scripts**. Scripts are categorized into 14 types:

| **Category** | **Description** |
| --- | --- |
| `auth` | Determination of authentication credentials. |
| `broadcast` | Scripts, which are used for host discovery by broadcasting and the discovered hosts, can be automatically added to the remaining scans. |
| `brute` | Executes scripts that try to log in to the respective service by brute-forcing with credentials. |
| `default` | Default scripts executed by using the `-sC` option. |
| `discovery` | Evaluation of accessible services. |
| `dos` | These scripts are used to check services for denial of service vulnerabilities and are used less as it harms the services. |
| `exploit` | This category of scripts tries to exploit known vulnerabilities for the scanned port. |
| `external` | Scripts that use external services for further processing. |
| `fuzzer` | This uses scripts to identify vulnerabilities and unexpected packet handling by sending different fields, which can take much time. |
| `intrusive` | Intrusive scripts that could negatively affect the target system. |
| `malware` | Checks if some malware infects the target system. |
| `safe` | Defensive scripts that do not perform intrusive and destructive access. |
| `version` | Extension for service detection. |
| `vuln` | Identification of specific vulnerabilities. |

* * *

### Default Scripts

`sudo nmap <target> -sC`

* * *

### Specific Scripts Category

`sudo nmap <target> --script <category>`

* * *

### Defined Scripts

`sudo nmap <target> --script <script-name>,<script-name>,...`

* * *

### Nmap - Specifying Scripts

`sudo nmap 10.129.2.28 -p 25 --script banner,smtp-commands`

| **Scanning Options** | **Description** |
| --- | --- |
| `10.129.2.28` | Scans the specified target. |
| `-p 25` | Scans only the specified port. |
| `--script banner,smtp-commands` | Uses specified NSE scripts. |

* * *

### Nmap - Aggressive Scan

`sudo nmap 10.129.2.28 -p 80 -A`

| **Scanning Options** | **Description** |
| --- | --- |
| `10.129.2.28` | Scans the specified target. |
| `-p 80` | Scans only the specified port. |
| `-A` | Performs service detection, OS detection, traceroute and uses defaults scripts to scan the target. |

* * *

## Vulnerability Assessment

* * *

### Nmap - Vuln Category
`sudo nmap 10.129.2.28 -p 80 -sV --script vuln`

| **Scanning Options** | **Description** |
| --- | --- |
| `10.129.2.28` | Scans the specified target. |
| `-p 80` | Scans only the specified port. |
| `-sV` | Performs service version detection on specified ports. |
| `--script vuln` | Uses all related scripts from specified category. |