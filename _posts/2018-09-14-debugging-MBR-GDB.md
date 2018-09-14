---
title: Debugging a MBR with GDB
description: Debugging a MBR with GDB
category: blog
---

When you develop your own operating system, one of the most recurrent problems is the inability (or difficulty) of debugging.

It is very typical, you have written all the code according to the specs. You've reviewed thousand of times, doing even some *symbolic code execution* in your head. It is tied, it can not possibly fail. But it does. Miserably.

Quite often specs are incomplete, there is some missing bit here or there, or are very complex (Hi Intel!) or you are simply blind or being stubborn about the code. Whatever it is, you need to dig deeper and know what's going really on.

There's a lot of people (it still amazes me how many) that prefer going with some variation of *printf debugging*. It is a technique that may be useful sometimes, but it is definitely very limited as it needs a lot of runtime support. To start with, you need somewhere to put the output of the *printf's*. In the end, and altough some [people doubt about it](https://softwareengineering.stackexchange.com/questions/78152/real-programmers-use-debuggers), yes, real programmers **do use** debuggers.

Recently i've changed my windows box for a linux one. It has not been that hard and i'm pleased with the support this operating system has if you happen to be a developer. Except for one thing. It's debugger.

It is not that GDB is not a **very powerful** debugger. It is. But it is targeted to... debugging. *What does this mean? Of course, it is a debugger, what else could it be possibliy targeted at?* you may think... Indeed it is, it has a lot of insight of the program being debugged, symbols, source code, sections mapping... to the point that is difficult or at least anti-intuitive to use for **raw binary debugging**.

In the windows world, things are different. You start a debugger and you see assembler everywhere, bytes here, words there, all hexadecimal numbers. Exactly the opposite as with GDB. In fact you can suffer for the **opposite** problem, if you are not careful with symbols, you may have to debug without connection with the source code.

What is better? It is just a matter of taste, you could say GDB is more oriented to debug *your* code, while windows debuggers are more oriented to binary (almost to the point of reversing).

When faced with system things like debugging the boot process of a machine, you know what side i'm in. But to be fair, it is not impossible to debug system things with GDB, for me, which to be honest i haven't spend enough time mastering the tool, is only cumbersome.

![GDB MBR debugging]({{ site.url }}/files/GDB.png 'Sloth...')

If you take a look at the screenshot, you'll notice that disassembly and addresses are for a flat 32-bit addressing mode (the typical used in protected mode, used by windows, linux, ... almost everybody?), but when the machine (a IA32 based machine) starts, it does so in **real mode**. So the debugger will work but you get a lot of problems to see what's going on as you see 32-bit code. 

Things get **even more interesting** if you get out of segment 0. When the machine boots, you can start debugging at physical address 0x7C00. In that point, CS:IP matches EIP, but when you **JMP FAR** to another segment, say to continue with the second stage loader... GDB still prints IP extended as real point of execution, so you basically don't see anything. To be more clear on this, CS:IP = 9000:0000 will be interpreted as EIP = 0, so you'll see the interrupt vector table interpreted as 32 -bit assembler code.

Oddly enough, single stepping works (as i'm using GDB as stub connected to a remote GDB server, QEMU in this case, which does the right thing), so it's a problem of visualization. But a hard one. At least, you can inspect raw memory and see where you are really at execution so you are not completely blind, and in these cases, that's possibly **all you need**.





