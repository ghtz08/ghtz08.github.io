---
layout: post
title:  "操作系统内存分页"
date:   2019-07-21 08:23:04 +0800
categories: 操作系统
---

玩过 Windows 内核的都应该知道 Windows 为了节省 4k 的内存，实现了自映射，将页目录本身当作了一个页表来寻址 4M 的虚拟空间（将 4M 空间的虚拟内存映射到物理内存），而这 4M 的虚拟地址空间恰恰就是从 0xc0000000 到 0xc03FFFFF 的空间，这一段空间正好是页表页目录的映射区域。下面我来说一下这怎么变成现实。  
首先有几点必须遵守的规则：

1. 一个页表映射的 4M 虚拟地址肯定是连续的。
2. 一个页表映射到的 1024 个 4K 的物理页不一定是连续的。
3. 实现自映射必须是页目录和页表结构一样。

有了以上的规则，我们要想理解（或者自己实现一个） Windows 的自映射就必须理解实际上 Windows 的页表虚拟地址都是连续的，映射到 0xC0000000 到 0xC03FFFFF，表现为一个页表数组，一共 1024 项，每项4096 字节，一共正好 4M，页目录作为这 4M 的页表。我们反着想，如果页目录成为了这 4M 的页表，那么肯定映射 0xC0000000 到 0xC03FFFFF 的地址。  
我们将端点的两个地址分解

```text
1100 0000  0000 0000  0000 0000  0000 0000
1100 0000  0011 1111  1111 1111  1111 1111
```

仔细观察发现高 10 位是相等的，而这高 10 位正好是页目录项的索引，我们知道他是 768，好了，页目录要作为第 768 页目录项对应的页表，现在这个页目录可以作为一个页表插入到这个页表数组的第 768 项的位置了，它开始身兼两职了，CR3 寄存器里面写的是它的物理地址，它作为页目录，此页面偏移 768 处也写入了这个物理地址，它作为页表。

现在考虑把这个页目录（页表）的虚拟映射地址换一下会怎样，别的页表不动。比如这个页目录映射到了0xDC000000 这个虚拟地址（仅仅据个例子）。然后分解地址 1101110000 0000000000 000000000000，通过计算发现其对应于页目录的 880 项，那好吧，我把此页面 880 偏移改成此页面的物理地址，别的偏移处仍然指向 0xC0000000 到 0xC03FFFFF 的虚拟地址对应的物理地址。看看会发生什么事情，如果把这个从 0xC0000000 到 0xC03FFFFF 游离出来的页面当作页表的话，那么它所映射的虚拟地址应该连续，但是它自己是游离出来的，它的虚拟地址和别的页表的虚拟地址根本不连续，所以这样做是不可以的，也就是说对映射地址的连续性要求是一个必要的条件。Windows 之所以这么简单的实现了自映射，完全归功于它的设计思想，实际上一个设计并没有多么复杂，只要有了完备性，剩下的就是一个自然结果了，比如 X 系统是复杂的，而且它在 UNIX 出现很久以后才出现，但是 UNIX 却完美地支持了 X 系统，这就说明 UNIX 很完备。Windows 的模块化作的很好，一致性作的也很好，它几乎将任何东西都标准化，每个进程的页表映射到同一个虚拟地址，在 Windows 看来，为每个进程提供一个一致的视图要比巧妙的杂糅式进程管理重要的多，所以它将虚拟地址空间作了很复杂的划分和规定。
Linux 正好走了相反的路，它对于虚拟内存空间什么也没有规定，只是说了一下映射规则，提供了一套完备的机制，策略问题根本没有提及，内核虚存空间各个进程完全共享，各个数据结构散落在各处，内核空间仅仅提供了直接映射和高端映射的若干种映射规则和分配规则，至于说每个空间范围干什么用，Linux 只字未提，所以说 Linux 的页表可以散落在物理内存的任何地方，因为物理内存和虚拟内存在内核的很大一部分是一一映射的，所以上面的虚拟地址连续的要求就很难满足，比如这可能要求物理内存连续，在 Linux 连续地址空间如此宝贵，浪费当作页表 Linux 觉得这不值得，再说，页表可以随时分配，为什么要连续呢？连续的话就要先预留 4M 页面，然后页表没有完全分配前会造成很多空洞，有几个进程能完全用完 4M 的页表呢？Linux 保守的回答了这个问题。

参考资料

1. [晨曦之光. linux的页表为什么没有实现自映射](https://www.oschina.net/question/234345_48121).
