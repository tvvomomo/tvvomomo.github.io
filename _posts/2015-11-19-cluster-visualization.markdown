---
layout: post
title:  "集群管理与可视化：Cloud Insight 已经开启！"
date:   2015-11-19 17:09:07
categories: Marketing
tags: cloudinsight clustervisualization
image: /images/cluster9.gif
---

我们一直在说 Cloud Insight 是一个[数据管理平台](http://www.oneapm.com/ci/feature.html)，或者说是一个次世代的系统监控工具。可能您会感觉很困惑：到底 Cloud Insight 强大之处在哪里？

其实，Cloud Insight 最为适宜场景就是，集群系统管理和监控、复杂繁多的基础设施管理和监控。

在一个月前，[Cloud Insight 实现了数据聚合、分组和过滤](http://news.oneapm.com/query-metric/)，就是为了能够更高效地组合性能指标，发现问题。

其中使用到了标签，来组合数据。

## 什么是标签

那什么是标签呢？其实我们在生活和工作中，都遇到过标签，它并不陌生。

* 在操作系统中使用标签来组织文件
* 在微信中通过标签来组织联系人

![集群管理与可视化：Cloud Insight 已经开启！](/images/cluster1.png)

虽然标签的使用场景非常广泛，但是标签的作用都是一样的：**为了更好地管理信息**。

同样，在 Cloud Insight 中，我们通过标签来更好地管理信息。

在 [Docker](http://news.oneapm.com/tag/docker/) 中，我们通过标签来标识不同的 Image：

![集群管理与可视化：Cloud Insight 已经开启！](/images/cluster2.png)

这样就能通过不同的 Images 来查看性能指标：

![集群管理与可视化：Cloud Insight 已经开启！](/images/cluster3.png)

## 标签和平台

那么标签和基础设施（在 Cloud Insight 中被称作平台）有什么关系呢？

随着业务量的增长，基础设施的架构也越来越复杂，机器的数量也越来越多。而这些机器，有可能是托管在某个数据中心的一批机器，和阿里云上购买的主机的一个集合。

不同的主机负责不同的模块，如存储、Web 服务器等等。想要把这些机器放到同一个监控工具中，进行同一管理，难度很大。

幸好有 Cloud Insight 支持主流的机器类型和数据库、中间件，能够采集从不同数据源中采集数据。

**而以标签为核心的管理方式，带来了诸多便利。**

如：`host:WEB1` 这个标签，代表着一台部署在广州的机器，其内部运行着 PostgreSQL NGINX 和 Varnish。可以通过这个标签，迅速检查这台主机的性能。

![集群管理与可视化：Cloud Insight 已经开启！](/images/cluster4.png)

而查看所有 NGINX 的性能呢？怎么办？相信很多老旧的监控工具，都不存在这种的功能。而 Cloud Insight 可以做到。

通过 `integration:nginx` 这个标签，可以检索出分布在北京、上海、广州甚至是国外的所有 Web 服务器性能。

![集群管理与可视化：Cloud Insight 已经开启！](/images/cluster5.png)

想要了解某个地域的主机性能表现，来考虑是否需要为这个地域的主机续费？Cloud Insight 也可以帮助你，快速做出决策。

![集群管理与可视化：Cloud Insight 已经开启！](/images/cluster6.png)

在 [什么是 Cloud Insight?](http://news.oneapm.com/what-is-cloud-insight/) 这篇文章中，我们提到了过滤出某两台主机的 Swap 使用量的平均值：

![集群管理与可视化：Cloud Insight 已经开启！](/images/cluster7.png)

而标签的使用，可以让数据聚合、分组和过滤更加便捷，也让机器的管理更加地清晰。

## 可视化

上一个版本的 Cloud Insight 我们实现了平台列表，来线性查看所有主机的状态，以及操作系统性能指标，和运行在上面的数据库、中间件的性能指标。

![集群管理与可视化：Cloud Insight 已经开启！](/images/cluster8.png)

而这个版本，我们为带来了更加强大的展现形式：平台拓扑。

![集群管理与可视化：Cloud Insight 已经开启！](/images/cluster9.gif)

通过标签的选取，对平台进行分组，快速找到出现问题的节点，所属的地域、模块、应用。

## 了解更多

如何添加标签信息呢？可以编辑配置文件来完成，详细信息，请访问 [Cloud Insight 文档 • 平台服务](http://www.oneapm.com/docs/ci/services/index.html)。

以 MySQL 为例，打开 `conf.d/mysql.yaml`，找到 `tags` 属性，就可以开始添加标签信息了。

<pre><code>
init_config:

instances:
  - server: localhost
    user: oneapm
    pass: YourPassword
    tags:
      - role:database
      - location:shanghai
      - user:chengmo
    options:
      replication: 0
      galera_cluster: 1
</code></pre>

在上面的例子中，我们添加了 3 个标签信息，分别代表：

* `role:database` 说明它是一台数据库
* `location:shanghai` 说明这台数据库部署在上海
* `user:chengmo` 说明这台服务器由 `chengmo` 主管

未来，我们会提供在界面中创建和编辑标签信息的功能，敬请期待。

[开始使用 Cloud Insight 吧！](http://www.oneapm.com/ci/feature.html)因为它真的可以帮助您更好地管理基础设施。
