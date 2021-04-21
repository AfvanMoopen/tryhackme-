# NIS - Linux Part I

Enhance your Linux knowledge with this beginner friendly room!

[NIS - Linux Part I](https://tryhackme.com/room/nislinuxone)

## Topic's

* Linux Fundamentals

## Task 1 What is this room about?

In this task, we will be looking back at ZTH Linux and a few other topics that seem to cause some trouble around the beginners. A requirement for this room is to finish the Learn Linux room - [https://tryhackme.com/room/zthlinux](https://tryhackme.com/room/zthlinux).

As it covers all the basic requirements and this is just a follow up to it in order to strengthen the understanding you gained throughout the room. In order to do so.
Below I will be asking a few questions related to that room, so please, make sure to complete it first :). If you didn't feel free to go through the tasks and come back to this once you finished the room.

The commands you are allowed to use in this room are:

* cat
* tac
* head
* tail
* xxd
* base64
* find
* grep
* echo
* xargs
* hexeditor
* tar
* gzip
* 7zip
* binwalk

Bear in mind, commands such as cd are not allowed.

*The SSH credentials are chad:Infinity121*

[Learn Linux](../Learn%20Linux/README.md)

What is shiba3's password?

`happynootnoises`

What is shiba4's password?

`test1234`

What is the root.txt flag?

`ad91979868d06e19d8e8a9c28be56e0c`

## Task 2 ls

This task should give you a better understanding of the command ls and a few of the switches that the command can take and what are some of the more efficient ones. Below is a screenshot of the help menu, however, feel free to use the man.

![](https://i.imgur.com/wtqmO0Y.png)

Hopefully, the above screenshot should help you go through a few of the tasks below, however further research is required. A good thing to know is that ls supports multiple ways of chaining switches. Such as:

* ls -x -y -z
* ls -xyz

In some cases, you would need to keep evidence of your findings. Below we will start with some basic commands you should be familiar with.

How do you run the ls command?

`ls`

How do you run the ls command to show all the files inside the folder?

`ls -a`

How do you run the ls command to not show the current directory and the previous directory in the output? (almost everything)

`ls -A`

How do you show information the information in a long listing format using ls?

`ls -l`

How do you show the size in readable format? e.g. k, Mb, etc

`ls -h`

How do you do a recursive ls?

`ls --recursive`

How many files did you locate in the home folder of the user?(non-hidden and not inside other folders)

```
chad@flamenco:~$ ls -lA
total 100
-rw-r----- 1 root base64     26 Aug  1 14:10 base64.txt
lrwxrwxrwx 1 root root        9 Aug  1 15:44 .bash_history -> /dev/null
-rw-r--r-- 1 chad chad      220 Apr  4  2018 .bash_logout
-rw-r--r-- 1 root root       27 Aug  1 14:01 .bashrc
drwxr-xr-x 2 root root     4096 Aug  3 17:14 bin
drwxrwxr-x 5 chad chad     4096 Aug  1 19:01 .binwalk
-rw-r----- 1 root binwalk 20118 Aug  1 18:57 binwalk.png
drwx------ 2 chad chad     4096 Aug  1 14:18 .cache
-rw-r----- 1 root cat        26 Aug  1 14:04 cat.txt
drwx------ 3 chad chad     4096 Aug  1 14:18 .gnupg
-rw-r----- 1 root grep       84 Aug  1 14:54 grep1.txt
-rw-r----- 1 root grep      224 Aug  1 14:53 grep.txt
-rw-r----- 1 root gz         46 Aug  2 10:40 gzip.txt.gz
-rw-r----- 1 root head       26 Aug  1 14:06 head.txt
-rw-r--r-- 1 chad chad      807 Apr  4  2018 .profile
-rw-r----- 1 root tac        26 Aug  1 14:05 tac.txt
-rw-r----- 1 root tail       26 Aug  1 14:07 tail.txt
-rw-r----- 1 root tar     10240 Aug  2 09:25 tarball.tar
-rw-r----- 1 root xxd        26 Aug  1 14:09 xxd.txt
-rw-r----- 1 root 7z        142 Aug  2 10:57 zip.7z
```

`13`

## Task 3 cat

The cat command is one of the most common Linux commands that people use, however, in some instances, the cat command cannot be used as it's removed.

Below is a screenshot of the cat command's help menu.

![](https://i.imgur.com/lOqBTdk.png)

But, as we are professionals we know about a few alternatives of going around it:

The first command we are going to learn about is **tac**. Yes, cat spelt backwards. It is similar to the command, with the downside of less functionality.

![](https://i.imgur.com/uy57zbm.png)

Thus being a good tool to add to your toolbelt when you are limited by your reverse shell.


Another tool that can be used is **head**. This is usually used to get the beginning part of a file, however, you can use it to your heart's content and grab as many lines as you want.

![](https://i.imgur.com/Z0PisaW.png)

One more tool that can be used to grab the content of a file is **tail**. This is similar to the head command, however, as the name implies it will grab the last part of a file.

![](https://i.imgur.com/1v44ORA.png)

Another useful command is **xxd**. this can be used to generate a hex dump of the content of a file. Then, if you want you can either just read the text from the right-hand side or convert from hex to ASCII.

![](https://i.imgur.com/m0Mi60o.png)

Similar to the above you can use the **base64** command to convert the text to base64 and then convert it back to ASCII.

![](https://i.imgur.com/Qq7UkLg.png)

What is the content of cat.txt?

```
chad@flamenco:~$ cat cat.txt
THM{11adbee391acdffee901}
```

`THM{11adbee391acdffee901}`

What is the content of tac.txt?

```
chad@flamenco:~$ cat tac.txt
THM{acab0111aaa687912139}
```

`THM{acab0111aaa687912139}`

What is the content of head.txt?

```
chad@flamenco:~$ cat head.txt
THM{894abac55f7962abc166}
```

`THM{894abac55f7962abc166}`

What is the content of tail.txt?

```
chad@flamenco:~$ cat tail.txt
THM{1689acafdd20751acff6}
```

`THM{1689acafdd20751acff6}`

What is the content of the xxd.txt?

```
chad@flamenco:~$ cat xxd.txt
THM{fac1aab210d6e4410acd}
```

What is the content of base64.txt?

```
chad@flamenco:~$ cat base64.txt
THM{aa462c1b2d44801c0a31}
```

`THM{aa462c1b2d44801c0a31}`

## Task 4 find

The find command is one of the most useful commands on a Linux operating system.

![](https://i.imgur.com/c763tFd.png)

This command can help us find specific files that match a pattern like: `find . -name *.txt`

Or we can use it to find files that have a specific extension: `find / -type f -name "*.bak"`

This simple command will start browsing the machine directory, finding all the files with extension .bak (backup).

![](https://i.imgur.com/TEw03hT.png)

But we can also use it to find files that have the SUID or SGID bit set like so: `find / -type f \( -perm -4000 -o -perm -2000 \) -exec ls -l {} \;`

This command combines permissions 4000 (SUID) and 2000 (SGID)

![](https://i.imgur.com/XmVTUyN.png)

How many .txt files did you find in the current folder?

```
chad@flamenco:~$ find ~ -type f -name "*.txt"
/home/chad/head.txt
/home/chad/grep.txt
/home/chad/base64.txt
/home/chad/tac.txt
/home/chad/tail.txt
/home/chad/cat.txt
/home/chad/grep1.txt
/home/chad/xxd.txt
```

`8`

How many SUID files have you found inside the home folder?

```
chad@flamenco:~$ find ~ -type f -perm -4000;
chad@flamenco:~$ 
```

`0`

## Task 5 grep

grep is a really useful command to grab text from files.

![](https://i.imgur.com/1WjzMSy.png)

Let's read through a few examples of grep commands and see how we can use them for our own benefit in a scenario.

`grep "word" file`

![](https://i.imgur.com/gfLmRxB.png)

Grep not only allows us to check if a certain word exists in the file but also outputs us the context in which the word had appeared. As you can see on the screenshot above, we were able to find an exact match to the word 'if' in the file script.py.

We can also compare two files with similar names using.

`grep "word" file*`

![](https://i.imgur.com/FLSIDGF.png)

How many times does the word "hacker" appear in the grep files? (including variations)

```
chad@flamenco:~$ grep "hack" grep*
grep1.txt:hacker hacker hacker hacker hacker hacker hacker hacker hacker hacker
grep.txt:A hacker is bad, especially the one that think that certain actions can go unnoticed.
grep.txt:A hacker always hacsk for money and meaningless junk.
grep.txt:A hacker is bad. Very, very bad.
grep.txt:Why would you be a hacker?
grep.txt:Don't be the hackerman.
```

`15`

## Task 6 sudo

sudo command allows certain users to execute a command as another user, according to settings in the /etc/sudoers file. By default, sudo requires that users authenticate themselves with a password of another user.

In the real-life scenario, sudo is mostly used to switch to root account and gain an ability to fully interact with the system.

![](https://i.imgur.com/CN0ckiJ.png)

`sudo -l` appears to be the most commonly used switch. It can always tell you which commands are you allowed to run as another user on the following system, and in some cases, can give you a clue to root access.

Is the user allowed to run the above command? (Yay/Nay)

```
chad@flamenco:~$ sudo -l
-rbash: /usr/lib/command-not-found: restricted: cannot specify `/' in command names
```

`Nay`

## Task 7 chmod

The chmod command sets the permissions of files or directories.

![](https://i.imgur.com/Ghyg1lm.png)

Those permissions are divided between three main characters:

1. User
2. Group
3. Other

All of them can rather read, write or execute a file. Permission to do so can be granted using chmod.

It can be done rather using letter notation or numerical values.

Let's take a look at the following command: `chmod u=rwx,g=rx,o=rw myfile`

* u = user is being giver read, write and execute permission
* g = group can now read and execute
* o = other can read and write

This long notion can be eliminated by numerical values for permission. There are exactly four of them:

* 0 stands for "no permission."
* 1 stands for "execute";
* 2 stands for "write";
* 4 stands for "read".

Those values can be easily combined by adding them up.

For example, permission to read, write and execute would be 7 (1 + 2 + 4).

`chmod 777 file`

The following command will grant full file access to everyone on the system. (Those numerical values can be easily calculated using an interactive [chmod-calculator](http://chmod-calculator.com/)).

**chmod** command comes in handy with ssh key files (id_rsa). By editing their permissions to 'user read-write only' we can use other people's id_rsa files to connect via ssh.

`chmod 600 id_rsa`

Read the above.

`No answer needed`

## Task 8 echo

echo is the most fundamental command found in most operating systems. It used to prints text to standard output, for example, terminal. It is mostly used in bash scripts in order to display output directly to user's console.

![](https://i.imgur.com/coZ6KbD.png)

echo can also be used to interact with other system commands and pass some value to them.

![](https://i.imgur.com/g8qlJV0.png)

echo also has a small trick which allows to print out any command output to console.

`echo "$( [command] )"`

What command would you use to echo the word "Hackerman" ?

`echo "Hackerman"`

## Task 9 xargs

xargs command builds and executes command lines from standard input. It allows you to run the same command on a large number of files.

![](https://i.imgur.com/aeF8ODy.png)

xargs is often used with the find command, in order to easily interact with its input.

Let's take a look at the given command: `find /tmp -name test -type f -print | xargs /bin/rm -f`

On the left side, we can see a command which should technically display all files under a name 'test'. xargs command on the left allows us to execute rm (remove) on those files and easily delete all of them.

Same can be done with reading all the files under the name 'test'.

How would you read all files with extension .bak using xargs?

`find / -name *.bak -type f -print | xargs /bin/cat`

## Task 10 hexeditor

Hexeditor is an awesome tool designed to read and modify hex of a file, this comes in handy especially when it comes to troubleshooting magic numbers for files such as JPG, WAV and any other types of files. This tool is also helpful when it comes to CTFs and text is hidden inside a file or when the magic number of a file was altered.

Another tool that is good for this kind of scenarios is called `strings` but we won't be talking about it in this part of our course.

![](https://i.imgur.com/qMRgTHV.png)

For this task, I will be providing you with resources to help you along your journey around challenges you might be facing in which you need the hexeditor tool.

A few resources I use for tasks that involve analysing files and fixing the magic number I use the following resources:

[https://en.wikipedia.org/wiki/List_of_file_signatures](https://en.wikipedia.org/wiki/List_of_file_signatures)

[https://gist.github.com/leommoore/f9e57ba2aa4bf197ebc5](https://gist.github.com/leommoore/f9e57ba2aa4bf197ebc5)

[https://www.garykessler.net/library/file_sigs.html](https://www.garykessler.net/library/file_sigs.html)

Read the above.

`No answer needed`

## Task 11 curl

The curl command transfers data to or from a network server, using one of the supported protocols (HTTP, HTTPS, FTP, FTPS, SCP, SFTP, TFTP, DICT, TELNET, LDAP or FILE). It is designed to work without any user interaction, so could be ideally used in a shell script.

curl is a huge tool with a lot of switches and possibilities. Let's take a look at some of the most important ones.

`curl http://www.ismycomputeron.com/`

![](https://i.imgur.com/57RQsie.png)

The most basic command. Fetches data from the website using the HTTP protocol, and display it using standard HTML code. This is essentially the same as "viewing the source" of the webpage.

The following command will limit the connection speed to 1,234 bytes/second: `curl --limit-rate 1234B http://www.ismycomputeron.com/`

Another example is saving the output to a file using either:

-o to save the file under a different name `curl -o loginpage.html https://tryhackme.com/login`

-O to save the file under the same name: `curl -O https://tryhackme.com/login`

Or, you might be interested in fetching the headers? `curl -I -s https://tryhackme.com`

How would you grab the header of https://tryhackme.com but grepping only the HTTP status code?

```
kali@kali:~/CTFs/tryhackme/NIS - Linux Part I$ curl -I -s https://tryhackme.com | grep HTTP
HTTP/2 200
```

`curl -I -s https://tryhackme.com | grep HTTP`

## Task 12 wget

The **wget** command downloads files from HTTP, HTTPS, or FTP connection a network.

![](https://i.imgur.com/dp9xFVk.png)

`wget http://somewebsite.com/files/images.zip`

Adding a **-b** switch will allow us to run wget in the background and return the terminal to its initial state.

`wget -b http://www.example.org/files/images.zip`

![](https://i.imgur.com/hDflpWK.png)

What command would you run to get the flag.txt from https://tryhackme.com/ ?

`wget https://tryhackme.com/flag.txt`

What command would you run to download recursively up to level 5 from https://tryhackme.com ?

`wget -r -l 5 https://tryhackme.com`

## Task 13 tar

**tar** is a command that allows creating, maintain, modify, and extracts files that are archived in the tar format.

![](https://i.imgur.com/2S09djq.png)

The most common example for tar extraction would be: `tar -xf archive.tar`

-x tells tar to extract files from an archive.

-f tells tar that the next argument will be the name of the archive to operate on.

What is the flag from the tar file?

```
chad@flamenco:~$ tar -xf tarball.tar
chad@flamenco:~$ cat flag.txt
THM{C0FFE1337101}
```

`THM{C0FFE1337101}`

## Task 14 gzip

**gzip** - a file format and a software application used for file compression and decompression. gzip-compressed files have .gz extension.

![](https://i.imgur.com/SefVKtG.png)

A gzip file can be decompressed using a simple `gzip -d file.gz` command, where -d stands for decompress.

What is the content of gzip.txt?

```
chad@flamenco:~$ gzip -d gzip.txt.gz
chad@flamenco:~$ cat gzip.txt
THM{0AFDECC951A}
```

`THM{0AFDECC951A}`

## Task 15 7zip

7-Zip is a free and open-source file archiver, a utility used to place groups of files within compressed containers known as "archives".

7z is as simple as the gzip or tar and you can use the following command:

`7z x file.zip` to extract the file

This tool comes in handy as it works with a lot more file extensions than other tools. You name the achieve extension and 7z should be the tool for you.

What is the flag inside the 7zip file?

```
chad@flamenco:~$ 7z x zip.7z

7-Zip [64] 16.02 : Copyright (c) 1999-2016 Igor Pavlov : 2016-05-21
p7zip Version 16.02 (locale=en_US.UTF-8,Utf16=on,HugeFiles=on,64 bits,1 CPU Intel(R) Xeon(R) CPU E5-2676 v3 @ 2.40GHz (306F2),ASM,AES-NI)

Scanning the drive for archives:
1 file, 142 bytes (1 KiB)

Extracting archive: zip.7z
--
Path = zip.7z
Type = 7z
Physical Size = 142
Headers Size = 122
Method = LZMA2:12
Solid = -
Blocks = 1

Everything is Ok

Size:       16
Compressed: 142
chad@flamenco:~$ cat 7zip.txt
THM{526accdf94}
```

`THM{526accdf94}`

## Task 16 binwalk

**binwalk** allows users to analyze and extract firmware images and helps in identifying code, files, and other information embedded in those, or inside another file, taking as an example steganography.

![](https://i.imgur.com/V8OySd8.png)

A simple command such as `binwalk file` allows us to perform a simple file scan and identify code information.

`binwalk -e` file allows us to extract files from firmware. This method is usually used in CTFs, where some important information can be hidden within the file.

`binwalk -Me` file does the same as `-e`, but recursively.

![](https://i.imgur.com/uZn3i9j.png)

What is the content of binwalk.txt?

```
chad@flamenco:~$ binwalk -e binwalk.png

DECIMAL       HEXADECIMAL     DESCRIPTION
--------------------------------------------------------------------------------
0             0x0             PNG image, 824 x 461, 8-bit/color RGBA, non-interlaced
78            0x4E            Zlib compressed data, best compression
20066         0x4E62          gzip compressed data, fastest compression, has original file name: "binwalk.txt", from Unix, last modified: 2020-08-01 18:54:33
chad@flamenco:~$ cat _binwalk.png.extracted/binwalk.txt
THM{af5548a12bc2de}
```

`THM{af5548a12bc2de}`
