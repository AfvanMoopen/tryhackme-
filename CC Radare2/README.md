# CC: Radare2

An in-depth crash course on Radare2

[CC: Radare2](https://tryhackme.com/room/ccradare2)

## Topic's

- Reverse Engineering
- Radar 2 Fundamentals

## Appendix archive

Password: `1 kn0w 1 5h0uldn'7!`

## Task 1 Intro

This room assumes that you have basic x86 assembly knowledge. If you do not I highly recommend doing the [Intro to x86-64](https://tryhackme.com/room/introtox8664) room before completing this done.

This room is also not designed to be a 100% teach everything on radare2. It is designed to teach you how some of the more common things in radare2 are used.

The included zip file has all the binaries you will need for this exercise.

With that out of the way let's get started!

1. Read the above

`No answer needed`

## Task 2 Command Line Options

A quick intro to some of the commonly used command line flags for radare2, some of these flags will be extremely useful for later tasks. Include all parts of the flag including the -. All flags can be found in the help menu

```
kali@kali:~/CTFs/tryhackme/CC: Radare2$ r2 -h
Usage: r2 [-ACdfLMnNqStuvwzX] [-P patch] [-p prj] [-a arch] [-b bits] [-i file]
          [-s addr] [-B baddr] [-m maddr] [-c cmd] [-e k=v] file|pid|-|--|=
 --           run radare2 without opening any file
 -            same as 'r2 malloc://512'
 =            read file from stdin (use -i and -c to run cmds)
 -=           perform !=! command to run all commands remotely
 -0           print \x00 after init and every command
 -2           close stderr file descriptor (silent warning messages)
 -a [arch]    set asm.arch
 -A           run 'aaa' command to analyze all referenced code
 -b [bits]    set asm.bits
 -B [baddr]   set base address for PIE binaries
 -c 'cmd..'   execute radare command
 -C           file is host:port (alias for -c+=http://%s/cmd/)
 -d           debug the executable 'file' or running process 'pid'
 -D [backend] enable debug mode (e cfg.debug=true)
 -e k=v       evaluate config var
 -f           block size = file size
 -F [binplug] force to use that rbin plugin
 -h, -hh      show help message, -hh for long
 -H ([var])   display variable
 -i [file]    run script file
 -I [file]    run script file before the file is opened
 -k [OS/kern] set asm.os (linux, macos, w32, netbsd, ...)
 -l [lib]     load plugin file
 -L           list supported IO plugins
 -m [addr]    map file at given address (loadaddr)
 -M           do not demangle symbol names
 -n, -nn      do not load RBin info (-nn only load bin structures)
 -N           do not load user settings and scripts
 -q           quiet mode (no prompt) and quit after -i
 -qq          quit after running all -c and -i
 -Q           quiet mode (no prompt) and quit faster (quickLeak=true)
 -p [prj]     use project, list if no arg, load if no file
 -P [file]    apply rapatch file and quit
 -r [rarun2]  specify rarun2 profile to load (same as -e dbg.profile=X)
 -R [rr2rule] specify custom rarun2 directive
 -s [addr]    initial seek
 -S           start r2 in sandbox mode
 -T           do not compute file hashes
 -u           set bin.filter=false to get raw sym/sec/cls names
 -v, -V       show radare2 version (-V show lib versions)
 -w           open file in write mode
 -x           open without exec-flag (asm.emu will not work), See io.exec
 -X           same as -e bin.usextr=false (useful for dyldcache)
 -z, -zz      do not load strings or load them even in raw
```

1. What flag to you set to analyze the binary upon entering the r2 console (equivalent to running aaa once your inside the console)

`-A`

2. How do you enable the debugger?

`-D`

3. How do you open the file in write mode?

`-w`

4. How do you enter the console without opening a file

`-`

## Task 3 Analyzation

Once inside the radare console you have a myriad of options to analyze your binary. Generally all analyzation commands start with the letter a. If you want to list all possible commands that can be done with your starting letter(s) you add a question mark to the end. For example `a?` would output `ab`,`aa`,`ac` along with a description on what each command does.

```
kali@kali:~/CTFs/tryhackme/CC: Radare2$ r2 -d z/example1
Process with PID 13998 started...
= attach 13998 13998
bin.baddr 0x5599ce1ed000
Using 0x5599ce1ed000
asm.bits 64
Warning: r_bin_file_hash: file exceeds bin.hashlimit
[0x7fec82b5c090]> aaa
[x] Analyze all flags starting with sym. and entry0 (aa)
[x] Analyze function calls (aac)
[x] Analyze len bytes of instructions for references (aar)
[x] Check for objc references
[x] Check for vtables
[TOFIX: aaft can't run in debugger mode.ions (aaft)
[x] Type matching analysis for all functions (aaft)
[x] Propagate noreturn information
[x] Use -AA or aaaa to perform additional experimental analysis.
[0x7fec82b5c090]> afl
0x5599ce1ed530    1 42           entry0
0x5599ce3edfd8    1 4132         reloc.__libc_start_main
0x5599ce1ed560    4 50   -> 44   sym.deregister_tm_clones
0x5599ce1ed5a0    4 66   -> 57   sym.register_tm_clones
0x5599ce1ed5f0    5 50           entry.fini0
0x5599ce1ed520    1 6            sym.imp.__cxa_finalize
0x5599ce1ed630    4 48   -> 42   entry.init0
0x5599ce1ed4f8    3 23           sym._init
0x5599ce1ed6e0    1 1            sym.__libc_csu_fini
0x5599ce1ed6e4    1 9            sym._fini
0x5599ce1ed66b    1 7            sym.secret_func
0x5599ce1ed680    4 93           sym.__libc_csu_init
0x5599ce1ed660    1 11           main
```

```
[0x7fec82b5c090]> ?
Usage: [.][times][cmd][~grep][@[@iter]addr!size][|>pipe] ; ...
Append '?' to any char command to get detailed help
Prefix with number to repeat command N times (f.ex: 3x)
| %var=value              alias for 'env' command
| *[?] off[=[0x]value]    pointer read/write data/values (see ?v, wx, wv)
| (macro arg0 arg1)       manage scripting macros
| .[?] [-|(m)|f|!sh|cmd]  Define macro or load r2, cparse or rlang file
| _[?]                    Print last output
| =[?] [cmd]              send/listen for remote commands (rap://, raps://, udp://, http://, <fd>)
| <[...]                  push escaped string into the RCons.readChar buffer
| /[?]                    search for bytes, regexps, patterns, ..
| ![?] [cmd]              run given command as in system(3)
| #[?] !lang [..]         Hashbang to run an rlang script
| a[?]                    analysis commands
| b[?]                    display or change the block size
| c[?] [arg]              compare block with given data
| C[?]                    code metadata (comments, format, hints, ..)
| d[?]                    debugger commands
| e[?] [a[=b]]            list/get/set config evaluable vars
| f[?] [name][sz][at]     add flag at current address
| g[?] [arg]              generate shellcodes with r_egg
| i[?] [file]             get info about opened file from r_bin
| k[?] [sdb-query]        run sdb-query. see k? for help, 'k *', 'k **' ...
| l [filepattern]         list files and directories
| L[?] [-] [plugin]       list, unload load r2 plugins
| m[?]                    mountpoints commands
| o[?] [file] ([offset])  open file at optional address
| p[?] [len]              print current block with format and length
| P[?]                    project management utilities
| q[?] [ret]              quit program with a return value
| r[?] [len]              resize file
| s[?] [addr]             seek to address (also for '0x', '0x1' == 's 0x1')
| t[?]                    types, noreturn, signatures, C parser and more
| T[?] [-] [num|msg]      Text log utility (used to chat, sync, log, ...)
| u[?]                    uname/undo seek/write
| v                       visual mode (v! = panels, vv = fcnview, vV = fcngraph, vVV = callgraph)
| w[?] [str]              multiple write operations
| x[?] [len]              alias for 'px' (print hexadecimal)
| y[?] [len] [[[@]addr    Yank/paste bytes from/to memory
| z[?]                    zignatures management
| ?[??][expr]             Help or evaluate math expression
| ?$?                     show available '$' variables and aliases
| ?@?                     misc help for '@' (seek), '~' (grep) (see ~??)
| ?>?                     output redirection
| ?|?                     help for '|' (pipe)
```

1. What command "Analyzes Everything" (all functions and their arguments: Same as running with radare with -A)

`aaa`

2. What command does basic analysis on functions?

`af`

3. How do you list all functions?

`afl`

4. How many functions are in the example1 binary?

```
[0x7fec82b5c090]> afl | wc -l
13
```

`12`

5. What is the name of the secret function in the example1 binary?

```
0x5599ce1ed66b    1 7            sym.secret_func
```

`secret_func`

## Task 4 Information

`i` is a command that shows general information of the binary. Like `a` it has many sub commands each with varying degrees of specificity.

```
[0x7fec82b5c090]> i?
Usage: i   Get info from opened file (see rabin2's manpage)
Output mode:
| '*'                Output in radare commands
| 'j'                Output in json
| 'q'                Simple quiet output
Actions:
| i|ij               Show info of current file (in JSON)
| iA                 List archs
| ia                 Show all info (imports, exports, sections..)
| ib                 Reload the current buffer for setting of the bin (use once only)
| ic                 List classes, methods and fields
| icc                List classes, methods and fields in Header Format
| icg                List classes as agn/age commands to create class hirearchy graphs
| icq                List classes, in quiet mode (just the classname)
| icqq               List classes, in quieter mode (only show non-system classnames)
| iC[j]              Show signature info (entitlements, ...)
| id[?]              Debug information (source lines)
| idp                Load pdb file information
| iD lang sym        demangle symbolname for given language
| ie                 Entrypoint
| iee                Show Entry and Exit (preinit, init and fini)
| iE                 Exports (global symbols)
| iE.                Current export
| ih                 Headers (alias for iH)
| iHH                Verbose Headers in raw text
| ii                 Imports
| iI                 Binary info
| ik [query]         Key-value database from RBinObject
| il                 Libraries
| iL [plugin]        List all RBin plugins loaded or plugin details
| im                 Show info about predefined memory allocation
| iM                 Show main address
| io [file]          Load info from file (or last opened) use bin.baddr
| iO[?]              Perform binary operation (dump, resize, change sections, ...)
| ir                 List the Relocations
| iR                 List the Resources
| is                 List the Symbols
| is.                Current symbol
| iS [entropy,sha1]  Sections (choose which hash algorithm to use)
| iS.                Current section
| iS=                Show ascii-art color bars with the section ranges
| iSS                List memory segments (maps with om)
| it                 File hashes
| iT                 File signature
| iV                 Display file version info
| iw                 try/catch blocks
| iX                 Display source files used (via dwarf)
| iz|izj             Strings in data sections (in JSON/Base64)
| izz                Search for Strings in the whole binary
| izzz               Dump Strings from whole binary to r2 shell (for huge files)
| iz- [addr]         Purge string via bin.str.purge
| iZ                 Guess size of binary program
```

1. What command shows all the information about the file that you're in?

`ia`

2. How do you get every string that is present in the binary?

`izz`

3. What if you want the address of the main function?

`iM`

4. What character do you add to the end of every command to get the output in JSON format?

`j`

5. How do you get the entrypoint of the file?

`ie`

6. What is the secret string hidden in the example2 binary?

```
[0x7fc4bbea5090]> izz
[Strings]
nth paddr      vaddr          len size section            type    string
――――――――――――――――――――――――――――――――――――――――――――――――――――――――――――――――――――――――
0   0x00000034 0x55923fba3034 4   10                      utf16le @8\t@
1   0x00000238 0x55923fba3238 27  28   .interp            ascii   /lib64/ld-linux-x86-64.so.2
2   0x00000289 0x55923fba3289 4   5    .note.gnu.build_id ascii    mt"
3   0x00000361 0x55923fba3361 9   10   .dynstr            ascii   libc.so.6
4   0x0000036b 0x55923fba336b 14  15   .dynstr            ascii   __cxa_finalize
5   0x0000037a 0x55923fba337a 17  18   .dynstr            ascii   __libc_start_main
6   0x0000038c 0x55923fba338c 27  28   .dynstr            ascii   _ITM_deregisterTMCloneTable
7   0x000003a8 0x55923fba33a8 14  15   .dynstr            ascii   __gmon_start__
8   0x000003b7 0x55923fba33b7 19  20   .dynstr            ascii   _Jv_RegisterClasses
9   0x000003cb 0x55923fba33cb 25  26   .dynstr            ascii   _ITM_registerTMCloneTable
10  0x000003e5 0x55923fba33e5 11  12   .dynstr            ascii   GLIBC_2.2.5
11  0x000005a9 0x55923fba35a9 4   5    .text              ascii   5z\n
12  0x000005f1 0x55923fba35f1 4   5    .text              ascii   =1\n
13  0x00000693 0x55923fba3693 4   5    .text              ascii   %p\a
14  0x0000069b 0x55923fba369b 4   5    .text              ascii   -p\a
15  0x000006d1 0x55923fba36d1 11  12   .text              ascii   \b[]A\A]A^A_
16  0x00000748 0x55923fba3748 4   5    .eh_frame          ascii   \e\f\a\b
17  0x00000778 0x55923fba3778 4   5    .eh_frame          ascii   \e\f\a\b
18  0x0000079f 0x55923fba379f 5   6    .eh_frame          ascii   ;*3$"
19  0x000007d9 0x55923fba37d9 4   5    .eh_frame          ascii   T\f\a\b
20  0x000007f9 0x55923fba37f9 4   5    .eh_frame          ascii   B\f\a\b
21  0x00001028 0x55923fba3000 44  45   .comment           ascii   GCC: (Debian 6.3.0-18+deb9u1) 6.3.0 20170516
22  0x000016a1 0x55923fba3001 10  11   .strtab            ascii   crtstuff.c
23  0x000016ac 0x55923fba300c 12  13   .strtab            ascii   __JCR_LIST__
24  0x000016b9 0x55923fba3019 20  21   .strtab            ascii   deregister_tm_clones
25  0x000016ce 0x55923fba302e 21  22   .strtab            ascii   __do_global_dtors_aux
26  0x000016e4 0x55923fba3044 14  15   .strtab            ascii   completed.6972
27  0x000016f3 0x55923fba3053 38  39   .strtab            ascii   __do_global_dtors_aux_fini_array_entry
28  0x0000171a 0x55923fba307a 11  12   .strtab            ascii   frame_dummy
29  0x00001726 0x55923fba3086 30  31   .strtab            ascii   __frame_dummy_init_array_entry
30  0x00001749 0x55923fba30a9 13  14   .strtab            ascii   __FRAME_END__
31  0x00001757 0x55923fba30b7 11  12   .strtab            ascii   __JCR_END__
32  0x00001763 0x55923fba30c3 16  17   .strtab            ascii   __init_array_end
33  0x00001774 0x55923fba30d4 8   9    .strtab            ascii   _DYNAMIC
34  0x0000177d 0x55923fba30dd 18  19   .strtab            ascii   __init_array_start
35  0x00001790 0x55923fba30f0 18  19   .strtab            ascii   __GNU_EH_FRAME_HDR
36  0x000017a3 0x55923fba3103 21  22   .strtab            ascii   _GLOBAL_OFFSET_TABLE_
37  0x000017b9 0x55923fba3119 15  16   .strtab            ascii   __libc_csu_fini
38  0x000017c9 0x55923fba3129 27  28   .strtab            ascii   _ITM_deregisterTMCloneTable
39  0x000017e5 0x55923fba3145 6   7    .strtab            ascii   _edata
40  0x000017ec 0x55923fba314c 30  31   .strtab            ascii   __libc_start_main@@GLIBC_2.2.5
41  0x0000180b 0x55923fba316b 12  13   .strtab            ascii   __data_start
42  0x00001818 0x55923fba3178 14  15   .strtab            ascii   __gmon_start__
43  0x00001827 0x55923fba3187 12  13   .strtab            ascii   __dso_handle
44  0x00001834 0x55923fba3194 14  15   .strtab            ascii   _IO_stdin_used
45  0x00001843 0x55923fba31a3 11  12   .strtab            ascii   secret_func
46  0x0000184f 0x55923fba31af 15  16   .strtab            ascii   __libc_csu_init
47  0x0000185f 0x55923fba31bf 11  12   .strtab            ascii   __bss_start
48  0x0000186b 0x55923fba31cb 4   5    .strtab            ascii   main
49  0x00001870 0x55923fba31d0 19  20   .strtab            ascii   _Jv_RegisterClasses
50  0x00001884 0x55923fba31e4 11  12   .strtab            ascii   __TMC_END__
51  0x00001890 0x55923fba31f0 25  26   .strtab            ascii   _ITM_registerTMCloneTable
52  0x000018aa 0x55923fba320a 27  28   .strtab            ascii   __cxa_finalize@@GLIBC_2.2.5
53  0x000018c7 0x55923fba3001 7   8    .shstrtab          ascii   .symtab
54  0x000018cf 0x55923fba3009 7   8    .shstrtab          ascii   .strtab
55  0x000018d7 0x55923fba3011 9   10   .shstrtab          ascii   .shstrtab
56  0x000018e1 0x55923fba301b 7   8    .shstrtab          ascii   .interp
57  0x000018e9 0x55923fba3023 13  14   .shstrtab          ascii   .note.ABI-tag
58  0x000018f7 0x55923fba3031 18  19   .shstrtab          ascii   .note.gnu.build-id
59  0x0000190a 0x55923fba3044 9   10   .shstrtab          ascii   .gnu.hash
60  0x00001914 0x55923fba304e 7   8    .shstrtab          ascii   .dynsym
61  0x0000191c 0x55923fba3056 7   8    .shstrtab          ascii   .dynstr
62  0x00001924 0x55923fba305e 12  13   .shstrtab          ascii   .gnu.version
63  0x00001931 0x55923fba306b 14  15   .shstrtab          ascii   .gnu.version_r
64  0x00001940 0x55923fba307a 9   10   .shstrtab          ascii   .rela.dyn
65  0x0000194a 0x55923fba3084 5   6    .shstrtab          ascii   .init
66  0x00001950 0x55923fba308a 8   9    .shstrtab          ascii   .plt.got
67  0x00001959 0x55923fba3093 5   6    .shstrtab          ascii   .text
68  0x0000195f 0x55923fba3099 5   6    .shstrtab          ascii   .fini
69  0x00001965 0x55923fba309f 7   8    .shstrtab          ascii   .rodata
70  0x0000196d 0x55923fba30a7 13  14   .shstrtab          ascii   .eh_frame_hdr
71  0x0000197b 0x55923fba30b5 9   10   .shstrtab          ascii   .eh_frame
72  0x00001985 0x55923fba30bf 11  12   .shstrtab          ascii   .init_array
73  0x00001991 0x55923fba30cb 11  12   .shstrtab          ascii   .fini_array
74  0x0000199d 0x55923fba30d7 4   5    .shstrtab          ascii   .jcr
75  0x000019a2 0x55923fba30dc 8   9    .shstrtab          ascii   .dynamic
76  0x000019ab 0x55923fba30e5 8   9    .shstrtab          ascii   .got.plt
77  0x000019b4 0x55923fba30ee 5   6    .shstrtab          ascii   .data
78  0x000019ba 0x55923fba30f4 4   5    .shstrtab          ascii   .bss
79  0x000019bf 0x55923fba30f9 8   9    .shstrtab          ascii   .comment
80  0x00002148 0x55923fba3882 8   8                       ascii   goodjob\n
```

`goodjob`

## Task 5 Navigating Through Memory

`s` is the command that is used to navigate through the memory of your binary. With it and its variations you can you can get information about where you are in the binary as well as move to different points in the binary.

Note: For user created functions that aren't main, you will have to add sym. before them for example sym.user_func

```
[0x7fc4bbea5090]> s?
Usage: s    # Help for the seek commands. See ?$? to see all variables
| s                 Print current address
| s.hexoff          Seek honoring a base from core->offset
| s:pad             Print current address with N padded zeros (defaults to 8)
| s addr            Seek to address
| s-                Undo seek
| s-*               Reset undo seek history
| s- n              Seek n bytes backward
| s--[n]            Seek blocksize bytes backward (/=n)
| s+                Redo seek
| s+ n              Seek n bytes forward
| s++[n]            Seek blocksize bytes forward (/=n)
| s[j*=!]           List undo seek history (JSON, =list, *r2, !=names, s==)
| s/ DATA           Search for next occurrence of 'DATA'
| s/x 9091          Search for next occurrence of \x90\x91
| sa [[+-]a] [asz]  Seek asz (or bsize) aligned to addr
| sb                Seek aligned to bb start
| sC[?] string      Seek to comment matching given string
| sf                Seek to next function (f->addr+f->size)
| sf function       Seek to address of specified function
| sf.               Seek to the beginning of current function
| sg/sG             Seek begin (sg) or end (sG) of section or file
| sl[?] [+-]line    Seek to line
| sn/sp ([nkey])    Seek to next/prev location, as specified by scr.nkey
| so [N]            Seek to N next opcode(s)
| sr pc             Seek to register
| ss                Seek silently (without adding an entry to the seek history)
```

1. How do you print out the current memory address your located at in the binary?

`s`

2. What command do you use to go to a specific point in memory with the syntax <command> <address>?

`s`

3. What command would you run to go 5 bytes forward?

`s+ 5`

4. What about 12 bytes backward?

`s- 12`

5. How do you undo the previous seek?

`s-`

6. How would go to the memory address of the main function?

`s main`

7. What if you wanted to go to the address of the rax register?

`sr rax`

8. Play around with the s command in the example1 and example2 binaries

`No anser needed`

## Task 6 Printing

`p` is a command that shows data in a myriad of formats. The command is useful for when you want to get information about what is happening in memory, and get some of the data that's contained in memory as well. With the p command it is also useful to know about the `@` symbol in radare. The `@` symbol is used to specify that something is an address in memory, for example if you wanted to specify you were talking about the memory address of the main function you would use `<command> @main`

```
[0x7fc4bbea5090]> p?
Usage: p[=68abcdDfiImrstuxz] [arg|len] [@addr]
| p[b|B|xb] [len] ([S])   bindump N bits skipping S bytes
| p[iI][df] [len]         print N ops/bytes (f=func) (see pi? and pdi)
| p[kK] [len]             print key in randomart (K is for mosaic)
| p-[?][jh] [mode]        bar|json|histogram blocks (mode: e?search.in)
| p2 [len]                8x8 2bpp-tiles
| p3 [file]               print stereogram (3D)
| p6[de] [len]            base64 decode/encode
| p8[?][j] [len]          8bit hexpair list of bytes
| p=[?][bep] [N] [L] [b]  show entropy/printable chars/chars bars
| pa[edD] [arg]           pa:assemble  pa[dD]:disasm or pae: esil from hex
| pA[n_ops]               show n_ops address and type
| pb[?] [n]               bitstream of N bits
| pB[?] [n]               bitstream of N bytes
| pc[?][p] [len]          output C (or python) format
| pC[aAcdDxw] [rows]      print disassembly in columns (see hex.cols and pdi)
| pd[?] [sz] [a] [b]      disassemble N opcodes (pd) or N bytes (pD)
| pf[?][.nam] [fmt]       print formatted data (pf.name, pf.name $<expr>)
| pF[?][apx]              print asn1, pkcs7 or x509
| pg[?][x y w h] [cmd]    create new visual gadget or print it (see pg? for details)
| ph[?][=|hash] ([len])   calculate hash for a block
| pj[?] [len]             print as indented JSON
| pm[?] [magic]           print libmagic data (see pm? and /m?)
| po[?] hex               print operation applied to block (see po?)
| pp[?][sz] [len]         print patterns, see pp? for more help
| pq[?][is] [len]         print QR code with the first Nbytes
| pr[?][glx] [len]        print N raw bytes (in lines or hexblocks, 'g'unzip)
| ps[?][pwz] [len]        print pascal/wide/zero-terminated strings
| pt[?][dn] [len]         print different timestamps
| pu[?][w] [len]          print N url encoded bytes (w=wide)
| pv[?][jh] [mode]        show variable/pointer/value in memory
| pwd                     display current working directory
| px[?][owq] [len]        hexdump of N bytes (o=octal, w=32bit, q=64bit)
| pz[?] [len]             print zoom view (see pz? for help)
```

1. How would you print the hex output of where you currently are in memory?

`px`

2. How would you print the disassembly of where you're currently at in memory?

`pd`

3. What if you wanted the disassembly of the main function?

`pd @ main`

4. What command prints out the emoji hexdump? (this is not useful at all I just find it funny)

```
[0xfffffffffffffff8]> px?
Usage: px[0afoswqWqQ][f]   # Print heXadecimal
| px                show hexdump
| px/               same as x/ in gdb (help x)
| px0               8bit hexpair list of bytes until zero byte
| pxa               show annotated hexdump
| pxA[?]            show op analysis color map
| pxb               dump bits in hexdump form
| pxc               show hexdump with comments
| pxd[?1248]        signed integer dump (1 byte, 2 and 4)
| pxe               emoji hexdump! :)
| pxf               show hexdump of current function
| pxh               show hexadecimal half-words dump (16bit)
| pxH               same as above, but one per line
| pxi               HexII compact binary representation
| pxl               display N lines (rows) of hexdump
| pxo               show octal dump
| pxq               show hexadecimal quad-words dump (64bit)
| pxQ[q]            same as above, but one per line
| pxr[j]            show words with references to flags and code (q=quiet)
| pxs               show hexadecimal in sparse mode
| pxt[*.] [origin]  show delta pointer table in r2 commands
| pxw               show hexadecimal words dump (32bit)
| pxW[q]            same as above, but one per line (q=quiet)
| pxx               show N bytes of hex-less hexdump
| pxX               show N words of hex-less hexdump
```

`pxe`

5. What if you decided you were too good for rows and you wanted the disassembly in column format?

`pC`

6. What is the value of the first variable in the main function for the example 3 binary?

`1`

7. What about the second variable?

`5`

## Task 7 The Mid-term

Congrats on getting to this point, you now know enough to pass the mid-term exam. The questions in this task will all be related to commands that were in previous tasks so if you skipped one, I recommend going back and doing it. As you probably guessed from the file name all exercises in this task will be done using the midterm binary file.

1. How many functions are in the binary?

```
0x7f25d4471090]> aaa
[x] Analyze all flags starting with sym. and entry0 (aa)
[x] Analyze function calls (aac)
[x] Analyze len bytes of instructions for references (aar)
[x] Check for objc references
[x] Check for vtables
[TOFIX: aaft can't run in debugger mode.ions (aaft)
[x] Type matching analysis for all functions (aaft)
[x] Propagate noreturn information
[x] Use -AA or aaaa to perform additional experimental analysis.
[0x7f25d4471090]> afl | wc -l
14
```

`13`

2. What is the value of the hidden string?

```
[0x7f25d4471090]> izz
[Strings]
nth paddr      vaddr          len size section   type    string
―――――――――――――――――――――――――――――――――――――――――――――――――――――――――――――――
0   0x00000034 0x55588848f034 4   10             utf16le @8\t@
1   0x00000238 0x55588848f238 27  28   .interp   ascii   /lib64/ld-linux-x86-64.so.2
2   0x00000361 0x55588848f361 9   10   .dynstr   ascii   libc.so.6
3   0x0000036b 0x55588848f36b 14  15   .dynstr   ascii   __cxa_finalize
4   0x0000037a 0x55588848f37a 17  18   .dynstr   ascii   __libc_start_main
5   0x0000038c 0x55588848f38c 27  28   .dynstr   ascii   _ITM_deregisterTMCloneTable
6   0x000003a8 0x55588848f3a8 14  15   .dynstr   ascii   __gmon_start__
7   0x000003b7 0x55588848f3b7 19  20   .dynstr   ascii   _Jv_RegisterClasses
8   0x000003cb 0x55588848f3cb 25  26   .dynstr   ascii   _ITM_registerTMCloneTable
9   0x000003e5 0x55588848f3e5 11  12   .dynstr   ascii   GLIBC_2.2.5
10  0x000005a9 0x55588848f5a9 4   5    .text     ascii   5z\n
11  0x000005f1 0x55588848f5f1 4   5    .text     ascii   =1\n
12  0x000006a3 0x55588848f6a3 4   5    .text     ascii   %`\a
13  0x000006ab 0x55588848f6ab 4   5    .text     ascii   -`\a
14  0x000006e1 0x55588848f6e1 11  12   .text     ascii   \b[]A\A]A^A_
15  0x00000760 0x55588848f760 4   5    .eh_frame ascii   \e\f\a\b
16  0x00000790 0x55588848f790 4   5    .eh_frame ascii   \e\f\a\b
17  0x000007b7 0x55588848f7b7 5   6    .eh_frame ascii   ;*3$"
18  0x000007f1 0x55588848f7f1 4   5    .eh_frame ascii   T\f\a\b
19  0x00000811 0x55588848f811 4   5    .eh_frame ascii   B\f\a\b
20  0x00000831 0x55588848f831 4   5    .eh_frame ascii   F\f\a\b
21  0x00001028 0x55588848f000 44  45   .comment  ascii   GCC: (Debian 6.3.0-18+deb9u1) 6.3.0 20170516
22  0x000016b9 0x55588848f001 10  11   .strtab   ascii   crtstuff.c
23  0x000016c4 0x55588848f00c 12  13   .strtab   ascii   __JCR_LIST__
24  0x000016d1 0x55588848f019 20  21   .strtab   ascii   deregister_tm_clones
25  0x000016e6 0x55588848f02e 21  22   .strtab   ascii   __do_global_dtors_aux
26  0x000016fc 0x55588848f044 14  15   .strtab   ascii   completed.6972
27  0x0000170b 0x55588848f053 38  39   .strtab   ascii   __do_global_dtors_aux_fini_array_entry
28  0x00001732 0x55588848f07a 11  12   .strtab   ascii   frame_dummy
29  0x0000173e 0x55588848f086 30  31   .strtab   ascii   __frame_dummy_init_array_entry
30  0x0000175d 0x55588848f0a5 9   10   .strtab   ascii   midterm.c
31  0x00001767 0x55588848f0af 13  14   .strtab   ascii   __FRAME_END__
32  0x00001775 0x55588848f0bd 11  12   .strtab   ascii   __JCR_END__
33  0x00001781 0x55588848f0c9 16  17   .strtab   ascii   __init_array_end
34  0x00001792 0x55588848f0da 8   9    .strtab   ascii   _DYNAMIC
35  0x0000179b 0x55588848f0e3 18  19   .strtab   ascii   __init_array_start
36  0x000017ae 0x55588848f0f6 18  19   .strtab   ascii   __GNU_EH_FRAME_HDR
37  0x000017c1 0x55588848f109 21  22   .strtab   ascii   _GLOBAL_OFFSET_TABLE_
38  0x000017d7 0x55588848f11f 15  16   .strtab   ascii   __libc_csu_fini
39  0x000017e7 0x55588848f12f 12  13   .strtab   ascii   midterm_func
40  0x000017f4 0x55588848f13c 27  28   .strtab   ascii   _ITM_deregisterTMCloneTable
41  0x00001810 0x55588848f158 6   7    .strtab   ascii   _edata
42  0x00001817 0x55588848f15f 30  31   .strtab   ascii   __libc_start_main@@GLIBC_2.2.5
43  0x00001836 0x55588848f17e 12  13   .strtab   ascii   __data_start
44  0x00001843 0x55588848f18b 14  15   .strtab   ascii   __gmon_start__
45  0x00001852 0x55588848f19a 12  13   .strtab   ascii   __dso_handle
46  0x0000185f 0x55588848f1a7 14  15   .strtab   ascii   _IO_stdin_used
47  0x0000186e 0x55588848f1b6 11  12   .strtab   ascii   secret_func
48  0x0000187a 0x55588848f1c2 15  16   .strtab   ascii   __libc_csu_init
49  0x0000188a 0x55588848f1d2 11  12   .strtab   ascii   __bss_start
50  0x00001896 0x55588848f1de 4   5    .strtab   ascii   main
51  0x0000189b 0x55588848f1e3 19  20   .strtab   ascii   _Jv_RegisterClasses
52  0x000018af 0x55588848f1f7 11  12   .strtab   ascii   __TMC_END__
53  0x000018bb 0x55588848f203 25  26   .strtab   ascii   _ITM_registerTMCloneTable
54  0x000018d5 0x55588848f21d 27  28   .strtab   ascii   __cxa_finalize@@GLIBC_2.2.5
55  0x000018f2 0x55588848f001 7   8    .shstrtab ascii   .symtab
56  0x000018fa 0x55588848f009 7   8    .shstrtab ascii   .strtab
57  0x00001902 0x55588848f011 9   10   .shstrtab ascii   .shstrtab
58  0x0000190c 0x55588848f01b 7   8    .shstrtab ascii   .interp
59  0x00001914 0x55588848f023 13  14   .shstrtab ascii   .note.ABI-tag
60  0x00001922 0x55588848f031 18  19   .shstrtab ascii   .note.gnu.build-id
61  0x00001935 0x55588848f044 9   10   .shstrtab ascii   .gnu.hash
62  0x0000193f 0x55588848f04e 7   8    .shstrtab ascii   .dynsym
63  0x00001947 0x55588848f056 7   8    .shstrtab ascii   .dynstr
64  0x0000194f 0x55588848f05e 12  13   .shstrtab ascii   .gnu.version
65  0x0000195c 0x55588848f06b 14  15   .shstrtab ascii   .gnu.version_r
66  0x0000196b 0x55588848f07a 9   10   .shstrtab ascii   .rela.dyn
67  0x00001975 0x55588848f084 5   6    .shstrtab ascii   .init
68  0x0000197b 0x55588848f08a 8   9    .shstrtab ascii   .plt.got
69  0x00001984 0x55588848f093 5   6    .shstrtab ascii   .text
70  0x0000198a 0x55588848f099 5   6    .shstrtab ascii   .fini
71  0x00001990 0x55588848f09f 7   8    .shstrtab ascii   .rodata
72  0x00001998 0x55588848f0a7 13  14   .shstrtab ascii   .eh_frame_hdr
73  0x000019a6 0x55588848f0b5 9   10   .shstrtab ascii   .eh_frame
74  0x000019b0 0x55588848f0bf 11  12   .shstrtab ascii   .init_array
75  0x000019bc 0x55588848f0cb 11  12   .shstrtab ascii   .fini_array
76  0x000019c8 0x55588848f0d7 4   5    .shstrtab ascii   .jcr
77  0x000019cd 0x55588848f0dc 8   9    .shstrtab ascii   .dynamic
78  0x000019d6 0x55588848f0e5 8   9    .shstrtab ascii   .got.plt
79  0x000019df 0x55588848f0ee 5   6    .shstrtab ascii   .data
80  0x000019e5 0x55588848f0f4 4   5    .shstrtab ascii   .bss
81  0x000019ea 0x55588848f0f9 8   9    .shstrtab ascii   .comment
82  0x00002178 0x55588848f887 14  14             ascii   \nyou_found_me\n
```

`you_found_me`

3. What is the return value of secret_func()?

```
[0x7f25d4471090]> pdf @ sym.secret_func
┌ 11: sym.secret_func ();
│           0x55588848f680      55             push rbp
│           0x55588848f681      4889e5         mov rbp, rsp
│           0x55588848f684      b804000000     mov eax, 4
│           0x55588848f689      5d             pop rbp
└           0x55588848f68a      c3             ret
```

`4`

4. What is the value of the first variable set in the main function(in decimal format)?

```
0x7f25d4471090]> pdf @ main
            ; DATA XREF from entry0 @ 0x55588848f54d
┌ 25: int main (int argc, char **argv, char **envp);
│           ; var int64_t var_8h @ rbp-0x8
│           ; var int64_t var_4h @ rbp-0x4
│           0x55588848f660      55             push rbp
│           0x55588848f661      4889e5         mov rbp, rsp
│           0x55588848f664      c745fc0c0000.  mov dword [var_4h], 0xc ; 12
│           0x55588848f66b      c745f8c00000.  mov dword [var_8h], 0xc0 ; 192
│           0x55588848f672      b800000000     mov eax, 0
│           0x55588848f677      5d             pop rbp
└           0x55588848f678      c3             ret
```

`12`

5. What about the second one(also in decimal format)?

`192`

6. What is the next function in memory after the main function?

```
[0x7f25d4471090]> s 0x55588848f678
[0x55588848f678]> s+ 1
[0x55588848f679]> pdf
┌ 7: sym.midterm_func ();
│ bp: 0 (vars 0, args 0)
│ sp: 0 (vars 0, args 0)
│ rg: 0 (vars 0, args 0)
│           0x55588848f679      55             push rbp
│           0x55588848f67a      4889e5         mov rbp, rsp
│           0x55588848f67d      90             nop
│           0x55588848f67e      5d             pop rbp
└           0x55588848f67f      c3             ret
```

```
[0x55588848f679]> afl | sort
0x55588848f4f8    3 23           sym._init
0x55588848f520    1 6            sym.imp.__cxa_finalize
0x55588848f530    1 42           entry0
0x55588848f560    4 50   -> 44   sym.deregister_tm_clones
0x55588848f5a0    4 66   -> 57   sym.register_tm_clones
0x55588848f5f0    5 50           entry.fini0
0x55588848f630    4 48   -> 42   entry.init0
0x55588848f660    1 25           main
0x55588848f679    1 7            sym.midterm_func
0x55588848f680    1 11           sym.secret_func
0x55588848f690    4 93           sym.__libc_csu_init
0x55588848f6f0    1 1            sym.__libc_csu_fini
0x55588848f6f4    1 9            sym._fini
0x55588868ffd8    1 4132         reloc.__libc_start_main
```

`midterm_func`

7. How do you get a hexdump of four bytes of the memory address your currently at?

`px 5`

## Task 8 Debugging

Recall that in the task "Command Line Options" you learned that the -d flag has radare enter debug mode. Debug mode allows you to set breakpoints and offers a lot of options to not only navigate through your binary, but to analyze the data that goes in and out of the registers as well.

```
[0x55588848f679]> d?
Usage: d   # Debug commands
| db[?]                    Breakpoints commands
| dbt[?]                   Display backtrace based on dbg.btdepth and dbg.btalgo
| dc[?]                    Continue execution
| dd[?]                    File descriptors (!fd in r1)
| de[-sc] [perm] [rm] [e]  Debug with ESIL (see de?)
| dg <file>                Generate a core-file (WIP)
| dH [handler]             Transplant process to a new handler
| di[?]                    Show debugger backend information (See dh)
| dk[?]                    List, send, get, set, signal handlers of child
| dL[?]                    List or set debugger handler
| dm[?]                    Show memory maps
| do[?]                    Open process (reload, alias for 'oo')
| doo[args]                Reopen in debug mode with args (alias for 'ood')
| doof[file]               Reopen in debug mode from file (alias for 'oodf')
| doc                      Close debug session
| dp[?]                    List, attach to process or thread id
| dr[?]                    Cpu registers
| ds[?]                    Step, over, source line
| dt[?]                    Display instruction traces
| dw <pid>                 Block prompt until pid dies
| dx[?]                    Inject and run code on target process (See gs)
```

```
[0x55588848f679]> db?
Usage: db    # Breakpoints commands
| db                        List breakpoints
| db*                       List breakpoints in r commands
| db sym.main               Add breakpoint into sym.main
| db <addr>                 Add breakpoint
| dbH <addr>                Add hardware breakpoint
| db- <addr>                Remove breakpoint
| db-*                      Remove all the breakpoints
| db.                       Show breakpoint info in current offset
| dbj                       List breakpoints in JSON format
| dbc <addr> <cmd>          Run command when breakpoint is hit
| dbC <addr> <cmd>          Run command but continue until <cmd> returns zero
| dbd <addr>                Disable breakpoint
| dbe <addr>                Enable breakpoint
| dbs <addr>                Toggle breakpoint
| dbf                       Put a breakpoint into every no-return function
| dbm <module> <offset>     Add a breakpoint at an offset from a module's base
| dbn [<name>]              Show or set name for current breakpoint
| dbi                       List breakpoint indexes
| dbi <addr>                Show breakpoint index in givengiven  offset
| dbi.                      Show breakpoint index in current offset
| dbi- <idx>                Remove breakpoint by index
| dbix <idx> [expr]         Set expression for bp at given index
| dbic <idx> <cmd>          Run command at breakpoint index
| dbie <idx>                Enable breakpoint by index
| dbid <idx>                Disable breakpoint by index
| dbis <idx>                Swap Nth breakpoint
| dbite <idx>               Enable breakpoint Trace by index
| dbitd <idx>               Disable breakpoint Trace by index
| dbits <idx>               Swap Nth breakpoint trace
| dbh x86                   Set/list breakpoint plugin handlers
| dbh- <name>               Remove breakpoint plugin handler
| dbt[?]                    Show backtrace. See dbt? for more details
| dbx [expr]                Set expression for bp in current offset
| dbw <addr> <r/w/rw>       Add watchpoint
| drx number addr len perm  Modify hardware breakpoint
| drx-number                Clear hardware breakpoint
```

1. How do you set a breakpoint?

`db`

3. What command is used to print out the values of all the registers?

`dr`

4. How do you run through the program until the program either ends or you hit the next breakpoint?

`dc`

5. What if you want to step through the binary one line at a time?

`ds`

6. How do you go forth 2 lines in the binary?

`ds 2`

7. How do you list out the indexes and memory addresses of all breakpoints?

`dbi`

8. Go back through all previous binaries and mess around with debug mode.

`No answer needed`

## Task 9 Visual Mode

While visual mode is by no means necessary and won't inherently teach you anything new about the binary you're currently running. It allows the assembly to more human readable and provides a lot of options to enhance the visual appeal of radare and can definitely improve efficiency. Therefore I would state it's a valuable tool that you should know how to use. All commands involving visual mode start with `v`

```
Visual mode help:
 ?        show this help
 ??       show the user-friendly hud
 %        in cursor mode finds matching pair, or toggle autoblocksz
 @        redraw screen every 1s (multi-user view)
 ^        seek to the begining of the function
 !        enter into the visual panels mode
 _        enter the flag/comment/functions/.. hud (same as VF_)
 =        set cmd.vprompt (top row)
 |        set cmd.cprompt (right column)
 .        seek to program counter
 \        toggle visual split mode
 "        toggle the column mode (uses pC..)
 /        in cursor mode search in current block
 :cmd     run radare command
 ;[-]cmt  add/remove comment
 0        seek to beginning of current function
 [1-9]    follow jmp/call identified by shortcut (like ;[1])
 ,file    add a link to the text file
 /*+-[]   change block size, [] = resize hex.cols
 </>      seek aligned to block size (seek cursor in cursor mode)
 a/A      (a)ssemble code, visual (A)ssembler
 b        browse symbols, flags, configurations, classes, ...
 B        toggle breakpoint
 c/C      toggle (c)ursor and (C)olors
 d[f?]    define function, data, code, ..
 D        enter visual diff mode (set diff.from/to
 e        edit eval configuration variables
 f/F      set/unset or browse flags. f- to unset, F to browse, ..
 gG       go seek to begin and end of file (0-$s)
 hjkl     move around (or HJKL) (left-down-up-right)
 i        insert hex or string (in hexdump) use tab to toggle
 mK/'K    mark/go to Key (any key)
 M        walk the mounted filesystems
 n/N      seek next/prev function/flag/hit (scr.nkey)
 g        go/seek to given offset
 O        toggle asm.pseudo and asm.esil
 p/P      rotate print modes (hex, disasm, debug, words, buf)
 q        back to radare shell
 r        refresh screen / in cursor mode browse comments
 R        randomize color palette (ecr)
 sS       step / step over
 t        browse types
 T        enter textlog chat console (TT)
 uU       undo/redo seek
 v        visual function/vars code analysis menu
 V        (V)iew graph using cmd.graph (agv?)
 wW       seek cursor to next/prev word
 xX       show xrefs/refs of current function from/to data/code
 yY       copy and paste selection
 z        fold/unfold comments in disassembly
 Z        toggle zoom mode
 Enter    follow address of jump/call
Function Keys: (See 'e key.'), defaults to:
  F2      toggle breakpoint
  F4      run to cursor
  F7      single step
  F8      step over
  F9      continue
```

1. How do you enter "graph mode" which allows everything to be organized in nice readable boxes?(A personal favorite of mine. Also note that the second character is uppercase)

`vV`

2. What character do you press to run normal radare commands inside visual mode?

`:`

3. How do you go back to the regular radare shell(leaving visual mode)?

`q`

4. What if you want to step through the binary inside Visual mode?

`s`

5. How do you add a comment?

`;`

6. Look through any of the binaries in Visual Mode and see just how much more beautiful everything looks.

`No answer needed`

## Task 10 Write Mode

Occasionally you might end up in a situation where a task is impossible to solve with the current instructions. For example take this code

```cpp
int val = 4;
if(val == 5) {
    printf("%s","You win!");
}
```

You will never be able to get it to print out You win! because under normal circumstances val will never be set equal to 5. This is where write mode comes in, it allows you to change instructions so you can get certain conditions to execute. All commands involving write mode start with `w`

```
0x55588848f6bb]> w?
Usage: w[x] [str] [<file] [<<EOF] [@addr]
| w[1248][+-][n]       increment/decrement byte,word..
| w foobar             write string 'foobar'
| w0 [len]             write 'len' bytes with value 0x00
| w6[de] base64/hex    write base64 [d]ecoded or [e]ncoded string
| wa[?] push ebp       write opcode, separated by ';' (use '"' around the command)
| waf f.asm            assemble file and write bytes
| waF f.asm            assemble file and write bytes and show 'wx' op with hexpair bytes of assembled code
| wao[?] op            modify opcode (change conditional of jump. nop, etc)
| wA[?] r 0            alter/modify opcode at current seek (see wA?)
| wb 010203            fill current block with cyclic hexpairs
| wB[-]0xVALUE         set or unset bits with given value
| wc                   list all write changes
| wc[?][jir+-*?]       write cache undo/commit/reset/list (io.cache)
| wd [off] [n]         duplicate N bytes from offset at current seek (memcpy) (see y?)
| we[?] [nNsxX] [arg]  extend write operations (insert instead of replace)
| wf[fs] -|file        write contents of file at current offset
| wh r2                whereis/which shell command
| wm f0ff              set binary mask hexpair to be used as cyclic write mask
| wo[?] hex            write in block with operation. 'wo?' fmi
| wp[?] -|file         apply radare patch file. See wp? fmi
| wr 10                write 10 random bytes
| ws pstring           write 1 byte for length and then the string
| wt[f][?] file [sz]   write to file (from current seek, blocksize or sz bytes)
| wts host:port [sz]   send data to remote host:port via tcp://
| ww foobar            write wide string 'f\x00o\x00o\x00b\x00a\x00r\x00'
| wx[?][fs] 9090       write two intel nops (from wxfile or wxseek)
| wv[?] eip+34         write 32-64 bit value honoring cfg.bigendian
| wz string            write zero terminated string (like w + \x00)
```

1. How do you write a string to the current memory address.

`w`

2. What command lists all write changes?

`wc`

3. What command modifies an instruction at the current memory address?

`wa`

4. Get the example4 binary to show the You win! message

`No answer needed`

## Task 11 The Final Exam

Congratulations on making it to this point. You should now be able to solve a crackme! Use all the tools you've learned and get that password! The binary to use for this task is the_final_exam!

1. What is the password that outputs the you win! message?

```
kali@kali:~/CTFs/tryhackme/CC Radare2/z$ ./the_final_exam
oekZ_Z_j
You win!
```

`oekZ_Z_j`
