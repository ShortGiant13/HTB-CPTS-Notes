The **File Transfer Protocol (FTP)** operates in the **application layer** of the TCP/IP stack (same layer as **HTTP** or **POP**). It allows file upload/download by opening two channels:

1.  **Control channel**: TCP port 21 for command/response exchange.
2.  **Data channel**: TCP port 20 for data transmission.

### Active vs. Passive FTP:

- **Active FTP**: The client opens port 21 for control and informs the server which client-side port to use for the data channel. This can be blocked by firewalls.
- **Passive FTP**: The server opens a port for the client to establish the data connection, bypassing firewall restrictions.

&nbsp;

### Commands and Status Codes:

FTP uses various commands (e.g., upload, download, delete files) with corresponding **status codes** to indicate success or failure. Not all commands are implemented on every server.

&nbsp;

### Security:

- **Clear-text protocol**: FTP transmits data in plain text, which can be intercepted unless encrypted.
- **Anonymous FTP**: Some servers allow anonymous access for file transfer without credentials, but this is usually limited for security reasons.

* * *

## TFTP

* * *

Trivial File Transfer Protocol (TFTP) is a simpler alternative to FTP, designed for file transfers between client and server processes. Unlike FTP, TFTP:

Does not require user authentication.  
Uses UDP instead of TCP, making it unreliable and needing application layer recovery.  
Key Points:

**No authentication:** TFTP does not support password-protected logins, relying solely on file permissions (read/write).  
**File access:** It operates in public directories where files are globally accessible (read/write).  
**Security limitations:** Due to its lack of security, TFTP is suitable only for local, protected networks.

* * *

### TFTP Commands:

| Command | Description |
| --- | --- |
| `connect` | Sets the remote host and port for file transfers. |
| `get` | Transfers files from the remote host to the local host. |
| `put` | Transfers files from the local host to the remote host. |
| `quit` | Exits TFTP. |
| `status` | Shows the current status (transfer mode, connection, timeout, etc.). |
| `verbose` | Turns verbose mode on/off to show more info during transfer. |

TFTP does not support **directory listing** like FTP.

* * *

### vsFTPd Configuration:

- Install with:  
    `sudo apt install vsftpd`

* * *

### Key vsFTPd Config Settings:

| Setting | Description |
| --- | --- |
| `listen=NO` | Run from **inetd** or as a **standalone** daemon? |
| `listen_ipv6=YES` | Listen on **IPv6**? |
| `anonymous_enable=NO` | Disable **anonymous access**? |
| `local_enable=YES` | Allow **local users** to login? |
| `dirmessage_enable=YES` | Display active directory messages? |
| `xferlog_enable=YES` | Enable **upload/download logging**? |
| `secure_chroot_dir=/var/run/vsftpd/empty` | Define **empty directory** for chroot. |
| `rsa_cert_file` & `rsa_private_key_file` | Set locations for **SSL certificate** and **private key**. |
| `ssl_enable=NO` | Disable **SSL encryption**. |

### FTP User Restrictions:

- `/etc/ftpusers`: Contains users denied FTP access, e.g., `guest`, `john`, `kevin`.

* * *

## Dangerous Settings

| **Setting** | **Description** |
| --- | --- |
| `anonymous_enable=YES` | Allowing anonymous login? |
| `anon_upload_enable=YES` | Allowing anonymous to upload files? |
| `anon_mkdir_write_enable=YES` | Allowing anonymous to create new directories? |
| `no_anon_password=YES` | Do not ask anonymous for password? |
| `anon_root=/home/username/ftp` | Directory for anonymous. |
| `write_enable=YES` | Allow the usage of FTP commands: STOR, DELE, RNFR, RNTO, MKD, RMD, APPE, and SITE? |

* * *

### Anonymous Login

`ftp 10.129.14.136`

* * *

### vsFTPd Detailed Output

| **Setting** | **Description** |
| --- | --- |
| `dirmessage_enable=YES` | Show a message when they first enter a new directory? |
| `chown_uploads=YES` | Change ownership of anonymously uploaded files? |
| `chown_username=username` | User who is given ownership of anonymously uploaded files. |
| `local_enable=YES` | Enable local users to login? |
| `chroot_local_user=YES` | Place local users into their home directory? |
| `chroot_list_enable=YES` | Use a list of local users that will be placed in their home directory? |

| **Setting** | **Description** |
| --- | --- |
| `hide_ids=YES` | All user and group information in directory listings will be displayed as "ftp". |
| `ls_recurse_enable=YES` | Allows the use of recurse listings. |

* * *

### Recursive Listing

`ftp> ls -R`

* * *

### Download All Available Files

`wget -m --no-passive ftp://anonymous:anonymous@10.129.14.136`

* * *

### Upload a File

`touch testupload.txt`

`ftp> put testupload.txt`

* * *
## Footprinting the Service
* * *

### Nmap FTP Scripts (FTP)
`find / -type f -name ftp* 2>/dev/null | grep scripts`

* * *

### Nmap Script Trace  
`sudo nmap -sV -p21 -sC -A 10.129.14.136 --script-trace`

* * *

### Service Interaction  
`nc -nv 10.129.14.136 21`

`telnet 10.129.14.136 21`

* * *

### OpenSSL
`openssl s_client -connect 10.129.14.136:21 -starttls ftp`

* * *
