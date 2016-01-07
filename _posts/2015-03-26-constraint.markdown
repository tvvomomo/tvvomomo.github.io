---
layout: post
title:  "从 1:1 想到 Constraint "
date:   2015-03-26 12:52:07
categories: Design
tags: WebsiteDesign
image:
---

公司搬了新家，离园区的西门特别近。中午跟同事准备去吃麦当劳，顺便路过黄焖鸡米饭，决定进去试试鲜。

点餐才发现没有菜单，因为提供的只有黄焖鸡米饭一种菜品。

同样的事情也发生在一次诡异的交谈上。一个学英美文学的室友，想知道我是做什么产品的，于是我就打开了 OneAPM 的后台。结果是意料之中的，他的表情从皱起的眉头，变换为更近紧凑的眉头。然后深呼一口气说：这都什么跟什么啊？

于是我就开始解释，总览页面中响应时间曲线、Apdex 曲线、吞吐量曲线、最慢的 5 个 traces，以及巴阿巴拉一些他似乎不太在乎的事情。

实在耐不住性子的他说：你知道有个东西叫 360 管家吗？一键查杀，你总知道吧？你们的东西可以直接告诉我，到底性能怎样了，不就好了吗？

下意识地，我的攻击机制被唤醒，开始长篇大论地论证：单一数值，是无法概括一个应用的性能的。

夜深，思量一下，其实挺有道理的。Google 中有人说过：

> [CREATIVITY LOVES CONSTRAINTS](http://www.fastcompany.com/702926/marissa-mayers-9-principles-innovation)

> This is one of my favorites. People think of creativity as this sort of unbridled thing, but engineers thrive on constraints. They love to think their way out of that little box: 'We know you said it was impossible, but we're going to do this, this, and that to get us there.

如果我们将用户对于性能的查看，如同黄焖鸡米饭一样，限制在一个数值。简单直观地告诉用户，你是否需要对性能做进一步地排查，是否能够降低用户使用门槛呢？

姑且，我们叫这个值为：`Useable`。0 是不可用，1 是可用。

每次在用户登录的时候，都必须查看这个数值，然后选择是否进入主页面，进行性能排查。

那么问题来了！

1.  Useable 的算法怎么来？
2.  每次强制观看这个数值，是否会引起用户反感？
3.  这种反感是否可以通过设计来规避？
4.  Useable 和其他性能指标之间的关系是什么？
5.  自然流转到性能排查的场景是什么？
6.  这种方法的真正意义是什么？

在回答这些问题前，让我们先从电影里找答案。

## 1-1 的 Mommy

其实之上的问题可以转化为：

1.  技术实现
2.  效果评估
3.  艺术指导
4.  成果转化
5.  使用场景
6.  实际收益

当 [Xavier Dloan] 新作 [Mommy](http://www.imdb.com/title/tt3612616/) 以 1:1 的形式，亮相国内各大影迷的电脑屏幕时，就算是如此窄的视野，大家也被主角扯开屏幕的那一幕给震惊到了。

以技术实现来说的话，1:1 在 35mm 胶片上如何实现，我实在找不到答案。但是按照维基百科词条：[长宽比（视频）](http://zh.wikipedia.org/wiki/%E9%95%B7%E5%AF%AC%E6%AF%94_(%E5%BD%B1%E5%83%8F))来看，4:1 的纵横比是通过 3 张胶片并排播放实现的：

> 4:1：Polyvision，使用三道35毫米胶片并排同时放映。只使用于一部视频，Abel Gance的Napoléon（1927年）。

就姑且认定它很好实现吧。那效果评估呢？1:1 的效果到底好不好呢？把观众的视野，限制在如此小的范围，带来了什么呢？

<script charset="ISO-8859-1" src="//fast.wistia.com/assets/external/E-v1.js" async></script><div class="wistia_responsive_padding" style="padding:56.25% 0 0 0;position:relative;"><div class="wistia_responsive_wrapper" style="height:100%;left:0;position:absolute;top:0;width:100%;"><div class="wistia_embed wistia_async_80e0i0x01l videoFoam=true" style="height:100%;width:100%">&nbsp;</div></div></div>

导演直接通过物化的压抑，表达受限的人生，和不可磨合的冲突。当情绪得以缓解，加上扯开屏幕的契机，来强化观众的心理感受。

如果按照这种形式解读下去，那艺术指导、成功转化、使用场景和实际收益，也就水到渠成了。

## 回到产品设计

如果只有一个数值来度量应用性能，那什么才是扯开屏幕的那一瞬间？

先从使用场景着手吧。如果只是一个单一数值：`Useable`，那使用场景可多了：

*   登录首屏
*   报警
*   状态传递
*   触发条件

在登录首屏通过一个数值，传递应用的健康程度，就是朋友诉求的。如果只是单一数值，那通过这个关键数值来作为报警的触发条件，和传递这个报警，也是非常自然的。除开报警的触发，那它作为一个 API，与其他监控软件或者协作软件，做对接也非常自然；就连状态传递，在社交的场景下，也是非常自然的。

有了使用场景，那如果我们去做这个需求，那技术上是否可行呢？

关键是，在 Application Performance Management 领域，还真有这样的一个数值：[Apdex](http://en.wikipedia.org/wiki/Apdex)。技术是可行的，简短来说，Apdex 就是度量应用的性能和受影响用户之间的关系，越接近于 0，就说明性能越差。

那大家是否接受这样一个数值呢？也就是效果评估。一个需求、一个概念、一个生态，或者说一个产品，是需要配合推广的。效果好不好往往又反求诸己地，返回到产品本身。如果 Mommy 这个电影很失败，那 1:1 只会是哗众取宠的方式而已。

再说说艺术指导吧，在设计上当然是越简单越好啦，所有有关应用的设计，几乎都是这样的。[IFTTT](https://ifttt.com/) 将复杂的 Recipe，封装在一个简单的操作 [DO](https://ifttt.com/products) 上。苹果的 HOME 键。MUJI 的 [Sleep](http://sleep.muji.net/zh/) 应用。

一个数值可以对应一组选色区间，可以对应一个小人的兴奋程度，可以对应一个灯泡的亮度，可以对应透明度，等等等。创意无限多，设计范围也很广。

至于实际收益，那试试 A/B 测试吧。具体这块，我也不是很懂。试一下，是否会带来实际的收益吧。

## 限制是为了什么

限制是为了把事情简单化，把复杂封装在产品设计者身上，最后将简单交给用户。

这就是我所理解的限制。为啥 USB 都要插入两次，才能成功？为什么办理证件的部门，就不能简单明了？

其实限制是和 Affordance 合作的。

如果你只是简单粗暴地告诉别人，通过大量的提示信息告诉用户，这个按钮不能点，那块是这样的，在这个状态下输入框无效。这也是限制；及其专制的限制。

如果把限制比喻成男性生殖器，那限制的受体是什么？你可能要问，限制为什么要受体？

Mommy 最后还是在感情达到高潮，气氛舒展到极致时，扯开了屏幕。点亮 Home 键，开始滑动解锁键。所以，回到使用场景这个关键点来谈，限制是需要受体的。

没有受体的限制，是死亡。就像生命受限在，生与死的临界点。最好的例子，就是之前只能 [Yo](http://www.justyo.co/) 别人的 App。虽说前期大量的媒体报道，到后期与智能家居的结合；但是最后还是名存实亡。

`Useable` 只是为了将用户遗忘产品，或者对应用性能未知的情况下，触发用户往更深层次的信息追溯的导火索。用户受影响了？那是什么导致的呢？看看响应时间吧，做一下 profiling，看看堆栈信息吧。自然而然地触发用户去使用。就像 1:1 的纵横比，影响着观众情绪，直至被释放的那一刻。

---

交互 5 大原则

*   Visibility - Can I see it?
*   Feedback - What is it doing now?
*   Affordance - How do I use it?• [李如一 • 设计词典：Affordance](http://www.qdaily.cn/display/articles/570.html)
*   Mapping - Where am I and where can I go?
*   Constraint - Why can't I do that?
*   Consistency - Is this familiar?
