# Forensics

This is a memory dump of compromised system, do some forensics kung-fu to explore the inside.

[Forensics](https://tryhackme.com/room/forensics)

## Topic's

* System Forensic
* Volatility Framework

## Task 1 Volatility forensics

This is a memory dump of the infected system download the file.

md5 hash of uncompressed file is ba44c4b977d28132faeb5fb8b06debce

1. Download the victim.zip

`No answer needed`

2. Whats is the OS of this Dump? (Just write OS name in small)

```
kali@kali:~/CTFs/tryhackme/Forensics/Task 1$ vol.py -f victim.raw imageinfo
Volatility Foundation Volatility Framework 2.6.1
INFO    : volatility.debug    : Determining profile based on KDBG search...
          Suggested Profile(s) : Win7SP1x64, Win7SP0x64, Win2008R2SP0x64, Win2008R2SP1x64_24000, Win2008R2SP1x64_23418, Win2008R2SP1x64, Win7SP1x64_24000, Win7SP1x64_23418
                     AS Layer1 : WindowsAMD64PagedMemory (Kernel AS)
                     AS Layer2 : FileAddressSpace (/home/kali/CTFs/tryhackme/Forensics/Task 1/victim.raw)
                      PAE type : No PAE
                           DTB : 0x187000L
                          KDBG : 0xf800028420a0L
          Number of Processors : 1
     Image Type (Service Pack) : 1
                KPCR for CPU 0 : 0xfffff80002843d00L
             KUSER_SHARED_DATA : 0xfffff78000000000L
           Image date and time : 2019-05-02 18:11:45 UTC+0000
     Image local date and time : 2019-05-02 11:11:45 -0700
```

`windows`

3. Whats is the PID of SearchIndexer ?

```
kali@kali:~/CTFs/tryhackme/Forensics/Task 1$ vol.py -f victim.raw --profile=Win7SP1x64 pslist | grep SearchIndexer
Volatility Foundation Volatility Framework 2.6.1
0xfffffa8003367060 SearchIndexer.         2180    504     11      629      0      0 2019-05-02 18:03:32 UTC+0000
```

`2180`

4. What is the last directory accessed by the user? (Just write last folder name as it is?

```                             
kali@kali:~/CTFs/tryhackme/Forensics/Task 1$ vol.py -f victim.raw --profil=Win7SP1x64 shellbags
Volatility Foundation Volatility Framework 2.6.1
Scanning for registries....
Gathering shellbag items and building path tree...
***************************************************************************
Registry: \??\C:\Users\victim\AppData\Local\Microsoft\Windows\UsrClass.dat 
Key: Local Settings\Software\Microsoft\Windows\Shell\BagMRU\1\2\0
Last updated: 2019-04-27 10:48:33 UTC+0000
Value   Mru   File Name      Modified Date                  Create Date                    Access Date                    File Attr                 Path
------- ----- -------------- ------------------------------ ------------------------------ ------------------------------ ------------------------- ----
0       0     deleted_files  2019-04-27 10:30:26 UTC+0000   2019-04-27 10:38:24 UTC+0000   2019-04-27 10:38:24 UTC+0000   NI, DIR                   Z:\logs\deleted_files
***************************************************************************
```

`deleted_files`

## Task 2 Task2

Dig little more...

1. There are many suspicious open port, which is it ?(protocol:port)

```
kali@kali:~/CTFs/tryhackme/Forensics/Task 2$ vol.py -f victim.raw --profile=Win7SP1x64 netscan
Volatility Foundation Volatility Framework 2.6.1
Offset(P)          Proto    Local Address                  Foreign Address      State            Pid      Owner          Created
0x5c201ca0         UDPv4    0.0.0.0:5005                   *:*                                   2464     wmpnetwk.exe   2019-05-02 18:05:14 UTC+0000
0x5c201ca0         UDPv6    :::5005                        *:*                                   2464     wmpnetwk.exe   2019-05-02 18:05:14 UTC+0000
0x5c49cbb0         UDPv4    0.0.0.0:59471                  *:*                                   1368     svchost.exe    2019-05-02 18:03:06 UTC+0000
0x5c4a31c0         UDPv4    0.0.0.0:59472                  *:*                                   1368     svchost.exe    2019-05-02 18:03:06 UTC+0000
0x5c4a31c0         UDPv6    :::59472                       *:*                                   1368     svchost.exe    2019-05-02 18:03:06 UTC+0000
0x5c4ac630         UDPv4    0.0.0.0:3702                   *:*                                   1368     svchost.exe    2019-05-02 18:03:14 UTC+0000
0x5c4ac630         UDPv6    :::3702                        *:*                                   1368     svchost.exe    2019-05-02 18:03:14 UTC+0000
0x5c519b30         UDPv4    0.0.0.0:3702                   *:*                                   1368     svchost.exe    2019-05-02 18:03:14 UTC+0000
0x5c537ec0         UDPv4    0.0.0.0:3702                   *:*                                   1368     svchost.exe    2019-05-02 18:03:14 UTC+0000
0x5c690360         UDPv4    0.0.0.0:0                      *:*                                   1004     svchost.exe    2019-05-02 18:02:56 UTC+0000
0x5c690360         UDPv6    :::0                           *:*                                   1004     svchost.exe    2019-05-02 18:02:56 UTC+0000
0x5c6918e0         UDPv4    0.0.0.0:5355                   *:*                                   1004     svchost.exe    2019-05-02 18:02:56 UTC+0000
0x5c6918e0         UDPv6    :::5355                        *:*                                   1004     svchost.exe    2019-05-02 18:02:56 UTC+0000
0x5c692940         UDPv4    0.0.0.0:5005                   *:*                                   2464     wmpnetwk.exe   2019-05-02 18:05:14 UTC+0000
0x5c692ae0         UDPv4    0.0.0.0:5355                   *:*                                   1004     svchost.exe    2019-05-02 18:02:56 UTC+0000
0x5c7bac70         UDPv4    0.0.0.0:5004                   *:*                                   2464     wmpnetwk.exe   2019-05-02 18:05:14 UTC+0000
0x5c7bac70         UDPv6    :::5004                        *:*                                   2464     wmpnetwk.exe   2019-05-02 18:05:14 UTC+0000
0x5c7f9600         UDPv4    0.0.0.0:3702                   *:*                                   1368     svchost.exe    2019-05-02 18:03:14 UTC+0000
0x5c7f9600         UDPv6    :::3702                        *:*                                   1368     svchost.exe    2019-05-02 18:03:14 UTC+0000
0x5c44e1b0         TCPv4    0.0.0.0:5357                   0.0.0.0:0            LISTENING        4        System         
0x5c44e1b0         TCPv6    :::5357                        :::0                 LISTENING        4        System         
0x5c528010         TCPv4    0.0.0.0:445                    0.0.0.0:0            LISTENING        4        System         
0x5c528010         TCPv6    :::445                         :::0                 LISTENING        4        System         
0x5c534c60         TCPv4    0.0.0.0:49156                  0.0.0.0:0            LISTENING        504      services.exe   
0x5c534c60         TCPv6    :::49156                       :::0                 LISTENING        504      services.exe   
0x5c535010         TCPv4    0.0.0.0:49156                  0.0.0.0:0            LISTENING        504      services.exe   
0x5c6de720         TCPv4    0.0.0.0:49154                  0.0.0.0:0            LISTENING        920      svchost.exe    
0x5c6de720         TCPv6    :::49154                       :::0                 LISTENING        920      svchost.exe    
0x5c6e0df0         TCPv4    0.0.0.0:49154                  0.0.0.0:0            LISTENING        920      svchost.exe    
0x5c717460         TCPv4    0.0.0.0:49155                  0.0.0.0:0            LISTENING        512      lsass.exe      
0x5ca3ecc0         UDPv6    ::1:1900                       *:*                                   1368     svchost.exe    2019-05-02 18:05:13 UTC+0000
0x5ca452c0         UDPv6    fe80::6998:27e6:5653:fc35:1900 *:*                                   1368     svchost.exe    2019-05-02 18:05:13 UTC+0000
0x5ca4c2c0         UDPv6    fe80::1503:ac56:439f:bb6c:1900 *:*                                   1368     svchost.exe    2019-05-02 18:05:13 UTC+0000
0x5ca517c0         UDPv4    0.0.0.0:5004                   *:*                                   2464     wmpnetwk.exe   2019-05-02 18:05:14 UTC+0000
0x5ca5a7c0         UDPv4    127.0.0.1:1900                 *:*                                   1368     svchost.exe    2019-05-02 18:05:13 UTC+0000
0x5ca5d7c0         UDPv4    169.254.252.53:1900            *:*                                   1368     svchost.exe    2019-05-02 18:05:13 UTC+0000
0x5ca655a0         UDPv4    127.0.0.1:61556                *:*                                   1368     svchost.exe    2019-05-02 18:05:13 UTC+0000
0x5caa6250         UDPv4    192.168.35.2:138               *:*                                   4        System         2019-05-03 06:32:31 UTC+0000
0x5cab3010         UDPv4    192.168.35.2:137               *:*                                   4        System         2019-05-03 06:32:31 UTC+0000
0x5cab65a0         UDPv4    169.254.252.53:137             *:*                                   4        System         2019-05-03 06:32:40 UTC+0000
0x5caefec0         UDPv4    169.254.252.53:138             *:*                                   4        System         2019-05-03 06:32:40 UTC+0000
0x5c932da0         TCPv4    0.0.0.0:135                    0.0.0.0:0            LISTENING        752      svchost.exe    
0x5c948330         TCPv4    0.0.0.0:135                    0.0.0.0:0            LISTENING        752      svchost.exe    
0x5c948330         TCPv6    :::135                         :::0                 LISTENING        752      svchost.exe    
0x5c9541a0         TCPv4    0.0.0.0:49152                  0.0.0.0:0            LISTENING        416      wininit.exe    
0x5c9541a0         TCPv6    :::49152                       :::0                 LISTENING        416      wininit.exe    
0x5c954900         TCPv4    0.0.0.0:49152                  0.0.0.0:0            LISTENING        416      wininit.exe    
0x5c996bd0         TCPv4    0.0.0.0:49153                  0.0.0.0:0            LISTENING        852      svchost.exe    
0x5c996bd0         TCPv6    :::49153                       :::0                 LISTENING        852      svchost.exe    
0x5c99c180         TCPv4    0.0.0.0:49153                  0.0.0.0:0            LISTENING        852      svchost.exe    
0x5cab60e0         TCPv4    192.168.35.2:139               0.0.0.0:0            LISTENING        4        System         
0x5cab95d0         TCPv4    169.254.252.53:139             0.0.0.0:0            LISTENING        4        System         
0x5cabcdd0         TCPv4    0.0.0.0:554                    0.0.0.0:0            LISTENING        2464     wmpnetwk.exe   
0x5cdd2950         TCPv4    0.0.0.0:49155                  0.0.0.0:0            LISTENING        512      lsass.exe      
0x5cdd2950         TCPv6    :::49155                       :::0                 LISTENING        512      lsass.exe      
0x5c949290         TCPv6    -:0                            c801:b602:80fa:ffff:c801:b602:80fa:ffff:0 CLOSED           1        ?????????????? 
0x5cad94a0         TCPv6    -:49158                        ::1:2869             CLOSED           2464     wmpnetwk.exe   
0x5d5e8960         TCPv4    0.0.0.0:10243                  0.0.0.0:0            LISTENING        4        System         
0x5d5e8960         TCPv6    :::10243                       :::0                 LISTENING        4        System         
0x5d5f79c0         TCPv4    0.0.0.0:554                    0.0.0.0:0            LISTENING        2464     wmpnetwk.exe   
0x5d5f79c0         TCPv6    :::554                         :::0                 LISTENING        2464     wmpnetwk.exe   
0x5de66420         UDPv4    0.0.0.0:0                      *:*                                   688      VBoxService.ex 2019-05-02 18:11:42 UTC+0000
0x5e00dbe0         UDPv6    fe80::1503:ac56:439f:bb6c:546  *:*                                   852      svchost.exe    2019-05-02 18:10:03 UTC+0000
0x5e0e43b0         UDPv4    0.0.0.0:68                     *:*                                   852      svchost.exe    2019-05-02 18:09:56 UTC+0000
0x5e11d1b0         UDPv6    fe80::6998:27e6:5653:fc35:546  *:*                                   852      svchost.exe    2019-05-02 18:10:03 UTC+0000
0x5e2a6010         UDPv6    ::1:61555                      *:*                                   1368     svchost.exe    2019-05-02 18:05:13 UTC+0000
0x5e37e680         UDPv4    192.168.35.2:1900              *:*                                   1368     svchost.exe    2019-05-02 18:05:13 UTC+0000
0x5e354410         TCPv4    0.0.0.0:2869                   0.0.0.0:0            LISTENING        4        System         
0x5e354410         TCPv6    :::2869                        :::0                 LISTENING        4        System         
0x5e362010         TCPv6    -:2869                         ::1:49158            CLOSED           4        System 
```

`udp:5005`

2. Vads tag and execute protection are strong indicators of malicious processes, can you find which are they? (Pid1;Pid2;Pid3...)

```
kali@kali:~/CTFs/tryhackme/Forensics/Task 2$ vol.py -f victim.raw --profile=Win7SP1x64 malfind
Volatility Foundation Volatility Framework 2.6.1
Process: explorer.exe Pid: 1860 Address: 0x3ee0000
Vad Tag: VadS Protection: PAGE_EXECUTE_READWRITE
Flags: CommitCharge: 1, MemCommit: 1, PrivateMemory: 1, Protection: 6

0x0000000003ee0000  00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00   ................
0x0000000003ee0010  00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00   ................
0x0000000003ee0020  00 00 ee 03 00 00 00 00 00 00 00 00 00 00 00 00   ................
0x0000000003ee0030  00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00   ................

0x0000000003ee0000 0000             ADD [EAX], AL
0x0000000003ee0002 0000             ADD [EAX], AL
0x0000000003ee0004 0000             ADD [EAX], AL
0x0000000003ee0006 0000             ADD [EAX], AL
0x0000000003ee0008 0000             ADD [EAX], AL
0x0000000003ee000a 0000             ADD [EAX], AL
0x0000000003ee000c 0000             ADD [EAX], AL
0x0000000003ee000e 0000             ADD [EAX], AL
0x0000000003ee0010 0000             ADD [EAX], AL
0x0000000003ee0012 0000             ADD [EAX], AL
0x0000000003ee0014 0000             ADD [EAX], AL
0x0000000003ee0016 0000             ADD [EAX], AL
0x0000000003ee0018 0000             ADD [EAX], AL
0x0000000003ee001a 0000             ADD [EAX], AL
0x0000000003ee001c 0000             ADD [EAX], AL
0x0000000003ee001e 0000             ADD [EAX], AL
0x0000000003ee0020 0000             ADD [EAX], AL
0x0000000003ee0022 ee               OUT DX, AL
0x0000000003ee0023 0300             ADD EAX, [EAX]
0x0000000003ee0025 0000             ADD [EAX], AL
0x0000000003ee0027 0000             ADD [EAX], AL
0x0000000003ee0029 0000             ADD [EAX], AL
0x0000000003ee002b 0000             ADD [EAX], AL
0x0000000003ee002d 0000             ADD [EAX], AL
0x0000000003ee002f 0000             ADD [EAX], AL
0x0000000003ee0031 0000             ADD [EAX], AL
0x0000000003ee0033 0000             ADD [EAX], AL
0x0000000003ee0035 0000             ADD [EAX], AL
0x0000000003ee0037 0000             ADD [EAX], AL
0x0000000003ee0039 0000             ADD [EAX], AL
0x0000000003ee003b 0000             ADD [EAX], AL
0x0000000003ee003d 0000             ADD [EAX], AL
0x0000000003ee003f 00               DB 0x0

Process: explorer.exe Pid: 1860 Address: 0x3f90000
Vad Tag: VadS Protection: PAGE_EXECUTE_READWRITE
Flags: CommitCharge: 2, PrivateMemory: 1, Protection: 6

0x0000000003f90000  00 00 00 00 00 00 00 00 4b 5b b2 8d 2d d2 00 01   ........K[..-...
0x0000000003f90010  ee ff ee ff 00 00 00 00 28 01 f9 03 00 00 00 00   ........(.......
0x0000000003f90020  28 01 f9 03 00 00 00 00 00 00 f9 03 00 00 00 00   (...............
0x0000000003f90030  00 00 f9 03 00 00 00 00 80 00 00 00 00 00 00 00   ................

0x0000000003f90000 0000             ADD [EAX], AL
0x0000000003f90002 0000             ADD [EAX], AL
0x0000000003f90004 0000             ADD [EAX], AL
0x0000000003f90006 0000             ADD [EAX], AL
0x0000000003f90008 4b               DEC EBX
0x0000000003f90009 5b               POP EBX
0x0000000003f9000a b28d             MOV DL, 0x8d
0x0000000003f9000c 2dd20001ee       SUB EAX, 0xee0100d2
0x0000000003f90011 ff               DB 0xff
0x0000000003f90012 ee               OUT DX, AL
0x0000000003f90013 ff00             INC DWORD [EAX]
0x0000000003f90015 0000             ADD [EAX], AL
0x0000000003f90017 0028             ADD [EAX], CH
0x0000000003f90019 01f9             ADD ECX, EDI
0x0000000003f9001b 0300             ADD EAX, [EAX]
0x0000000003f9001d 0000             ADD [EAX], AL
0x0000000003f9001f 0028             ADD [EAX], CH
0x0000000003f90021 01f9             ADD ECX, EDI
0x0000000003f90023 0300             ADD EAX, [EAX]
0x0000000003f90025 0000             ADD [EAX], AL
0x0000000003f90027 0000             ADD [EAX], AL
0x0000000003f90029 00f9             ADD CL, BH
0x0000000003f9002b 0300             ADD EAX, [EAX]
0x0000000003f9002d 0000             ADD [EAX], AL
0x0000000003f9002f 0000             ADD [EAX], AL
0x0000000003f90031 00f9             ADD CL, BH
0x0000000003f90033 0300             ADD EAX, [EAX]
0x0000000003f90035 0000             ADD [EAX], AL
0x0000000003f90037 008000000000     ADD [EAX+0x0], AL
0x0000000003f9003d 0000             ADD [EAX], AL
0x0000000003f9003f 00               DB 0x0

Process: svchost.exe Pid: 1820 Address: 0x24f0000
Vad Tag: VadS Protection: PAGE_EXECUTE_READWRITE
Flags: CommitCharge: 128, MemCommit: 1, PrivateMemory: 1, Protection: 6

0x00000000024f0000  20 00 00 00 e0 ff 07 00 0c 00 00 00 01 00 05 00   ................
0x00000000024f0010  00 42 00 50 00 30 00 70 00 60 00 00 00 00 00 00   .B.P.0.p.`......
0x00000000024f0020  48 8b 45 28 c7 00 00 00 00 00 c7 40 04 00 00 00   H.E(.......@....
0x00000000024f0030  00 48 8b 45 28 48 8d 40 08 48 89 c2 48 8b 45 20   .H.E(H.@.H..H.E.

0x00000000024f0000 2000             AND [EAX], AL
0x00000000024f0002 0000             ADD [EAX], AL
0x00000000024f0004 e0ff             LOOPNZ 0x24f0005
0x00000000024f0006 07               POP ES
0x00000000024f0007 000c00           ADD [EAX+EAX], CL
0x00000000024f000a 0000             ADD [EAX], AL
0x00000000024f000c 0100             ADD [EAX], EAX
0x00000000024f000e 0500004200       ADD EAX, 0x420000
0x00000000024f0013 50               PUSH EAX
0x00000000024f0014 0030             ADD [EAX], DH
0x00000000024f0016 007000           ADD [EAX+0x0], DH
0x00000000024f0019 60               PUSHA
0x00000000024f001a 0000             ADD [EAX], AL
0x00000000024f001c 0000             ADD [EAX], AL
0x00000000024f001e 0000             ADD [EAX], AL
0x00000000024f0020 48               DEC EAX
0x00000000024f0021 8b4528           MOV EAX, [EBP+0x28]
0x00000000024f0024 c70000000000     MOV DWORD [EAX], 0x0
0x00000000024f002a c7400400000000   MOV DWORD [EAX+0x4], 0x0
0x00000000024f0031 48               DEC EAX
0x00000000024f0032 8b4528           MOV EAX, [EBP+0x28]
0x00000000024f0035 48               DEC EAX
0x00000000024f0036 8d4008           LEA EAX, [EAX+0x8]
0x00000000024f0039 48               DEC EAX
0x00000000024f003a 89c2             MOV EDX, EAX
0x00000000024f003c 48               DEC EAX
0x00000000024f003d 8b4520           MOV EAX, [EBP+0x20]

Process: svchost.exe Pid: 1820 Address: 0x4d90000
Vad Tag: VadS Protection: PAGE_EXECUTE_READWRITE
Flags: CommitCharge: 256, MemCommit: 1, PrivateMemory: 1, Protection: 6

0x0000000004d90000  20 00 00 00 e0 ff 0f 00 0c 00 00 00 01 00 05 00   ................
0x0000000004d90010  00 42 00 50 00 30 00 70 00 60 00 00 00 00 00 00   .B.P.0.p.`......
0x0000000004d90020  ba fc ff ff ff 03 55 20 03 55 5c b9 04 00 1a 00   ......U..U\.....
0x0000000004d90030  4c 8b c5 ff 95 e0 37 00 00 8b 4d 24 89 08 48 8d   L.....7...M$..H.

0x0000000004d90000 2000             AND [EAX], AL
0x0000000004d90002 0000             ADD [EAX], AL
0x0000000004d90004 e0ff             LOOPNZ 0x4d90005
0x0000000004d90006 0f000c00         STR WORD [EAX+EAX]
0x0000000004d9000a 0000             ADD [EAX], AL
0x0000000004d9000c 0100             ADD [EAX], EAX
0x0000000004d9000e 0500004200       ADD EAX, 0x420000
0x0000000004d90013 50               PUSH EAX
0x0000000004d90014 0030             ADD [EAX], DH
0x0000000004d90016 007000           ADD [EAX+0x0], DH
0x0000000004d90019 60               PUSHA
0x0000000004d9001a 0000             ADD [EAX], AL
0x0000000004d9001c 0000             ADD [EAX], AL
0x0000000004d9001e 0000             ADD [EAX], AL
0x0000000004d90020 bafcffffff       MOV EDX, 0xfffffffc
0x0000000004d90025 035520           ADD EDX, [EBP+0x20]
0x0000000004d90028 03555c           ADD EDX, [EBP+0x5c]
0x0000000004d9002b b904001a00       MOV ECX, 0x1a0004
0x0000000004d90030 4c               DEC ESP
0x0000000004d90031 8bc5             MOV EAX, EBP
0x0000000004d90033 ff95e0370000     CALL DWORD [EBP+0x37e0]
0x0000000004d90039 8b4d24           MOV ECX, [EBP+0x24]
0x0000000004d9003c 8908             MOV [EAX], ECX
0x0000000004d9003e 48               DEC EAX
0x0000000004d9003f 8d               DB 0x8d

Process: wmpnetwk.exe Pid: 2464 Address: 0x280000
Vad Tag: VadS Protection: PAGE_EXECUTE_READWRITE
Flags: CommitCharge: 16, MemCommit: 1, PrivateMemory: 1, Protection: 6

0x0000000000280000  41 ba 80 00 00 00 48 b8 38 a1 e6 ff fe 07 00 00   A.....H.8.......
0x0000000000280010  48 ff 20 90 41 ba 81 00 00 00 48 b8 38 a1 e6 ff   H...A.....H.8...
0x0000000000280020  fe 07 00 00 48 ff 20 90 41 ba 82 00 00 00 48 b8   ....H...A.....H.
0x0000000000280030  38 a1 e6 ff fe 07 00 00 48 ff 20 90 41 ba 83 00   8.......H...A...

0x0000000000280000 41               INC ECX
0x0000000000280001 ba80000000       MOV EDX, 0x80
0x0000000000280006 48               DEC EAX
0x0000000000280007 b838a1e6ff       MOV EAX, 0xffe6a138
0x000000000028000c fe07             INC BYTE [EDI]
0x000000000028000e 0000             ADD [EAX], AL
0x0000000000280010 48               DEC EAX
0x0000000000280011 ff20             JMP DWORD [EAX]
0x0000000000280013 90               NOP
0x0000000000280014 41               INC ECX
0x0000000000280015 ba81000000       MOV EDX, 0x81
0x000000000028001a 48               DEC EAX
0x000000000028001b b838a1e6ff       MOV EAX, 0xffe6a138
0x0000000000280020 fe07             INC BYTE [EDI]
0x0000000000280022 0000             ADD [EAX], AL
0x0000000000280024 48               DEC EAX
0x0000000000280025 ff20             JMP DWORD [EAX]
0x0000000000280027 90               NOP
0x0000000000280028 41               INC ECX
0x0000000000280029 ba82000000       MOV EDX, 0x82
0x000000000028002e 48               DEC EAX
0x000000000028002f b838a1e6ff       MOV EAX, 0xffe6a138
0x0000000000280034 fe07             INC BYTE [EDI]
0x0000000000280036 0000             ADD [EAX], AL
0x0000000000280038 48               DEC EAX
0x0000000000280039 ff20             JMP DWORD [EAX]
0x000000000028003b 90               NOP
0x000000000028003c 41               INC ECX
0x000000000028003d ba               DB 0xba
0x000000000028003e 83               DB 0x83
0x000000000028003f 00               DB 0x0
```

`1860;1820;2464`

## Task 3 IOC SAGA

In lats task you have identified malicious processes, so lets dig into them and find some IOC's. you just need to find them and fill the blanks (You may search them on VirusTotal for more details :)

1. `'www.go****.ru'` (write full url without any quotation marks)

```
kali@kali:~/CTFs/tryhackme/Forensics/Task 3$ strings victim.raw | grep 'www.go.....ru'
www.google.ru
www.go4win.ru
www.gocaps.ru
www.goporn.ru
      <URL>http://www.google.ru/</URL>
```

`www.goporn.ru`

2. `'www.i****.com'` (write full url without any quotation marks)

```
kali@kali:~/CTFs/tryhackme/Forensics/Task 3$ strings victim.raw | grep 'www.i.....com'
http://www.iciba.com/search?s=%si
www.ikaka.com
```

`www.ikaka.com`

3. `'www.ic******.com'`

```
kali@kali:~/CTFs/tryhackme/Forensics/Task 3$ strings victim.raw | grep 'www.ic.......com'
www.icsalabs.com
```

`www.icsalabs.com`

4. `202.***.233.***` (Write full IP)

```
kali@kali:~/CTFs/tryhackme/Forensics/Task 3$ strings victim.raw | grep '202.....233....'
202.107.233.211
```

`202.107.233.211`

5. `***.200.**.164` (Write full IP)

```
kali@kali:~/CTFs/tryhackme/Forensics/Task 3$ strings victim.raw | grep '....200....164'
phttp://209.200.12.164/drm/provider_license_v7.php
```

`209.200.12.164`

6. `209.190.***.***`

```
kali@kali:~/CTFs/tryhackme/Forensics/Task 3$ strings victim.raw | grep '209.190........'
`http://209.190.122.186/drm/license-savenow.asp
```

`209.190.122.186`

7. What is an unique environmental variable of PID 2464

```
kali@kali:~/CTFs/tryhackme/Forensics/Task 3$ vol.py -f victim.raw --profile=Win7SP1x64 envars | grep 2464
Volatility Foundation Volatility Framework 2.6.1
    2464 wmpnetwk.exe         0x00000000002c47a0 ALLUSERSPROFILE                C:\ProgramData
    2464 wmpnetwk.exe         0x00000000002c47a0 APPDATA                        C:\Windows\ServiceProfiles\NetworkService\AppData\Roaming
    2464 wmpnetwk.exe         0x00000000002c47a0 CommonProgramFiles             C:\Program Files\Common Files
    2464 wmpnetwk.exe         0x00000000002c47a0 CommonProgramFiles(x86)        C:\Program Files (x86)\Common Files
    2464 wmpnetwk.exe         0x00000000002c47a0 CommonProgramW6432             C:\Program Files\Common Files
    2464 wmpnetwk.exe         0x00000000002c47a0 COMPUTERNAME                   VICTIM-PC
    2464 wmpnetwk.exe         0x00000000002c47a0 ComSpec                        C:\Windows\system32\cmd.exe
    2464 wmpnetwk.exe         0x00000000002c47a0 FP_NO_HOST_CHECK               NO
    2464 wmpnetwk.exe         0x00000000002c47a0 LOCALAPPDATA                   C:\Windows\ServiceProfiles\NetworkService\AppData\Local
    2464 wmpnetwk.exe         0x00000000002c47a0 NUMBER_OF_PROCESSORS           1
    2464 wmpnetwk.exe         0x00000000002c47a0 OANOCACHE                      1
    2464 wmpnetwk.exe         0x00000000002c47a0 OS                             Windows_NT
    2464 wmpnetwk.exe         0x00000000002c47a0 Path                           C:\Windows\system32;C:\Windows;C:\Windows\System32\Wbem;C:\Windows\System32\WindowsPowerShell\v1.0\
    2464 wmpnetwk.exe         0x00000000002c47a0 PATHEXT                        .COM;.EXE;.BAT;.CMD;.VBS;.VBE;.JS;.JSE;.WSF;.WSH;.MSC
    2464 wmpnetwk.exe         0x00000000002c47a0 PROCESSOR_ARCHITECTURE         AMD64
    2464 wmpnetwk.exe         0x00000000002c47a0 PROCESSOR_IDENTIFIER           Intel64 Family 6 Model 42 Stepping 7, GenuineIntel
    2464 wmpnetwk.exe         0x00000000002c47a0 PROCESSOR_LEVEL                6
    2464 wmpnetwk.exe         0x00000000002c47a0 PROCESSOR_REVISION             2a07
    2464 wmpnetwk.exe         0x00000000002c47a0 ProgramData                    C:\ProgramData
    2464 wmpnetwk.exe         0x00000000002c47a0 ProgramFiles                   C:\Program Files
    2464 wmpnetwk.exe         0x00000000002c47a0 ProgramFiles(x86)              C:\Program Files (x86)
    2464 wmpnetwk.exe         0x00000000002c47a0 ProgramW6432                   C:\Program Files
    2464 wmpnetwk.exe         0x00000000002c47a0 PSModulePath                   C:\Windows\system32\WindowsPowerShell\v1.0\Modules\
    2464 wmpnetwk.exe         0x00000000002c47a0 PUBLIC                         C:\Users\Public
    2464 wmpnetwk.exe         0x00000000002c47a0 SystemDrive                    C:
    2464 wmpnetwk.exe         0x00000000002c47a0 SystemRoot                     C:\Windows
    2464 wmpnetwk.exe         0x00000000002c47a0 TEMP                           C:\Windows\SERVIC~2\NETWOR~1\AppData\Local\Temp
    2464 wmpnetwk.exe         0x00000000002c47a0 TMP                            C:\Windows\SERVIC~2\NETWOR~1\AppData\Local\Temp
    2464 wmpnetwk.exe         0x00000000002c47a0 USERDOMAIN                     WORKGROUP
    2464 wmpnetwk.exe         0x00000000002c47a0 USERNAME                       VICTIM-PC$
    2464 wmpnetwk.exe         0x00000000002c47a0 USERPROFILE                    C:\Windows\ServiceProfiles\NetworkService
    2464 wmpnetwk.exe         0x00000000002c47a0 windir                         C:\Windows
    2464 wmpnetwk.exe         0x00000000002c47a0 windows_tracing_flags          3
    2464 wmpnetwk.exe         0x00000000002c47a0 windows_tracing_logfile        C:\BVTBin\Tests\installpackage\csilogfile.log
```

`OANOCACHE`
