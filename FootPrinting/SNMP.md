# SNMP (Simple Network Management Protocol) Overview

SNMP is a protocol used to manage and monitor network devices such as routers, switches, servers, and printers. It operates on the **Application Layer** and follows a **client-server model** with three key components:

- **Manager**: Queries and controls network devices.
- **Agent**: Resides on network devices and responds to requests from the manager.
- **MIB (Management Information Base)**: A structured database containing information about managed devices.

### Ports Used

- **UDP 161** → SNMP communication (default)
- **UDP 162** → SNMP Trap (alerts sent to managers)

&nbsp;

* * *

## MIB

* * *

**MIB (Management Information Base)** is a standardized format for storing device information in SNMP. It's an ASCII text file (written in ASN.1) that defines queryable SNMP objects in a hierarchical structure. Each entry includes an **OID**, unique address, name, type, access rights, and description. MIBs don’t store data but describe where to find specific information and its format.

&nbsp;

* * *

## OID

* * *

An OID identifies a node in a hierarchical namespace using a sequence of numbers in dot notation (e.g., `1.3.6.1.2.1`). Each number represents a branch in the tree structure, and longer sequences specify more detailed information. Some nodes are references to sub-nodes. OIDs can be looked up in the [**Object Identifier Registry**](https://www.alvestrand.no/objectid/) to find corresponding MIBs.

&nbsp;

* * *

## SNMPv1

* * *

SNMPv1 is the first version of the SNMP protocol used for network management and monitoring. It enables information retrieval, device configuration, and event notifications (traps). However, it lacks **authentication** and **encryption**, making data vulnerable to interception and modification.

&nbsp;

* * *

## SNMPv2

* * *

SNMPv2 exists in different versions, with **v2c** (community-based SNMP) being the widely used one today. While it offers improved features over SNMPv1, its security is still weak. The **community string**, which acts as a security measure, is transmitted in **plain text**, making it vulnerable to interception.

&nbsp;

* * *

## SNMPv3

* * *

SNMPv3 significantly enhances security with features like **authentication** (using a username and password) and **data encryption** (via a pre-shared key). However, this added security comes with increased complexity, requiring more detailed configuration compared to SNMPv2c.

&nbsp;

* * *

## Community Strings

* * *

Community strings act as passwords in SNMP to control access to device information. Despite SNMPv3 offering improved security, many organizations still rely on SNMPv2 due to its simpler setup. This continued use poses risks because:

- **Lack of Encryption:** Community strings are sent in plain text, making them vulnerable to interception.
- **Mismanagement Risks:** Administrators may overlook how attackers can exploit this, further increasing security concerns.

&nbsp;

* * *

## Default Configuration

* * *

- **Community Strings:**
    - **`public`** → Read-only access (common default)
    - **`private`** → Read-write access (dangerous if unchanged)
- **SNMPv1 and SNMPv2c** → Use plain-text community strings (insecure).
- **SNMPv3** → Offers encryption, integrity, and authentication (recommended for security).
- **Default MIB Files:** Often included in networking devices, providing detailed system info.

&nbsp;

**SNMP**:  [All the settings](https://www.net-snmp.org/docs/man/snmpd.conf.html)

### SNMP Daemon Config

`cat /etc/snmp/snmpd.conf | grep -v "#" | sed -r '/^\s*$/d'`

&nbsp;

* * *

## Dangerous Settings

* * *

Some dangerous settings that the administrator can make with SNMP are:

| **Settings** | **Description** |
| --- | --- |
| `rwuser noauth` | Provides access to the full OID tree without authentication. |
| `rwcommunity <community string> <IPv4 address>` | Provides access to the full OID tree regardless of where the requests were sent from. |
| `rwcommunity6 <community string> <IPv6 address>` | Same access as with `rwcommunity` with the difference of using IPv6. |

* * *

## Footprinting the Service

* * *

### SNMPwalk

`snmpwalk -v2c -c public 10.129.14.128`

* * *

### Bruteforcing community strings of the SNMP service

`onesixtyone -c /opt/useful/seclists/Discovery/SNMP/snmp.txt 10.129.14.128`

* * *

### Bruteforcing Individual OIDs and Enumerate

`braa <community string>@<IP>:.1.3.6.* # Syntax`

`braa public@10.129.14.128:.1.3.6.*`

* * *

&nbsp;

&nbsp;