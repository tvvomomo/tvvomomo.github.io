---
layout: post
title:  "当我们在谈论 DevOps，我们在谈论什么？"
date:   2016-01-14 13:02:37
categories: Marketing
tags: devops cloudinsight chatops
image:
---

走过 C 轮的 OneAPM，旗下的产品已经日渐丰满，从应用性能监控的 Application Insight 到系统监控工具 Cloud Insight，再到安全产品 OneRASP，以及日志分析工具 .LogInsight。而今，我们一直困顿于一个问题：**监控帮助人们更快地发现了问题，而解决问题的落点在哪？**。

机器还没有智能到可以帮助我们修复代码中的问题，没有办法解决一个页面的 SQL 时长长达 5000 ms 的问题，也没有办法权衡计算资源的流向。

当我们深入 DevOps 这个方法论，以及思考 Slack 的流行，我们发现「**协作**」和「**沟通**」，也许是帮助我们更高效地解决问题的解决之道。

## DevOps 和 ChatOps

DevOps 这个一再被人们提及的概念，在实际操作中，这个方法论其背后的实际操作方法，和项目管理的手段是什么样的，甚少有人可以给出一个完美的答案。

DevOps 在最初，是让开发、运维、QA 之间加强沟通，通过不同的工具来消除隔阂。而隔阂是因为所秉持的价值观、方法论的不同，而造成的隔阂；说得实际一点，就是因为 KPI 这座大山造成了不同部门之间的对责任的逃避，而 DevOps 是倡导大家一起来面对问题、解决问题。

开发环境和部署环境的快速迁移，帮助产品快速上线；越来越多的监控工具，明确负载问题是计算资源不足的问题，还是代码质量的问题。

我们来看 [stackshare](http://stackshare.io/) 排名前 10 名的 DevOps 工具：

![](/images/stackshare.png)

如今的 DevOps 工具大致可以分为两类：

* 更快的 Build Test Deploy
* 更好的 Monitoring

但是大家似乎忽略了一个方向，就是上文提到的：「**协作**」和「**沟通**」。

Slack 的流行让这个方向又重获生机：ChatOps。

![](/images/chatops@github.jpg)

ChatOps 是诞生于 GitHub 的一种基于会话驱动的协作开发方法，过去团队之间的通讯和开发操作是两层皮，导致各种不透明和低效率。ChatOps 将开发工具带入开发者聊天室，通过定制的插件和脚本，一个聊天机器人能够执行聊天中输入的各种命令，实现在聊天平台上的团队协作开发自动化，把团队沟通和执行统一整合到一个可视化更高的聊天环境中，“聊着天就把事情办了”。

![](http://www.wired.com/wp-content/uploads/2015/10/ubot-schem-1024x426.jpg)

我们来看下 [WIRED](http://www.wired.com/2015/10/the-most-important-startups-hardest-worker-isnt-a-person/) 杂志对 GitHub 系统主管 Sam Lambert 的采访：

> 当你走进 GitHub 的大厅，在前台的 iPad 上登录后，所有计划与你会面的人都会收到一份通知。GitHub 的系统主管 Sam Lambert 对 Wired 网站说，Hubot 是公司工作最努力的员工。这是公司内部的一个玩笑。其实，Hubot 是嵌入到 Github 聊天系统里的软件，或者说，它是个聊天机器人。

> 通过向 Hubot 发送信息，工程师们可以升级服务器上的系统，删除数据库中的数据，甚至让全部的服务器下线。在公司外部，Hubot 被称作是 ChatOps 工具。就是说，它能够处理运营任务，比如设置新服务器和数据库，或者升级 GitHub 网站背后的代码。ChatOps 是 Github 自造的单词，不过，这种想法来源于软件界的 DevOps 运动。通过一些新型的软件，工程师们可以让公司内部的大量硬件和软件实现自动化设置和升级。ChatOps 添加了对话元素。“GitHub 网站每天的升级都是通过聊天机器人完成的。” Lambert 说。

ChatOps 这项 GitHub 自创的运动，的的确确为 DevOps 中「加强沟通」带来了实质性的进展。

而硅谷的 Slack 这块团队协作的明星产品的流行，说明 ChatOps 正在往产品化的方向上行进。

## 协作工具

通过会话来加强沟通，从而更高效地解决问题。在国内，就要说说 Bearychat、瀑布 IM、简聊这三款 ChatOps 产品了。

BearyChat 的出现是因为 团队工具庞杂，每天产生大量信息，这些信息散落在各种服务里，其中重要信息很可能会被忽略 。所以一个汇集信息、提升工作效率的工具成为一种刚需。

当然，除了界面，产品本身的功能是最重要的，BearyChat 在团队讨论上除了基本的文字沟通，还接入了各种第三方服务，比如图片和视频直接显示，网页链接直接抓内容，以及展示 Github 代码片段、Trello 列表等功能，尽可能将影响团队协作流程的操作简化（如点击、跳转等等）。

除了整合梳理信息的功能，BearyChat 还有一个类似于 Workflow 的机制，他们称之为机器人。在小编看来，Workflow 是提升效率的关键功能，Mac 内有自建的 Automator，网络上有著名的 IFTTT。从 *If this then that* 的逻辑上延伸，BearyChat 的机器人应该是内容的搬运工，比如项目代码一旦更新会自动发送到讨论组这样的功能。

瀑布 IM 是一款团队协作沟通工具。公司可以通过瀑布 IM 的频道和分组功能将公司部门分组原封不动的搬到瀑布 IM 上，使用门槛比较低。 其显著的特点在于它整合了很多第三方工具，包括 Tower、Asana、Worktable、Teambition 和 Evernote 等 40 多款工具，帮助用户在这一个地方就能完成所有的沟通，不需要在各种IM工具中往返切换，提高了团队沟通的效率。此外，用户还可以在瀑布IM里共享文件、置顶频道、添加链接等。

![](/images/3rd.png)

作为 Teambition 旗下的即时聊天应用，简聊通过话题创建不同的频道，以提高即时聊天的效率。团队协作产品意在消除不同项目中团队成员沟通的阻碍，保证工作的协调和同步。但团队协作工具的 Timeline 和项目讨论可能出现由于半即时性带来沟通效率问题。此时，如果团队在协作工具外令选用即时通讯工具，就不可避免地会导致内容割裂、无法统一管理等问题。简聊是 Teambition 团队对这个问题给出的答案：团队协作 + 即时沟通。

简聊的主要功能包含分话题聊天，支持传送文件，数据保存于云端，可以对每一个话题设置成员，是否公开，还能通过电子邮件参与讨论。当一个话题完结后，存档话题。

## 协作工具的未来

**康威定律说：设计系统的组织，最终产生的设计等同于组织之内、之间的沟通结构。**

如果一款团队协作工具，做到了通用，却没有根据组织内部的沟通方式、工作方式来做配合，最终沟通和协作的效率会大打折扣。

好在简聊可以通过第三方工具来自定义机器人。那如何更深入地与其他产品合作呢？

![](/images/custom.png)

如果我们对工作中的人群进行划向：设计师、研发人员、项目管理人员等等，而这些人所需要的工具是不一样的。

设计师需要 Adobe 的 Creative Cloud 来随时随地访问他们的文件，需要 Behance 和 Dribble 的推送来扩展视野。研发人员以 Cloud Insight 研发组为例，需要 GitLab 的变更管理，需要 Docker 的管理工具，需要基于 Bara 的测试环境部署的变更管理等等。

而不同工种的关注点，和沟通方式也是不一样的，并且围绕的话题、和传输的文件的特征也是不一样的。

单就运维这个工种来说，他们需要关注的点包括：

* 每一项指标的历史曲线图
* 报警产生的事件
* 出现异常点时的解决方案
* 高级工程师和初级工程师之间的沟通
* 对基础设施的控制

也就是说，如果一个团队协作工具不能够针对某个特定工种，根据他们的关注点做适配，最终沦为一个通用级的工具，是没有未来的。这也是加速了像 Slack、简聊这样的团队协作工具，和其他第三方工具合作的原因。

而今，Cloud Insight 和简聊的合作，就是为了打造一个适合运维工程师，协作与沟通的工具。

Cloud Insight 是一款系统监控工具，能够监控操作系统、数据库、中间件的运行情况，并根据指标产生报警。也可以通过 SDK 上传任何指标，来做集中的展现和管理，包括：业务指标、应用性能监控指标等等。

![](/images/infra.png)

因此，参与到 Cloud Insight 这个工具的人员，不止运维人员还包括研发人员、管理人员，甚至运营人员。而团队协作也是 Cloud Insight 不可或缺的功能。

![](/images/events.png)

## 结语

性能监控的意义在于让运维变得高效、智能。团队沟通、协作的根本目的也在于通过一切方式提高效率。DevOps 是倡导大家一起来面对问题、解决问题，通过 Cloud Insight 及时发现性能问题，这时候再因为简聊的快捷和易用让解决问题的过程不那么痛苦。有着同样的情怀和期许，Cloud Insight 和团队协作工具的合作势必成为一件 `1 + 1 > 2` 的事。

---

##### 参考文章

The Most Important Startup's Hardest Worker Isn't a Person

* 英文稿：[The Most Important Startup's Hardest Worker Isn't a Person](http://www.wired.com/2015/10/the-most-important-startups-hardest-worker-isnt-a-person/)
* 中文稿：[GitHub 工作最努力的员工是一个聊天机器人](http://www.ifanr.com/576121)

[DevOps 年中盘点：国外最受欢迎的 10 篇技术文章（下）](http://m.csdn.net/article/2015-08-14/2825457)

[IT 人士耻于下问但又不可不知的“聊天运营” —— ChatOps](http://www.ctocio.com/ccnews/17686.html)
