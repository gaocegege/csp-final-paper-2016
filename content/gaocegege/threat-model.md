# 威胁模型

目前在互联网上，不受信任的应用的数量正在快速增长，让不受信任的应用程序能够更好地运行在系统中，是一件相当困难的事情。这些不受信用的应用程序可能隐藏有带有攻击意图的代码，它们获取操作系统高权限、读取文件系统中的敏感数据（比如密码、照片、文档等）、植入广告、导致系统崩溃等等。而攻击手段有很多，其中包括代码注入、Cross Site Scripting(CSS)、缓冲区溢出、Return-oriented programming(ROP) 等等 [Miwa]。

传统的操作系统并没有很好地解决这方面的问题，在隔离方面，仅仅依靠运行时的简单隔离是不够的。新的趋势使得操作系统应该在处理器，内存等硬件层面和进程，文件系统等软件层面进行更加细致的隔离和容错。这也是沙箱技术关注的焦点。沙箱技术有很多种，它们实现的方式也是不同的，但是其目的都是为了限制代码的运行，给予其隔离的运行环境。

// TODO：完善威胁模型

## Reference

* [Miwa] Miwa, Shinsuke, et al. "Design and Implementation of an Isolated Sandbox with Mimetic Internet Used to Analyze Malwares." DETER. 2007.