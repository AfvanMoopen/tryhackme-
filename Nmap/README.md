# Nmap

Get experience with Nmap, a powerful network scanning tool.

[Nmap](https://tryhackme.com/room/rpnmap)

## Topic's

* NMAP Fundamentals
* Network Enumeration

## Task 1 Introduction to Port Scanning

When it comes to hacking, knowledge is power. The more knowledge you have about a target system or network, the more options you have available. This makes it imperative that proper enumeration is carried out before any exploitation attempts are made.

Say we have been given an IP (or multiple IP addresses) to perform a security audit on. Before we do anything else, we need to get an idea of the “landscape” we are attacking. What this means is that we need to establish which services are running on the targets. For example, perhaps one of them is running a webserver, and another is acting as a Windows Active Directory Domain Controller. The first stage in establishing this “map” of the landscape is something called port scanning. When a computer runs a network service, it opens a networking construct called a “port” to receive the connection.  Ports are necessary for making multiple network requests or having multiple services available. For example, when you load several webpages at once in a web browser, the program must have some way of determining which tab is loading which web page. This is done by establishing connections to the remote webservers using different ports on your local machine. Equally, if you want a server to be able to run more than one service (for example, perhaps you want your webserver to run both HTTP and HTTPS versions of the site), then you need some way to direct the traffic to the appropriate service. Once again, ports are the solution to this. Network connections are made between two ports – an open port listening on the server and a randomly selected port on your own computer. For example, when you connect to a web page, your computer may open port 49534 to connect to the server’s port 443.

![](https://i.imgur.com/3XAfRpI.png)

As in the previous example, the diagram shows what happens when you connect to numerous websites at the same time. Your computer opens up a different, high-numbered port (at random), which it uses for all its communications with the remote server.

Every computer has a total of 65535 available ports; however, many of these are registered as standard ports. For example, a HTTP Webservice can nearly always be found on port 80 of the server. A HTTPS Webservice can be found on port 443. Windows NETBIOS can be found on port 139 and SMB can be found on port 445. It is important to note; however, that especially in a CTF setting, it is not unheard of for even these standard ports to be altered, making it even more imperative that we perform appropriate enumeration on the target.

If we do not know which of these ports a server has open, then we do not have a hope of successfully attacking the target; thus, it is crucial that we begin any attack with a port scan. This can be accomplished in a variety of ways – usually using a tool called nmap, which is the focus of this room. Nmap can be used to perform many different kinds of port scan – the most common these will be introduced in upcoming tasks; however, the basic theory is this: nmap will connect to each port of the target in turn. Depending on how the port responds, it can be determined as being open, closed, or filtered (usually by a firewall). Once we know which ports are open, we can then look at enumerating which services are running on each port – either manually, or more commonly using nmap.

So, why nmap? The short answer is that its currently the industry standard for a reason: no other port scanning tool comes close to matching its functionality (although some newcomers are now matching it for speed). It is an extremely powerful tool – made even more powerful by its scripting engine

which can be used to scan for vulnerabilities, and in some cases even perform the exploit directly! Once again, this will be covered more in upcoming tasks.

For now, it is important that you understand: what port scanning is; why it is necessary; and that nmap is the tool of choice for any kind of initial enumeration.

1. What networking constructs are used to direct traffic to the right application on a server?

`Ports`

2. How many of these are available on any network-enabled computer?

`65535`

3. [Research] How many of these are considered "well-known"? (These are the "standard" numbers mentioned in the task)

`1024`

## [Task 2] Deploy!

Nmap is an incredibly valuable tool in the world of penetration testing. In this room, we will cover the basics of using Nmap to effectively scan a target, gaining insight for further attacks!

Here's a link to the companion video for this room in case you're stuck! [Link](https://www.youtube.com/watch?v=AEbVb5xQqzs)

1. Deploy the machine!

    > `No answer needed`

## [Task 3] Nmap Quiz 

A short quiz on the more useful switches that we can use with Nmap. All you'll need for this is the help menu for nmap. Include all parts of the switch unless otherwise specified, this includes -

1. First, how do you access the help menu?

    > `-h`

2. Often referred to as a stealth scan, what is the first switch listed for a 'Syn Scan'?

    > `-sS`

3. Not quite as useful but how about a 'UDP Scan'?

    > `-sU`

4. What about operating system detection?

    > `-O`

5. How about service version detection?

    > `-sV`

6. Most people like to see some output to know that their scan is actually doing things, what is the verbosity flag?

    > `-V`

7. What about 'very verbose'? (A personal favorite)

    > `-VV`

8. Sometimes saving output in a common document format can be really handy for reporting, how do we save output in xml format?

    > `-oX`

9. Aggressive scans can be nice when other scans just aren't getting the output that you want and you really don't care how 'loud' you are, what is the switch for enabling this?

    > `-A`

10. How do I set the timing to the max level, sometimes called 'Insane'?

    > `-T5`

11. What about if I want to scan a specific port?

    > `-p`

12. How about if I want to scan every port?

    > `-p-`

13. What if I want to enable using a script from the nmap scripting engine? For this, just include the first part of the switch without the specification of what script to run.

    > `--script`

14. What if I want to run all scripts out of the vulnerability category?

    > `--script vuln`

15. What switch should I include if I don't want to ping the host?

    > `-Pn`

### Nmap Scanning

Perform some basic nmap scanning and learn to read through the results

1. Let's go ahead and start with the basics and perform a syn scan on the box provided. What will this command be without the host IP address?

    > `nmap -sS`

    ```
    Starting Nmap 7.80 ( https://nmap.org ) at 2020-09-10 02:12 EDT
    Nmap scan report for 10.10.187.161
    Host is up (0.035s latency).
    Not shown: 998 closed ports
    PORT   STATE SERVICE
    22/tcp open  ssh
    80/tcp open  http

    Nmap done: 1 IP address (1 host up) scanned in 1.02 seconds
    ```

2. After scanning this, how many ports do we find open under 1000?

    > `2`

3. What communication protocol is given for these ports following the port number?

    > `tcp`

4. Perform a service version detection scan, what is the version of the software running on port 22?

    `sudo nmap -sS -sV $IP`

    ```
    Starting Nmap 7.80 ( https://nmap.org ) at 2020-09-10 02:14 EDT
    Nmap scan report for 10.10.187.161
    Host is up (0.065s latency).
    Not shown: 998 closed ports
    PORT   STATE SERVICE VERSION
    22/tcp open  ssh     OpenSSH 6.6.1p1 Ubuntu 2ubuntu2.10 (Ubuntu Linux; protocol 2.0)
    80/tcp open  http    Apache httpd 2.4.7 ((Ubuntu))
    Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

    Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
    Nmap done: 1 IP address (1 host up) scanned in 8.86 seconds
    ```

    > `6.6.1p1`

5. Perform an aggressive scan, what flag isn't set under the results for port 80?

    `sudo nmap -A -sS -sV $IP`

    ```
    Starting Nmap 7.80 ( https://nmap.org ) at 2020-09-10 02:15 EDT
    Nmap scan report for 10.10.187.161
    Host is up (0.034s latency).
    Not shown: 998 closed ports
    PORT   STATE SERVICE VERSION
    22/tcp open  ssh     OpenSSH 6.6.1p1 Ubuntu 2ubuntu2.10 (Ubuntu Linux; protocol 2.0)
    | ssh-hostkey: 
    |   1024 65:d2:54:74:02:d9:8f:44:c2:f1:0c:04:bd:c2:cc:6c (DSA)
    |   2048 2b:6e:c1:a2:05:ee:8b:55:84:c4:06:a2:62:1d:38:c9 (RSA)
    |   256 dd:22:8a:1a:bd:02:e4:de:54:66:6a:bf:7c:2a:13:4b (ECDSA)
    |_  256 05:d5:4d:63:0c:10:58:ba:75:7a:b4:d6:08:1f:3f:7c (ED25519)
    80/tcp open  http    Apache httpd 2.4.7 ((Ubuntu))
    | http-cookie-flags: 
    |   /: 
    |     PHPSESSID: 
    |_      httponly flag not set
    | http-robots.txt: 1 disallowed entry 
    |_/
    |_http-server-header: Apache/2.4.7 (Ubuntu)
    | http-title: Login :: Damn Vulnerable Web Application (DVWA) v1.10 *Develop...
    |_Requested resource was login.php
    No exact OS matches for host (If you know what OS is running on it, see https://nmap.org/submit/ ).
    TCP/IP fingerprint:
    OS:SCAN(V=7.80%E=4%D=9/10%OT=22%CT=1%CU=43388%PV=Y%DS=2%DC=T%G=Y%TM=5F59C48
    OS:7%P=x86_64-pc-linux-gnu)SEQ(SP=107%GCD=1%ISR=10C%TI=Z%CI=I%II=I%TS=8)OPS
    OS:(O1=M508ST11NW6%O2=M508ST11NW6%O3=M508NNT11NW6%O4=M508ST11NW6%O5=M508ST1
    OS:1NW6%O6=M508ST11)WIN(W1=68DF%W2=68DF%W3=68DF%W4=68DF%W5=68DF%W6=68DF)ECN
    OS:(R=Y%DF=Y%T=40%W=6903%O=M508NNSNW6%CC=Y%Q=)T1(R=Y%DF=Y%T=40%S=O%A=S+%F=A
    OS:S%RD=0%Q=)T2(R=N)T3(R=N)T4(R=Y%DF=Y%T=40%W=0%S=A%A=Z%F=R%O=%RD=0%Q=)T5(R
    OS:=Y%DF=Y%T=40%W=0%S=Z%A=S+%F=AR%O=%RD=0%Q=)T6(R=Y%DF=Y%T=40%W=0%S=A%A=Z%F
    OS:=R%O=%RD=0%Q=)T7(R=Y%DF=Y%T=40%W=0%S=Z%A=S+%F=AR%O=%RD=0%Q=)U1(R=Y%DF=N%
    OS:T=40%IPL=164%UN=0%RIPL=G%RID=G%RIPCK=G%RUCK=G%RUD=G)IE(R=Y%DFI=N%T=40%CD
    OS:=S)

    Network Distance: 2 hops
    Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

    TRACEROUTE (using port 111/tcp)
    HOP RTT      ADDRESS
    1   34.14 ms 10.8.0.1
    2   34.61 ms 10.10.187.161

    OS and Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
    Nmap done: 1 IP address (1 host up) scanned in 24.09 seconds
    ```

    > `httponly`

6. Perform a script scan of vulnerabilities associated with this box, what denial of service (DOS) attack is this box susceptible to? Answer with the name for the vulnerability that is given as the section title in the scan output. A vuln scan can take a while to complete. In case you get stuck, the answer for this question has been provided in the hint, however, it's good to still run this scan and get used to using it as it can be invaluable.

    `nmap --script vuln $IP`

    ```
    Starting Nmap 7.80 ( https://nmap.org ) at 2020-09-10 02:24 EDT
    Nmap scan report for 10.10.187.161
    Host is up (0.054s latency).
    Not shown: 998 closed ports
    PORT   STATE SERVICE
    22/tcp open  ssh
    |_clamav-exec: ERROR: Script execution failed (use -d to debug)
    80/tcp open  http
    |_clamav-exec: ERROR: Script execution failed (use -d to debug)
    | http-cookie-flags: 
    |   /: 
    |     PHPSESSID: 
    |       httponly flag not set
    |   /login.php: 
    |     PHPSESSID: 
    |_      httponly flag not set
    |_http-csrf: Couldn't find any CSRF vulnerabilities.
    |_http-dombased-xss: Couldn't find any DOM based XSS.
    | http-enum: 
    |   /login.php: Possible admin folder
    |   /robots.txt: Robots file
    |   /config/: Potentially interesting directory w/ listing on 'apache/2.4.7 (ubuntu)'
    |   /docs/: Potentially interesting directory w/ listing on 'apache/2.4.7 (ubuntu)'
    |_  /external/: Potentially interesting directory w/ listing on 'apache/2.4.7 (ubuntu)'
    | http-slowloris-check: 
    |   VULNERABLE:
    |   Slowloris DOS attack
    |     State: LIKELY VULNERABLE
    |     IDs:  CVE:CVE-2007-6750
    |       Slowloris tries to keep many connections to the target web server open and hold
    |       them open as long as possible.  It accomplishes this by opening connections to
    |       the target web server and sending a partial request. By doing so, it starves
    |       the http server's resources causing Denial Of Service.
    |       
    |     Disclosure date: 2009-09-17
    |     References:
    |       https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2007-6750
    |_      http://ha.ckers.org/slowloris/
    |_http-stored-xss: Couldn't find any stored XSS vulnerabilities.

    Nmap done: 1 IP address (1 host up) scanned in 323.65 seconds
    ```

    > `http-slowloris-check`

