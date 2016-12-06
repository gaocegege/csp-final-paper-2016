# csp-final-paper-2016

* [Sandboxing in Linux: From Smartphone to Cloud](http://www.ijcaonline.org/archives/volume148/number8/borate-2016-ijca-911256.pdf)

## System Call Interposition

* [A Secure Environment for Untrusted Helper Applications(Confining the Wily Hacker)](https://www.usenix.org/legacy/publications/library/proceedings/sec96/full_papers/goldberg/goldberg.pdf)
* [Ostia: A Delegating Architecture for Secure System Call Interposition](http://benpfaff.org/papers/ostia.pdf)

第一篇是这个领域最经典的一篇论文，阐述了 janus 的实现，第二篇是比较近的论文，是在 janus 的基础上做了一些事情。

System Call Interposition 是一个很古老的概念了，最著名的实现是 ptrace，ptrace 最早是在 Version 6 Unix 上被引入的，它也被广泛使用在各类 debugger 之中。Linux 平台上最著名的 gdb 就是使用了 ptrace 来做 system call 的监控的。

在之后，Janus 在 1996 年被提出，建立在 ptrace 的基础上，用 ptrace 来做 system call 的监控的。但是 janus 在用 ptrace 的过程中有很多问题，[Janus: an Approach for Confinement of Untrusted Applications](http://www2.eecs.berkeley.edu/Pubs/TechRpts/1999/CSD-99-1056.pdf) 提到了 ptrace 不能满足 janus 的地方，[Traps and Pitfalls: Practical Problems in System Call Interposition Based Security Tools](http://www.isoc.org/isoc/conferences/ndss/03/proceedings/papers/11.pdf) 提到了 janus 的很多其他问题。

值得一提，systrace 是重度参考了 janus 的实现，[Improving Host Security with System Call Policies](http://www.citi.umich.edu/u/provos/papers/systrace.pdf) 详细阐述了其实现

这部分可以我来看，基本这些我觉得够了，再深入就不是对 Sandbox，而是对 System Call Interposition 的论文了。主要是前面的两篇，后面都是发散。

## SFI

* [Native Client](#)
* [Efficient Software-Based Fault Isolation](https://crypto.stanford.edu/cs155/papers/sfi.pdf)
* [Language-Independent Sandboxing of Just-In-Time Compilation and Self-Modifying Code](http://citeseerx.ist.psu.edu/viewdoc/download?doi=10.1.1.207.6665&rep=rep1&type=pdf)

## Capabilities

* [Confused Deputy](http://dl.acm.org/citation.cfm?id=871709)
* [Capsicum: practical capabilities for UNIX](http://www.cl.cam.ac.uk/research/security/capsicum/papers/2010usenix-security-capsicum-website.pdf)
