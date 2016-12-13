# System Call Interposition(SCI)

在浏览器中，有很多互联网上的内容是需要下载，然后使用本地的查看器打开的。比如 PDF 文件等，需要通过 Adobe Reader 等专用软件打开。但是下载的内容有可能是隐藏有恶意代码的，但是本地的查看器并不一定将其视为不可信的内容，因此下载的内容在被相应的查看器打开时，恶意代码可能会被错误地执行了。对 System Call Interposition（以下称为 SCI）的研究，最开始的动机就是如何限制打开浏览器下载内容的助手应用，希望能够限制助手应用本身进行系统调用的权限。Janus [goldberg1996secure] 是 Goldberg 在 1996 年发表的论文，也是第一个成型的 SCI 系统实现，是这一领域奠基性的论文。随后有很多相关研究就此展开，研究者们继续完善 Janus，并且提出了新的解决思路。Ostia [garfinkel2004ostia] 是 2004 年被提出的，它指出 Janus 存在很多缺点，而这些缺点的产生是因为架构问题，而不是 SCI 所固有的。因此它提出了一种新的架构，并且验证了他们的工作是比 Janus 的实现要高效而且可以解决 Janus 的缺点与问题。

## Janus

TODO

## Ostia

TODO

## Reference

@inproceedings{goldberg1996secure,
  title={A secure environment for untrusted helper applications: Confining the wily hacker},
  author={Goldberg, Ian and Wagner, David and Thomas, Randi and Brewer, Eric A and others},
  booktitle={Proceedings of the 6th conference on USENIX Security Symposium, Focusing on Applications of Cryptography},
  volume={6},
  pages={1--1},
  year={1996}
}

@inproceedings{garfinkel2004ostia,
  title={Ostia: A Delegating Architecture for Secure System Call Interposition.},
  author={Garfinkel, Tal and Pfaff, Ben and Rosenblum, Mendel and others},
  booktitle={NDSS},
  year={2004}
}