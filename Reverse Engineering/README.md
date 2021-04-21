# Reverse Engineering

This room focuses on teaching the basics of assembly through reverse engineering

## Topic's

- Reverse Engineering

## Appendix archive

Password: `1 kn0w 1 5h0uldn'7!`

## Task 1 Debugging and File Permission

In this task, we'll be learning the basics of reverse engineering and assembly. Here are some important things to do before starting the task:

- These files have been compiled with the lowest level of optimisation on Unix based machines and are intended to be run on Linux/Mac.
- Make sure you set up a debugger - it would be good to get comfortable with radare2 which can be downloaded from [here](https://github.com/radare/radare2). You can also use other debuggers like gdb, which come installed in most Unix based operating systems.
- When these files have been downloaded, change the permissions of these files using the command `chmod +x filename`

These tasks will make use of crackme files. The objective of these files is to understand the assembly code to uncover the right password for the file.

Here are some of the important things you will learn in this course:

- If statements in assembly
- Loops in assembly
- standard function calls in assembly
- Calling Convention in assembly

1. Set up debugger(if you haven't already)

`No answer needed`

## Task 2 crackme1

This first crackme file will give you an introduction to if statements and basic function calling in assembly.

```
kali@kali:~/CTFs/tryhackme/Reverse Engineering$ file crackme1.bin
crackme1.bin: ELF 64-bit LSB shared object, x86-64, version 1 (SYSV), dynamically linked, interpreter /lib64/ld-linux-x86-64.so.2, for GNU/Linux 3.2.0, BuildID[sha1]=3864320789154e8960133afdf58ddf65f6f8273d, not stripped
kali@kali:~/CTFs/tryhackme/Reverse Engineering$ strings crackme1.bin

enter password
password is correct
password is incorrect
hax0r

```

1. what is the correct password

```
kali@kali:~/CTFs/tryhackme/Reverse Engineering$ ./crackme1.bin
enter password
hax0r
password is correct
```

`hax0r`

## Task 3 crackme2

This is the second crackme file - Unlike the first file, this will involve examining registers, how and where values are compared

```
kali@kali:~/CTFs/tryhackme/Reverse Engineering$ r2 -d ./crackme2.bin
Process with PID 78763 started...
= attach 78763 78763
bin.baddr 0x55fb45bf4000
Using 0x55fb45bf4000
asm.bits 64
Warning: r_bin_file_hash: file exceeds bin.hashlimit
[0x7f58df4df090]> aaa
[x] Analyze all flags starting with sym. and entry0 (aa)
[x] Analyze function calls (aac)
[x] Analyze len bytes of instructions for references (aar)
[x] Check for objc references
[x] Check for vtables
[TOFIX: aaft can't run in debugger mode.ions (aaft)
[x] Type matching analysis for all functions (aaft)
[x] Propagate noreturn information
[x] Use -AA or aaaa to perform additional experimental analysis.
[0x7f58df4df090]> afl
0x55fb45bf4610    1 42           entry0
0x55fb45df4fe0    1 4124         reloc.__libc_start_main
0x55fb45bf4640    4 50   -> 40   sym.deregister_tm_clones
0x55fb45bf4680    4 66   -> 57   sym.register_tm_clones
0x55fb45bf46d0    5 58   -> 51   entry.fini0
0x55fb45bf4600    1 6            sym..plt.got
0x55fb45bf4710    1 10           entry.init0
0x55fb45bf4810    1 2            sym.__libc_csu_fini
0x55fb45bf4814    1 9            sym._fini
0x55fb45bf47a0    4 101          sym.__libc_csu_init
0x55fb45bf471a    6 122          main
0x55fb45bf45a0    3 23           sym._init
0x55fb45bf45d0    1 6            sym.imp.puts
0x55fb45bf45e0    1 6            sym.imp.__stack_chk_fail
0x55fb45bf4000    2 25           map.home_kali_CTFs_tryhackme_Reverse_Engineering_crackme2.bin.r_x
0x55fb45bf45f0    1 6            sym.imp.__isoc99_scanf
[0x7f58df4df090]> pdf @main
            ; DATA XREF from entry0 @ 0x55fb45bf462d
┌ 122: int main (int argc, char **argv, char **envp);
│           ; var int64_t var_ch @ rbp-0xc
│           ; var int64_t var_8h @ rbp-0x8
│           0x55fb45bf471a      55             push rbp
│           0x55fb45bf471b      4889e5         mov rbp, rsp
│           0x55fb45bf471e      4883ec10       sub rsp, 0x10
│           0x55fb45bf4722      64488b042528.  mov rax, qword fs:[0x28]
│           0x55fb45bf472b      488945f8       mov qword [var_8h], rax
│           0x55fb45bf472f      31c0           xor eax, eax
│           0x55fb45bf4731      488d3dec0000.  lea rdi, qword str.enter_your_password ; 0x55fb45bf4824 ; "enter your password"
│           0x55fb45bf4738      e893feffff     call sym.imp.puts       ; int puts(const char *s)
│           0x55fb45bf473d      488d45f4       lea rax, qword [var_ch]
│           0x55fb45bf4741      4889c6         mov rsi, rax
│           0x55fb45bf4744      488d3ded0000.  lea rdi, qword [0x55fb45bf4838] ; "%d"
│           0x55fb45bf474b      b800000000     mov eax, 0
│           0x55fb45bf4750      e89bfeffff     call sym.imp.__isoc99_scanf ; int scanf(const char *format)
│           0x55fb45bf4755      8b45f4         mov eax, dword [var_ch]
│           0x55fb45bf4758      3d7c130000     cmp eax, 0x137c
│       ┌─< 0x55fb45bf475d      750e           jne 0x55fb45bf476d
│       │   0x55fb45bf475f      488d3dd50000.  lea rdi, qword str.password_is_valid ; 0x55fb45bf483b ; "password is valid"
│       │   0x55fb45bf4766      e865feffff     call sym.imp.puts       ; int puts(const char *s)
│      ┌──< 0x55fb45bf476b      eb0c           jmp 0x55fb45bf4779
│      │└─> 0x55fb45bf476d      488d3dd90000.  lea rdi, qword str.password_is_incorrect ; 0x55fb45bf484d ; "password is incorrect"
│      │    0x55fb45bf4774      e857feffff     call sym.imp.puts       ; int puts(const char *s)
│      │    ; CODE XREF from main @ 0x55fb45bf476b
│      └──> 0x55fb45bf4779      b800000000     mov eax, 0
│           0x55fb45bf477e      488b55f8       mov rdx, qword [var_8h]
│           0x55fb45bf4782      644833142528.  xor rdx, qword fs:[0x28]
│       ┌─< 0x55fb45bf478b      7405           je 0x55fb45bf4792
│       │   0x55fb45bf478d      e84efeffff     call sym.imp.__stack_chk_fail ; void __stack_chk_fail(void)
│       └─> 0x55fb45bf4792      c9             leave
└           0x55fb45bf4793      c3             ret
[0x7f58df4df090]>
```

```
kali@kali:~/CTFs/tryhackme/Reverse Engineering$ echo 'ibase=16; 137C' | bc
4988
```

1. What is the correct password?

```
kali@kali:~/CTFs/tryhackme/Reverse Engineering$ ./crackme2.bin
enter your password
4988
password is valid
```

`4988`

## Task 4 crackme3

This crackme will be significantly more challenging - it involves learning how loops work, and how they are represented in assembly

```
kali@kali:~/CTFs/tryhackme/Reverse Engineering$ r2 -d ./crackme3.bin
Process with PID 78868 started...
= attach 78868 78868
bin.baddr 0x5613e9b22000
Using 0x5613e9b22000
asm.bits 64
Warning: r_bin_file_hash: file exceeds bin.hashlimit
[0x7f6163ed6090]> aaa
[x] Analyze all flags starting with sym. and entry0 (aa)
[x] Analyze function calls (aac)
[x] Analyze len bytes of instructions for references (aar)
[x] Check for objc references
[x] Check for vtables
[TOFIX: aaft can't run in debugger mode.ions (aaft)
[x] Type matching analysis for all functions (aaft)
[x] Propagate noreturn information
[x] Use -AA or aaaa to perform additional experimental analysis.
[0x7f6163ed6090]> afl
0x5613e9b22610    1 42           entry0
0x5613e9d22fe0    1 4124         reloc.__libc_start_main
0x5613e9b22640    4 50   -> 40   sym.deregister_tm_clones
0x5613e9b22680    4 66   -> 57   sym.register_tm_clones
0x5613e9b226d0    5 58   -> 51   entry.fini0
0x5613e9b22600    1 6            sym..plt.got
0x5613e9b22710    1 10           entry.init0
0x5613e9b22840    1 2            sym.__libc_csu_fini
0x5613e9b22844    1 9            sym._fini
0x5613e9b227d0    4 101          sym.__libc_csu_init
0x5613e9b2271a    9 170          main
0x5613e9b225a0    3 23           sym._init
0x5613e9b225d0    1 6            sym.imp.puts
0x5613e9b225e0    1 6            sym.imp.__stack_chk_fail
0x5613e9b22000    2 25           map.home_kali_CTFs_tryhackme_Reverse_Engineering_crackme3.bin.r_x
0x5613e9b225f0    1 6            sym.imp.__isoc99_scanf
[0x7f6163ed6090]> pdf @main
            ; DATA XREF from entry0 @ 0x5613e9b2262d
┌ 170: int main (int argc, char **argv, char **envp);
│           ; var int64_t var_28h @ rbp-0x28
│           ; var int64_t var_23h @ rbp-0x23
│           ; var int64_t var_21h @ rbp-0x21
│           ; var int64_t var_20h @ rbp-0x20
│           ; var int64_t var_8h @ rbp-0x8
│           0x5613e9b2271a      55             push rbp
│           0x5613e9b2271b      4889e5         mov rbp, rsp
│           0x5613e9b2271e      4883ec30       sub rsp, 0x30
│           0x5613e9b22722      64488b042528.  mov rax, qword fs:[0x28]
│           0x5613e9b2272b      488945f8       mov qword [var_8h], rax
│           0x5613e9b2272f      31c0           xor eax, eax
│           0x5613e9b22731      66c745dd617a   mov word [var_23h], 0x7a61 ; 'az'
│           0x5613e9b22737      c645df74       mov byte [var_21h], 0x74 ; 't' ; 116
│           0x5613e9b2273b      488d3d120100.  lea rdi, qword str.enter_your_password ; 0x5613e9b22854 ; "enter your password"
│           0x5613e9b22742      e889feffff     call sym.imp.puts       ; int puts(const char *s)
│           0x5613e9b22747      488d45e0       lea rax, qword [var_20h]
│           0x5613e9b2274b      4889c6         mov rsi, rax
│           0x5613e9b2274e      488d3d130100.  lea rdi, qword [0x5613e9b22868] ; "%s"
│           0x5613e9b22755      b800000000     mov eax, 0
│           0x5613e9b2275a      e891feffff     call sym.imp.__isoc99_scanf ; int scanf(const char *format)
│           0x5613e9b2275f      c745d8000000.  mov dword [var_28h], 0
│       ┌─< 0x5613e9b22766      eb2f           jmp 0x5613e9b22797
│      ┌──> 0x5613e9b22768      8b45d8         mov eax, dword [var_28h]
│      ╎│   0x5613e9b2276b      4898           cdqe
│      ╎│   0x5613e9b2276d      0fb65405e0     movzx edx, byte [rbp + rax - 0x20]
│      ╎│   0x5613e9b22772      8b45d8         mov eax, dword [var_28h]
│      ╎│   0x5613e9b22775      4898           cdqe
│      ╎│   0x5613e9b22777      0fb64405dd     movzx eax, byte [rbp + rax - 0x23]
│      ╎│   0x5613e9b2277c      38c2           cmp dl, al
│     ┌───< 0x5613e9b2277e      7413           je 0x5613e9b22793
│     │╎│   0x5613e9b22780      488d3de40000.  lea rdi, qword str.password_is_incorrect ; 0x5613e9b2286b ; "password is incorrect"
│     │╎│   0x5613e9b22787      e844feffff     call sym.imp.puts       ; int puts(const char *s)
│     │╎│   0x5613e9b2278c      b800000000     mov eax, 0
│    ┌────< 0x5613e9b22791      eb1b           jmp 0x5613e9b227ae
│    │└───> 0x5613e9b22793      8345d801       add dword [var_28h], 1
│    │ ╎│   ; CODE XREF from main @ 0x5613e9b22766
│    │ ╎└─> 0x5613e9b22797      837dd802       cmp dword [var_28h], 2
│    │ └──< 0x5613e9b2279b      7ecb           jle 0x5613e9b22768
│    │      0x5613e9b2279d      488d3ddd0000.  lea rdi, qword str.password_is_correct ; 0x5613e9b22881 ; "password is correct"
│    │      0x5613e9b227a4      e827feffff     call sym.imp.puts       ; int puts(const char *s)
│    │      0x5613e9b227a9      b800000000     mov eax, 0
│    │      ; CODE XREF from main @ 0x5613e9b22791
│    └────> 0x5613e9b227ae      488b4df8       mov rcx, qword [var_8h]
│           0x5613e9b227b2      6448330c2528.  xor rcx, qword fs:[0x28]
│       ┌─< 0x5613e9b227bb      7405           je 0x5613e9b227c2
│       │   0x5613e9b227bd      e81efeffff     call sym.imp.__stack_chk_fail ; void __stack_chk_fail(void)
│       └─> 0x5613e9b227c2      c9             leave
└           0x5613e9b227c3      c3             ret
[0x7f6163ed6090]>
[0x5613e9b2275f]> ds
[0x5613e9b22766]> pdf @main
            ; DATA XREF from entry0 @ 0x5613e9b2262d
┌ 170: int main (int argc, char **argv, char **envp);
│           ; var int64_t var_28h @ rbp-0x28
│           ; var int64_t var_23h @ rbp-0x23
│           ; var int64_t var_21h @ rbp-0x21
│           ; var int64_t var_20h @ rbp-0x20
│           ; var int64_t var_8h @ rbp-0x8
│           0x5613e9b2271a      55             push rbp
│           0x5613e9b2271b      4889e5         mov rbp, rsp
│           0x5613e9b2271e      4883ec30       sub rsp, 0x30
│           0x5613e9b22722      64488b042528.  mov rax, qword fs:[0x28]
│           0x5613e9b2272b      488945f8       mov qword [var_8h], rax
│           0x5613e9b2272f      31c0           xor eax, eax
│           0x5613e9b22731      66c745dd617a   mov word [var_23h], 0x7a61 ; 'az'
│           0x5613e9b22737      c645df74       mov byte [var_21h], 0x74 ; 't' ; 116
│           0x5613e9b2273b      488d3d120100.  lea rdi, qword str.enter_your_password ; 0x5613e9b22854 ; "enter your password"
│           0x5613e9b22742      e889feffff     call sym.imp.puts       ; int puts(const char *s)
│           0x5613e9b22747      488d45e0       lea rax, qword [var_20h]
│           0x5613e9b2274b      4889c6         mov rsi, rax
│           0x5613e9b2274e      488d3d130100.  lea rdi, qword [0x5613e9b22868] ; "%s"
│           0x5613e9b22755      b800000000     mov eax, 0
│           0x5613e9b2275a      e891feffff     call sym.imp.__isoc99_scanf ; int scanf(const char *format)
│           0x5613e9b2275f b    c745d8000000.  mov dword [var_28h], 0
│           ;-- rip:
│       ┌─< 0x5613e9b22766      eb2f           jmp 0x5613e9b22797
│      ┌──> 0x5613e9b22768      8b45d8         mov eax, dword [var_28h]
│      ╎│   0x5613e9b2276b      4898           cdqe
│      ╎│   0x5613e9b2276d      0fb65405e0     movzx edx, byte [rbp + rax - 0x20]
│      ╎│   0x5613e9b22772      8b45d8         mov eax, dword [var_28h]
│      ╎│   0x5613e9b22775      4898           cdqe
│      ╎│   0x5613e9b22777      0fb64405dd     movzx eax, byte [rbp + rax - 0x23]
│      ╎│   0x5613e9b2277c      38c2           cmp dl, al
│     ┌───< 0x5613e9b2277e      7413           je 0x5613e9b22793
│     │╎│   0x5613e9b22780      488d3de40000.  lea rdi, qword str.password_is_incorrect ; 0x5613e9b2286b ; "password is incorrect"
│     │╎│   0x5613e9b22787      e844feffff     call sym.imp.puts       ; int puts(const char *s)
│     │╎│   0x5613e9b2278c      b800000000     mov eax, 0
│    ┌────< 0x5613e9b22791      eb1b           jmp 0x5613e9b227ae
│    │└───> 0x5613e9b22793      8345d801       add dword [var_28h], 1
│    │ ╎│   ; CODE XREF from main @ 0x5613e9b22766
│    │ ╎└─> 0x5613e9b22797      837dd802       cmp dword [var_28h], 2
│    │ └──< 0x5613e9b2279b b    7ecb           jle 0x5613e9b22768
│    │      0x5613e9b2279d      488d3ddd0000.  lea rdi, qword str.password_is_correct ; 0x5613e9b22881 ; "password is correct"
│    │      0x5613e9b227a4      e827feffff     call sym.imp.puts       ; int puts(const char *s)
│    │      0x5613e9b227a9      b800000000     mov eax, 0
│    │      ; CODE XREF from main @ 0x5613e9b22791
│    └────> 0x5613e9b227ae      488b4df8       mov rcx, qword [var_8h]
│           0x5613e9b227b2      6448330c2528.  xor rcx, qword fs:[0x28]
│       ┌─< 0x5613e9b227bb      7405           je 0x5613e9b227c2
│       │   0x5613e9b227bd      e81efeffff     call sym.imp.__stack_chk_fail ; void __stack_chk_fail(void)
│       └─> 0x5613e9b227c2      c9             leave
└           0x5613e9b227c3      c3             ret
[0x5613e9b22766]> px @ rbp-0x28
- offset -       0 1  2 3  4 5  6 7  8 9  A B  C D  E F  0123456789ABCDEF
0x7ffc957a00a8  0000 0000 0061 7a74 3132 3300 1356 0000  .....azt123..V..
0x7ffc957a00b8  1026 b2e9 1356 0000 c001 7a95 fc7f 0000  .&...V....z.....
0x7ffc957a00c8  00ab 71c8 1002 107d d027 b2e9 1356 0000  ..q....}.'...V..
0x7ffc957a00d8  cacc d163 617f 0000 c801 7a95 fc7f 0000  ...ca.....z.....
0x7ffc957a00e8  0000 0000 0100 0000 1a27 b2e9 1356 0000  .........'...V..
0x7ffc957a00f8  d9c7 d163 617f 0000 0000 0000 0000 0000  ...ca...........
0x7ffc957a0108  4133 bc38 9e6d 2fdc 1026 b2e9 1356 0000  A3.8.m/..&...V..
0x7ffc957a0118  0000 0000 0000 0000 0000 0000 0000 0000  ................
0x7ffc957a0128  0000 0000 0000 0000 4133 dc76 0e94 f18f  ........A3.v....
0x7ffc957a0138  4133 1aee 5979 ca8e 0000 0000 0000 0000  A3..Yy..........
0x7ffc957a0148  0000 0000 0000 0000 0000 0000 0000 0000  ................
0x7ffc957a0158  0100 0000 0000 0000 c801 7a95 fc7f 0000  ..........z.....
0x7ffc957a0168  d801 7a95 fc7f 0000 8011 f063 617f 0000  ..z........ca...
0x7ffc957a0178  0000 0000 0000 0000 0000 0000 0000 0000  ................
0x7ffc957a0188  1026 b2e9 1356 0000 c001 7a95 fc7f 0000  .&...V....z.....
0x7ffc957a0198  0000 0000 0000 0000 0000 0000 0000 0000  ................
```

1. What are the first 3 letters of the correct password?

```
kali@kali:~/CTFs/tryhackme/Reverse Engineering$ ./crackme3.bin
enter your password
azt
password is correct
```

`azt`
