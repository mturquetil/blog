+++
title = "Leviathan1"
authorTwitter = "" #do not include @
date = "2020-01-15"
cover = ""
tags = ["linux", "binary"]
description = "Dynamic analysis of a crack-me"
showFullContent = true
isTitle = true
+++

# Access level
```bash
ssh leviathan1@leviathan.labs.overthewire.org -p 2223
# Password: rioGegei8m
```

# Level goal
Pass the password check of the challenge binary.

# Explanation
First of all we can try to simply execute the binary.

```bash
leviathan1@leviathan:~$ ./check
password: aaaaaaaaaaaaaaaaaaa
Wrong password, Good Bye ...
```

Starting from this there's two possibility to investigate on the binary, either dynamically or statically.

To keep it simple I'll explain the challenge with the dynamic one and give a quick peak at the static analysis.

</br>

# Dynamic analysis

A good reflex approaching an unknown binary is to use the tools <a href="https://man7.org/linux/man-pages/man1/ltrace.1.html">ltrace</a> and <a href="https://man7.org/linux/man-pages/man1/strace.1.html">strace</a> to get a primer view of the program.

In order to use <a href="https://man7.org/linux/man-pages/man1/ltrace.1.html">ltrace</a> we have to make sure the binary is dynamically linked. (<a href="https://medium.com/@StueyGK/static-libraries-vs-dynamic-libraries-af78f0b5f1e4">Static Libraries vs. Dynamic Libraries</a>)

The **file** command gives us informations about the binary:

```bash
leviathan1@leviathan:~$ file ./check
./check: setuid ELF 32-bit LSB executable, Intel 80386, version 1 (SYSV), dynamically linked, interpreter /lib/ld-linux.so.2, for GNU/Linux 2.6.32, BuildID[sha1]=c735f6f3a3a94adcad8407cc0fda40496fd765dd, not stripped
```

After confirming the binary is dynamically linked we can now try to use <a href="https://man7.org/linux/man-pages/man1/ltrace.1.html">ltrace</a>:
```bash
leviathan1@leviathan:~$ ltrace ./check
__libc_start_main(0x804853b, 1, 0xffffd744, 0x8048610 <unfinished ...>
printf("password: ")                                                                                                             = 10
getchar(1, 0, 0x65766f6c, 0x646f6700password: aaaaaaa)                                                                           = 97
getchar(1, 0, 0x65766f6c, 0x646f6700)                                                                                            = 97
getchar(1, 0, 0x65766f6c, 0x646f6700)                                                                                            = 97
strcmp("aaa", "sex")                                                                                                             = -1
puts("Wrong password, Good Bye ..."Wrong password, Good Bye ...)                                                                 = 29
+++ exited (status 0) +++
```

Decrypting the output we see an interesting call to the <a href="https://man7.org/linux/man-pages/man3/strcmp.3.html">strcmp</a> C function.
This function basically compare two strings and return 0 if both are equal, else it return -1. The "aaa" string is the one I submitted and the other one belongs to the binary.

Knowing this we can now pass the <a href="https://man7.org/linux/man-pages/man3/strcmp.3.html">strcmp</a> check by submitting the **"sex"** string

```bash
leviathan1@leviathan:~$ ./check
password: sex
$ whoami
leviathan2
```

Here it is! We got a shell and are able to grab the password located in **/etc/leviathan_pass/leviathan2**.


</br>

# Static analysis: the dumb way
As promised, we will now see how to pass the password check using the static analysis.

To perform this analysis we first need to disassemble the binary and try to understand how it works. As this challenge dissasembly won't be too long, we can use <a href="https://linux.die.net/man/1/objdump">objdump</a>.

```bash
leviathan1@leviathan:~$ objdump -Mintel -d ./check | grep -A 50 "<main>"
0804853b <main>:
 804853b:       8d 4c 24 04             lea    ecx,[esp+0x4]
 804853f:       83 e4 f0                and    esp,0xfffffff0
 8048542:       ff 71 fc                push   DWORD PTR [ecx-0x4]
 8048545:       55                      push   ebp
 8048546:       89 e5                   mov    ebp,esp
 8048548:       53                      push   ebx
 8048549:       51                      push   ecx
 804854a:       83 ec 20                sub    esp,0x20
 804854d:       c7 45 f0 73 65 78 00    mov    DWORD PTR [ebp-0x10],0x786573
 8048554:       c7 45 e9 73 65 63 72    mov    DWORD PTR [ebp-0x17],0x72636573
 804855b:       66 c7 45 ed 65 74       mov    WORD PTR [ebp-0x13],0x7465
 8048561:       c6 45 ef 00             mov    BYTE PTR [ebp-0x11],0x0
 8048565:       c7 45 e5 67 6f 64 00    mov    DWORD PTR [ebp-0x1b],0x646f67
 804856c:       c7 45 e0 6c 6f 76 65    mov    DWORD PTR [ebp-0x20],0x65766f6c
 8048573:       c6 45 e4 00             mov    BYTE PTR [ebp-0x1c],0x0
 8048577:       83 ec 0c                sub    esp,0xc
 804857a:       68 90 86 04 08          push   0x8048690
 804857f:       e8 3c fe ff ff          call   80483c0 <printf@plt>
 8048584:       83 c4 10                add    esp,0x10
 8048587:       e8 44 fe ff ff          call   80483d0 <getchar@plt>
 804858c:       88 45 f4                mov    BYTE PTR [ebp-0xc],al
 804858f:       e8 3c fe ff ff          call   80483d0 <getchar@plt>
 8048594:       88 45 f5                mov    BYTE PTR [ebp-0xb],al
 8048597:       e8 34 fe ff ff          call   80483d0 <getchar@plt>
 804859c:       88 45 f6                mov    BYTE PTR [ebp-0xa],al
 804859f:       c6 45 f7 00             mov    BYTE PTR [ebp-0x9],0x0
 80485a3:       83 ec 08                sub    esp,0x8
 80485a6:       8d 45 f0                lea    eax,[ebp-0x10]
 80485a9:       50                      push   eax
 80485aa:       8d 45 f4                lea    eax,[ebp-0xc]
 80485ad:       50                      push   eax
 80485ae:       e8 fd fd ff ff          call   80483b0 <strcmp@plt>
 80485b3:       83 c4 10                add    esp,0x10
 80485b6:       85 c0                   test   eax,eax
 80485b8:       75 2b                   jne    80485e5 <main+0xaa>
 80485ba:       e8 21 fe ff ff          call   80483e0 <geteuid@plt>
 80485bf:       89 c3                   mov    ebx,eax
 80485c1:       e8 1a fe ff ff          call   80483e0 <geteuid@plt>
 80485c6:       83 ec 08                sub    esp,0x8
 80485c9:       53                      push   ebx
 80485ca:       50                      push   eax
 80485cb:       e8 40 fe ff ff          call   8048410 <setreuid@plt>
 80485d0:       83 c4 10                add    esp,0x10
 80485d3:       83 ec 0c                sub    esp,0xc
 80485d6:       68 9b 86 04 08          push   0x804869b
 80485db:       e8 20 fe ff ff          call   8048400 <system@plt>
 80485e0:       83 c4 10                add    esp,0x10
 80485e3:       eb 10                   jmp    80485f5 <main+0xba>
 80485e5:       83 ec 0c                sub    esp,0xc
 80485e8:       68 a3 86 04 08          push   0x80486a3
```

I mentionned in the title "the dumb way" because I think this challenge can be solve only with logical reasoning and no assembly knowledge.

The keystone to complete the password check is the <a href="https://man7.org/linux/man-pages/man3/strcmp.3.html">strcmp</a> function.

Let's extract the function call and a some lines before it:
```bash
sub    esp,0x8
lea    eax,[ebp-0x10]
push   eax
lea    eax,[ebp-0xc]
push   eax
call   80483b0 <strcmp@plt>
```

We see here interesting reference to **ebp-0x10** and **ebp-0xc**, as we know the strcmp function is defined as follow: strcmp(const char *s1, const char *s2).
</br>
</br>
Suppose that these reference may be the strings pointers isn't a bad idea here. Looking up in the code we can retrieve the **ebp-0x10** reference

```bash
mov    DWORD PTR [ebp-0x10],0x786573
```

0x786573 is equal to ascii string "xes" in <a href="https://en.wikipedia.org/wiki/Endianness">little endian</a> which translate in "sex" for humans.
Guessing **ebp-0xc** is our input we just have to submit our "discovered" string and get the shell!
