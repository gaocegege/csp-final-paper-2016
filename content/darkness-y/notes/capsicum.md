## 整体概述
* Capsicum是一个轻量级的capability & sandbox框架，被纳入了FreeBSD9，由Capsicum运行时连接器（capability-aware run-time linker）和底层组件（libcapsicum）组成。它扩展了UNIX POSIX（而不是替换），提供了几个新的kernel primitives（sandboxed capability mode & capabilites），以及用户态的sandbox API。

---

## 重要名词
* Privilege separation （compartmentalisation），编程实现比较复杂，且有显著的技术限制，目前的OS也均不是基于此设计的。
* fine-grained access control，这里包括MAC和DAC（mandatory / discretionary access control）：前者由用户自己来控制，在obj被访问时由OS检查权限；后者则能够呈现出multi-level security，通过run-time hooks由内核在不同地方进行检查，在confused deputies问题上表现出鲁棒性，但是极其inflexible。

这些方式都需要大量的programmer effort，同时在bootstrap的时候需要提升权限，分析显示，由于inherent flaws 或者incorrect use，会导致缺陷。

某种程度上是融合了MAC和micro-kernel，并且保证了performace，即micro-kernel-like compartmentalisation。

 * capability-centric research, capability-oriented programming

---

## 目标问题
解决比如单个app以一个user操作多个种类的数据（表现出不同的权限限制），比如一个浏览器需要parse HTML; scripting languages, images and videos from many untrusted sources，但由于它拥有完整的权限，所以可以访问所有的资源（ambient authority）。
在这个问题上细粒度的访问控制（MAC）是有价值的，但是不足以保证足够的权限分离。

对于有些应用（比如gzip），多个用户均可使用，建立单一的sb是不合适的。


---

## Capability mode
* 一个**进程级别**的credential flag，通过sys call（*cap_enter*）设置，一旦设置不能清除，且会被继承给它的子进程（通过继承或者message-passing）。
* 进入capability mode的进程，其会限制全局namespace的访问、一些系统管理接口的调用（比如/dev、PCI bus的访问、ioctl、reboot、kldload等）、以及对于一些sys call（对于需要全局namespace的会被禁止，其他的则被限制）的调用。

## Capabilities
* Mac OS X和Mach对capability与传统fd之间的关系的维护采取了不同的策略，对开发者造成困扰。
* 尽可能复用现有的API，capsicum选择拓展现有的fd abstraction，引入新的fd类型，即capability，来封装并保护原生的fd。
* fd本身就具有unforgeable的特性，且会被子进程或者通过IPC channel继承（比如fork，exec，UNIX domain socket）。
* *一个权限太宽的反例*：对于一个只读的fd，对于其元数据的操作（比如fchmod）依旧是被允许的。
* *cap_new*接受fd以及一个权限的mask作为参数，生成对应的capability，如果参数fd本身就是capability，则新的mask必须是其子集，通过*fget*进行capability的权限检查。fd会在内核中将其转化为对应的引用，从而确保任何对fd的访问不会绕过这里的权限检查。大概有60中可能的mask权限。
* capability可以作用于dir的fd，过程中一些传递的cases被限制，使得dir capabilities能够delegate fs namespace subsets（这里可以插个图**fig.4**），以保证避免诸如chroot vulnerability的类似情况等。
* 禁止通过fexecve进行提权。

## 启动流程与目标
* 创建sb的时候不通过fd，mem mapping等形式泄露资源（undesired resources）
* libcapsicum通过cap_enter切断sb与全局ns；关闭那些在delagation中没有被identified的fd；通过fexecve来对address space进行flush。


---

## 内核修改
* 在内核服务的实现部分就进行修改，而不是简单的过滤sys call，这样的话一个单一的限制可以在一个地方就完成（e.g.: namei）
* fget会检查权限，并返回对应的struct的引用，如果验证失败，返回ENOTCAPABLE。这里通过改变fget的signature（什么意思？）使得可以通过compiler来发现遗漏的code paths，保证所有情况都被处理。
* 对PID，process descriptor采用了类似Mach中的方式。

## run-time environment
* fork, exec等都依赖与全局的ns
* 通过对路经的硬编码绕开全局ns
* 不像别的方法，没有specific的IDL（Interface Description Language），尽可能兼容已有app
* libcapsicum的fdlist抽象使得复杂的层次化app能够被设定capabilities并传入sb（相当于sb template机制）

---

## Adapting app
* 需要分析程序来确定资源的依赖关系，并使其以分布式系统的方式变成（模块之间通过msg或者共享内存来通信，而不是address space）
* 两种选择：直接用capability mode或者libcapsicum
* 对于结构简单的程序（比如只有一个I/O循环），可以直接使用cap_enter，这样的overhead是极小的。样例：tcpdump
* 对于已经有权限分离机制的程序，结构相对复杂，但是已经有了通过fd来传递的概念，通过简单的修改（同样使用cap_enter），在安全性上可以获得显著的提升，带来的overhead也是很小的。样例：dhclient，Chromium
* 在确定好边界的前提下通过libcapsicum API来进行改写，以在保证performance的同时提升安全性。样例：gzip
* Chromium在不同的操作系统下有不同的策略：Windows, ACLs + SIDs; Linux, chroot; Mac OS X, Seatbelt; Linux, SELinux; Linux, seccomp; FreeBSD, Capsicum，分别是两种DAC-based，两种MAC-based，两种capability-based，见**fig.12**，详细对比见第五章的分析。
