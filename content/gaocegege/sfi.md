# Software-based Fault Isolation(SFI)

浏览器是一个泛用性很强的应用，因此浏览器要求支持以插件的方式进行功能扩展。同时，为了提高浏览器中代码执行的性能，目前很多浏览器都在探索在浏览器中执行 Native 代码的方法。其中谷歌浏览器支持将 Native 代码运行在一个使用 Software-based Fault Isolation(以下称为 SFI) 的沙箱中。SFI 是在 1994 年在 Wahbe 的论文中第一次出现的 [wahbe1994efficient]，SFI 通过修改程序二进制的方式，使得程序的二进制代码符合一些规则，来完成对于错误的隔离。在后来的论文中，谷歌结合了 SFI 和其他的一些沙箱技术，实现了一个面向 Native 代码运行环境的沙箱，这项技术一直到现在仍在被谷歌 Chrome 浏览器使用。

## Efficient Software-based Fault Isolation

TODO

## Native Client

TODO

## Reference

@inproceedings{wahbe1994efficient,
  title={Efficient software-based fault isolation},
  author={Wahbe, Robert and Lucco, Steven and Anderson, Thomas E and Graham, Susan L},
  booktitle={ACM SIGOPS Operating Systems Review},
  volume={27},
  number={5},
  pages={203--216},
  year={1994},
  organization={ACM}
}