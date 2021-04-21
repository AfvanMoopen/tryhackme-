# Avengers Blog

Learn to hack into Tony Stark's machine! You will enumerate the machine, bypass a login portal via SQL injection and gain root access by command injection.

[Avengers Blog](https://tryhackme.com/room/avengers)

## Topic's

- Cookie Enumeration
- Web Header Eumeration
- Network Enumeration
- Web Poking
- FTP Enumeration
- SQL Injection
- Command Injection

## Appendix archive

Password: `1 kn0w 1 5h0uldn'7!`

## Task 1 Deploy

Connect to our network and deploy the Avengers Blog machine

1. Connect to our network by going to your [access page](https://tryhackme.com/access). This is important as you will not be able to access the machine without connecting!

`No answer needed`

2. Deploy the machine by clicking the green "Deploy" button on this task and access its webserver.

`No answer needed`

## Task 2 Cookies

HTTP Cookies is a small piece of data sent from a website and stored on the user's computer by the user's web browser while the user is browsing. They're intended to remember things such as your login information, items in your shopping cart or language you prefer.

Advertisers can use also tracking cookies to identify which sites you've previously visited or where about's on a web-page you've clicked. Some tracking cookies have become so intrusive, many anti-virus programs classify them as spyware.

You can view & dynamically update your cookies directly in your browser. To do this, press **F12** (or right click and select Inspect) to open the developer tools on your browser, then click Application and then Cookies.

1. On the deployed Avengers machine you recently deployed, get the flag1 cookie value.

![](./2020-11-07_13-14.png)

`cookie_secrets`

## Task 3 HTTP Headers

HTTP Headers let a client and server pass information with a HTTP request or response. Header names and values are separated by a single colon and are integral part of the HTTP protocol.

![](https://i.imgur.com/GlCdRIM.png)

The main two HTTP Methods are POST and GET requests. The GET method us used to request data from a resource and the POST method is used to send data to a server.

We can view requests made to and from our browser by opening the Developer Tools again and navigating to the Network tab. Have this tab open and refresh the page to see all requests made. You will be able to see the original request made from your browser to the web server.

1. Look at the HTTP response headers and obtain flag 2.

![](./2020-11-07_13-16.png)

`headers_are_important`

## Task 4 Enumeration and FTP

In this task we will scan the machine with nmap (a network scanner) and access the FTP service using reusable credentials.

Lets get started by scanning the machine, you will need nmap. If you don't have the application installed you can use our web-based AttackBox that has nmap pre-installed.

In your terminal, execute the following command: `nmap <machine_ip> -v`

This will scan the machine and determine what services on which ports are running. For this machine, you will see the following ports open:

![](https://i.imgur.com/UjXizy4.png)

- Port 80 has a HTTP web server running on
- Port 22 is to SSH into the machine
- Port 21 is used for FTP (file transfer)

We've accessed the web server, lets now access the FTP service. If you read the Avengers web page, you will see that Rocket made a post asking for Groot's password to be reset, the post included his old password too!

In your terminal, execute the following command: `ftp <machine_ip>`

We will be asked for a username (groot) and a password (iamgroot). We should have now successfully logged into the FTP share using Groots credentials!

1. Look around the FTP share and read flag 3!

```
kali@kali:~/CTFs/tryhackme/Avengers Blog$ sudo nmap -A -sS -sC -sV -O 10.10.109.199
[sudo] password for kali:
Starting Nmap 7.80 ( https://nmap.org ) at 2020-11-07 13:18 CET
Nmap scan report for 10.10.109.199
Host is up (0.032s latency).
Not shown: 997 closed ports
PORT   STATE SERVICE VERSION
21/tcp open  ftp     vsftpd 3.0.3
22/tcp open  ssh     OpenSSH 7.6p1 Ubuntu 4ubuntu0.3 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey:
|   2048 d2:63:f4:db:ad:9d:c0:74:49:f7:60:2b:45:46:58:6a (RSA)
|   256 9a:f1:be:29:ef:2a:51:a5:27:46:48:00:9d:01:ed:29 (ECDSA)
|_  256 39:b0:3a:78:40:95:79:2e:99:af:f6:a3:c1:9a:19:75 (ED25519)
80/tcp open  http    Node.js Express framework
|_http-title: Avengers! Assemble!
No exact OS matches for host (If you know what OS is running on it, see https://nmap.org/submit/ ).
TCP/IP fingerprint:
OS:SCAN(V=7.80%E=4%D=11/7%OT=21%CT=1%CU=32301%PV=Y%DS=2%DC=T%G=Y%TM=5FA690A
OS:F%P=x86_64-pc-linux-gnu)SEQ(SP=102%GCD=1%ISR=106%TI=Z%CI=Z%II=I%TS=A)OPS
OS:(O1=M508ST11NW7%O2=M508ST11NW7%O3=M508NNT11NW7%O4=M508ST11NW7%O5=M508ST1
OS:1NW7%O6=M508ST11)WIN(W1=68DF%W2=68DF%W3=68DF%W4=68DF%W5=68DF%W6=68DF)ECN
OS:(R=Y%DF=Y%T=40%W=6903%O=M508NNSNW7%CC=Y%Q=)T1(R=Y%DF=Y%T=40%S=O%A=S+%F=A
OS:S%RD=0%Q=)T2(R=N)T3(R=N)T4(R=Y%DF=Y%T=40%W=0%S=A%A=Z%F=R%O=%RD=0%Q=)T5(R
OS:=Y%DF=Y%T=40%W=0%S=Z%A=S+%F=AR%O=%RD=0%Q=)T6(R=Y%DF=Y%T=40%W=0%S=A%A=Z%F
OS:=R%O=%RD=0%Q=)T7(R=Y%DF=Y%T=40%W=0%S=Z%A=S+%F=AR%O=%RD=0%Q=)U1(R=Y%DF=N%
OS:T=40%IPL=164%UN=0%RIPL=G%RID=G%RIPCK=G%RUCK=G%RUD=G)IE(R=Y%DFI=N%T=40%CD
OS:=S)

Network Distance: 2 hops
Service Info: OSs: Unix, Linux; CPE: cpe:/o:linux:linux_kernel

TRACEROUTE (using port 443/tcp)
HOP RTT      ADDRESS
1   31.18 ms 10.8.0.1
2   31.46 ms 10.10.109.199

OS and Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 25.19 seconds
```

![](./2020-11-07_13-20.png)

`iamgroot`

```
kali@kali:~/CTFs/tryhackme/Avengers Blog$ ftp 10.10.109.199
Connected to 10.10.109.199.
220 (vsFTPd 3.0.3)
Name (10.10.109.199:kali): groot
331 Please specify the password.
Password:
230 Login successful.
Remote system type is UNIX.
Using binary mode to transfer files.
ftp> ls -la
200 PORT command successful. Consider using PASV.
150 Here comes the directory listing.
dr-xr-xr-x    3 65534    65534        4096 Oct 04  2019 .
dr-xr-xr-x    3 65534    65534        4096 Oct 04  2019 ..
drwxr-xr-x    2 1001     1001         4096 Oct 04  2019 files
226 Directory send OK.
ftp> cd files
250 Directory successfully changed.
ftp> ls -la
200 PORT command successful. Consider using PASV.
150 Here comes the directory listing.
drwxr-xr-x    2 1001     1001         4096 Oct 04  2019 .
dr-xr-xr-x    3 65534    65534        4096 Oct 04  2019 ..
-rw-r--r--    1 0        0              33 Oct 04  2019 flag3.txt
226 Directory send OK.
ftp> get flag3.txt
local: flag3.txt remote: flag3.txt
200 PORT command successful. Consider using PASV.
150 Opening BINARY mode data connection for flag3.txt (33 bytes).
226 Transfer complete.
33 bytes received in 0.00 secs (21.1322 kB/s)
ftp> exit
221 Goodbye.
```

```
kali@kali:~/CTFs/tryhackme/Avengers Blog$ cat flag3.txt
8fc651a739befc58d450dc48e1f1fd2e
```

`8fc651a739befc58d450dc48e1f1fd2e`

## Task 5 GoBuster

Lets use a fast directory discovery tool called GoBuster. This program will locate a directory that you can use to login to Mr. Starks Tarvis portal!

GoBuster is a tool used to brute-force URIs (directories and files), DNS subdomains and virtual host names. For this machine, we will focus on using it to brute-force directories.

You can either download GoBuster, or use the Kali Linux machine that has it pre-installed.

Lets run GoBuster with a wordlist (on Kali they're located under /usr/share/wordlists): `gobuster dir -u http://<machine_ip> -w <word_list_location>`

1. What is the directory that has an Avengers login?

```
kali@kali:~/CTFs/tryhackme/Avengers Blog$ gobuster dir -u 10.10.109.199 -w /usr/share/wordlists/dirb/common.txt
===============================================================
Gobuster v3.0.1
by OJ Reeves (@TheColonial) & Christian Mehlmauer (@_FireFart_)
===============================================================
[+] Url:            http://10.10.109.199
[+] Threads:        10
[+] Wordlist:       /usr/share/wordlists/dirb/common.txt
[+] Status codes:   200,204,301,302,307,401,403
[+] User Agent:     gobuster/3.0.1
[+] Timeout:        10s
===============================================================
2020/11/07 13:18:56 Starting gobuster
===============================================================
/assets (Status: 301)
/css (Status: 301)
/home (Status: 302)
/Home (Status: 302)
/img (Status: 301)
/js (Status: 301)
/logout (Status: 302)
/portal (Status: 200)
===============================================================
2020/11/07 13:19:33 Finished
===============================================================
```

`/portal`

## Task 6 SQL Injection

You should now see the following page above. We're going to manually exploit this page using an attack called SQL injection.

SQL Injection is a code injection technique that manipulates an SQL query. You can execute you're own SQL that could destroy the database, reveal all database data (such as usernames and passwords) or trick the web server in authenticating you.

To exploit SQL, we first need to know how it works. A SQL query could be `SELECT * FROM Users WHERE username = {User Input} AND password = {User Input 2}` , if you insert additional SQL as the {User Input} we can manipulate this query. For example, if I have the {User Input 2} as `' 1=1` we could trick the query into authenticating us as the ' character would break the SQL query and 1=1 would evaluate to be true.

To conclude, having our first {User Input} as the username of the account and {User Input 2} being the condition to make the query true, the final query would be:
`SELECT * FROM Users WHERE username = `admin`AND password =`' 1=1``

This would authenticate us as the admin user.

1. Log into the Avengers site. View the page source, how many lines of code are there?

[view-source:http://10.10.109.199/home](view-source:http://10.10.109.199/home)

`223`

## Task 7 Remote Code Execution and Linux

You should be logged into the Jarvis access panel! Here we can execute commands on the machine.. I wonder if we can exploit this to read files on the system.

Try executing the ls command to list all files in the current directory. Now try joining 2 Linux commands together to list files in the parent directory: `cd ../; ls` doing so will show a file called flag5.txt, we can add another command to read this file: `cd ../; ls; cat flag5.txt`

But oh-no! The cat command is disallowed! We will have to think of another Linux command we can use to read it!

1. Read the contents of flag5.txt

`cd ../; ls; tac flag5.txt`

![](./2020-11-07_13-31.png)

`d335e2d13f36558ba1e67969a1718af7`
