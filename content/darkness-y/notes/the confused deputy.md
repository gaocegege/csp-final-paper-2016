## 简单梳理
* commercial timesharing services
* compiler被安装在SYSX目录下，执行时提供文件名就可以得到一些optional debugging output，过程中会有一些统计性质的文件（STAT）文件生成，为了使得compiler可以操作这些文件（home files license），OS会是的compiler对其所在的目录有W权限。
* 如果SYSX下，存在另一些文件，比如BILL，如果有意将其作为参数执行compiler，相关信息就会泄露。
* 谁应该为此负责？流程本身没有问题，问题在于赋予home files license的时候，但是如果对其中可能产生的情况一一甄别过于复杂，且增加相关约束的边际成本将越来越高。
* compiler的权限来自两方面：invoker，从而能够输出debugging output； files license，从而能够生成统计性质的文件（the compiler serves two masters and carries some authority from each to perform its respective duties）。
* 一开始通过修改系统，提供了一个叫switch hats的sys call，使得compiler在做不同的事情的时候有不同的权限。但是如果存在多于两种权限或者不同于访问文件的安全机制时，这样的修改很难被范化。
* capability作为解决方案，使得compiler在完成某部分任务时仅仅被赋予相关的的capability，通过对权限显式的指派*（不是很懂）*，可以使得授权过程不需要知道具体实际的文件名。
* 由于基于capability的系统不仅统一化了很多不同的naming functions，同时使得原来基础的安全机制不在必要，其可以替换掉原先access-list based sys中的ad-hoc机制，所以最终实现的系统反而更加轻量级，performance也很好。（这里的实现成为了[KeyKOS](https://en.wikipedia.org/wiki/KeyKOS) ）
* 别的一些系统尝试在原传统的机制之上扩展出capabilities，但是往往它们因此陷入很多combined disadvantages而没能从combined advantages中获益。作者认为capabilities的实现必须作为整个系统的基础底层架构。