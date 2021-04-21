# Hacking with Powershell

Learn the basics of Powershell and Powershell Scripting

[Hacking with Powershell](https://tryhackme.com/room/powershell)

## Topic's

* PowerShell Fundamentals

## Task 1 Objectives

In this room, we'll be exploring the following concepts:

* What is Powershell and how it works
* Basic Powershell commands
* Windows enumeration with Powershell
* Powershell scripting

You can control the machine in your browser or RDP into the instance with the following credentials:

* Username: Administrator
* Password: BHN2UVw0Q

Please note that this machine does not respond to ping (ICMP) and may take a few minutes to boot up.

Read the above and deploy the machine!

`No answer needed`

## Task 2 What is Powershell?

Powershell is the Windows Scripting Language and shell environment that is built using the .NET framework.

This also allows Powershell to execute .NET functions directly from its shell. Most Powershell commands, called cmdlets, are written in .NET. Unlike other scripting languages and shell environments, the output of these cmdlets are objects - making Powershell somewhat object oriented. This also means that running cmdlets allows you to perform actions on the output object(which makes it convenient to pass output from one cmdlet to another). The normal format of a cmdlet is represented using **Verb-Noun**; for example the cmdlet to list commands is called `Get-Command`.

Common verbs to use include:

* Get
* Start
* Stop
* Read
* Write
* New
* Out

To get the full list of approved verbs, visit [this](https://docs.microsoft.com/en-us/powershell/scripting/developer/cmdlet/approved-verbs-for-windows-powershell-commands?view=powershell-7) link.

What is the command to get help about a particular cmdlet(without any parameters)?

`Get-Help`

## Task 3 Basic Powershell Commands

Now that we've understood how cmdlets works - let's explore how to use them! The main thing to remember here is that Get-Command and Get-Help are your best friends! 

**Using Get-Help**

Get-Help displays information about a cmdlet. To get help about a particular command, run the following:

`Get-Help Command-Name`

You can also understand how exactly to use the command by passing in the `-examples` flag. This would return output like the following: 

![](https://i.imgur.com/U5Mlirh.png)

**Using Get-Command**

Get-Command gets all the cmdlets installed on the current Computer. The great thing about this cmdlet is that it allows for pattern matching like the following

`Get-Command Verb-*` or `Get-Command *-Noun`

Running Get-Command New-* to view all the cmdlets for the verb new displays the following: 

![](https://i.imgur.com/KEzbPUI.png)

**Object Manipulation**

In the previous task, we saw how the output of every cmdlet is an object. If we want to actually manipulate the output, we need to figure out a few things:

* passing output to other cmdlets
* using specific object cmdlets to extract information

The Pipeline(|) is used to pass output from one cmdlet to another. A major difference compared to other shells is that instead of passing text or string to the command after the pipe, powershell passes an object to the next cmdlet. Like every object in object oriented frameworks, an object will contain methods and properties. You can think of methods as functions that can be applied to output from the cmdlet and you can think of properties as variables in the output from a cmdlet. To view these details, pass the output of a cmdlet to the Get-Member cmdlet

`Verb-Noun | Get-Member`

An example of running this to view the members for Get-Command is:

`Get-Command | Get-Member -MemberType Method`

![](https://i.imgur.com/OlwXSbS.png)

From the above flag in the command, you can see that you can also select between methods and properties.

**Creating Objects From Previous cmdlets**

One way of manipulating objects is pulling out the properties from the output of a cmdlet and creating a new object. This is done using the `Select-Object` cmdlet. 

Here's an example of listing the directories and just selecting the mode and the name:

![](https://i.imgur.com/Zdxicjj.png)

You can also use the following flags to select particular information:

* first - gets the first x object
* last - gets the last x object
* unique - shows the unique objects
* skip - skips x objects

**Filtering Objects**

When retrieving output objects, you may want to select objects that match a very specific value. You can do this using the Where-Object to filter based on the value of properties.

The general format of the using this cmdlet is

`Verb-Noun | Where-Object -Property PropertyName -operator Value`

`Verb-Noun | Where-Object {$_.PropertyName -operator Value}`

The second version uses the $_ operator to iterate through every object passed to the Where-Object cmdlet.

**Powershell is quite sensitive so make sure you don't put quotes around the command!**

Where `-operator` is a list of the following operators:

* -Contains: if any item in the property value is an exact match for the specified value
* -EQ: if the property value is the same as the specified value
* -GT: if the property value is greater than the specified value

For a full list of operators, use [this](https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.core/where-object?view=powershell-6) link.

Here's an example of checking the stopped processes:

![](https://i.imgur.com/obTvbWW.png)

**Sort Object**

When a cmdlet outputs a lot of information, you may need to sort it to extract the information more efficiently. You do this by pipe lining the output of a cmdlet to the `Sort-Object` cmdlet.

The format of the command would be

`Verb-Noun | Sort-Object`

Here's an example of sort the list of directories:

![](https://i.imgur.com/xob5cqe.png)

Now that you've understood the basics of how Powershell works, let try some commands to apply this knowledge!

What is the location of the file "interesting-file.txt"

```ps1
Get-ChildItem -Path C:\ -Include interesting-file.txt -File -Recurse -ErrorAction SilentlyContinue
```

```ps1
PS C:\Users\Administrator> Get-ChildItem -Path C:\ -Include interesting-file.* -File -Recurse -ErrorAction SilentlyContinue


    Directory: C:\Program Files


Mode                LastWriteTime         Length Name
----                -------------         ------ ----
-a----        10/3/2019  11:38 PM             23 interesting-file.txt.txt
```

`C:\Program Files`

Specify the contents of this file

```ps1
Get-Content -Path 'C:\Program Files\interesting-file.txt.txt'
```

```
PS C:\Users\Administrator> Get-Content -Path 'C:\Program Files\interesting-file.txt.txt'
notsointerestingcontent
```

`notsointerestingcontent`

How many cmdlets are installed on the system(only cmdlets, not functions and aliases)?

```ps1
Get-Command | Where-Object -Property CommandType -eq Cmdlet | measure
```

```
PS C:\Users\Administrator> Get-Command | Where-Object -Property CommandType -eq Cmdlet | measure


Count    : 6638
Average  :
Sum      :
Maximum  :
Minimum  :
Property :
```

Get the MD5 hash of interesting-file.txt

```ps1
Get-FileHash 'C:\Program Files\interesting-file.txt.txt' -Algorithm MD5
```

```ps1
PS C:\Users\Administrator> Get-FileHash 'C:\Program Files\interesting-file.txt.txt' -Algorithm MD5

Algorithm       Hash                                                                   Path
---------       ----                                                                   ----
MD5             49A586A2A9456226F8A1B4CEC6FAB329                                       C:\Program Files\interesting-file.txt.txt
```

`49A586A2A9456226F8A1B4CEC6FAB329`

What is the command to get the current working directory?

```ps1
Get-Location
```

```
PS C:\Users\Administrator> Get-Location

Path
----
C:\Users\Administrator
```

`Get-Location`

Does the path "C:\Users\Administrator\Documents\Passwords" Exist(Y/N)?

`Get-Location "C:\Users\Administrator\Documents\Passwords"`

```
PS C:\Users\Administrator> Get-Location "C:\Users\Administrator\Documents\Passwords"
Get-Location : A positional parameter cannot be found that accepts argument 'C:\Users\Administrator\Documents\Passwords'.
At line:1 char:1
+ Get-Location "C:\Users\Administrator\Documents\Passwords"
+ ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
    + CategoryInfo          : InvalidArgument: (:) [Get-Location], ParameterBindingException
    + FullyQualifiedErrorId : PositionalParameterNotFound,Microsoft.PowerShell.Commands.GetLocationCommand
```

`A positional parameter cannot be found that accepts argument`

`N`

What command would you use to make a request to a web server?

[Invoke-WebRequest](https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.utility/invoke-webrequest?view=powershell-7.1&viewFallbackFrom=powershell-6)

`Invoke-WebRequest`

Base64 decode the file b64.txt on Windows.

```ps1
Get-ChildItem -Path C:/ -Include *b64.txt* -Recurse -File
certutil -decode "C:\Users\Administrator\Desktop\b64.txt" out.txt
Get-Content out.txt
```

```
PS C:\Users\Administrator> Get-ChildItem -Path C:/ -Include *b64.txt* -Recurse -File


    Directory: C:\Users\Administrator\Desktop


Mode                LastWriteTime         Length Name
----                -------------         ------ ----
-a----        10/3/2019  11:56 PM            432 b64.txt

PS C:\Users\Administrator> certutil -decode "C:\Users\Administrator\Desktop\b64.txt" out.txt
Input Length = 432
Output Length = 323
CertUtil: -decode command completed successfully.
PS C:\Users\Administrator> Get-Content out.txt
this is the flag - ihopeyoudidthisonwindows
the rest is garbage
the rest is garbage
the rest is garbage
the rest is garbage
the rest is garbage
the rest is garbage
the rest is garbage
the rest is garbage
the rest is garbage
the rest is garbage
the rest is garbage
the rest is garbage
the rest is garbage
the rest is garbage
```

`ihopeyoudidthisonwindows`

## Task 4 Enumeration

The first step when you have gained initial access to any machine would be to enumerate. We'll be enumerating the following:

* users
* basic networking information
* file permissions
* registry permissions
* scheduled and running tasks
* insecure files

Your task will be to answer the following questions to enumerate the machine using Powershell commands! 

How many users are there on the machine?

`Get-LocalUser`

```
PS C:\Users\Administrator> Get-LocalUser

Name           Enabled Description
----           ------- -----------
Administrator  True    Built-in account for administering the computer/domain
DefaultAccount False   A user account managed by the system.
duck           True
duck2          True
Guest          False   Built-in account for guest access to the computer/domain
```

`5`

Which local user does this SID(S-1-5-21-1394777289-3961777894-1791813945-501) belong to?

```
PS C:\Users\Administrator> Get-LocalUser -SID "S-1-5-21-1394777289-3961777894-1791813945-501"

Name  Enabled Description
----  ------- -----------
Guest False   Built-in account for guest access to the computer/domain
```

`Guest`

How many users have their password required values set to False?

```
PS C:\Users\Administrator> Get-LocalUser | Where-Object -Property PasswordRequired -Match false

Name           Enabled Description
----           ------- -----------
DefaultAccount False   A user account managed by the system.
duck           True
duck2          True
Guest          False   Built-in account for guest access to the computer/domain
```

`4`

How many local groups exist?

```
PS C:\Users\Administrator> Get-LocalGroup | measure


Count    : 24
Average  :
Sum      :
Maximum  :
Minimum  :
Property :
```

`24`

What command did you use to get the IP address info?

```
PS C:\Users\Administrator> Get-NetIPAddress


IPAddress         : fe80::c52:2e9:f5f5:e5f4%7
InterfaceIndex    : 7
InterfaceAlias    : Local Area Connection* 3
AddressFamily     : IPv6
Type              : Unicast
PrefixLength      : 64
PrefixOrigin      : WellKnown
SuffixOrigin      : Link
AddressState      : Preferred
ValidLifetime     : Infinite ([TimeSpan]::MaxValue)
PreferredLifetime : Infinite ([TimeSpan]::MaxValue)
SkipAsSource      : False
PolicyStore       : ActiveStore

IPAddress         : 2001:0:2851:782c:c52:2e9:f5f5:e5f4
InterfaceIndex    : 7
InterfaceAlias    : Local Area Connection* 3
AddressFamily     : IPv6
Type              : Unicast
PrefixLength      : 64
PrefixOrigin      : RouterAdvertisement
SuffixOrigin      : Link
AddressState      : Preferred
ValidLifetime     : Infinite ([TimeSpan]::MaxValue)
PreferredLifetime : Infinite ([TimeSpan]::MaxValue)
SkipAsSource      : False
PolicyStore       : ActiveStore

IPAddress         : fe80::58ed:5a52:416c:8123%5
InterfaceIndex    : 5
InterfaceAlias    : Ethernet
AddressFamily     : IPv6
Type              : Unicast
PrefixLength      : 64
PrefixOrigin      : WellKnown
SuffixOrigin      : Link
AddressState      : Preferred
ValidLifetime     : Infinite ([TimeSpan]::MaxValue)
PreferredLifetime : Infinite ([TimeSpan]::MaxValue)
SkipAsSource      : False
PolicyStore       : ActiveStore

IPAddress         : fe80::5efe:10.10.26.11%6
InterfaceIndex    : 6
InterfaceAlias    : Reusable ISATAP Interface {90ABCE23-305A-4BDE-AA39-4FFDA7413134}
AddressFamily     : IPv6
Type              : Unicast
PrefixLength      : 128
PrefixOrigin      : WellKnown
SuffixOrigin      : Link
AddressState      : Deprecated
ValidLifetime     : Infinite ([TimeSpan]::MaxValue)
PreferredLifetime : Infinite ([TimeSpan]::MaxValue)
SkipAsSource      : False
PolicyStore       : ActiveStore

IPAddress         : ::1
InterfaceIndex    : 1
InterfaceAlias    : Loopback Pseudo-Interface 1
AddressFamily     : IPv6
Type              : Unicast
PrefixLength      : 128
PrefixOrigin      : WellKnown
SuffixOrigin      : WellKnown
AddressState      : Preferred
ValidLifetime     : Infinite ([TimeSpan]::MaxValue)
PreferredLifetime : Infinite ([TimeSpan]::MaxValue)
SkipAsSource      : False
PolicyStore       : ActiveStore

IPAddress         : 10.10.26.11
InterfaceIndex    : 5
InterfaceAlias    : Ethernet
AddressFamily     : IPv4
Type              : Unicast
PrefixLength      : 16
PrefixOrigin      : Dhcp
SuffixOrigin      : Dhcp
AddressState      : Preferred
ValidLifetime     : 00:56:48
PreferredLifetime : 00:56:48
SkipAsSource      : False
PolicyStore       : ActiveStore

IPAddress         : 127.0.0.1
InterfaceIndex    : 1
InterfaceAlias    : Loopback Pseudo-Interface 1
AddressFamily     : IPv4
Type              : Unicast
PrefixLength      : 8
PrefixOrigin      : WellKnown
SuffixOrigin      : WellKnown
AddressState      : Preferred
ValidLifetime     : Infinite ([TimeSpan]::MaxValue)
PreferredLifetime : Infinite ([TimeSpan]::MaxValue)
SkipAsSource      : False
PolicyStore       : ActiveStore
```

`Get-NetIPAddress`

How many ports are listed as listening?

```
PS C:\Users\Administrator> Get-NetTCPconnection -State Listen

LocalAddress                        LocalPort RemoteAddress                       RemotePort State       AppliedSetting OwningProcess
------------                        --------- -------------                       ---------- -----       -------------- -------------
::                                  49677     ::                                  0          Listen                     712
::                                  49668     ::                                  0          Listen                     704
::                                  49667     ::                                  0          Listen                     1704
::                                  49666     ::                                  0          Listen                     984
::                                  49665     ::                                  0          Listen                     480
::                                  49664     ::                                  0          Listen                     600
::                                  47001     ::                                  0          Listen                     4
::                                  5985      ::                                  0          Listen                     4
::                                  3389      ::                                  0          Listen                     976
::                                  445       ::                                  0          Listen                     4
::                                  135       ::                                  0          Listen                     832
0.0.0.0                             49677     0.0.0.0                             0          Listen                     712
0.0.0.0                             49668     0.0.0.0                             0          Listen                     704
0.0.0.0                             49667     0.0.0.0                             0          Listen                     1704
0.0.0.0                             49666     0.0.0.0                             0          Listen                     984
0.0.0.0                             49665     0.0.0.0                             0          Listen                     480
0.0.0.0                             49664     0.0.0.0                             0          Listen                     600
0.0.0.0                             3389      0.0.0.0                             0          Listen                     976
10.10.26.11                         139       0.0.0.0                             0          Listen                     4
0.0.0.0                             135       0.0.0.0                             0          Listen                     832


PS C:\Users\Administrator> Get-NetTCPconnection -State Listen | measure


Count    : 20
Average  :
Sum      :
Maximum  :
Minimum  :
Property :
```

`20`

What is the remote address of the local port listening on port 445?

```
PS C:\Users\Administrator> Get-NetTCPconnection -State Listen -LocalPort 445

LocalAddress                        LocalPort RemoteAddress                       RemotePort State       AppliedSetting OwningProcess
------------                        --------- -------------                       ---------- -----       -------------- -------------
::                                  445       ::                                  0          Listen                     4
```

`::`

How many patches have been applied?

```
PS C:\Users\Administrator> Get-Hotfix | measure


Count    : 20
Average  :
Sum      :
Maximum  :
Minimum  :
Property :
```

`20`

When was the patch with ID KB4023834 installed?

```
PS C:\Users\Administrator> Get-Hotfix -Id KB4023834

Source        Description      HotFixID      InstalledBy          InstalledOn
------        -----------      --------      -----------          -----------
EC2AMAZ-5M... Update           KB4023834     EC2AMAZ-5M13VM2\A... 6/15/2017 12:00:00 AM
```

`6/15/2017 12:00:00 AM`

Find the contents of a backup file.

```
PS C:\Users\Administrator> Get-Hotfix -Id KB4023834

Source        Description      HotFixID      InstalledBy          InstalledOn
------        -----------      --------      -----------          -----------
EC2AMAZ-5M... Update           KB4023834     EC2AMAZ-5M13VM2\A... 6/15/2017 12:00:00 AM


PS C:\Users\Administrator> Get-ChildItem -Path C:\ -Include *.bak* -File -Recurse -ErrorAction SilentlyContinue


    Directory: C:\Program Files (x86)\Internet Explorer


Mode                LastWriteTime         Length Name
----                -------------         ------ ----
-a----        10/4/2019  12:42 AM             12 passwords.bak.txt
PS C:\Users\Administrator> Get-Content "C:\Program Files (x86)\Internet Explorer\passwords.bak.txt"
backpassflag
```

Search for all files containing API_KEY

```ps1
Get-ChildItem C:\* -Recurse | Select-String -pattern API_KEY
```

```
C:\Users\Public\Music\config.xml:1:API_KEY=fakekey123
Select-String : The file C:\Windows\appcompat\Programs\Amcache.hve cannot be read: The process cannot access the file 'C:\Windows\appcompat\Programs\Amcache.hve' because it is being used by another process
At line:1 char:31
+ Get-ChildItem C:\* -recurse | Select-String -pattern API_KEY
+                               ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
    + CategoryInfo          : InvalidArgument: (:) [Select-String], ArgumentException
    + FullyQualifiedErrorId : ProcessingFile,Microsoft.PowerShell.Commands.SelectStringCommand
```

`fakekey123`

What command do you do to list all the running processes?

```
PS C:\Users\Administrator> Get-Process

Handles  NPM(K)    PM(K)      WS(K)     CPU(s)     Id  SI ProcessName
-------  ------    -----      -----     ------     --  -- -----------
    118       8    20864      12720       0.25   1724   0 amazon-ssm-agent
    189      13     5920      21036       9.53   3908   3 conhost
    199      10     1800       3948       0.25    524   0 csrss
    118       8     1316       3620       0.09    592   1 csrss
    174      10     1524       4060       0.84   2964   3 csrss
    316      19    13400      29328       0.13    924   1 dwm
    342      31    18172      56032       2.06   2052   3 dwm
   1217      53    20052      72704       2.28   2348   3 explorer
      0       0        0          4                 0   0 Idle
     71       6      956       4676       0.02   1764   0 LiteAgent
    403      23    10636      42068       0.23   2148   1 LogonUI
    909      21     4512      13044       0.48    712   0 lsass
    122       8     1912       6348       0.22   2060   0 MpCmdRun
    167      10     2296       8284       0.02   3640   0 MpCmdRun
    190      13     2796       9432       0.03   2312   0 msdtc
    596      66   139720     200652     440.52   1808   0 MsMpEng
    174      27     3728       9204       0.06   2444   0 NisSrv
    721      44   217176     320300     360.64   3924   3 powershell
    277      12     2308      10268       0.23   2696   3 rdpclip
    264      14     4552      20156       0.19   2656   3 RuntimeBroker
    571      29    11824      35304       0.20   3212   3 SearchUI
    228       9     2892       6448       0.48    704   0 services
    840      33    21808      42320       0.63   3124   3 ShellExperienceHost
    363      14     3728      18276       0.19   2136   3 sihost
     54       2      384       1204       0.09    392   0 smss
    424      22     5512      15348       0.13   1704   0 spoolsv
    605      32    11664      22020       1.20    104   0 svchost
    477      17    10380      18324       0.47    480   0 svchost
    442      34    11076      18960       0.94    544   0 svchost
    677      21     5868      19512       0.70    788   0 svchost
    545      16     3684       9368       0.58    832   0 svchost
    752      26    49872      72640      22.13    976   0 svchost
   1451      56    23328      45636       3.23    984   0 svchost
    553      28     6624      16360       0.33   1048   0 svchost
    609      37     7500      19684       0.47   1172   0 svchost
    158       9     1668       6840       0.02   1180   0 svchost
    196      11     1952       7832       0.03   1744   0 svchost
    219      16     5184      15972       0.53   1788   0 svchost
    292      18     4324      19300       0.09   2876   3 svchost
    870       0      128        140      23.47      4   0 System
    369      19     6560      17364       0.19   2544   3 taskhostw
    282      24     4452      14628       0.08   3024   3 taskhostw
     92       8      912       4800       0.03    600   0 wininit
    167       9     2076      12580       0.11    644   1 winlogon
    183       8     1788       7268       0.11   3004   3 winlogon
```

`Get-Process`

What is the path of the scheduled task called new-sched-task?

```
PS C:\Users\Administrator> Get-Scheduledtask -TaskName new-sched-task

TaskPath                                       TaskName                          State
--------                                       --------                          -----
\                                              new-sched-task                    Ready
```

`\`

Who is the owner of the C:\

```
PS C:\Users\Administrator> Get-Acl C:\


    Directory:


Path Owner                       Access
---- -----                       ------
C:\  NT SERVICE\TrustedInstaller CREATOR OWNER Allow  268435456...
```

## Task 5 Basic Scripting Challenge

Now that we have run powershell commands, let's actually try write and run a script to do more complex and powerful actions. 

For this ask, we'll be using PowerShell ISE(which is the Powershell Text Editor). To show an example of this script, let's use a particular scenario. Given a list of port numbers, we want to use this list to see if the local port is listening. Open the listening-ports.ps1 script on the Desktop using Powershell ISE. Powershell scripts usually have the .ps1 file extension. 

```ps1
$system_ports = Get-NetTCPConnection -State Listen

$text_port = Get-Content -Path C:\Users\Administrator\Desktop\ports.txt

foreach($port in $text_port){

    if($port -in $system_ports.LocalPort){

        echo $port

     }

}
```

On the first line, we want to get a list of all the ports on the system that are listening. We do this using the Get-NetTCPConnection cmdlet. We are then saving the output of this cmdlet into a variable. The convention to create variables is used as:

```ps1
$variable_name = value
```

On the next line, we want to read a list of ports from the file. We do this using the Get-Content cmdlet. Again, we store this output in the variables. The simplest next step is iterate through all the ports in the file to see if the ports are listening. To iterate through the ports in the file, we use the following

```ps1
foreach($new_var in $existing_var){}
```

This particular code block is used to loop through a set of object. Once we have each individual port, we want to check if this port occurs in the listening local ports. Instead of doing another for loop, we just use an if statement with the `-in` operator to check if the port exists the LocalPort property of any object. A full list of if statement comparison operators can be found [here](https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.core/about/about_comparison_operators?view=powershell-6). To run script, just call the script path using Powershell or click the green button on Powershell ISE:

![](https://i.imgur.com/eMTXaFo.png)

Now that we've seen what a basic script looks like - it's time to write one of your own. The emails folder on the Desktop contains copies of the emails John, Martha and Mary have been sending to each other(and themselves). Answer the following questions with regards to these emails(try not to open the files and use a script to answer the questions). 

Scripting may be a bit difficult, but [here](https://learnxinyminutes.com/docs/powershell/) is a good resource to use: 

What file contains the password?

```
PS C:\Users\Administrator> $path = "C:\Users\Administrator\Desktop\emails\*"

PS C:\Users\Administrator> $string_pattern = "password"

PS C:\Users\Administrator> $command = Get-ChildItem -Path $path -Recurse | Select-String -Pattern $String_pattern

PS C:\Users\Administrator> echo $command

Desktop\emails\john\Doc3.txt:6:I got some errors trying to access my passwords file - is there any way you can help? Here is the output I got
Desktop\emails\martha\Doc3M.txt:6:I managed to fix the corrupted file to get the output, but the password is buried somewhere in these logs:
Desktop\emails\martha\Doc3M.txt:106:password is johnisalegend99
```

`Doc3M`

What is the password?

`johnisalegend99`

What files contains an HTTPS link?

```
PS C:\Users\Administrator> $string_pattern = "HTTP"

PS C:\Users\Administrator> $command = Get-ChildItem -Path $path -Recurse | Select-String -Pattern $String_pattern

PS C:\Users\Administrator> echo $command

Desktop\emails\mary\Doc2Mary.txt:5:https://www.howtoworkwell.rand/
```

## Task 6 Intermediate Scripting

Now that you've learnt a little bit about how scripting works - let's try something a bit more interesting. Sometimes we may not have utilities like nmap and python available, and we are forced to write scripts to do very rudimentary tasks. Why don't you try writing a simple port scanner using Powershell. Here's the general approach to use:

* Determine IP ranges to scan(in this case it will be localhost) and you can provide the input in any way you want
* Determine the port ranges to scan
* Determine the type of scan to run(in this case it will be a simple TCP Connect Scan)

How many open ports did you find between 130 and 140(inclusive of those two)?


```ps1
for($i=130; $i -le 140; $i++){ Test-NetConnection localhost -Port $i }
```

```
PS C:\Users\Administrator> for($i=130; $i -le 140; $i++){ Test-NetConnection localhost -Port $i }
WARNING: TCP connect to localhost:130 failed


ComputerName           : localhost
RemoteAddress          : ::1
RemotePort             : 130
InterfaceAlias         : Loopback Pseudo-Interface 1
SourceAddress          : ::1
PingSucceeded          : True
PingReplyDetails (RTT) : 0 ms
TcpTestSucceeded       : False

WARNING: TCP connect to localhost:131 failed
ComputerName           : localhost
RemoteAddress          : ::1
RemotePort             : 131
InterfaceAlias         : Loopback Pseudo-Interface 1
SourceAddress          : ::1
PingSucceeded          : True
PingReplyDetails (RTT) : 0 ms
TcpTestSucceeded       : False

WARNING: TCP connect to localhost:132 failed
ComputerName           : localhost
RemoteAddress          : ::1
RemotePort             : 132
InterfaceAlias         : Loopback Pseudo-Interface 1
SourceAddress          : ::1
PingSucceeded          : True
PingReplyDetails (RTT) : 0 ms
TcpTestSucceeded       : False

WARNING: TCP connect to localhost:133 failed
ComputerName           : localhost
RemoteAddress          : ::1
RemotePort             : 133
InterfaceAlias         : Loopback Pseudo-Interface 1
SourceAddress          : ::1
PingSucceeded          : True
PingReplyDetails (RTT) : 0 ms
TcpTestSucceeded       : False

WARNING: TCP connect to localhost:134 failed
ComputerName           : localhost
RemoteAddress          : ::1
RemotePort             : 134
InterfaceAlias         : Loopback Pseudo-Interface 1
SourceAddress          : ::1
PingSucceeded          : True
PingReplyDetails (RTT) : 0 ms
TcpTestSucceeded       : False

ComputerName     : localhost
RemoteAddress    : ::1
RemotePort       : 135
InterfaceAlias   : Loopback Pseudo-Interface 1
SourceAddress    : ::1
TcpTestSucceeded : True

WARNING: TCP connect to localhost:136 failed
ComputerName           : localhost
RemoteAddress          : ::1
RemotePort             : 136
InterfaceAlias         : Loopback Pseudo-Interface 1
SourceAddress          : ::1
PingSucceeded          : True
PingReplyDetails (RTT) : 0 ms
TcpTestSucceeded       : False

WARNING: TCP connect to localhost:137 failed
ComputerName           : localhost
RemoteAddress          : ::1
RemotePort             : 137
InterfaceAlias         : Loopback Pseudo-Interface 1
SourceAddress          : ::1
PingSucceeded          : True
PingReplyDetails (RTT) : 0 ms
TcpTestSucceeded       : False

WARNING: TCP connect to localhost:138 failed
ComputerName           : localhost
RemoteAddress          : ::1
RemotePort             : 138
InterfaceAlias         : Loopback Pseudo-Interface 1
SourceAddress          : ::1
PingSucceeded          : True
PingReplyDetails (RTT) : 0 ms
TcpTestSucceeded       : False

WARNING: TCP connect to localhost:139 failed
ComputerName           : localhost
RemoteAddress          : ::1
RemotePort             : 139
InterfaceAlias         : Loopback Pseudo-Interface 1
SourceAddress          : ::1
PingSucceeded          : True
PingReplyDetails (RTT) : 0 ms
TcpTestSucceeded       : False

WARNING: TCP connect to localhost:140 failed
ComputerName           : localhost
RemoteAddress          : ::1
RemotePort             : 140
InterfaceAlias         : Loopback Pseudo-Interface 1
SourceAddress          : ::1
PingSucceeded          : True
PingReplyDetails (RTT) : 0 ms
TcpTestSucceeded       : False
```

`11`
