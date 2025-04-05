# MySQL

- **MySQL** is an open-source relational database management system (RDBMS) developed by **Oracle**.
    
- Works on the **client-server model**:
    
    - **MySQL Server** handles data storage and distribution.
        
    - **MySQL Clients** send SQL queries to retrieve, insert, modify, or delete data.
        
- Data is stored in **tables** with columns and rows.
    
- Often used in **web applications** like **WordPress**, which stores posts, usernames, and passwords in a MySQL database.
    
- Errors returned from MySQL (especially in web apps) can **leak internal logic**, often exploited during **SQL injection** attacks.
    
- Best Practices while configuring MySQL: [Security Issues](https://dev.mysql.com/doc/refman/8.0/en/general-security-issues.html)
- Frequently used as part of:
    
    - **LAMP Stack**: Linux, Apache, MySQL, PHP
        
    - **LEMP Stack**: Linux, Nginx, MySQL, PHP
        

**Default Port**:

- **3306** (TCP) – Default port for MySQL server.

* * *

## Default Configuration

* * *

| **Section** | **Directive** | **Value** |
| --- | --- | --- |
| `[client]` | `port` | 3306 |
|     | `socket` | /var/run/mysqld/mysqld.sock |
| `[mysqld_safe]` | `pid-file` | /var/run/mysqld/mysqld.pid |
|     | `socket` | /var/run/mysqld/mysqld.sock |
|     | `nice` | 0   |
| `[mysqld]` | `skip-host-cache` | *(no value - flag is set)* |
|     | `skip-name-resolve` | *(no value - flag is set)* |
|     | `user` | mysql |
|     | `pid-file` | /var/run/mysqld/mysqld.pid |
|     | `socket` | /var/run/mysqld/mysqld.sock |
|     | `port` | 3306 |
|     | `basedir` | /usr |
|     | `datadir` | /var/lib/mysql |
|     | `tmpdir` | /tmp |
|     | `lc-messages-dir` | /usr/share/mysql |
|     | `explicit_defaults_for_timestamp` | *(no value - flag is set)* |
|     | `symbolic-links` | 0   |
| `[include]` | `!includedir` | /etc/mysql/conf.d/ |

**Key Notes:**

- **`skip-name-resolve`** disables DNS lookups (can affect remote hostname auth).
    
- **`datadir`** defines where DB files are stored.
    
- **`user`** under which MySQL service runs is usually `mysql`.
    

* * *

## Dangerous Settings / Misconfigurations:

* * *

| Setting | Description | Risk |
| --- | --- | --- |
| `user` | OS-level user MySQL runs as | If misconfigured, may allow privilege escalation or file access |
| `password` | Password stored in config | If stored in plaintext and file is world-readable, credentials can be stolen |
| `admin_address` | IP bound for admin interface | If exposed externally, could be targeted for attacks |
| `debug`, `sql_warnings` | Verbose error output | Can leak internal errors and assist in SQL injection |
| `secure_file_priv` | Controls import/export directory | Misuse can lead to arbitrary file read/write |

* * *

## Footprinting the Service

* * *

#### Scanning MySQL Server

`sudo nmap 10.129.14.128 -sV -sC -p3306 --script mysql*`

* * *

#### Interaction with the MySQL Server

`mysql -u root -h 10.129.14.132`

`mysql -u root -pP4SSw0rd -h 10.129.14.128`

* * *

## MySQL Commands

* * *

Some of the commands we should remember and write down for working with MySQL databases are described below in the table.

| **Command** | **Description** |
| --- | --- |
| `mysql -u <user> -p<password> -h <IP address>` | Connect to the MySQL server. There should **not** be a space between the '-p' flag, and the password. |
| `show databases;` | Show all databases. |
| `use <database>;` | Select one of the existing databases. |
| `show tables;` | Show all available tables in the selected database. |
| `show columns from <table>;` | Show all columns in the selected database. |
| `select * from <table>;` | Show everything in the desired table. |
| `select * from <table> where <column> = "<string>";` | Search for needed `string` in the desired table. |

* * *

&nbsp;