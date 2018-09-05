---
title: Compiler construction
description: Compiler construction
category: blog
---

This is going to be the first post of a series wich target compiler technology.

Compilers are complex beasts, but we have been writing and studying them for a lot of years, so they are better understood than other kind software, eg: operating systems 
(by the way, i plan also to write a series of posts on OS development, stay tuned).

I'm writing this yet-another-compiler-post because i find that there isn't meaningful explanations on the topic out there, everybody seems to be fine
after explaining lex + yacc.

In order to comment certain patterns and design elements for this software, i've written a full blown compiler.
It takes as input language a very simple object oriented language based on the usual supects.
This language has inheritance, interfaces, (simple) I/O, method overloading as well as the typical primitive types and arrays.
It is strong, statically typed and it targets .NET IL (so with an IL assembler, you can have functional programs).

My plan is to start where others leave, and touch a number of things such as Builder + Visitor patterns applied, types, symbols and 
a complete semantics checker. And finally a backend for .NET IL. IL is a very high level assembler and the underlying virtual machine is a stack one.
This means the backend will be quite easy to write, but would be interesting to develop one for, say, nasm, as complexity increases when the underlying
 machine is a register-based one.

In the meantime, i've publish full source code in my github repo under [oolc](https://github.com/yandroskaos/oolc) so you can take a look.


