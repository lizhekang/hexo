title: 啤酒尿布大数据
date: 2016-02-03 11:20:56
categories:
- 计算机
tags:
- 大数据
- 数据挖掘
- 机器学习
---

## 啤酒尿布大数据

{% asset_img 68671433926620704.png 啤酒与尿布 %}

### 啤酒与尿布

“啤酒与尿布”是一个非常经典的故事，无论是在营销界还是数据挖掘相关领域。

“啤酒”与“尿布”，两种看似毫无关联性的商品（如果你脑洞大，可能会想到喝酒失禁这样的场面或者小孩不喝奶喝啤酒）到底会有什么故事呢。如果我现在说周五只要将啤酒和尿布放到同一个区域，销售量会提升30个百分点（当然这个案例比较适合美国市场），你肯定惊讶：喔~为什么会这样？谁会想到这样的好点子？

某个工程师在整理结算数据的时候，发现每到周五晚上，尿布和啤酒的销量有正相关性，真多亏了零售业全面推行电子化，不然谁会想到尿布和啤酒居然有相关性！

后来通过调查才发现，原来美国周五晚上，很多父亲都会到超市买周末看球赛（大联盟）需要和的啤酒，通常这个时候都会帮家里带些尿布。

不管这故事是不是真的，反正我已经听过好几个版本了无论在市场营销的课堂上还是在大数据的课程里，但是这就是“数据挖掘”在做的和“数据挖掘”能给我们带来的。

### 大数据其实并不神秘

“喔~大数据耶~”经常听到有人激动高呼，就像近几年很火的“H5”，人们都觉得很炫酷很高深，但“她们”都绝非空中楼阁，或者我们很早就跟“她们”打交道了。

也许当了解到什么是[数据](https://zh.wikipedia.org/wiki/%E6%95%B0%E6%8D%AE)，其实大数据离我们已经不远了。就像上面提到的“啤酒与尿布”的故事，每个顾客消费的记录、商品的信息都是那位工程师能拿到的数据，然后他通过统计学等手段“挖掘”到数据中潜在的关联性。也许数据还不够“大”（鉴于当时还是九十年代），他还能通过比较原始的方法发现数据的关联性。让我们看看维基百科上对[大数据](https://zh.wikipedia.org/wiki/%E5%A4%A7%E6%95%B8%E6%93%9A)的定义：所涉及的数据量规模巨大到无法通过人工，在合理时间内达到截取、管理、处理、并整理成为人类所能解读的形式的信息。

这就是大数据。

其实大数据并不神秘，只是数据大到我们不能用常规方式或者旧的手段在有效的时间里面获取有用的信息罢了。

我能说，就一架战斗机完成喷漆，油漆点的数据量都能达到PB（1PB = 1024TB）级么，只是我们现在还不关心那些油漆罢了。

### What it means

我想说：It means everything.

其实简单点，我们现在可以通过[天池](https://tianchi.aliyun.com/)等大数据比赛了解企业或者政府都想通过大数据解决一些什么问题，当然没有公开的研究数不胜数。

在大数据方面的尝试，我一直感觉广东省政府还是走在前列的。在天池赛题区可以看到两道交通相关的题：“市民出行公交线路选乘预测”和“公交线路客流预测”。假如给出一个确定的方法（算法）能够比较准确预测，What it means？这就意味着能够我们能解决交通线路客流不均衡和出行拥堵等问题。喔~大数据就是那么帅。

当然，还有一些企业用大数据给用户推荐商品，好好回味一下亚马逊给用户推荐是如此地舒服贴心和淘宝推荐的S*；喔~当然还有FB的好友推荐算法，简直精准到没朋友。

What else?

我们甚至可以利用大数据察觉商业趋势、判定研究质量、避免疾病扩散、打击犯罪……说到打击犯罪，这部美剧《Person of Interest》完美讲述了What it means。看看剧中那台计算机都在做什么，而且我相信类似的技术会在十年内实现，或者已经实现只是我们并不知道而已。不过根据不确定性原理（uncertainty principle）我相信机器预测的结果不可能这么准确，可以忽略这句话。

### How it work

大数据的无限的应用场景注定必然需要跨学科、跨领域的支持，例如：需要做金融预测，必然需要金融领域和计算机领域的人协同完成。但简单地说，数据挖掘的过程通常都是：首先根据一些先验知识建立一个模型，然后根据模型编写“一段”程序，让计算机“自己”去挖掘数据中有用的信息。

在这个过程中就会有很多相关的领域，简单地我们可以分为海量数据应用和系统建设技术。

神经网络、贝叶斯、决策树等这些我们称之为算法的东东其实就可以分类到海量数据应用的领域。在这个领域，计算机科学家想的是如何在现有的硬件处理能力下提高数据挖掘的效率和准确度。我自己也尝试过用神经网络预测Stack Overflow网站的最佳评论（在[Presentation](http://presentations.lk-life.com/nnsof/)里有我在解决这个问题的方法和思路）。

而我们经常听到的分布式计算、云计算之类的名词其实就是系统建设技术的领域。由于大数据的数据的确有点大，一个不小心就是PB级别的，所以就会涉及到如何存储、如何查找海量数据的问题，这些更像是基础设施建设，服务于海量数据应用的领域，保证数据科学家可以高效利用数据。

### Btw

鄙人不是相关领域的专家，只是有所耳闻、有所涉猎而已，本文也不是什么正经的科普文，只是畅谈一些自己的见解罢了，如果各路大牛发现纰漏，有望多多指教。