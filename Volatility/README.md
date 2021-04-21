# Volatility

Learn how to perform memory forensics with Volatility!

[Volatility](https://tryhackme.com/room/bpvolatility)

## Topic's

- System Forensic
- Volatility Framework

## Appendix archive

Password: `1 kn0w 1 5h0uldn'7!`

## Task 1 Intro

Volatility is a free memory forensics tool developed and maintained by Volatility labs. Regarded as the gold standard for memory forensics in incident response, Volatility is wildly expandable via a plugins system and is an invaluable tool for any Blue Teamer.

![](https://i.imgur.com/gjY5jsj.png)

The virtual machine for this room can also be downloaded from https://darkstar7471.com, the credentials for the machine are as follows: voluser:volatility

Install Volatility onto your workstation of choice or use the provided virtual machine. On Debian-based systems such as Kali this can be done via `apt-get install volatility`

`No answer needed`

## Task 2 Obtaining Memory Samples

Obtaining a memory capture from machines can be done in numerous ways, however, the easiest method will often vary depending on what you're working with. For example, live machines (turned on) can have their memory captured with one of the following tools:

- FTK Imager - [Link](https://accessdata.com/product-download/ftk-imager-version-4-2-0)
- Redline - [Link](https://www.fireeye.com/services/freeware/redline.html) \*Requires registration but Redline has a very nice GUI
- DumpIt.exe
- win32dd.exe / win64dd.exe - \*Has fantastic psexec support, great for IT departments if your EDR solution doesn't support this

These tools will typically output a .raw file which contains an image of the system memory. The .raw format is one of the most common memory file types you will see in the wild.

Offline machines, however, can have their memory pulled relatively easily as long as their drives aren't encrypted. For Windows systems, this can be done via pulling the following file:

`%SystemDrive%/hiberfil.sys`

hiberfil.sys, better known as the Windows hibernation file contains a compressed memory image from the previous boot. Microsoft Windows systems use this in order to provide faster boot-up times, however, we can use this file in our case for some memory forensics!

Things get even more exciting when we start to talk about virtual machines and memory captures. Here's a quick sampling of the memory capture process/file containing a memory image for different virtual machine hypervisors:

- VMware - .vmem file
- Hyper-V - .bin file
- Parallels - .mem file
- VirtualBox - .sav file \*This is only a partial memory file. You'll need to dump memory like a normal bare-metal system for this hypervisor

These files can often be found simply in the data store of the corresponding hypervisor and often can be simply copied without shutting the associated virtual machine off. This allows for virtually zero disturbance to the virtual machine, preserving it's forensic integrity.

What memory format is the most common?

`.raw`

The Window's system we're looking to perform memory forensics on was turned off by mistake. What file contains a compressed memory image?

`hiberfil.sys`

How about if we wanted to perform memory forensics on a VMware-based virtual machine?

`.vmem`

## Task 3 Examining Our Patient

Now that we've collected our memory image let's dig into it! For those using their own workstation for this activity, I've provided a download link to our memory sample attached to this task. If you're using the workstation I've provided as a VM for this activity you'll find the memory image in the 'voluser' home directory.

First, let's figure out what profile we need to use. Profiles determine how Volatility treats our memory image since every version of Windows is a little bit different. Let's see our options now with the command `volatility -f MEMORY_FILE.raw imageinfo`

`No answer needed`

Running the imageinfo command in Volatility will provide us with a number of profiles we can test with, however, only one will be correct. We can test these profiles using the pslist command, validating our profile selection by the sheer number of returned results. Do this now with the command `volatility -f MEMORY_FILE.raw --profile=PROFILE pslist`. What profile is correct for this memory image?

```
kali@kali:~/CTFs/tryhackme/Volatility$ sudo python /opt/volatility/vol.py -f cridex.vmem --profile=WinXPSP2x86 pslist
Volatility Foundation Volatility Framework 2.6.1
Offset(V)  Name                    PID   PPID   Thds     Hnds   Sess  Wow64 Start                          Exit
---------- -------------------- ------ ------ ------ -------- ------ ------ ------------------------------ ------------------------------
0x823c89c8 System                    4      0     53      240 ------      0
0x822f1020 smss.exe                368      4      3       19 ------      0 2012-07-22 02:42:31 UTC+0000
0x822a0598 csrss.exe               584    368      9      326      0      0 2012-07-22 02:42:32 UTC+0000
0x82298700 winlogon.exe            608    368     23      519      0      0 2012-07-22 02:42:32 UTC+0000
0x81e2ab28 services.exe            652    608     16      243      0      0 2012-07-22 02:42:32 UTC+0000
0x81e2a3b8 lsass.exe               664    608     24      330      0      0 2012-07-22 02:42:32 UTC+0000
0x82311360 svchost.exe             824    652     20      194      0      0 2012-07-22 02:42:33 UTC+0000
0x81e29ab8 svchost.exe             908    652      9      226      0      0 2012-07-22 02:42:33 UTC+0000
0x823001d0 svchost.exe            1004    652     64     1118      0      0 2012-07-22 02:42:33 UTC+0000
0x821dfda0 svchost.exe            1056    652      5       60      0      0 2012-07-22 02:42:33 UTC+0000
0x82295650 svchost.exe            1220    652     15      197      0      0 2012-07-22 02:42:35 UTC+0000
0x821dea70 explorer.exe           1484   1464     17      415      0      0 2012-07-22 02:42:36 UTC+0000
0x81eb17b8 spoolsv.exe            1512    652     14      113      0      0 2012-07-22 02:42:36 UTC+0000
0x81e7bda0 reader_sl.exe          1640   1484      5       39      0      0 2012-07-22 02:42:36 UTC+0000
0x820e8da0 alg.exe                 788    652      7      104      0      0 2012-07-22 02:43:01 UTC+0000
0x821fcda0 wuauclt.exe            1136   1004      8      173      0      0 2012-07-22 02:43:46 UTC+0000
0x8205bda0 wuauclt.exe            1588   1004      5      132      0      0 2012-07-22 02:44:01 UTC+0000
```

`WinXPSP2x86`

Take a look through the processes within our image. What is the process ID for the smss.exe process? If results are scrolling off-screen, try piping your output into less

`368`

In addition to viewing active processes, we can also view active network connections at the time of image creation! Let's do this now with the command `volatility -f MEMORY_FILE.raw --profile=PROFILE netscan`. Unfortunately, something not great is going to happen here due to the sheer age of the target operating system as the command netscan doesn't support it.

`No answer needed`

It's fairly common for malware to attempt to hide itself and the process associated with it. That being said, we can view intentionally hidden processes via the command `psxview`. What process has only one 'False' listed?

```
kali@kali:~/CTFs/tryhackme/Volatility$ sudo python /opt/volatility/vol.py -f cridex.vmem --profile=WinXPSP2x86 psxview
Volatility Foundation Volatility Framework 2.6.1

Offset(P)  Name                    PID pslist psscan thrdproc pspcid csrss session deskthrd ExitTime
---------- -------------------- ------ ------ ------ -------- ------ ----- ------- -------- --------
0x02498700 winlogon.exe            608 True   True   True     True   True  True    True
0x02511360 svchost.exe             824 True   True   True     True   True  True    True
0x022e8da0 alg.exe                 788 True   True   True     True   True  True    True
0x020b17b8 spoolsv.exe            1512 True   True   True     True   True  True    True
0x0202ab28 services.exe            652 True   True   True     True   True  True    True
0x02495650 svchost.exe            1220 True   True   True     True   True  True    True
0x0207bda0 reader_sl.exe          1640 True   True   True     True   True  True    True
0x025001d0 svchost.exe            1004 True   True   True     True   True  True    True
0x02029ab8 svchost.exe             908 True   True   True     True   True  True    True
0x023fcda0 wuauclt.exe            1136 True   True   True     True   True  True    True
0x0225bda0 wuauclt.exe            1588 True   True   True     True   True  True    True
0x0202a3b8 lsass.exe               664 True   True   True     True   True  True    True
0x023dea70 explorer.exe           1484 True   True   True     True   True  True    True
0x023dfda0 svchost.exe            1056 True   True   True     True   True  True    True
0x024f1020 smss.exe                368 True   True   True     True   False False   False
0x025c89c8 System                    4 True   True   True     True   False False   False
0x024a0598 csrss.exe               584 True   True   True     True   False True    True
```

`csrss.exe`

In addition to viewing hidden processes via psxview, we can also check this with a greater focus via the command 'ldrmodules'. Three columns will appear here in the middle, InLoad, InInit, InMem. If any of these are false, that module has likely been injected which is a really bad thing. On a normal system the grep statement above should return no output. Which process has all three columns listed as 'False' (other than System)?

```
kali@kali:~/CTFs/tryhackme/Volatility$ sudo python /opt/volatility/vol.py -f cridex.vmem --profile=WinXPSP2x86 ldrmodules
Volatility Foundation Volatility Framework 2.6.1
Pid      Process              Base       InLoad InInit InMem MappedPath
-------- -------------------- ---------- ------ ------ ----- ----------
       4 System               0x7c900000 False  False  False \WINDOWS\system32\ntdll.dll
     368 smss.exe             0x48580000 True   False  True  \WINDOWS\system32\smss.exe
     368 smss.exe             0x7c900000 True   True   True  \WINDOWS\system32\ntdll.dll
     584 csrss.exe            0x00460000 False  False  False \WINDOWS\Fonts\vgasys.fon
     584 csrss.exe            0x4a680000 True   False  True  \WINDOWS\system32\csrss.exe
     584 csrss.exe            0x75b40000 True   True   True  \WINDOWS\system32\csrsrv.dll
     584 csrss.exe            0x75b50000 True   True   True  \WINDOWS\system32\basesrv.dll
     584 csrss.exe            0x7e720000 True   True   True  \WINDOWS\system32\sxs.dll
     584 csrss.exe            0x77e70000 True   True   True  \WINDOWS\system32\rpcrt4.dll
     584 csrss.exe            0x7c800000 True   True   True  \WINDOWS\system32\kernel32.dll
     584 csrss.exe            0x77dd0000 True   True   True  \WINDOWS\system32\advapi32.dll
     584 csrss.exe            0x77fe0000 True   True   True  \WINDOWS\system32\secur32.dll
     584 csrss.exe            0x7e410000 True   True   True  \WINDOWS\system32\user32.dll
     584 csrss.exe            0x7c900000 True   True   True  \WINDOWS\system32\ntdll.dll
```

`csrss.exe`

Processes aren't the only area we're concerned with when we're examining a machine. Using the 'apihooks' command we can view unexpected patches in the standard system DLLs. If we see an instance where Hooking module: <unknown> that's really bad. This command will take a while to run, however, it will show you all of the extraneous code introduced by the malware.

`No answer needed`

Injected code can be a huge issue and is highly indicative of very very bad things. We can check for this with the command `malfind`. Using the full command `volatility -f MEMORY_FILE.raw --profile=PROFILE malfind -D <Destination Directory>` we can not only find this code, but also dump it to our specified directory. Let's do this now! We'll use this dump later for more analysis. How many files does this generate?

```
kali@kali:~/CTFs/tryhackme/Volatility$ sudo python /opt/volatility/vol.py -f cridex.vmem --profile=WinXPSP2x86 malfind -D /tmp
Volatility Foundation Volatility Framework 2.6.1
WARNING : volatility.debug    : For best results please install distorm3
Process: csrss.exe Pid: 584 Address: 0x7f6f0000
Vad Tag: Vad  Protection: PAGE_EXECUTE_READWRITE
Flags: Protection: 6

0x000000007f6f0000  c8 00 00 00 91 01 00 00 ff ee ff ee 08 70 00 00   .............p..
0x000000007f6f0010  08 00 00 00 00 fe 00 00 00 00 10 00 00 20 00 00   ................
0x000000007f6f0020  00 02 00 00 00 20 00 00 8d 01 00 00 ff ef fd 7f   ................
0x000000007f6f0030  03 00 08 06 00 00 00 00 00 00 00 00 00 00 00 00   ................



Process: winlogon.exe Pid: 608 Address: 0x13410000
Vad Tag: VadS Protection: PAGE_EXECUTE_READWRITE
Flags: CommitCharge: 4, MemCommit: 1, PrivateMemory: 1, Protection: 6

0x0000000013410000  00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00   ................
0x0000000013410010  00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00   ................
0x0000000013410020  00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00   ................
0x0000000013410030  00 00 00 00 25 00 25 00 01 00 00 00 00 00 00 00   ....%.%.........



Process: winlogon.exe Pid: 608 Address: 0xf9e0000
Vad Tag: VadS Protection: PAGE_EXECUTE_READWRITE
Flags: CommitCharge: 4, MemCommit: 1, PrivateMemory: 1, Protection: 6

0x000000000f9e0000  00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00   ................
0x000000000f9e0010  00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00   ................
0x000000000f9e0020  00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00   ................
0x000000000f9e0030  00 00 00 00 25 00 25 00 01 00 00 00 00 00 00 00   ....%.%.........



Process: winlogon.exe Pid: 608 Address: 0x4ee0000
Vad Tag: VadS Protection: PAGE_EXECUTE_READWRITE
Flags: CommitCharge: 4, MemCommit: 1, PrivateMemory: 1, Protection: 6

0x0000000004ee0000  00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00   ................
0x0000000004ee0010  00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00   ................
0x0000000004ee0020  00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00   ................
0x0000000004ee0030  00 00 00 00 25 00 25 00 01 00 00 00 00 00 00 00   ....%.%.........



Process: winlogon.exe Pid: 608 Address: 0x554c0000
Vad Tag: VadS Protection: PAGE_EXECUTE_READWRITE
Flags: CommitCharge: 4, MemCommit: 1, PrivateMemory: 1, Protection: 6

0x00000000554c0000  00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00   ................
0x00000000554c0010  00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00   ................
0x00000000554c0020  00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00   ................
0x00000000554c0030  00 00 00 00 28 00 28 00 01 00 00 00 00 00 00 00   ....(.(.........



Process: winlogon.exe Pid: 608 Address: 0x4dc40000
Vad Tag: VadS Protection: PAGE_EXECUTE_READWRITE
Flags: CommitCharge: 4, MemCommit: 1, PrivateMemory: 1, Protection: 6

0x000000004dc40000  00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00   ................
0x000000004dc40010  00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00   ................
0x000000004dc40020  00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00   ................
0x000000004dc40030  00 00 00 00 23 00 23 00 01 00 00 00 00 00 00 00   ....#.#.........



Process: winlogon.exe Pid: 608 Address: 0x4c540000
Vad Tag: VadS Protection: PAGE_EXECUTE_READWRITE
Flags: CommitCharge: 4, MemCommit: 1, PrivateMemory: 1, Protection: 6

0x000000004c540000  00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00   ................
0x000000004c540010  00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00   ................
0x000000004c540020  00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00   ................
0x000000004c540030  00 00 00 00 22 00 22 00 01 00 00 00 00 00 00 00   ....".".........



Process: winlogon.exe Pid: 608 Address: 0x5de10000
Vad Tag: VadS Protection: PAGE_EXECUTE_READWRITE
Flags: CommitCharge: 4, MemCommit: 1, PrivateMemory: 1, Protection: 6

0x000000005de10000  00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00   ................
0x000000005de10010  00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00   ................
0x000000005de10020  00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00   ................
0x000000005de10030  00 00 00 00 22 00 22 00 01 00 00 00 00 00 00 00   ....".".........



Process: winlogon.exe Pid: 608 Address: 0x6a230000
Vad Tag: VadS Protection: PAGE_EXECUTE_READWRITE
Flags: CommitCharge: 4, MemCommit: 1, PrivateMemory: 1, Protection: 6

0x000000006a230000  00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00   ................
0x000000006a230010  00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00   ................
0x000000006a230020  00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00   ................
0x000000006a230030  00 00 00 00 2b 00 2b 00 01 00 00 00 00 00 00 00   ....+.+.........



Process: winlogon.exe Pid: 608 Address: 0x73f40000
Vad Tag: VadS Protection: PAGE_EXECUTE_READWRITE
Flags: CommitCharge: 4, MemCommit: 1, PrivateMemory: 1, Protection: 6

0x0000000073f40000  00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00   ................
0x0000000073f40010  00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00   ................
0x0000000073f40020  00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00   ................
0x0000000073f40030  00 00 00 00 2a 00 2a 00 01 00 00 00 00 00 00 00   ....*.*.........



Process: explorer.exe Pid: 1484 Address: 0x1460000
Vad Tag: VadS Protection: PAGE_EXECUTE_READWRITE
Flags: CommitCharge: 33, MemCommit: 1, PrivateMemory: 1, Protection: 6

0x0000000001460000  4d 5a 90 00 03 00 00 00 04 00 00 00 ff ff 00 00   MZ..............
0x0000000001460010  b8 00 00 00 00 00 00 00 40 00 00 00 00 00 00 00   ........@.......
0x0000000001460020  00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00   ................
0x0000000001460030  00 00 00 00 00 00 00 00 00 00 00 00 e0 00 00 00   ................



Process: reader_sl.exe Pid: 1640 Address: 0x3d0000
Vad Tag: VadS Protection: PAGE_EXECUTE_READWRITE
Flags: CommitCharge: 33, MemCommit: 1, PrivateMemory: 1, Protection: 6

0x00000000003d0000  4d 5a 90 00 03 00 00 00 04 00 00 00 ff ff 00 00   MZ..............
0x00000000003d0010  b8 00 00 00 00 00 00 00 40 00 00 00 00 00 00 00   ........@.......
0x00000000003d0020  00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00   ................
0x00000000003d0030  00 00 00 00 00 00 00 00 00 00 00 00 e0 00 00 00   ................
```

`12`

Last but certainly not least we can view all of the DLLs loaded into memory. DLLs are shared system libraries utilized in system processes. These are commonly subjected to hijacking and other side-loading attacks, making them a key target for forensics. Let's list all of the DLLs in memory now with the command `dlllist`

`No answer needed`

Now that we've seen all of the DLLs running in memory, let's go a step further and pull them out! Do this now with the command `volatility -f MEMORY_FILE.raw --profile=PROFILE --pid=PID dlldump -D <Destination Directory>` where the PID is the process ID of the infected process we identified earlier (questions five and six). How many DLLs does this end up pulling?

```
kali@kali:~/CTFs/tryhackme/Volatility$ sudo python /opt/volatility/vol.py -f cridex.vmem --profile=WinXPSP2x86 --pid=584 dlldump -D /tmp
Volatility Foundation Volatility Framework 2.6.1
Process(V) Name                 Module Base Module Name          Result
---------- -------------------- ----------- -------------------- ------
0x822a0598 csrss.exe            0x04a680000 csrss.exe            OK: module.584.24a0598.4a680000.dll
0x822a0598 csrss.exe            0x07c900000 ntdll.dll            OK: module.584.24a0598.7c900000.dll
0x822a0598 csrss.exe            0x075b40000 CSRSRV.dll           OK: module.584.24a0598.75b40000.dll
0x822a0598 csrss.exe            0x077f10000 GDI32.dll            OK: module.584.24a0598.77f10000.dll
0x822a0598 csrss.exe            0x07e720000 sxs.dll              OK: module.584.24a0598.7e720000.dll
0x822a0598 csrss.exe            0x077e70000 RPCRT4.dll           OK: module.584.24a0598.77e70000.dll
0x822a0598 csrss.exe            0x077dd0000 ADVAPI32.dll         OK: module.584.24a0598.77dd0000.dll
0x822a0598 csrss.exe            0x077fe0000 Secur32.dll          OK: module.584.24a0598.77fe0000.dll
0x822a0598 csrss.exe            0x075b50000 basesrv.dll          OK: module.584.24a0598.75b50000.dll
0x822a0598 csrss.exe            0x07c800000 KERNEL32.dll         OK: module.584.24a0598.7c800000.dll
0x822a0598 csrss.exe            0x07e410000 USER32.dll           OK: module.584.24a0598.7e410000.dll
0x822a0598 csrss.exe            0x075b60000 winsrv.dll           OK: module.584.24a0598.75b60000.dll
```

`12`

## Task 4 Post Actions

Now that we've performed some basic forensics, let's go a step further and see what the community at large has to say about the items we've discovered. Check out the following two sites and upload the injected code we yanked out of our previous section. You can pull this code either via SCP with the box above, your local volatility workstation, or via a download link attached to this task.

- VirusTotal - [Link](https://www.virustotal.com/gui/home/upload)
- Hybrid Analysis - [Link](https://www.hybrid-analysis.com/)

Upload the extracted files to VirusTotal for examination.

Upload the extracted files to Hybrid Analysis for examination - Note, this will also upload to VirusTotal but for the sake of demonstration we have done this separately.

What malware has our sample been infected with? You can find this in the results of VirusTotal and Hybrid Anaylsis.

- https://hybrid-analysis.com/file-collection/5ebefdc9ef012d294567a3a8
- https://www.virustotal.com/gui/file/e00a1143fea8568f5bcbe2793c6b87032ba57f2fdd122266ea799658169d36b2/detection

`Cridex`

## Task 5 Extra Credit

Interested in going further? Here's a slew of awesome resources (both paid and free in no particular order) to check out and learn more!

**AlienVault Open Threat Exchange (OTX)** - An open-source threat tracking system. Create pulses based on your malware analysis work and check out the work of others. [Link](https://otx.alienvault.com/dashboard/new)

![](https://i.imgur.com/JKq44GK.png)

**SANS 408** - Windows Forensic Analysis [Link](https://www.sans.org/blog/for408-windows-forensic-analysis-has-been-renumbered-to-for500-windows-forensics-analysis/)

"[Memory Forensics with Vol(a|u)tility](https://youtu.be/dB5852eAgpc)" - A great talk on learning the basics of [Volatility](https://github.com/kevthehermit/VolUtility) and the GUI plugin VolUtility made by [@chupath1ngee](https://twitter.com/chupath1ngee?lang=en)

"The Art of Memory Forensics" - [Link](https://www.amazon.com/Art-Memory-Forensics-Detecting-Malware/dp/1118825098/ref=sr_1_2?crid=W5I8MKDUXVWD&keywords=the+art+of+memory+forensics&qid=1582172165&sprefix=the+art+of+memory+%2Caps%2C152&sr=8-2)

![](https://i.imgur.com/j2BBJxa.jpg)

**MemLabs** - A collection of CTF-style memory forensic labs [Link](https://github.com/stuxnet999/MemLabs)

Check out the resources provided above!

`No answer needed`
