# Sandboxing 相关技术以及使用场景

为了能够深入浅出地进行介绍，方便理解，本节将以一个现代的浏览器作为应用场景进行切入，在浏览器上，沙箱有着非常多的应用：

* 网页应用，现代的浏览器会在沙箱中运行你打开的网页代码。诸如 Javascript 之类的网页代码在浏览器的沙箱中只有有限的权限，它们并不能直接与文件系统等等进行交互，因此 Javascript 中的恶意代码的破坏性仅限于在沙箱内。
* 浏览器插件，浏览器并不会直接将插件进程运行在操作系统上，而是会将其运行在一个沙箱进程中。比如 Adobe Flash 插件，就是如此。这样的隔离方式使得插件如同网页应用一样不能直接与操作系统进行交互，防止两者之间相互的攻击。
* 各类下载内容的查看器，以 PDF 文件为例，浏览器下载的 PDF 文件会使用系统内的 PDF 阅读器打开，目前一些 PDF 阅读器正在使用沙箱技术来解析 PDF 文件，其中最有名的是 Adobe Reader。它运行在一个沙箱中，因此如果 PDF 文件中有恶意内容，也不会影响到操作系统。
* 浏览器本身，浏览器是沙箱技术运用最广泛的桌面应用之一。浏览器不止将其请求的网页应用和插件等运行在容器中，浏览器本身也是运行在一个独立而隔离的沙箱中的。低权限的沙箱运行可以保证即使恶意内容破坏了网页应用所在的沙箱，也会被浏览器的沙箱所限制 [1]。

本节将介绍 System Call Interposition(SCI)、Software-based Fault Isolation(SFI) 和 Capabilities 等技术在浏览器中的体现。System Call Interposition(SCI) 通过对系统调用的跟踪和限制，来使得应用不可以调用一些敏感的系统调用，或者不能将敏感的参数传入到系统调用中。Software-based Fault Isolation(SFI) 通过对指令的重写，使得应用运行时的故障不会往外传播，同时在一定程度上限制了代码的控制流。

// TODO: add intro to Capabilities.

这是三种完全不同的思路，它们的目的都是隔离代码的运行环境，构建出一个高度隔离的，低特权级的沙箱环境。

## [System Call Interposition(SCI)](./sci.md)

## [Software-based Fault Isolation(SFI)](./sfi.md)

## Reference

* [1] http://www.howtogeek.com/169139/sandboxes-explained-how-theyre-already-protecting-you-and-how-to-sandbox-any-program/