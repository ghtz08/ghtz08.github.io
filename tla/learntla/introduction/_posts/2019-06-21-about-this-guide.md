---
layout: post
title:  "关于这个指南"
date:   2019-06-21 16:53:23 +0800
categories: [TLA+, Learn TLA+ 的翻译, 前言]
permalink: /tla/learntla/introduction/about-this-guide
---

这个指南重点介绍了使你尽可能快地启动和运行所需的最低依赖，以及一些改进模型的中间技术。这会忽略或简单介绍 TLA+ 的绝大部分，以便使这一小部分更容易学习。当这并不意味着其他部分无聊、困难或无用。只是它们超出了这个指南的范围。

我假定你已经有以下背景知识：

- **你是一个经验丰富的程序员**。这不是一个对编程的介绍，TLA+ 也不是一个对用户友好的工具。
- **你熟悉测试**。如果你之前没有使用过单元测试，那么学习单元测试会更有用。
- **你知道一些数学知识**。TLA+ 借鉴了大量的数学结构和语法。如果你听说过德摩根定律，知道集合是什么，并且理解 `(P => Q) => (~Q => ~P)` 的意思，就没问题了。否则，虽然可以继续学习，但内容可能不是那么易懂。

**你需要下载** [TLA+ Toolbox](https://github.com/tlaplus/tlaplus/releases/latest)（官网说需要 Java 1.8 及以上，实测 Windows 下 Oracle Java 1.8 可用，Oracle Java 1.12 不可用）。还可以访问以下资源：

- [PlusCal 手册](http://lamport.azurewebsites.net/tla/high-level-view.html)：PlusCal 是 TLA+ 的算法接口。虽然我们会在本指南中详细介绍 PlusCal 的所有内容，但有一个完整的语法参考也是很好的。我们将使用手册的 p 版本。
- [TLA+ 速查表](/tla/assets/introduction/summary-standalone.pdf)：就和它名字一样，这包括这个指南范围之外的语法。
- [《Specifying Systems》](http://lamport.azurewebsites.net/tla/book.html)：《Specifying Systems》由 TLA+ 的创造者 Leslie Lamport 编写，至今仍是在该主题上最全面的书籍。它比本指南高级得多，但是你应该知道它的存在。

[//]: # (TLA+ 速查表的地址：http://lamport.azurewebsites.net/tla/summary-standalone.pdf)

翻译自 [ABOUT THIS GUIDE](https://learntla.com/introduction/about-this-guide/)
