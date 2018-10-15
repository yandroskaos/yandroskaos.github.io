---
title: Should i UEFI or should i go
description: First steps with UEFI
category: blog
---

As promised, i am in the process to retake a hobby OS i made 10 years ago.

And where do you start from? Yeah, the bootloader. I had a precious MBR-BIOS-based bootloader that mapped a second stage loader which ended loading the kernel and doing all that things that must be done, you know, selectors, stack, A20 line, memory map, where did i boot from?, video parameters, relocate modules in memory... pure joy :D.
Take a look if you don't trust me [here](https://github.com/yandroskaos/XkyOS/tree/master/XkyOS/Source/Core/Boot).

But those days are gone (thankfully!) and UEFI is the thing to support and to use. For those who don't know what does UEFI stand for, it means Unified Extensible Firmware Interface. And it is a civilized way of booting, maybe too much civilized to be honest. So civilized that when your boot code is called you are already in **protected mode** (in 32 bits).

What's more, as i'm updating also to move to x64, UEFI gently leaves the cpu in long mode, with **pagination enabled in identity mode**... (*a little tear drops*).
Well, i must say that this is a qualitative change, i believe that lots of *i'm doing my own OS* initiatives got stuck at the boot phase after dealing with lots of partial and incomplete information just to reach the point where you can actually do something. And a whole lot others just finished this phase and got tired after so much work.

Everything you need about UEFI is [here](http://www.uefi.org/sites/default/files/resources/UEFI_Spec_2_7.pdf), a *minimal* guide of just 2899 pages :D. To be fair it covers **everything** and a number of different architectures and it is no more impressive than the typical hardware specs like Intel's.

So i must say i am quite pleased, i'm no planning of supporting any legacy thing of any type, no more BIOS based booting *just in case*, i'm in the future and **i like it**. To show why, in a couple of hours i was able to set the environment, and make a minimal UEFI app which already gets the memory map (goodbye `int 15h`) and available video modes. You can take a look at it in this [link](https://github.com/yandroskaos/exohype/tree/master/boot/src).

Next steps include writing robust code to really get what i need from system data, and after remapping memory and pack that retrieved system data, load the kernel and jump in.

I will tell you in other posts!

