# Linux Remote Management Protocols Overview:

- In Linux environments, **remote server management** is essential for supporting systems across different geographical locations.
    
- These protocols allow administrators to **log in remotely** to servers and **manage or troubleshoot** them without being physically present.
    
- Remote access solutions **preserve the same working environment**, making remote administration **efficient and seamless**.
    

* * *

### Key Advantages

- **Eliminates need for physical access** to servers during troubleshooting.
    
- Saves time and resources by enabling **remote support** from anywhere.
    
- Helps maintain **system uptime and responsiveness** by allowing immediate remote interventions.
    

* * *

### Security Relevance for Pentesters

- These services are **commonly exposed** on public-facing servers.
    
- **Misconfigurations** or **default settings** can lead to **unauthorized access**.
    
- Understanding these protocols is critical for identifying potential **attack vectors** during penetration tests.
    

&nbsp;

* * *

# SSH (Secure Shell) – Overview

- SSH enables **encrypted, direct communication** between two computers over **insecure networks**, typically on **TCP port 22**.
    
- Prevents third-party interception by using **strong encryption** for both data and authentication.
    
- Supported across all major operating systems:
    
    - **Linux & macOS**: Native support.
        
    - **Windows**: Requires installation (e.g., OpenSSH, PuTTY).
        
- **Two protocol versions** exist:
    
    - **SSH-1**: Obsolete and vulnerable to **MITM attacks**.
        
    - **SSH-2**: Modern, secure, and widely adopted.
        

* * *

### Authentication Methods:

OpenSSH has six different authentication methods:

1.  Password authentication
2.  Public-key authentication
3.  Host-based authentication
4.  Keyboard authentication
5.  Challenge-response authentication
6.  GSSAPI authentication

* * *

### Public Key Authentication (Most Common)

- **Server sends a certificate** to the client for verification.
    
- Client authenticates using a **private key + passphrase**, while the **server stores the public key**.
    
- Connection establishment steps:
    
    1.  Server provides cryptographic challenge.
        
    2.  Client decrypts with private key.
        
    3.  Server verifies solution → connection granted.
        
- **Benefits**:
    
    - Enter passphrase **once per session**.
        
    - Ensures **no password is transmitted**.
        
    - Provides **secure authentication** even across many systems.
        

* * *

## Default Configuration:

* * *

Located in: `/etc/ssh/sshd_config`

```bash
cat /etc/ssh/sshd_config | grep -v "#" | sed -r '/^\s*$/d'

Include /etc/ssh/sshd_config.d/*.conf  
ChallengeResponseAuthentication no  
UsePAM yes  
X11Forwarding yes  
PrintMotd no  
AcceptEnv LANG LC_*  
Subsystem sftp /usr/lib/openssh/sftp-server

```

<span style="color: #f1c40f;">**NOTE:**</span> Most parameters are **commented out by default** and require **manual configuration**.

* * *

## Dangerous Settings

* * *

| Setting | Description |
| --- | --- |
| `PasswordAuthentication yes` | Allows password-based login → susceptible to brute force |
| `PermitEmptyPasswords yes` | Accepts empty passwords |
| `PermitRootLogin yes` | Direct root login allowed |
| `Protocol 1` | Uses vulnerable SSH-1 protocol |
| `X11Forwarding yes` | Enables GUI forwarding (potential command injection) |
| `AllowTcpForwarding yes` | Permits TCP port forwarding |
| `PermitTunnel yes` | Enables tunneling |
| `DebianBanner yes` | Shows OS-specific banner on login |

* * *

## Footprinting the Service

* * *

### SSH-Audit

Installation: `git clone https://github.com/jtesta/ssh-audit.git && cd ssh-audit`

&nbsp;      ` ./ssh-audit.py 10.129.14.132`

* * *

### Change Authentication Method

`ssh -v cry0l1t3@10.129.14.132`

* * *

For potential brute-force attacks, we can specify the authentication method with the SSH client option `PreferredAuthentications`.  
`ssh -v cry0l1t3@10.129.14.132 -o PreferredAuthentications=password`

* * *

## Rsync

* * *

#### Scanning for Rsync

`sudo nmap -sV -p 873 127.0.0.1`

* * *

#### Probing for Accessible Shares

`nc -nv 127.0.0.1 873`

* * *

#### Enumerating an Open Share

`rsync -av --list-only rsync://127.0.0.1/dev`

* * *

## R-Services

* * *

- A legacy suite of services used for **remote access and command execution** between Unix hosts over **TCP/IP**.
    
- Originally developed by **CSRG at UC Berkeley** and widely adopted in **Solaris, HP-UX, AIX**, etc.
    
- Replaced by **SSH** due to **critical security flaws**.
    
- **Unencrypted communication** → highly vulnerable to **MITM attacks** and credential interception.
    

* * *

### Key Characteristics

- Services run over **TCP ports 512–514**.
    
- Communication relies on **r-commands**, a collection of client programs designed for specific remote tasks.
    
- Transmit **plaintext passwords and session data**, making them insecure for modern use.
    

* * *

### Common Ports Used by R-Services

| Port | Protocol | Description |
| --- | --- | --- |
| 512 | TCP | `rexec` – Remote command execution |
| 513 | TCP | `rlogin` – Remote login to a host |
| 514 | TCP | `rsh` – Remote shell access |

* * *

### Commonly Abused R-Commands

| **Command** | **Service Daemon** | **Port** | **Transport Protocol** | **Description** |
| --- | --- | --- | --- | --- |
| `rcp` | `rshd` | 514 | TCP | Copy a file or directory bidirectionally from the local system to the remote system (or vice versa) or from one remote system to another. It works like the `cp` command on Linux but provides `no warning to the user for overwriting existing files on a system`. |
| `rsh` | `rshd` | 514 | TCP | Opens a shell on a remote machine without a login procedure. Relies upon the trusted entries in the `/etc/hosts.equiv` and `.rhosts` files for validation. |
| `rexec` | `rexecd` | 512 | TCP | Enables a user to run shell commands on a remote machine. Requires authentication through the use of a `username` and `password` through an unencrypted network socket. Authentication is overridden by the trusted entries in the `/etc/hosts.equiv` and `.rhosts` files. |
| `rlogin` | `rlogind` | 513 | TCP | Enables a user to log in to a remote host over the network. It works similarly to `telnet` but can only connect to Unix-like hosts. Authentication is overridden by the trusted entries in the `/etc/hosts.equiv` and `.rhosts` files. |

* * *

#### /etc/hosts.equiv

`cat /etc/hosts.equiv`

* * *

### Scanning for R-Services

`sudo nmap -sV -p 512,513,514 10.0.17.2`

* * *

### Logging in Using Rlogin

`rlogin 10.0.17.2 -l htb-student`

* * *

### Listing Authenticated Users Using Rwho

`rwho`

* * *

### Listing Authenticated Users Using Rusers

`rusers -al 10.0.17.5`

* * *

&nbsp;
