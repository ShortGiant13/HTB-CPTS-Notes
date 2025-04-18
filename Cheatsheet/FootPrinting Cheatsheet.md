## Infrastructure-based Enumeration

* * *

| **Command** | **Description** |
| --- | --- |
| `curl -s https://crt.sh/\?q\=<target-domain>\&output\=json \| jq .` | Certificate transparency. |
| `for i in $(cat ip-addresses.txt);do shodan host $i;done` | Scan each IP address in a list using Shodan. |

* * *

## Host-based Enumeration

* * *

### FTP

| **Command** | **Description** |
| --- | --- |
| `ftp <FQDN/IP>` | Interact with the FTP service on the target. |
| `nc -nv <FQDN/IP> 21` | Interact with the FTP service on the target. |
| `telnet <FQDN/IP> 21` | Interact with the FTP service on the target. |
| `openssl s_client -connect <FQDN/IP>:21 -starttls ftp` | Interact with the FTP service on the target using encrypted connection. |
| `wget -m --no-passive ftp://anonymous:anonymous@<target>` | Download all available files on the target FTP server. |

* * *

### SMB

| **Command** | **Description** |
| --- | --- |
| `smbclient -N -L //<FQDN/IP>` | Null session authentication on SMB. |
| `smbclient //<FQDN/IP>/<share>` | Connect to a specific SMB share. |
| `rpcclient -U "" <FQDN/IP>` | Interaction with the target using RPC. |
| `samrdump.py <FQDN/IP>` | Username enumeration using Impacket scripts. |
| `smbmap -H <FQDN/IP>` | Enumerating SMB shares. |
| `crackmapexec smb <FQDN/IP> --shares -u '' -p ''` | Enumerating SMB shares using null session authentication. |
| `enum4linux-ng.py <FQDN/IP> -A` | SMB enumeration using enum4linux. |

* * *

### NFS

| **Command** | **Description** |
| --- | --- |
| `showmount -e <FQDN/IP>` | Show available NFS shares. |
| `mount -t nfs <FQDN/IP>:/<share> ./target-NFS/ -o nolock` | Mount the specific NFS share to ./target-NFS |
| `umount ./target-NFS` | Unmount the specific NFS share. |

* * *

### DNS

| **Command** | **Description** |
| --- | --- |
| `dig ns <domain.tld> @<nameserver>` | NS request to the specific nameserver. |
| `dig any <domain.tld> @<nameserver>` | ANY request to the specific nameserver. |
| `dig axfr <domain.tld> @<nameserver>` | AXFR request to the specific nameserver. |
| `dnsenum --dnsserver <nameserver> --enum -p 0 -s 0 -o found_subdomains.txt -f ~/subdomains.list <domain.tld>` | Subdomain brute forcing. |

* * *

### SMTP

| **Command** | **Description** |
| --- | --- |
| `telnet <FQDN/IP> 25` |     |
| `smtp-user-enum -M VRFY -U /path/to/wordlist.txt -t 10.129.42.195`  <br><br/>OR  <br><br/>`smtp-user-enum -M RCPT -U /path/to/wordlist.txt -t 10.129.42.195` | This will return a **confirmed system username**. |
| `sudo nmap 10.129.14.128 -p25 --script smtp-open-relay -v` | Check for Open Relay using Nmap |
| `msfconsole`  <br><br/>`use auxiliary/scanner/smtp/` | Various SMTP attacks |

* * *

### IMAP/POP3

| **Command** | **Description** |
| --- | --- |
| `curl -k 'imaps://<FQDN/IP>' --user <user>:<password>` | Log in to the IMAPS service using cURL. |
| `openssl s_client -connect <FQDN/IP>:imaps` | Connect to the IMAPS service. |
| `openssl s_client -connect <FQDN/IP>:pop3s` | Connect to the POP3s service. |

* * *

### SNMP

| **Command** | **Description** |
| --- | --- |
| `snmpwalk -v2c -c <community string> <FQDN/IP>` | Querying OIDs using snmpwalk. |
| `onesixtyone -c community-strings.list <FQDN/IP>` | Bruteforcing community strings of the SNMP service. |
| `braa <community string>@<FQDN/IP>:.1.*` | Bruteforcing SNMP service OIDs. |

* * *

### MySQL

| **Command** | **Description** |
| --- | --- |
| `mysql -u <user> -p<password> -h <FQDN/IP>` | Login to the MySQL server. |
| `sudo nmap 10.129.14.128 -sV -sC -p3306 --script mysql*` | Scanning With Nmap |

| **Command** | **Description** |
| --- | --- |
| `mysql -u <user> -p<password> -h <IP address>` | Connect to the MySQL server. There should **not** be a space between the '-p' flag, and the password. |
| `show databases;` | Show all databases. |
| `use <database>;` | Select one of the existing databases. |
| `show tables;` | Show all available tables in the selected database. |
| `show columns from <table>;` | Show all columns in the selected database. |
| `select * from <table>;` | Show everything in the desired table. |
| `select * from <table> where <column> = "<string>";` | Search for needed `string` in the desired table. |
| `SELECT email FROM myTable WHERE name = '<name>';` | SQL Query to Get Email |

* * *

### MSSQL

| **Command** | **Description** |
| --- | --- |
| `mssqlclient.py <user>@<FQDN/IP> -windows-auth` | Log in to the MSSQL server using Windows authentication. |

* * *

### Oracle TNS

| **Command** | **Description** |
| --- | --- |
| `sudo nmap -p1521 -sV 10.129.204.235 --open` | Nmap Scanning for Oracle TNS |
| `sudo nmap -p1521 -sV 10.129.204.235 --open --script oracle-sid-brute` | Nmap - SID Bruteforcing |
| `./odat.py all -s 10.129.204.235` | ODAT Command |
| `sqlplus scott/tiger@10.129.204.235/XE` | SQLplus - Log In |
| `sqlplus scott/tiger@10.129.204.235/XE as sysdba` | Oracle RDBMS - Database Enumeration |
| `echo "Oracle File Upload Test" > testing.txt`  <br>`./odat.py utlfile -s 10.129.204.235 -d XE -U scott -P tiger --sysdba --putFile C:\\inetpub\\wwwroot testing.txt ./testing.txt` | Oracle RDBMS - File Upload using ODAT |

* * *

### IPMI

| **Command** | **Description** |
| --- | --- |
| `msf6 auxiliary(scanner/ipmi/ipmi_version)` | IPMI version detection. |
| `msf6 auxiliary(scanner/ipmi/ipmi_dumphashes)` | Dump IPMI hashes. |

* * *

### Linux Remote Management

| **Command** | **Description** |
| --- | --- |
| `ssh-audit.py <FQDN/IP>` | Remote security audit against the target SSH service. |
| `ssh <user>@<FQDN/IP>` | Log in to the SSH server using the SSH client. |
| `ssh -i private.key <user>@<FQDN/IP>` | Log in to the SSH server using private key. |
| `ssh <user>@<FQDN/IP> -o PreferredAuthentications=password` | Enforce password-based authentication. |

* * *

### Windows Remote Management

| **Command** | **Description** |
| --- | --- |
| `rdp-sec-check.pl <FQDN/IP>` | Check the security settings of the RDP service. |
| `xfreerdp /u:<user> /p:"<password>" /v:<FQDN/IP>` | Log in to the RDP server from Linux. |
| `evil-winrm -i <FQDN/IP> -u <user> -p <password>` | Log in to the WinRM server. |
| `wmiexec.py <user>:"<password>"@<FQDN/IP> "<system command>"` | Execute command using the WMI service. |

* * *

### Oracle TNS

| **Command** | **Description** |
| --- | --- |
| `./odat.py all -s <FQDN/IP>` | Perform a variety of scans to gather information about the Oracle database services and its components. |
| `sqlplus <user>/<pass>@<FQDN/IP>/<db>` | Log in to the Oracle database. |
| `./odat.py utlfile -s <FQDN/IP> -d <db> -U <user> -P <pass> --sysdba --putFile C:\\insert\\path file.txt ./file.txt` |     |

* * *

&nbsp;
