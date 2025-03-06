- SMTP is used for sending emails over an IP network.
- Works between email clients and outgoing mail servers or between SMTP servers.
- Commonly used with IMAP or POP3 for fetching emails.

### Ports & Encryption

- **Port 25:** Default SMTP port for sending emails (often blocked for spam prevention).
- **Port 587:** Used for sending mail with authentication (STARTTLS encryption).
- **Port 465:** Used for encrypted SMTP connections (SSL/TLS).

* * *

## WorkFlow

**Mail User Agent (MUA)** → Sends email.  
**Mail Submission Agent (MSA)** → Validates the email.  
**Mail Transfer Agent (MTA)** → Relays the email.  
**Mail Delivery Agent (MDA)** → Delivers to the recipient’s mailbox.  
**Mailbox Access via POP3/IMAP.**

**Client (MUA) ➞ Submission Agent (MSA) ➞ Open Relay (MTA) ➞ Mail Delivery Agent (MDA) ➞ Mailbox (POP3/IMAP)**

* * *

## Default Settings

**Port:** **25** (Unencrypted, often blocked), **587** (STARTTLS), **465** (SSL/TLS).  
**Authentication:** Required (SMTP-AUTH).  
**Relay Restrictions:** Only allows emails from authenticated users or trusted IPs.  
**Encryption:** STARTTLS or SSL/TLS enabled.

* * *

## Special Commands

| **Command** | **Description** |
| --- | --- |
| `AUTH PLAIN` | AUTH is a service extension used to authenticate the client. |
| `HELO` | The client logs in with its computer name and thus starts the session. |
| `MAIL FROM` | The client names the email sender. |
| `RCPT TO` | The client names the email recipient. |
| `DATA` | The client initiates the transmission of the email. |
| `RSET` | The client aborts the initiated transmission but keeps the connection between client and server. |
| `VRFY` | The client checks if a mailbox is available for message transfer. |
| `EXPN` | The client also checks if a mailbox is available for messaging with this command. |
| `NOOP` | The client requests a response from the server to prevent disconnection due to time-out. |
| `QUIT` | The client terminates the session. |

* * *

### Telnet - HELO/EHLO

We can use the telnet tool to initialize a TCP connection with the SMTP server. The actual initialization of the session is done with the command mentioned above, HELO or EHLO.

`telnet 10.129.14.128 25`

`HELO mail1.inlanefreight.htb`

`EHLO mail1`

* * *

#### Telnet - VRFY

`telnet 10.129.14.128 25`

`VRFY root`

* * *

## Dangerous Settings

* * *

#### Open Relay Configuration

`mynetworks = 0.0.0.0/0`

* * *

### Footprinting the Service

* * *

#### Nmap

`sudo nmap 10.129.14.128 -sC -sV -p25`

**Nmap - Open Relay**

`sudo nmap 10.129.14.128 -p25 --script smtp-open-relay -v`

* * *

&nbsp;