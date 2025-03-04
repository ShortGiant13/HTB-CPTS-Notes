### Network File System (NFS):

NFS allows access to file systems over a network as if they were local, but it is primarily used with **Linux** and **Unix systems**, unlike **SMB**, which is mostly used in **Windows environments**.

### NFS Versions:

- **NFSv2**: Older, uses **UDP**, limited features.
- **NFSv3**: Adds better error reporting, variable file sizes, and improved performance.
- **NFSv4**: Introduces **Kerberos** authentication, **ACLs**, stateful protocol, and better security/performance.

* * *

## Default Configuration

* * *

### Exports File

`cat /etc/exports`

Defines shared folders, sets permissions for hosts/subnets, and adds options for control.

| **Option** | **Description** |
| --- | --- |
| `rw` | Read and write permissions. |
| `ro` | Read only permissions. |
| `sync` | Synchronous data transfer. (A bit slower) |
| `async` | Asynchronous data transfer. (A bit faster) |
| `secure` | Ports above 1024 will not be used. |
| `insecure` | Ports above 1024 will be used. |
| `no_subtree_check` | This option disables the checking of subdirectory trees. |
| `root_squash` | Assigns all permissions to files of root UID/GID 0 to the UID/GID of anonymous, which prevents `root` from accessing files on an NFS mount. |

* * *

## Dangerous Settings

* * *

| **Option** | **Description** |
| --- | --- |
| `rw` | Read and write permissions. |
| `insecure` | Ports above 1024 will be used. |
| `nohide` | If another file system was mounted below an exported directory, this directory is exported by its own exports entry. |
| `no_root_squash` | All files created by root are kept with the UID/GID 0. |

* * *

## Footprinting the Service

* * *

### Nmap
`sudo nmap 10.129.14.128 -p111,2049 -sV -sC`  
`sudo nmap --script nfs* 10.129.14.128 -sV -p111,2049`

* * *

### Show Available NFS Shares
`showmount -e 10.129.14.128`

* * *

### Mounting NFS Share
`Sh0r7Gi4n7@htb[/htb]$ mkdir target-NFS`  
`Sh0r7Gi4n7@htb[/htb]$ sudo mount -t nfs 10.129.14.128:/ ./target-NFS/ -o nolock`  
`Sh0r7Gi4n7@htb[/htb]$ cd target-NFS`  
`Sh0r7Gi4n7@htb[/htb]$ tree .`

* * *

### List Contents with Usernames & Group Names
`ls -l mnt/nfs/`

* * *

### List Contents with UIDs & GUIDs
`ls -n mnt/nfs/`

**It is important to note that if the `root_squash option` is set, we cannot edit the `backup.sh` file even as root.**

* * *

### Unmounting
`Sh0r7Gi4n7@htb[/htb]$ cd ..`  
`Sh0r7Gi4n7@htb[/htb]$ sudo umount ./target-NFS`

* * *