---
layout: post
title:  "产品设计，真的不是开玩笑。"
date:   2015-04-23 21:52:07
categories: Design
tags: axure MaterialDesign Behance
image: /images/material.png
---

## 玩笑话说太多

事情还要说在国贸的一次会面说起。北航的好朋友，让我去跟他们北京东区的人会面，谈一谈他们最近要做的 App。

于是，就来到了个张口闭口 O2O、范冰冰、10 亿投入的咖啡馆。聊了两个应用：

*   针对 APEC 青年组学生代表的实习 App
*   以美容院为出发点，最后整合整个线下支付的 [Square](https://squareup.com/global/en/register) 类似品

任何一个拿出来，就是秒杀 BAT 的大项目啊。最后谈到产品设计上，偷偷摸摸地问我，做一个可以运行的高保真原型 App，需要多少钱。

要可流畅运行，先有设计吧，然后拿 [Quartz Composer](https://developer.apple.com/library/mac/documentation/GraphicsImaging/Conceptual/QuartzComposerUserGuide/qc_intro/qc_intro.html) 做个交互。这还不是流畅的，流畅就写个原生 App。那事情也就大了。

最后事情也是不了了之。

事情还同样发生在公司。刚转来做产品的研发工程师，和一些产品应届生，在和设计沟通时，根本拿不出来可读的线框图。

给大家看几个例子吧：

![](http://ww1.sinaimg.cn/large/005Ntf0Hgw1erfvjm3ab9j30p20h0ac7.jpg)

自定义分析功能设计

如果你用过 [Google Analytics](http://www.google.com/analytics/)，或者同类产品，可能会知道他要做什么：

*   选择性能指标；
*   可以在指标数据上，对维度进行选择；
*   可以在指标数据上，对属性进行筛选。

![](http://ww1.sinaimg.cn/large/005Ntf0Hgw1erfvjrlmakj30fk0kcq51.jpg)

*部分优化后的自定义分析功能*

去除掉维度，也就是 `group_by` 这种难以理解的概念，外包成：查看方式。

参考 Google Analytics 的筛选功能，和前端工程师确定复合式搜索框的实现难度，搜索一下是否存在现成的 js 控件。

并且将不同类别的操作，都往：

*   指标选择
*   查看方式选择
*   筛选条件添加和删除

这 3 个交互上靠，其实就是可以规整出一个虽然不完美，但是设计师至少能懂的线框图了。

其实这个产品的工作还是挺好的，有详细的文档，有良好的沟通方式，有 UML 等等等等。关键是，最后也拿着我的原型，将自己的原型改成了这样的样子。并且将需求不断细分，最后细分为一周就可以完成的需求。

所以耳朵软的人，就是可爱。

有时候，线框画得好，真的是降低了很多沟通成本。同事说过一句话，还是很在理的：

> 你少做一些工作，其实就是把这份工作转移给，对这个事情比你不清楚的人。其实对方所要做的工作，会是你的好几倍。

## 产品设计，除了线框的其他事

有的时候有了线框真的不够！

从最近改我们的登录和注册说起吧。开始以为我们线上的登录和注册，完美无瑕，除了设计。

![](http://ww1.sinaimg.cn/large/005Ntf0Hgw1erfvjmyci4j30u00majtf.jpg)

后来，做了高保真的设计，完全是 1:1 的高保真，要改之后，才发现一大堆问题。高保真，使用代码实现的高保真。CSS 样式全写好了。

*   前后端验证的正则表达式不一致
*   mobile 端注册存在着产品设计问题
*   邮箱验证和手机验证冲突的问题
*   莫名其妙的 404
*   等等等让人崩溃的问题

所以，走测试的时候，才发现在给前人擦屁股。而且，此人有肛瘘。

*   竟然不是 Ajax，是前端验证一遍，后端按照前端的验证再走一遍
*   开发觉得到第 2 步发现第 1 步验证没通过，需要跳转至第 1 步
*   .......

好吧，那我们从头再来吧。最后把前人埋了。

## 那些需要想全了的流程

再来看看，最近要做的一个多用户的需求。潇洒的产品，只给了一张线框图，和一些解释性文字。

![](http://ww1.sinaimg.cn/mw690/005Ntf0Hgw1erfvjksphcj30uh0kwn0i.jpg)

勉强还能看懂吧。那 admin 如何邀请 user 呢？user 怎么注册呢？user 视图和 admin 一样吗？ 那个过滤框是拿来干嘛的？还有手机？手机需要验证吗？

哇哇哇，这些后续流程沟通后，发现对方完全没有考虑。就一个多用户的需求，其实对应的线框，起码有 6 - 12 个。简单举例：

![](http://ww2.sinaimg.cn/mw690/005Ntf0Hgw1erfvjoiom5j30qf0gjade.jpg)

*admin 视图下的团队成员查看*

![](http://ww1.sinaimg.cn/mw690/005Ntf0Hgw1erfvjnz35ij30qa0eyq5c.jpg)

*交付权限和删除用户交互*

![](http://ww4.sinaimg.cn/mw690/005Ntf0Hgw1erfvjnag69j30q50ghtc5.jpg)

*收到邀请邮件后的注册流程*

有时候，需求只是简单三个字，就像我爱你。其实背后承担的责任，和琐碎的细节，是爱情这个虚妄外表下，最繁复的事情。

## 画个好线框

今天，不谈产品设计，这么大的概念。工期、技术难度、投资回报率，这些恼人的细节问题，都制约着产品设计。所以，我们只谈如何画好一个线框图，该怎么办？

诚心推荐：

1.  一定要看的 [Material Design](http://www.google.com/design/spec/material-design/introduction.html)
2.  一定要会的 [Axure](http://www.axure.com/)
3.  一定要逛的 [Behance](https://www.behance.net/)
4.  一定要试的 Widgets
5.  一定要追的 前沿动态

### Material Design

虽然我们公司的设计师们，很不看好 Material Design，但是每个做产品的，至少在 iOS 人机交互指南 和 Material Design 上，要通读过一样。

并且视图去理解他。这样至少在以后画线框的时候，至少 8px 是个基本单位，至少会开始强迫自己使用辅助线来规整自己的原型图。

以及理解所有的过场动画，在使用 Axure 的时候，知道他们的过场的不自然。去检索相关动画的控件，大脑里有曲线的概念。

知道所有的卡片，或者说控件，是需要在 z 轴上有着上下级关系。从哪来到哪去的原则，会根植在心中。

### Axure

这是个体力活，没什么好说的。就像计算能力、空间思维能力，都是做数学题，不停思考，锻炼出的脑部肌肉。

尝试用 Axure 实现一个简单的 HighCharts 曲线图，看看 Axure 的瓶颈在哪里。变量都不会用，你以为 Axure 是 Windows 的画图工具呢？

### Behance & Widgets

无聊就逛逛 Behance，我关注的一个[俄国设计师](https://www.behance.net/nazir)好像还不错的样子。

以为逛完，就完了吗？[Axure Widgets](http://www.axure.com/community/widget-libraries) 有很多免费 Widgets，可以在网上免费下载到，例如烂大街的 [Bootstrap 3](http://getbootstrap.com/)；以及一些 Web Font: [FontAwesome](http://fortawesome.github.io/Font-Awesome/)。有没想过自己，也做一个 Widgets?

我闲来无事，也按照 Behance 上的设计，自己做了一些 Widgets，方便自己使用。如果 OneAPM UI 确定以后，我也打算做一个 OneAPM Widgets，供同事使用。

![](http://ww4.sinaimg.cn/mw690/005Ntf0Hgw1erfwgs63myj30zv0lster.jpg)

### 前沿

[Sketch](http://bohemiancoding.com/sketch/) 试过没有？Facebook 出的 [Origami](https://facebook.github.io/origami/)，试过没有？[iOS 8 人机交互指南](https://developer.apple.com/library/ios/documentation/UserExperience/Conceptual/MobileHIG/)，看过了吗？[Windows 10](http://www.digitalartsonline.co.uk/news/interactive-design/windows-10-ui-design-new-os-looks-10-times-better-than-windows-8/) 的设计，有什么可取之处？

打开视野，毕竟上网没那么贵了。

---

1. [Quartz Composer 如何入门？](http://www.zhihu.com/question/20956344)
2. [Material Design Layout - Metrics & Keylines](http://www.google.com/design/spec/layout/metrics-keylines.html#metrics-keylines-baseline-grids)
3. [Material Design Animation - Authentic Motion](http://www.google.com/design/spec/animation/authentic-motion.html#authentic-motion-mass-weight)
4. [What is material? - Objects in 3D Space](http://www.google.com/design/spec/what-is-material/objects-in-3d-space.html#)
5. [Free Axure Widgets Library for Twitter Bootstrap 3.0](http://axutopia.com/twitter-bootstrap/)
6. [如何在 Axure 中使用 FontAwesome](http://www.axure.com/c/forum/tips-tricks-examples/8732-font-awesome-widget-library-v7-icon-fonts.html)
