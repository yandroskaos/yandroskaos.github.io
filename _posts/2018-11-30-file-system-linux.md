---
title: Example Filesystem for Linux
description: Example Filesystem for Linux
category: blog
---

Now I develop on Linux and I am curious on its internals. After reading a number of books on the topic, looks **very** similar to Windows at 10,000 feet view.

Before somebody starts to throw stones at me, let me explain a bit more.

Both systems are mainly monolithic kernels built around paging as memory protection feature for processes. System call mechanisms is similar. Dynamic modules can be loaded on both. 

While Windows uses abstractions for both processes and threads, Linux builds them around the task concept but it's almost the same. You can think of tasks as threads which live in a shared address space which they point at. Windows only makes this more clear by saving the address space in the process abstraction.

Similar philosophy for device management, also similar concepts for interrupt handling (Windows DPC's looks like Linux's interrupt bottom halves).

Similar executable file formats, both built around COFF...

Of course there are differences like how exceptions are sent to user-mode, being signals an integral concept in Linux and having Microsoft implemented SEH for that task, and every single implementation details is, obviously, different.

To explore Linux a bit more, I wanted to develop a minimal Linux filesystem. That would allow me to build kernel modules and play with the interfaces for filesystems as well as dig a bit deeper in Linux.

Things started bad, not gonna say otherwise. The first module I compiled and launched made the kernel pagefault with a stacktrace as beautiful as this:

```
Nov 29 19:32:08 kernel: [...] RIP: 0010:sget_userns+0x3ed/0x490
Nov 29 19:32:08 kernel: [...] RSP: 0018:ffffc0df82cd3cf0 EFLAGS: 00010246
Nov 29 19:32:08 kernel: [...] RAX: ffffffffc0bde070 RBX: 0000000000000000 RCX: 0000000000000000
Nov 29 19:32:08 kernel: [...] RDX: ffff9fc89d7a50e8 RSI: ffffffffc0bde43c RDI: ffff9fc89d7a53db
Nov 29 19:32:08 kernel: [...] RBP: ffffc0df82cd3d30 R08: ffff9fc67f8651b8 R09: ffff9fc8a8803c80
Nov 29 19:32:08 kernel: [...] R10: ffffc0df82cd3c10 R11: 0000000000000388 R12: ffffffff8d878990
Nov 29 19:32:08 kernel: [...] R13: ffffffff8f0bbcf4 R14: ffffffffc0bde040 R15: ffff9fc89d7a5000
Nov 29 19:32:08 kernel: [...] FS:  00007ff845691080(0000) GS:ffff9fc8b1480000(0000) knlGS:0000000000000000
Nov 29 19:32:08 kernel: [...] CS:  0010 DS: 0000 ES: 0000 CR0: 0000000080050033
Nov 29 19:32:08 kernel: [...] CR2: ffffffffc0bde070 CR3: 00000001ed784002 CR4: 00000000003606e0
Nov 29 19:32:08 kernel: [...] Call Trace:
Nov 29 19:32:08 kernel: [...]  ? ns_test_super+0x20/0x20
Nov 29 19:32:08 kernel: [...]  ? set_bdev_super+0x40/0x40
Nov 29 19:32:08 kernel: [...]  sget+0x7d/0xa0
Nov 29 19:32:08 kernel: [...]  ? ns_test_super+0x20/0x20
Nov 29 19:32:08 kernel: [...]  mount_bdev+0x117/0x290
Nov 29 19:32:08 kernel: [...]  ? 0xffffffffc0bdd000
Nov 29 19:32:08 kernel: [...]  tfs_mount+0x1b/0x60 [tfs]
Nov 29 19:32:08 kernel: [...]  mount_fs+0x37/0x150
Nov 29 19:32:08 kernel: [...]  ? alloc_vfsmnt+0x1b3/0x230
Nov 29 19:32:08 kernel: [...]  vfs_kern_mount.part.23+0x5d/0x110
Nov 29 19:32:08 kernel: [...]  do_mount+0x5ed/0xce0
Nov 29 19:32:08 kernel: [...]  ? memdup_user+0x4f/0x80
Nov 29 19:32:08 kernel: [...]  SyS_mount+0x98/0xe0
Nov 29 19:32:08 kernel: [...]  do_syscall_64+0x73/0x130
Nov 29 19:32:08 kernel: [...]  entry_SYSCALL_64_after_hwframe+0x3d/0xa2
```

Which made me stare for minutes... But was not unexpected, it was not going to be that easy.

One of the best things of Linux is it is open-source. So I could compile a kernel for debugging **with source code** (tears here).

But also, having available source code allowed me to look at it to see if I was missing some flagrant point, because a reproducible page fault pointed to some misconception, missing initialization or the like.

And that was it, i had forgotten telling the filesystem manager that my module required a device backing the data.

Concretely this as part of filesystem type definition:
```
.fs_flags = FS_REQUIRES_DEV
```

From there on, it is not that everything worked flawlessly as i had an error that made the filesystem look for inexistant sectors. I forgot to divide some byte offset by the sector size, thus interpreting byte offsets as sector offsets. This made the filesystem show only the root directory but everything beneath it was an *empty folder*.

Later I had forgotten strings in disk were stored as pascal strings (size + data) instead of C strings (data + 0), so some **dentries** got funny names.

But in the end I made it work. It is a readonly filesystem for the filesystem I defined for a previous project. I plan to add now the support for writing and maybe also handle permissions as now it is only for root.

As a summary, I like the filesystem interface for Linux, some parts are not as polished as when writing a Windows file system minifilter but on the other hand you can get a module up and running in less time.

You can find the code [here](https://github.com/yandroskaos/tfs).

