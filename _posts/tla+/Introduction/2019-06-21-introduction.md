---
layout: post
title:  "前言"
date:   2019-06-21 15:11:03 +0800
categories: TLA+
permalink: /tla+/introduction
---

## 什么是 TLA+？

TLA+ 是一个*形式化规范语言*（formal specification language）。它是设计系统和算法的工具，通过编程验证系统没有关键（critical）的 BUG。它相当于蓝图的软件。

## 为什么要用 TLA+？

这里有一个简单的 TLA+ 规范，表示人们交易单个物品。你能发现 BUG 吗？

```TLA+
People == {"alice", "bob"}
Items == {"ore", "sheep", "brick"}
(* --algorithm trade
variable owner_of \in [Items -> People]

process giveitem \in 1..3 \* 最多可进行三次交易
variables item \in Items,
          owner = owner_of[item],
          to \in People,
          origin_of_trade \in People
begin Give:
    if origin_of_trade = owner then
        owner_of[item] := to;
    end if;
end process;
end algorithm; *)
```

既然我们检查了要把物品卖掉的那个人是不是它的所有者，那么我们应该可以防止诈骗，对吗？如果我们运行模型检测器，会发现这不是真的。因为 Alice 可以和她自己交易一件物品，然后在流程运行结束之前，先完成一个并行的交易，将相同的物品给 Bob。这个交易完成后，Alice 又拿回了项目。我们的算法在竞态条件失败了，我们之所以知道这一点是因为 TLA+ 尝试了所有可能的状态和时间轴。

有几种不同的方法来修复这个问题，但我们修复后还能适用于两个或更多的人呢？在 TLA+ 中，验证这些只需要简单的 `People == {"alice", "bob", "eve"}`。如果我们可以同时交易多个商品，它可以正常工作吗？`variable items \in SUBSET Items`。如果有很多 sheep，ore，和 bricks？`amount_owned = [People \X Items -> 0..5]`。如果在三个人和其它每一个人都交易 1 个 ore 和 1 个 sheep 时， Eve 和 Alice 交易 0 个 brick，会怎么样？如果这在可能的状态空间里，TLA+ 也会验证它。

## TLA+ 会很难使用吗？

形式化方法以难到只有在关键系统中使用才有价值而闻名。这意味着所有的指南都是假设读者工作在一个关键的系统上而编写的。在这个系统中，他们必须彻底了解 TLA+，以确保他们的系统不会因为 BUG 害死人。

如果对你来说危险的 BUG 是“一些人死了”，那么形式化方法确实很难。如果对你来说危险的 BUG 是“没有人会死，但我们的客户很生气，我们不得不花两周的时间来跟踪和修复这个 BUG”，那么你需要的 TLA+ 的一小部分实际上是很容易学习的。只要找到一个对初学者友好的指南就好了。

## 友好的指南在哪里？

你好！本指南以一种简单、实用的方式涵盖了 TLA+ 的基础知识。如果你想从初级开始，你可以先了解更多我们在[**这里**](/tla+/introduction/about-this-guide)讨论的内容。如果你想马上深入，为什么不试试[**旋风之旅**](/tla+/introduction/example)。

无论如何，欢迎来的 TLA+！

翻译自 [INTRODUCTION](https://learntla.com/introduction/)
