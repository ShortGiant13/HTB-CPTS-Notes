## IMAP / POP3 Overview

- **IMAP (Internet Message Access Protocol)** allows online email management on a remote server.
- **POP3 (Post Office Protocol 3)** only allows listing, retrieving, and deleting emails from the server.
- IMAP enables multiple clients to sync emails, while POP3 downloads emails locally and removes them from the server.
- IMAP uses **port 143 (unencrypted)** and **port 993 (SSL/TLS encrypted)**.
- POP3 uses **port 110 (unencrypted)** and **port 995 (SSL/TLS encrypted)**.

* * *

## Default Setting

Dovecot Core Settings : [Default Configuration](https://doc.dovecot.org/2.3/settings/core/)

Dovecot Service Configuration : [Service Config](https://doc.dovecot.org/2.3/configuration_manual/service_configuration/)

* * *

## Dangerous Settings

| Setting | Description |
| --- | --- |
| `auth_debug` | Enables all authentication debug logging. |
| `auth_debug_passwords` | This setting adjusts log verbosity, the submitted passwords, and the scheme gets logged. |
| `auth_verbose` | Logs unsuccessful authentication attempts and their reasons. |
| `auth_verbose_passwords` | Passwords used for authentication are logged and can also be truncated. |
| `auth_anonymous_username` | This specifies the username to be used when logging in with the ANONYMOUS SASL mechanism. |

* * *

## IMAP Commands

| **Command** | **Description** |
| --- | --- |
| `1 LOGIN username password` | User's login. |
| `1 LIST "" *` | Lists all directories. |
| `1 CREATE "INBOX"` | Creates a mailbox with a specified name. |
| `1 DELETE "INBOX"` | Deletes a mailbox. |
| `1 RENAME "ToRead" "Important"` | Renames a mailbox. |
| `1 LSUB "" *` | Returns a subset of names from the set of names that the User has declared as being `active` or `subscribed`. |
| `1 SELECT INBOX` | Selects a mailbox so that messages in the mailbox can be accessed. |
| `1 UNSELECT INBOX` | Exits the selected mailbox. |
| `1 FETCH <ID> all` | Retrieves data associated with a message in the mailbox. |
| `1 CLOSE` | Removes all messages with the `Deleted` flag set. |
| `1 LOGOUT` | Closes the connection with the IMAP server. |
| `1 SEARCH admin` | Searches the word that has been input |
| `1 FETCH 1 BODY []` | Fetches latest email |
| `1 FETCH 1:* BODY[]` | Fetches all emails |

* * *

## POP3 Commands

| **Command** | **Description** |
| --- | --- |
| `USER username` | Identifies the user. |
| `PASS password` | Authentication of the user using its password. |
| `STAT` | Requests the number of saved emails from the server. |
| `LIST` | Requests from the server the number and size of all emails. |
| `RETR id` | Requests the server to deliver the requested email by ID. |
| `DELE id` | Requests the server to delete the requested email by ID. |
| `CAPA` | Requests the server to display the server capabilities. |
| `RSET` | Requests the server to reset the transmitted information. |
| `QUIT` | Closes the connection with the POP3 server. |

* * *

## Footprinting the Service

* * *

### Nmap - IMAP / POP3

`sudo nmap 10.129.14.128 -sV -p110,143,993,995 -sC`

* * *

### OpenSSL - TLS Encrypted Interaction POP3

`openssl s_client -connect 10.129.14.128:pop3s`

* * *

### OpenSSL - TLS Encrypted Interaction IMAP

`openssl s_client -connect 10.129.14.128:imaps`

* * *

&nbsp;