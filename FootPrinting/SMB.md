# SMB (Server Message Block):

SMB is a **client-server protocol** used for accessing files, directories, and network resources like printers. It facilitates **network communication** for various services between systems, particularly in Windows environments. It can also be used cross-platform via **Samba** for Linux/Unix.

### Key Features:

- **Access Control**: SMB allows access to files and services based on **Access Control Lists (ACLs)**, specifying permissions like read, write, and execute for users or groups.
- **Connection Establishment**: Utilizes **TCP** with a **three-way handshake** to establish communication between client and server.
- **Compatibility**: Supports backward compatibility, allowing communication between newer and older Windows versions.

### Uses:

- **File Sharing**: An SMB server can share parts of its file system.
- **Resource Sharing**: SMB is also used for sharing network devices like printers.

* * *

## Samba

* * *

### Samba and CIFS:

- **Samba** is an implementation of the **SMB protocol** for **Unix-based systems**. It allows these systems to communicate with **Windows** systems, supporting file sharing and other services.
- **CIFS** (Common Internet File System) is a **dialect of SMB** and is associated with **SMB v1**, which is an older version of the protocol. CIFS is now generally considered outdated.

### Key Points:

- **Ports**:
    - SMB over **NetBIOS** uses TCP **137-139**.
    - **CIFS** uses TCP **port 445**.
- **Newer SMB Versions**:
    - **SMB v2** and **SMB v3** offer improved performance and security and are recommended in modern networks.
- **Outdated SMB v1 (CIFS)**:
    - Considered insecure and generally deprecated, but still used in some legacy environments.

* * *

### Default Configuration

Samba offers a wide range of [settings](https://www.samba.org/samba/docs/current/man-html/smb.conf.5.html) that we can configure.

`cat /etc/samba/smb.conf | grep -v "#\|\;"`

| **Setting** | **Description** |
| --- | --- |
| `[sharename]` | The name of the network share. |
| `workgroup = WORKGROUP/DOMAIN` | Workgroup that will appear when clients query. |
| `path = /path/here/` | The directory to which user is to be given access. |
| `server string = STRING` | The string that will show up when a connection is initiated. |
| `unix password sync = yes` | Synchronize the UNIX password with the SMB password? |
| `usershare allow guests = yes` | Allow non-authenticated users to access defined share? |
| `map to guest = bad user` | What to do when a user login request doesn't match a valid UNIX user? |
| `browseable = yes` | Should this share be shown in the list of available shares? |
| `guest ok = yes` | Allow connecting to the service without using a password? |
| `read only = yes` | Allow users to read files only? |
| `create mask = 0700` | What permissions need to be set for newly created files? |

* * *

### Dangerous Settings

| **Setting** | **Description** |
| --- | --- |
| `browseable = yes` | Allow listing available shares in the current share? |
| `read only = no` | Forbid the creation and modification of files? |
| `writable = yes` | Allow users to create and modify files? |
| `guest ok = yes` | Allow connecting to the service without using a password? |
| `enable privileges = yes` | Honor privileges assigned to specific SID? |
| `create mask = 0777` | What permissions must be assigned to the newly created files? |
| `directory mask = 0777` | What permissions must be assigned to the newly created directories? |
| `logon script = script.sh` | What script needs to be executed on the user's login? |
| `magic script = script.sh` | Which script should be executed when the script gets closed? |
| `magic output = script.out` | Where the output of the magic script needs to be stored? |

* * *

### Restart Samba

`sudo systemctl restart smbd`

* * *

### SMBclient - Connecting to the Share

`smbclient -N -L //10.129.14.128`

* * *

### Download Files from SMB

`smb: \> get prep-prod.txt`

* * *

### Samba Status

`smbstatus`

* * *

## Footprinting the Service

* * *

### Nmap

`sudo nmap 10.129.14.128 -sV -sC -p139,445`

* * *

### RPCclient

`rpcclient -U "" 10.129.14.128`

* * *

### RPCclient - Enumeration

`rpcclient $> srvinfo`  
`rpcclient $> enumdomains`  
`rpcclient $> querydominfo`  
`rpcclient $> netshareenumall`  
`rpcclient $> netsharegetinfo notes`

* * *

### Rpcclient - User Enumeration

`rpcclient $> enumdomusers`  
`rpcclient $> queryuser <0x3e9>`

* * *

### Rpcclient - Group Information

`rpcclient $> querygroup 0x201`

* * *

### Brute Forcing User RIDs

`for i in $(seq 500 1100);do rpcclient -N -U "" 10.129.14.128 -c "queryuser 0x$(printf '%x\n' $i)" | grep "User Name\|user_rid\|group_rid" && echo "";done`

* * *

### Impacket - Samrdump.py

`samrdump.py 10.129.14.128`

* * *

### SMBmap

`smbmap -H 10.129.14.128`

* * *

### CrackMapExec

`crackmapexec smb 10.129.14.128 --shares -u '' -p ''`

* * *

### Enum4Linux-ng - Enumeration

`./enum4linux-ng.py 10.129.14.128 -A`

* * *