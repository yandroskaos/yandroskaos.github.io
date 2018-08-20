---
title: An initial evaluation of ROP-based JIT compilation
description: ROP-based compilation
category: papers, languages
---

Return-oriented programming (ROP) is a security exploit technique that allows an attacker to execute code in the presence of security defences.  
By modifying the contents of the runtime stack, the program control flow can be changed to execute specific machine sequences called gadgets.  
This new way of thinking about program flow may be useful for improving the runtime performance of specific language features such as structural reflection, dynamic code evaluation, and function composition.  
This article presents an initial evaluation of ROP as a JIT-compilation technique.  
We compare runtime performance, memory consumption and compilation time of four different back-ends, including ROP, of a simple stack-based virtual machine.

[CLick here download the paper in PDF][1]

[1]:{{ site.url }}/files/an-initial-evaluation-of-rop-based-jit-compilation.pdf
