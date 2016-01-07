---
layout: post
title:  "Google Tag Manager 入门指南"
date:   2015-06-04 16:52:07
categories: Marketing
tags: Google, GoogleTagManager
image: /images/gtm.png
---

咱们 OneAPM 的监控，使用了百度统计、Google Analytics，然后百度推广也能看到一些数据。运营部门监控了线下的数据。

够了吗？为什么突然要使用 Google Tag Manager？

## 为什么要使用 Google Tag Manager

其实很多人都提过以下的需求：

*   你知道网站中每个视频都多少人点过吗？他们快进了嘛？关闭了吗？
*   能不能告诉我，用户都是从哪些页面跳转到注册的？
*   有多少用户在注册时，到第二步的时候，跳出了？

也就是说，线上的监控和线下的监控以外，还有一块是空白的：用户行为监控。

用户行为监控，除开官网、帮助中心以外，还可以深入到产品细节里去：

*   页面改版了，用户能不能找到导航的位置？
*   应用列表突然换成了拓扑图，用户回到应用列表的几率是多大？
*   上了多用户管理，那么多少用户尝试邀请了其他同事，加入到团队里来？

这些 Google Analytics 都可以实现。那么 Google Analytics 是如何实现的呢？

**以 Web App 为例，并以最新的 Universal Analytics 为基础。**

*   analytics.js 相当于 OneAPM Agent，是处理事件的捕获、处理和传输的。
*   元数据由前端工程师，手动加入。格式如下：

|Value|Type|Required|Description|
|---|
|Category|String|Yes|Typically the object that was interacted with (e.g. button)|
|Action|String|Yes|The type of interaction (e.g. click)|
|Label|String|No|Useful for categorizing events (e.g. nav buttons)|
|Value|Number|No|Values must be non-negative. Useful to pass counts (e.g. 4 times)|

该部分就相当于 OneAPM 的插桩技术。举例，如果你要给一个 button 添加一个事件监听器，并传输给 analytics.js。

具体详情，请查看：[Google Developers - Universal Analytics Web Tracking (analytics.js)](https://developers.google.com/analytics/devguides/collection/analyticsjs/events)

那么问题来了！这样一个事件、一个事件地加，岂不是要累死前端工程师。而且沟通成本也很大：

*   工程师要了解的整个的监控思路
*   营销活动可能在变
*   网站内容可能因内容、设计的改动，也在变

这就是为什么要用 Google Tag Manager 的原因！

> 就是为了利用现有的 Class, ID, DOM 等等变量，建立触发器；在无需改动网页代码的情况下，通过建立触发器，触发事件并监控。

> 最终目的就是，最小化程序员的工作量，把一部分工作量转移到 Digtal Marketing 人员的身上。然后，灵活性也比较强。

**说得高级一点，就是：解耦。**

## 入门

基本概念，我就不赘述了，[Google Tag Manager](https://support.google.com/tagmanager#topic=3441530) 文档和网上一些教程里都有。简单来说，你得了解：

*   代码：建立 Tag 跟踪事件，或者跟踪网页浏览，以及高级的电子商务类的东东（没用过）。
*   触发器：取代传统的手动触发，触发器就是可以通过设定各种变量，与表达式之间的关系，来判断是否触发 Tag 中的事件、浏览等动作，并传递至 analytics.js。
*   变量：变量就是 HTML 中的 Class Name, ID Name, DOM Name, Click Text 等等。当然您也可以自定义。
*   数据层：变量有很多，但是用户的每次行动，产生的消息到底传递了什么数据给数据层，数据层又接收到什么数据了呢？调 Bug 的时候，这块还挺重要的。

这 4 个概念。可以前往：[Goggle Tag Manager 帮助 - [v2] 代码、触发器、变量和数据层](https://support.google.com/tagmanager/answer/6103657?hl=zh-Hans&ref_topic=3441530)，进行了解。

## 实践

好吧，我们开始实践吧。第一步都省略，我们就假定你创建好了账号，并且把 Tag Manager 的代码传到了线上。

### 注意 1：ID 问题

需要注意的是 Google Analytics 的跟踪 ID，是以 UA 开头的一串字符串，其中包含了数字和中划线。

而 Google Tag Manager 则用 GTM-XXXXXX 代替了。每次创建代码时，你都需要输入 Google Analytics 的 UA 串，也就是跟踪 ID。但是部署 Google Tag Manager 到你的 Web App 中时，授权则用的是 GTM- 串。

### 注意 2：部署前请先创建一个代码，发布后才能部署成功。

哈，这个问题很简单，[stackoverflow 里也有人遇到这个问题](http://stackoverflow.com/questions/29243170/404-error-for-google-tag-manager)。

在 GET [http://www.googletagmanager.com/gtm.js?id=GTM-XXXXXX](http://www.googletagmanager.com/gtm.js?id=GTM-XXXXXX) 时，404了。

赶紧随便创建个什么代码吧，例如监控所有的页面浏览，然后发布出去，就不会 404 了。

### 注意 3：您可能会用到的工具。

1.  [Page Analytics (by Google)](https://chrome.google.com/webstore/detail/page-analytics-by-google/fnbdnhhicmebfgdgglcdacdapkcihcoh?hl=zh-CN)
2.  [Google Analytics Debugger](https://chrome.google.com/webstore/detail/google-analytics-debugger/jnkmfdileelhofjcijamephohjechhna?hl=zh-CN)

Google Analytics Debugger 可以查看到数据层的数据，并且可以看到每个变量的值，以及每次的操作触发的变量值的改变。以及你所预发布的代码，是否可行、有效。

[OneAPM.com](http://oneapm.com/) 有 OneAPM 宣传片，我们想看看用户到底有多少播放了这个视频。

![](http://ww3.sinaimg.cn/mw690/576e84bajw1eznmxkfs8kj20ym0rek1o.jpg)

首先，创意一个代码，选择 Google Analytics，如果你的 Universal Analytics 就选这个。填入上文说的 UA 串后，选择事件。

![](http://ww2.sinaimg.cn/mw690/576e84bajw1eznmz5xxinj20qv0plq50.jpg)

刚入文的第一个困惑来了，这个时间类别、操作、标签和值是什么？怎么还可以添加变量进去？

如果你使用过 Google Analytics 很好想通。

![](http://ww2.sinaimg.cn/mw690/576e84bajw1eznmz5cbr7j20kp0ckwgb.jpg)

这块对应的就是 Google Analytics 的事件类型、操作、标签的。这块有什么用呢？

*   一个视频除开播放，还有暂停、快进、快退、关闭，这些操作你可能都需要监控
*   进入登录页，是一个操作，那么要监控登录的来源页，那么这些来源页就是标签

说白了，这块就是做信息组织和架构的。如果你要监控的东西特别多，那肯定需要对整个监控的东西进行整理、组织。这块就是起到这个作用。

那么，为什么可以加入参数呢？有时候变量里面就可以包括播放、暂停、快进这样的值，那么你就可以使用变量，来作为通配符了。是为了节省代码的创建数量，方便你自己。

紧着往下看，会看到「更多设置」和「高级设置」。这块，我暂时也不会，请直接跳过吧。或者自行学习、搜索。

最后是设定触发条件，由于观看视频是个点击时间，所以选择「点击」。

如果你没有创建过「触发器」，那么新建一个「触发器」。

![](http://ww2.sinaimg.cn/mw690/576e84bajw1eznmz4ye97j20pz06fjs7.jpg)

直到触发器中的判断条件，都符合点击视频这个动作后，保存这段代码就可以了。

## 如何撰写判断条件

1.  打开 [Google Analytics Debugger](https://chrome.google.com/webstore/detail/google-analytics-debugger/jnkmfdileelhofjcijamephohjechhna?hl=zh-CN)
2.  重现一次你想监控的事件，你会得到以下视图
![](http://ww1.sinaimg.cn/mw690/576e84bajw1eznmz4o85rj20m80chtbs.jpg)
3.  观察数据层的变量的值，决定哪些可以作为判定条件
4.  如果你要设定的事件，涉及多个网页，并且变量的值不统一，那进行思考，决定可以做哪些变化
5.  多创建几个代码，或者通过事件类型、标签、操作加入通配符，来灵活处理

好了，基本就是这样了，更多的技巧还有待探索和学习。共同进步！

## 现状

目前使用 Tag Manager 监控的事情包括：

*   进入注册
*   完成第一个表单，并点击下一步
*   完成第二个表单，并点击下一步
*   点击「重新发送邮件」按钮
*   点击激活链接，进入登录（此部分待验证，是否监控成功）

## 未来

觉得 Google Tag Manager 就止步于此了吗？Too Naive! 未来我们可以利用 Google Tag Manager 来监控很多东西：

*   多用户需求上线以后，到底有没有人邀请其他用户加入团队
*   页面改版，提供皮肤选择后，用户最喜欢哪一个皮肤
*   提供深色背景和白色背景，让用户做选择，进行 A/B 测试

以及所有与用户行为相关的事情。但是使用 Google Tag Manager 存在以下几个瓶颈：

*   如果 id class 分类得不好，有些事情无法监控，需要程序员配合
*   如果产品频繁更新，那监控的预备操作的工作量大
*   可能会对性能造成影响
*   可能会被墙掉

总之，先用起来吧。
