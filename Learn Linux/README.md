# Learn Linux

A guided room designed to teach you the Linux basics!

[Learn Linux](https://tryhackme.com/room/zthlinux)

## Topic's

* Linux Fundamentals

## Intro

This room is designed to teach you about Linux concepts, and tools.

Because of this, this room expects no prior knowledge. The only expectation this room has is an eagerness to learn, and a willingness to google if you're stuck :).

This room has a natural flow to it; however, if you are experienced in Linux, and just want a refresher on a specific topic, you can jump around as need be.

1. Read the above.

`No answer needed`

2. Deploy the machine attached to this task!

`No answer needed`

## Methodology 

After careful consideration, I've deemed the best way to go about this is to introduce various concepts in sections, with each section being more complex and requiring knowledge from the previous section. To better enable the transition between section, I've split each section into different users in the VM; when you finish a section you'll have to complete a challenge and then you'll be able to move onto the next section.

`No answer needed`

## [Section 1: SSH] - Intro 

SSH is the act of remotely accessing a machine. SSH allows you to run commands interactively on the remote machine. This is done through the use of a program on the target machine, which allows the ssh client to interface with the target host.

While the most common usage of a regular operating system is graphical(allowing you to see pictures, web browsers, file managers etc.) SSH works through a command line, meaning anything done on the target machine will be done through a command prompt similar to this.

![https://i.imgur.com/4QD2LNB.png](https://i.imgur.com/4QD2LNB.png)

It may look intimidating at first, but you'll soon find out you can do much of the same functionality that you're able to do using graphical user interfaces!

It is an invaluable tool, and how you will be accessing this machine to learn and to do the challenges. Depending on the operating system there are different ways of SSHing into a machine. This section will purely focus on the windows way(PuTTY), and after we learn more about linux commands, and how they work, we'll return back to this section and learn about the linux method.

1. Read the above.

`No answer needed`

## [Section 1: SSH] - Putty and ssh 

Disclaimer: please do not use putty if you are already on Linux. Use the instructions for the ssh binary down below.

The download for putty can be found [here](https://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html), once you download it go through the install process. Once you've installed it, open it and you should see this screen

![https://i.imgur.com/LN2dIZG.png](https://i.imgur.com/LN2dIZG.png)

The field that we are most interested in is "Host Name (or IP Address)". This is where the `MACHINE_IP` that you got when you deployed the machine comes into play. That's because the format of SSH connections is <user>@<host>, and in this case host is equal to `MACHINE_IP`. The user for this trial will be shiba1, so the completed "Host Name" field should like this.

![https://i.imgur.com/ADpZspQ.jpg](https://i.imgur.com/ADpZspQ.jpg)

(Note: the 10.10.10.10 is just an example, and you should replace that with `MACHINE_IP`)

From there click open, and you'll be greeted with 

![https://i.imgur.com/nlzlrun.jpg](https://i.imgur.com/nlzlrun.jpg)

Click yes(this prompt is purely for verification purposes) and you'll be prompted for a password:

![](https://i.imgur.com/9mMhNv4.jpg)

Enter "**shiba1**" and click enter, and you'll have logged in!

![](https://i.imgur.com/v0N82PO.jpg)

Note: sometimes putty just may not work, in that case follow the ssh binary guide listed below!

As an alternative to putty, you may have an ssh binary on your computer. That binary is accessed by going to your terminal(cmd/MacOS Terminal), and typing ssh.

![](https://i.imgur.com/SNNCMwf.jpg)

The syntax on how to use this command is `ssh <user>@<host>`. So to ssh into the machine you'll need to type in `ssh shiba1@MACHINE_IP`. It will prompt you for the user password, which in this case is also shiba1. 

![](https://i.imgur.com/VO2Q6qc.png)

That's it, you can now run commands interactively!!! :)))

1. SSH into the server

`No answer needed`

## [Section 2: Running Commands] - Basic Command Execution

Now that you've logged into the server, you're gonna wanna do things, and everything that can be done over SSH is done by running commands. To start out, we'll take a look at some of the basic commands, and the first command will be `echo`. Type `echo hello`, and press enter and you'll see your input echoed back at you.

![](https://i.imgur.com/QGPXIFc.jpg)

Much like the word it's named after, `echo` returns whatever is inputted into it. Congratulations, you've just ran your first Linux command!

1. Read the above

`No answer needed`

## [Section 2: Running Commands] - Manual Pages and Flags

Most of the commands you'll learn about have a lot of options that are not immediately known at first glance, these options are known as flags, and have the format `<command> <flag> <input>`. These flags can be learned about using the `man` command. The man command has the format `man <command>`. Therefore, to learn about flags for the echo command, we would type `man echo`. Typing that shows us a nicely formatted document:

![](https://i.imgur.com/FixWl4Y.jpg)

We get alot of information, but the flags are stored in the description section. For example the flag to remove the newline is -n. So to output "`Shiba`" without the newline you would type `echo -n Shiba`.        

Note: Some commands support the -h flag, meaning you can type `<command> -h` and get a list of flags and other useful information without using man.

1. How would you output hello without a newline

`echo -n hello`

## [Section 3: Basic File Operations] - ls

ls is a command that lists information about every file/directory in the directory. Just running the ls command outputs the name of every file in the directory.

![](https://i.imgur.com/AMpO7qK.jpg)

As with other commands ls has many flags that can manipulate the output.  For example `ls -a` shows all files/directories including ones that start with `.`

![](https://i.imgur.com/Wp9M8pF.jpg)

1. What flag outputs all entries

`-a`

2. What flag outputs things in a "long list" format 

`-l`

## [Section 3: Basic File Operations] - cat

cat short for concatenate, does exactly that, it outputs the contents of files to the console. For example, given a file called a.txt which contains the data "hello", cat `a.txt` would output hello.

![](https://i.imgur.com/fFfe9tz.jpg)

Note: cat supports the --help flag meaning that you can see useful flags without going to the man page!

![](https://i.imgur.com/vzjsPck.jpg)

1. What flag numbers all output lines? 

`-n`

## [Section 3: Basic File Operations] - touch

touch is a pretty simple command, it creates files. Given the command touch b.txt, b.txt would be created.

![](https://i.imgur.com/4Yw3YKY.png)

1. Read the above!

`No answer needed`

## [Section 3: Basic File Operations] - Running A Binary

Occasionally there will be times when you want to run downloaded or user created programs. This is done by providing the full path to the binary, for example say you download a binary that outputs noot, providing the full path to that binary will execute it. 

![](https://i.imgur.com/PKDp1eF.jpg)

Note: A binary is just executable code, think a windows exe file

This seems like a good time to mention Relative Paths! Every time you intend on running the binary, you don't need to provide a full path, you can use Relative Paths.

Relative Paths:

The chart below will assume the current path is /tmp/aa

| Relative Path	| Meaning | Absolute Path | Relative Path | Running a binary with a Relative Path | Running A Binary with an Absolute Path |
|---|---|---|---|---|---|---|
| . | Current Directory | /tmp/aa | . | ./hello | /tmp/aa/hello |
| .. | Directory before the current directory	/tmp | .. | ../hello | /tmp/hello |
| ~ | The user's home directory | /home/<current user> | ~ | ~/hello | /home/<user>/hello |

These shortcuts are incredibly efficient, and save time. These shortcuts for every command, so if I were to run `ls .` it would be the same as running `ls <current directory>`

![](https://i.imgur.com/hfre7mJ.jpg)

1. How would you run a binary called hello using the directory shortcut . ?

`./hello`

2. How would you run a binary called hello in your home directory using the shortcut ~ ?

`~/hello`

3. How would you run a binary called hello in the previous directory using the shortcut .. ?

`../hello`

## Binary - Shiba1

Now that you've learned basic file operations, you can solve the first challenge! This challenge is pretty simple, create a file called noot.txt.

Once you're done run the binary and you'll be given the password for the user shiba2!

Note: the name of the binary is shiba1, as shown in the title

1. What's the password for shiba2

`pinguftw`

## su

Now that we have our next user password, it seems like a good time to cover su. su is a command that allows you to change the user, without logging out and logging back in again. For example if you wanted to switch to shiba2 while you're the user shiba1, you would type `su shiba2` . You would then be prompted for a password and if you entered shiba2's password you would then become shiba2

![](https://i.imgur.com/iZhPtJf.jpg)

Note: Typing su on its own is equivalent to typing `su root`.

1. How do you specify which shell is used when you login?

`-s`

## [Section 4 - Linux Operators]: Intro

This section will cover the operators most commonly used to interact with programs. The operators that will be covered in this section are ">, >>, &, &&, and $" Over the following tasks you will come to learn what each of these operators do.

1. Read the above

`No answer needed`

## [Section 4: Linux Operators]: ">"

`>` is the operator for output redirection. Meaning that you can redirect the output of any command to a file. For example if I were to run `echo hello > file`, then instead of outputting hello to the console, it would save that output to a file called file.

![](https://i.imgur.com/subOGoB.jpg)

It is worth noting that if you were to use this operator on a file that already exists, it would completely erase the contents of that file and replace it with the output from your command

![](https://i.imgur.com/qpvaaLO.jpg)

1. How would you output twenty to a file called test

`echo twenty > test`

## [Section 4: Linux Operators]: ">>"

`>>` does mainly the same thing as `>`, with one key difference. `>>` appends the output of a command to a file, instead of erasing it.

![](https://i.imgur.com/QHZHCdt.jpg)

1. Read the above

`No answer needed`

## [Section 4: Linux Operators]: "&&"

`&&` means as you might expect "and". Meaning `&&` allows you to execute a second command after the first one has executed successfully. Meaning `ls && echo hello` will work fine, but `dljahfrsdkjlhfsdhjklfsdhkljfh && echo hello` will fail.

![](https://i.imgur.com/2LcM4I3.jpg)

Note: Since the second command happens after the first command, you can use something created in the first command in the second command.

![](https://i.imgur.com/1XjZbLe.jpg)

1. Read the above

`No answer needed`

## [Section 4: Linux Operators]: "&"

Much unlike &&, & has nothing to do with and at all(try saying that 10 times fast). & is a background operator, meaning say you run a command that takes 10 seconds to run, normally you wouldn't be able to run commands during that period; however, with & that command will still execute and you'll be able to run other commands.

![](https://i.imgur.com/5XPAUBq.jpg)

Note: I can't exactly show time in an image, but trust me I really did wait the 5 seconds :)

1. Read the above

`No answer needed`

## [Section 4: Linux Operators]: "$"

The $ is an unusually special operator, as it is used to denote environment variables. These are variables set by the computer(you can set them yourself but we'll get into that) that are used to affect different processes and how they work. Meaning that if you edit these variables you can change how certain processes work on your computer. For example your current user is always stored in an environment variable called $USER. You can view these variables with the echo command.

![](https://i.imgur.com/bEGpRfG.jpg)

Naturally this means environment variables can be used as input for other commands as well, for example say I wanted to create a file which is the name of our current user, I could do `touch $USER`.

![](https://i.imgur.com/81jNcME.jpg)

Recall that the >> operator appends output to a file.

Environment variables can also be set pretty easily, just running `export <varname>=<value>` will set that as an environment variable

![](https://i.imgur.com/qjCpT08.jpg)

1. How would you set nootnoot equal to 1111

`export nootnoot=1111`

2. What is the value of the home environment variable

`/home/shiba2`

## [Section 4: Linux Operators]: "|"

Continuing with the trend of very special operators, we have the pipe. The pipe is unique because while operators like >> allow you to store the output of a command, the | operator allows you to take the output of a command and use it as input for a second command.

For example, I can use `cat` to get the output of a file, and pipe that into `grep` to search for a specific string(Note: We will learn more about grep later, but for now just know that it's a command used to find specific strings in an input).  

![](https://i.imgur.com/psOTao5.jpg)

It is worth noting that not all commands support the pipe, and some that do support it require you to use - instead of input, for example `cat -`. So always check to see if the command does support it

1. Read the above!

`No answer needed`

## [Section 4: Linux Operators] - ";"

The ; operator works a lot like &&, however it does not require the first command to execute successfully. This means that you can do `dkhsgffgsafgfasdgfasfghkgdsgfs; ls` and you would still see the output of ls.

![](https://i.imgur.com/3FjiVnU.jpg)

1. Read the above.

`No answer needed`

## Binary - shiba2

This challenge is pretty simple. The binary is checking to see if the environment variable "test1234" exists, and if it's set equal to the current $USER environment variable.

1. What is shiba3's password

`happynootnoises`

## [Section 5 - Advanced File Operations]: Intro

Much like windows, files have a lot of complexity to them. Multiple different parameters have to be modified to allow certain users to read to files, write to files, and execute certain files. This section will cover modifying these parameters.

1. Read the above.

`No answer needed`

## [Section 5 - Advanced File Operators]: A bit of background.

Recall that ls has different flags that allow you to view information about different types of files.

![](https://i.imgur.com/uqIivIH.jpg)

This image has all of the attributes that will be covered in this section. More specifically we're interested in these three.

![](https://i.imgur.com/eetqAI3.jpg)

These attributes are(in order) the file permissions, owner of the file, and group that the file is in.

The next few tasks will go over the command to modify these attributes.

1. Read the above!

`No answer needed`

## [Section 5: Advanced File Operations]: chmod

chmod allows you to set the different permissions for a file, and control who can read it. The syntax of this command is typically `chmod <permissions> <file>` . 

The interesting part is how the permissions are set. They're set using a three digit number, where each digit controls a specific permission, meaning the first digit controls the permissions for a user, the second digit controls the permission for a group, the third digit controls permissions for everyone that's not a part of the user or group.

Now the value of the digits control whether they can read, write or execute it or do all three, and to properly calculate it some math needs to be done.

| Digit | Meaning |
|---|---|
| 1 | That file can be executed |
| 2 | That file can be written to |
| 3 | That file can be executed and written to |
| 4 | That file can be read |
| 5 | That file can be read and executed |
| 6 | That file can be written to and read |
| 7 | That file can be read, written to, and executed |

The way these values are calculated is this. The digit 1 means the file can be executed, the digit 2 means it can be written to, and the digit 4 means it can be read. You get the different permissions by adding these digits together. For example 1+2 is 3 meaning that file can be executed and written to. Now let's see how it all works in perspective.

| Command: | Meaning |
|---|---|
| chmod 341 file | The file can be executed and written to by the user that owns the file. The file can be read by the group that owns the file. The file can be executed by everyone else. |
| chmod 777 file | The file can be read, written to, and executed by the user that owns the file. The file can be read, written to, and executed by the group that owns the file. The file can be read, written to, and executed by everyone else |
| chmod 455 | The file can be read by the user that owns the file. The file can be read and executed by the group that owns the file. The file can be read to and executed by everyone else |

ls provides a helpful way of viewing the permissions of files in the current directory.

![](https://i.imgur.com/MPSlodl.jpg)

Recall that file permissions are divided into three sections, user and group and everyone else. The same is true here; however, everything starts from the second hyphen not the first, so we can just forget the first hyphen for now. Note that everything is in sequential order, so the first three characters control permissions for the user, the second three characters control permissions for the group, and the final three characters control permissions for everyone else

![](https://i.imgur.com/ZNaY6Iw.jpg)

(Forgive the artist's rendition. U = user, G = group, E = everyone else)

rw means as you might expect "read and write", meaning the user has read write perms to the file. Following that logic, that means members of the group and everyone else have only read perms. To convert that to numbers the permissions for that file in number form are 644. We can test this by trying to change the permissions

![](https://i.imgur.com/hu9mkJC.jpg)

When we try to change the perms to 644 nothing happens because the perms are already 644. The interesting part is while we can write data to .profile with echo while the perms are 644, we can't when we change the perms to 544, because we took away our own write perms. Following that logic, that means we can completely lock ourselves out of writing to a file we already own!

Note: It is possible to give someone no perms to a file, You can just put 0 as the digit. 770 Means that everyone that isnt a part of the user or group cant do anything to the file.

1. What permissions mean the user can read the file, the group can read and write to the file, and no one else can read, write or execute the file?

`460`

2. What permissions mean the user can read, write, and execute the file, the group can read, write, and execute the file, and everyone else can read, write, and execute the file.

`777`

## [Section 5: Advanced File Operations] - chown

Recall that ls shows us our username twice.

![](https://i.imgur.com/8kYdiUp.jpg)

These attributes are the user, and group attributes resepectively. Recall that we can edit the permissions for these attributes, so it stands to reason that we can also change these attributes. That is done using the chown command, which allows us to change the user and group for any file. The syntax for this command is chown user:group file. For example if we wanted to change the owner of file to shiba2 as well as the group to shiba2, we could use `chown shiba2:shiba2 file`.    

Note: You can only use chown if you are "above" that other user, meaning that chown is best done with the root(administrator) user.

![](https://i.imgur.com/Q0NwmUk.jpg)

You can also use chown without specifying a group. So you can just use chown user file if you only wanted to change the user but keep the group.

![](https://i.imgur.com/kFWp8pL.jpg)

1. How would you change the owner of file to paradox

`chown paradox file`

2. What about the owner and the group of file to paradox

`chown paradox:paradox file`

3. What flag allows you to operate on every file in the directory at once? 

`-R`

## [Section 5: Advanced File Operations] - rm

Let's take a break from all the permissions and math, and look at something that can completely destroy your whole Linux system if used carelessly! rm as you might have guessed means remove, and that's exactly what it does.

![](https://i.imgur.com/UOEEgzo.jpg)

As you can imagine this is incredibly dangerous, as you can remove some very important files, and render your system completely unusable. It is not worth noting that you need write permissions to the file to deleted so you cant just delete any file if you're a regular user.

![](https://i.imgur.com/UkPkxAh.png)

1. What flag deletes every file in a directory

`-r`

2. How do you suppress all warning prompts

`-f`

## [Section 5: Advanced File Operations] - mv

mv allows you to move files from one place to another. The syntax for the command is `mv <file> <destination>`. so if I wanted to move a file to my home directory I could type `mv file ~`.

![](https://i.imgur.com/2TQwVJj.jpg)

Note: You can also use mv to change the name of file, `mv file ~/ghfds` will rename file to ghfds. 

![](https://i.imgur.com/8LhG68n.png)

1. How would you move file to /tmp

`mv file /tmp`

## [Section 5: Advanced File Operations] - cp

cp does mainly the same thing as mv, except instead of moving the file it duplicates(copies) it. The syntax is also the same as mv, meaning the syntax is `cp <file> <destination>` .

![](https://i.imgur.com/B958Kyj.png)

1. 

`No answer needed`

## [Section 5: Advanced file Operations] - cd && mkdir

In windows there are folders. Folders allow you to store multiple files in a single group, which makes them easier to organize and access. Linux has the exact same thing, except their called directories. 

Linux allows you to change the location of the current directory through the use of the cd command. The syntax of the cd command is this, cd <directory>.

![](https://i.imgur.com/4ToyThJ.jpg)

Relative Paths are supported, as well as absolute paths. Our command line provides a nice section for seeing exactly what directory you're in, so you'll never be lost!

This brings us to mkdir, occasionally you'll want to make a new directory to store files in, and that is done using mkdir, the syntax of mkdir is `mkdir <directory name>`.

![](https://i.imgur.com/Wd1D6H4.jpg)

Note: As you might have noticed, ls shows directories aswell

1. Using relative paths, how would you cd to your home directory. 

`cd ~`

2. Using absolute paths how would you make a directory called test in /tmp

`mkdir /tmp/test`

## [Section 5: Advanced File Operations] ln

ln is a weird one, because it has two different main uses. One of those is what's known as "hard linking", which completely duplicates the file, and links the duplicate to the original copy. Meaning What ever is done to the created link, is also done to the original file. The ln syntax is `ln source destination`.

![](https://i.imgur.com/GFFFO2u.jpg)

It's important to be very careful with hard links, as depending on what you're doing it can be very easy to erase data from a file.

The next form of linking is symbolic linking(symlink). While a hard linked file contains the data in the original file, a symbolic link is just a glorified reference. Meaning that the actual symbolic link has no data in it at all, it's just a reference to another file.

The syntax for a symbolic link is the exact same, but it uses the -s flag, so to create a symbolic link, you would run `ln -s <file> <destination>`.

![](https://i.imgur.com/9E6e92K.jpg)

ls even shows that its a symbolic link with the arrow pointing to what the link is referencing. It is important to note the permissions on the symlink. It has full 777 perms meaning that in theory you can execute the symlink, however since it is just a reference, in reality it has the same perms as the original file.

![](https://i.imgur.com/7r8Jmow.jpg)

1. How would I link /home/test/testfile to /tmp/test

`ln /home/test/testfile /tmp/test`

## [Section 5 - Advanced File Operations]: find

find is an incredibly powerful, but incredibly simple command. It allows you to do just as it says, find files. It does this by listing every file in the current directory, so if you ran find /tmp it would list every file in /tmp. 

![](https://i.imgur.com/3zcvXKh.jpg)

It is worth noting that find is recursive, so it searches every directory that is in the original directory you provided. Meaning that if you were to run `find /`, it would list every file on the OS. Another thing worth noting is that it can only list files in directories that you have permission to access, meaning you cant just list every file as every user.

The true power of this command though comes from the parameters you can provide it. You can use `find dir -user` , to list every file owned by a specific user; you can use `find dir -group` to list every file owned by a specific group. The sheer customizability of the command is it's most powerful feature.

![](https://i.imgur.com/Ckqt2ax.jpg)

This is one command I highly recommend reading the manual page on to learn all of it's options. This command is invaluable when working with files.

1. How do you find files that have specific permissions?

`-perm`

2. How would you find all the files in /home

`find /home`

3. How would you find all the files owned by paradox on the whole system

`find / -user paradox`

## [Section 5: Advanced File Operations] - grep

I can say without reservation that grep is one of the most useful commands to learn. It allows you find data inside of data. When working with large files, or a large output, it is arguably the best way to narrow the output down to better find what your looking for. The syntax of the command is `grep <string> <file>` however file is optional if you're using piping.

Note: You can search multiple files at the same time, meaning you can theoretically do `grep <string> <file> <file2>`

For instance let's say you know have the file name of test1234, but you don't know where it is on the system. find can be used to list every file on the OS, and grep can be used to find the actual file.

![](https://i.imgur.com/IR7M08S.jpg)

grep comes and saves the day! What about if you have a bunch of data, and you wanna see if the string hello is in it, and if so what line number it's at.

![](https://i.imgur.com/xPWWXQe.jpg)

I'm sure you can see just how useful this command is. When searching logs for the cause of an error message, when parsing large amounts of data for that specific piece, when searching every file in a directory for that one line that you may need to change.

Another important thing to note is that grep supports regular expressions, you see I wasn't being entirely truthful with you(props if you get the reference) when I says the syntax is `grep <string> <file>`, the syntax is actually `grep <regular expression> <file>`. Unfortunately regular expressions are out of the scope of this room, but I highly encourage you to read up on regular expressions, as they increase the power of grep tenfold.

1. What flag lists line numbers for every string found?

`-n`

2. How would I search for the string boop in the file aaaa in the directory /tmp

`grep boop /tmp/aaaa`

## Binary - Shiba3

We've been through a lot in this section, and the challenge for this binary will reflect that. The first step is actually finding the binary, I'm not heartless though, so I'll give you the name of the binary. The name of the binary is shiba4.

The actual binary will check for two things, it will be checking that there's a directory called test in your home directory, how you create that is up to you. It will also be checking that inside the directory there's a file called test1234.

1. What is shiba4's password

`test1234`

## [Section 6: Miscellaneous]: Intro

Even though we've gone over how the Linux operating system works, and some of it's most useful features and commands, there are some useful commands and concepts that haven't been covered in previous sections. So this section is dedicated to all those miscellaneous commands and concepts that are useful to know.

1. Read the above

`No answer needed`

## [Section 6: Miscellaneous]: sudo

Throughout this room you might have seen me mention the root user. The root user is the equivalent of the administrator user on Windows, and like Windows certain commands, and certain things you download from the internet will require admin permissions.

That's where sudo comes in. sudo is Linux's run as administrator button, and the syntax goes `sudo <command>`.

![](https://i.imgur.com/ejD2Dib.jpg)

Note: whoami is just a command that states your current user.

As you can see when using sudo the command is run as root. It is important to note that you need to have your current user's password to use it. Again like Windows, not every user has permission to use sudo, but most Linux OS' set up a user that has permissions when you install it. 

Assuming you create a new user that you also want to give sudo permissions to, the man page for sudo has a section on how to add user permissions. It is also worth noting you can configure sudo to run commands as other users, again the man page has a section on that(sudo has a very nice man page)

1. How do you specify which user you want to run a command as.

`-u`

2. How would I run whoami as user jen?

`sudo -u jen whoami`

3. How do you list your current sudo privileges(what commands you can run, who you can run them as etc.)

`-l`

## [Section 6: Miscellaneous]: Adding users and groups

You know about how to modify permissions for users and groups, therefore it's helpful to know how to create them. Luckily Linux provides a nice helpful way to do this, with `adduser` and `addgroup`. The syntax for both of these commands are `adduser username` and `addgroup groupname`.

![](https://i.imgur.com/wWwteJA.jpg)

![](https://i.imgur.com/sEWC06K.jpg)

It's important to note that only root has permissions to add users and groups, as seen with the failure when I attempted to run the commands without sudo. You may be wondering how to add a user to a group. that is done with the usermod command, the syntax for that is `usermod -a -G <groups seperated by commas> <user>`. Meaning if I wanted to add the user noot to b I would run `usermod -a -G b noot`.

![](https://i.imgur.com/vmxNa4e.jpg)

Note: id is a command that allows you to view basic information about a user.

1. How would I add the user test to the group test

`sudo usermod -a -G test test`

## [Section 6: Miscellaneous]: nano

Up until this point you may have seen me only using >> to add content to a file. Luckily that's not the only way to do things, nano is a terminal based text editor. The syntax for nano is `nano <file you want to write to>`. For example typing nano test will take you to this screen.

![](https://i.imgur.com/s0uJCNT.png)

From here type until your heart's content!

![](https://i.imgur.com/Kj5VuJW.png)

Nano has alot of commands inside of it's text editor, and the text editor as a whole probably warrants a room on it's own, but 99.9 percent of the time you're gonna wanna use `ctrl+x`

Note: ^X means ctrl+x, most of the time you see ^<key> the ^ means control

Once you press ctrl +x you'll be prompted with this screen

![](https://i.imgur.com/XqkoQbk.png)

From there press Y and you'll be asked what you want to name the file.

![](https://i.imgur.com/ao053v7.png)

Type whatever you want the file to be named, press enter and you'll be greeted with your terminal screen! You'll notice the file is there and everything you typed is there.

![](https://i.imgur.com/xlMG3H7.png)

Now you can actually edit text files! :)

1. Read the above

`No answer needed`

## [Section 6: Miscellaneous]: Basic shell scripting

Linux provides us a way to run commands one after another without using any special operators. This is done by storing the commands you want to run in a file with a .sh extension

![](https://i.imgur.com/HODFyrK.png)

If we save and run bash s.sh it executes those commands in order.

![](https://i.imgur.com/EGB71ea.png)

It echoed out hello, then it echoed out whoami, then it ran whoami exactly as the file said it should. Congratulations that's your first bash script!

It is worth noting that the sh extension isn't technically needed if you provide a shebang(#!) , and then the path to the shell we want to use to run our command Ahttps://imgur.com/a5AX8U4.

![](https://i.imgur.com/a5AX8U4.png)

From there we can remove the sh extension, and make the file executable.

![](https://i.imgur.com/UH9LQkO.png)

Note: The shebang MUST be in the beginning of the file

1. Read the above.

`No answer needed`

## [Section 6: Miscellaneous]: Important Files and Directories

Throughout this room you've seen a lot of files and directories, so I'm using this task to define what the most important of those files and directories do.

Before that however, it is worth noting exactly how the linux file system works. Everything on the linux file system extends from "/". / is the equivalent of C: in Windows. Meaning that for example if you were to delete "/", you would delete every file on your system.

`/etc/passwd` - Stores user information - Often used to see all the users on a system

![](https://i.imgur.com/EhQNy3H.png)

`/etc/shadow` - Has all the passwords of these users

Not showing for obvious reasons

`/tmp` - Every file inside it gets deleted upon shutdown - used for temporary files

![](https://i.imgur.com/SEtC1rs.png)

`/etc/sudoers` - Used to control the sudo permissions of every user on the system -

Long to show

`/home` - The directory where all your downloads, documents etc are. - The equivalent on Windows is C:\Users\<user>

![](https://i.imgur.com/YQnZLbJ.png)

`/root` - The root user's home directory - The equivilent on Windows is C:\Users\Administrator

Not showing because of spoiler ;)

`/usr` - Where all your software is installed 

![](https://i.imgur.com/e0l4Ybh.png)

`/bin` and `/sbin` - Used for system critical files - DO NOT DELETE

Too big to show, but contains all of the basic programs needed for linux to function

`/var` - The Linux miscellaneous directory, a myriad of processes store data in /var

![](https://i.imgur.com/tyIiCeg.png)

`$PATH` - Stores all the binaries you're able to run - same as $PATH on Windows

$PATH is an environment variable that contains all the binaries you're able to execute.

![](https://i.imgur.com/vQYYnlr.png)

It is worth noting that the paths in $PATH(hah!) are separated by colons. Every executable file that is in any of those paths you are able to run just by typing the name of the executable instead of the full path.

![](https://i.imgur.com/8csjrPX.png)

Note: which is just a command that shows you where an executable is in any of the PATH directories.

1. Read the above

`No answer needed`

## [Section 6 - Miscellaneous]: Installing packages(apt)

This is a bit hard to make a task on because depending on the Linux OS you install, this information may be entirely worthless. Therefore, I have deemed it best to show how to install packages using the most popular package manager, that being apt. A package is essentially a program, you can think of it like an exe file on windows. To install packages you need root permissions, as each package will likely modify some system critical directories such as /usr. The syntax to install packages is `apt install package`.

![](https://i.imgur.com/VaGFf6H.png)

Note: python-dev is a random package and just the first one that came to mind

apt downloads the package from a repository(list of programs). It then installs it and gives you your command prompt

back, much like when you install a program on Windows.

![](https://i.imgur.com/OfsAQ6N.png)

apt has a lot of sub commands, and again warrants a room on it's own but most of the time you're going to be googling what you want and you'll find the name of the package to install.

1. Read the above

`No answer needed`

## [Section 6: Miscellaneous]: Processes

Every binary you execute on linux, is a process while it's run. A process is just another word for a running program. A list of user created processes can be viewed with the `ps` command

![](https://i.imgur.com/pP9hnen.png)

While this is technically all the user created processes, it's likely not the information you're looking for if you're going to be examining processes. To view a list of all system processes, you have to use the `-ef` flag

![](https://i.imgur.com/WiLQ5Rq.png)

Every process that is currently running on the system is listed, along with some basic information about the process. Arguably the most interesting part about that list is the second column, the 3-5 digit numbers. Those are known as Process ID's(PID's) and they're how you interact with the processes. 90% of the time you'll likely be wanting to stop these processes and that's done with the kill command(an apt choice of naming I know). 


The syntax of kill is `kill <PID>`.

![](https://i.imgur.com/o8u8nhh.png)

![](https://i.imgur.com/UKq8sEN.png)

After running kill, the process no longer shows up! Another useful way to interact with PID's is through the command top. top shows you what processes are taking up the most system resources, which allows you to manage the resource allocation on your system by killing unneeded processes.

![](https://i.imgur.com/E3OeMWn.png)

Note: The top man page has descriptions for what every value means, and how they affect your system; I highly recommend reading it!

1. Read the above!

`No answer needed`

## Fin ~

We've been through a lot, but we're finally at the end. I hope everyone of you enjoyed this room and learned something :)

1. :)

`No answer needed`

## Bonus Challenge - The True Ending

I'm not gonna leave you without one final little parting gift. This is a penetration site, and it wouldn't feel right if I didn't hide a flag. There's one flag on this machine and it's in /root/root.txt, everything you need to get there is in this room, So I leave you with this. Good luck and have fun! :)

1. Finish this room off! What is the root.txt flag

`ad91979868d06e19d8e8a9c28be56e0c`
