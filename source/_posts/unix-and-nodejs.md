title: Unix哲学与Node.js
date: 2014-11-01 12:04:50
description: PingHackers | Unix哲学与Node.js | 在软件这个迭代更新迅速的领域，Unix不得不说是一个奇葩中的奇葩，自从设计完成以后以来基本上没有做过太多的修改，但是却持续地发光发热了几十年，支撑它的不仅仅是它精妙的设计，还有前辈们因为Unix而奠定的Unix哲学，影响了不仅仅是一个操作系统，而是几代Hackers的对程序的理解，对架构的设计。
tags:
- unix
- nodejs
- 翻译
---
**作者**：戴嘉华

**转载请注明出处，保留原文链接和作者信息**

* * *

翻译自：[Unix Philosophy and Node.js](http://blog.izs.me/post/48281998870/unix-philosophy-and-node-js)。

* * *

**[译注]**

在软件这个迭代更新迅速的领域，Unix不得不说是一个奇葩中的奇葩，自从设计完成以后以来基本上没有做过太多的修改，但是却持续地发光发热了几十年，支撑它的不仅仅是它精妙的设计，还有前辈们因为Unix而奠定的Unix哲学，影响了不仅仅是一个操作系统，而是几代Hackers的对程序的理解，对架构的设计。你几乎可以在那些最优秀的Hackers写的每一行代码中看到Unix哲学的身影。

本文的作者[isz](http://blog.izs.me)是[npm](https://github.com/npm/npm)（Node.js包管理器）的主要代码贡献者，在他的文章中，可以窥见Unix哲学对Node.js设计，文化所产生的影响。

![block](https://raw.githubusercontent.com/livoras/blog-images/master/alice.jpg)

以下是正文：

* * *

在TxJS的另外一天，我做了一个演讲，提到了Unix哲学是Node.js的模式、主张、和文化很重要的一部分。像往常一样，在视频流出之前，我提前把展示用到的[幻灯片](http://j.mp/node-patterns-pdf)放网上。

出于某些原因，简短地提及“Unix哲学”引起了一些人的忿怒。当时我只有25分钟，但是我讲的每一页幻灯片都可以单出拿出来做一个演讲，所以我只好尽量地把精华抽出来讲。视频不能很好地还原当时的场景。但是我的目的是能够引起大家的讨论，所以如果它引起了一些批评，也许我的目标就达到了。毕竟，无知地唱高调只适合说教，我想我最好解释一下。

<!-- more -->

Eirc S. Raymond在他的*[The Art Of Unix Programming](http://www.catb.org/esr/writings/taoup/html/ch01s06.html)*收集了一些关于Unix哲学的最佳阐述。他详细讨论了17条特定的原则，但是我最喜欢的关于Unix哲学的阐述是Salus在他的*A Quarter Century of Unix*引用Doug McIlroy的简洁的陈述：

> 这就是Unix的哲学：

> 写只做一件事并且把这件事做好的程序。

> 写能组合在一起运作的程序。

> 写能处理文本流的程序，因为文本流是通用的接口。

McIlroy接下来稍微详细地给出了4点自己的陈述：

> (i) 每个程序只做好一件事情。当需要完成新的任务的时候，写一个新的程序而不是原有程序上添加新功能。

> (ii) 让每一个程序的输出可以成为另外一个程序的输入，甚至是未知的程序的输入。不要在输入中混杂着无用的信息。避免严格按列排列二进制的输入格式。不要依赖交互式输入。

> (iii) 在设计和编写软件，甚至是操作系统的时候，要尽快地尝试，最好是在几个星期之内就完成。要毫无顾虑地删掉笨拙的地方和重写它们。（译注：深有体会，绝B认同。）

> (iv) 使用工具而不是蹩脚的编码来减轻编程任务，即使你知道你最后还是不得不自己来构建这些工具或者要放弃它们。

在X桌面系统（译注：X是Linux下的桌面系统，没有它Linux就没有界面了）Mike Gancarz把Unix哲学总结成了9点：

> 1. 精小就是优雅。
> 2. 让每个程序只做好一件事情。
> 3. 尽快地完成原型。
> 4. 可移植性高于效率。
> 5. 用纯文本来存放数据。
> 6. 使用软件来加强你的优势。
> 7. 使用Shell脚本来提高利用率和可移植性。
> 8. 不要迷恋界面。
> 9. 让每一个程序都成为过滤器。

最后一点和Ryan Dahl（译注：Node.js作者）经常说的“一切程序都是代理”（Every program is a proxy）产生了强烈的共鸣。前三点其实是[James Halliday](http://substack.net/)（译注：花名substack，疯了一样贡献了几百个Node.js模块，Node.js届无人不知的巨巨）的赖以生存的法则。

人们经常错误地迷失在Unix哲学的某些方面，而通常一叶障目不见森林。Unix哲学并不是一种特定的程序实现，或某种Unix操作系统或程序特有的东西。它并不是文件描述符，管道，套接字，或者信号量。这些误解就像是，除非一个人说[Pali语](http://en.wikipedia.org/wiki/Pali)，否则就不是佛教徒一样。

Unix哲学是软件开发的曙光，而不是软件中的一种特定技术开发。它是一种值得我们追求的理想境界，也许听起来有点讽刺：它是一种让指引我们要注重实用性而不是理想主义的理想境界。

在Node里面，人们之间分享和交互基本的构建的单元不是命令行中的二进制数据，而是通过`require`加载进来的模块。文本流*是*通用的接口，在Node.js里面其实就是JavaScript流对象，而不是标准输入输出中的管道。（标准输入输出的管道当然也在JavaScript流对象中体现出来了，因为它是我们通用的接口，我们还有其他选择吗？）

所以就从JavaScript的角度来说，我会说一下我是怎么表述Unix哲学的。哎～可惜我不是McIlroy，我也没有时间和能力去把它写得更精简，大家就将就一下吧：

> 写只做好一件事的模块。写新的模块而不是增加旧的模块的复杂性。

> 写鼓励组合而不是鼓励扩展的模块。

> 写能够处理数据流的模块，因为它是通用的接口。

> 写对数据来源和去向都无知的模块。

> 写某块来解决你知道的问题，那么你就可以知道哪些问题你是不知道的。

> 写小的模块。迅速地迭代。无情地重构。勇敢地重写。

> 迅速地写模块来满足你的需求，写几个测试来合乎规范。避免臃肿的文档。为你fix掉的每个bug写测试。

> 能工作优于完美

> 功能专注优于功能丰富

> 兼容性优于纯粹性

> 简单优于任何东西

Unix哲学是一种实用主义意识形态。是关于如何在写*好的软件*和写*任何软件*这两种需求中取得平衡。它是一套实用的建议，牺牲开发成本中的稳健性来获取更低的维护成本。

在现实世界当中，作为人类，我们在编写和调试程序的时候面临这一种相当不公平的约束，编码和调试的成本永远不可能降低到0。这种观念是有情景的，并且可以应用到所有的层次当中。我们都承认，我们没有聪明到写一次就知道如何把我们要写的软件写对，因为只有当我们把问题解决的时候我们才能完全理解问题。

**不是所有规则都是神圣不可侵犯的！**事实上，在很多情景下面，这些规则都是有争议甚至有时候是完全相反的。即便这样，如果我们把让我们程序的单元保持精小，加以简单通用的接口，我们就可以把所有零散的部件组合成一个高品质的齿轮，那么在我们滚动的时候，可以轻松自在地把笨拙的部分换出去。

没有线索清晰地表明Unix哲学和软件分享文化有什么关系。但是，它无疑是来自于我们一直在其中讨论如何让我们的软件更加自由的社区。根据这些原则来开发的软件，会更加容易地被分享，重用，重改和维护。

（完）