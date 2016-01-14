---
layout: post
title:  "使用 Cloud Insight SDK 监控北京空气质量！"
date:   2015-12-23 13:23:07
categories: Marketing
tags: cloudinsight beijing airpollution
image: http://news.oneapm.com/content/images/2015/12/pmcover--1-.jpg
---

本文参考一篇帖子 [pm25，关爱老大](https://www.v2ex.com/t/245413#reply4)，征求原作者同意后改写。

现在越来越多的 App 都开始有广告了。特别是空气质量监测，和天气类的 App，广告还是蛮多的，眼花缭乱，真是够了。

![](http://news.oneapm.com/content/images/2015/12/1-3.png)

最近刚好在用一款系统监控工具 Cloud Insight，它提供的 SDK 可以把任一数据上传到他们那做展示。

灵机一动，作为一个程序员，自己动手丰衣足食，没什么不能解决的。

    pip install -i http://pypi.oneapm.com/simple --upgrade oneapm-ci-sdk

这就安装好了。

简单用 ipython 看了看接口文档， gauge 是主要的发数据的接口，好像 increment 也可以，但是不懂是搞啥的，貌似数据类型不一样。

##PM 2.5 API

首先得找一个 PM 2.5 API，参考了一下这个教程：[Air Quality Widget - New Improved Feed](http://aqicn.org/faq/2015-07-28/air-quality-widget-new-improved-feed/)。里面的资料显示，美国驻京使馆也用的是这里的数据，应该还算准确吧。

![](http://news.oneapm.com//content/images/2015/12/2-3.png)

注意看教程里，他们请求的地址为：

    http://feed.aqicn.org/feed/beijing/en/feed.v1.json

请求这个地址，就可以得到数据啦。

说到这个，其实国内很多 App 和网站都在用 [PM25.in](http://aqicn.org/faq/2015-07-28/air-quality-widget-new-improved-feed/)。用的人挺多的，就是发邮件速度有点慢，注册之后获取 Token 的邮件一直都没发给我！

##接入 Cloud Insight

先介绍下 Cloud Insight 吧，就是一款系统监控工具，支持 Ubuntu、MySQL、Docker 的监控。但是他们提供 SDK 可以自定义上传数据，所以我们就用它来承接 PM 2.5 的数据吧。

他们也提供任一指标的报警功能，所以也可以通过设置报警，来发邮件提醒给我。

![](http://news.oneapm.com//content/images/2015/12/3-4.png)

Cloud Insight SDK 和 [StatsD](http://aqicn.org/faq/2015-07-28/air-quality-widget-new-improved-feed/) 原理很像，SDK 的详情可以[参考文档](http://www.oneapm.com/docs/ci/sdk.html)。

源代码如下：

<pre><code>
import requests

from oneapm_ci_sdk import statsd

PM25_API_URL = "http://feed.aqicn.org/feed/%s/en/feed.v1.json"

def get_city_data(city):
    try:
        res = requests.get(PM25_API_URL % city)
    except:
        return 0
    else:
        return res.json()['aqi']['val']

def using_sdk():
    statsd.gauge('airquality.beijing.pm25', float(get_city_data('beijing')))
    statsd.gauge('airquality.shanghai.pm25', float(get_city_data('shanghai')))
    statsd.gauge('airquality.guangzhou.pm25', float(get_city_data('guangzhou')))
    statsd.gauge('airquality.xuchang.pm25', float(get_city_data('xuchang'))) # 家里。。

if __name__ == '__main__':
    using_sdk()
</code></pre>

首先通过 API 把数值取出来，然后通过 `stats.gauge` 对指标进行赋值，就可以了。呼~接下来是产品内部的使用了。

啦啦啦~自定义仪表盘开个 Air Quaility 仪表盘，数据选进来，就可以看各个城市的 PM 2.5 的实时数值了。

![](http://news.oneapm.com/content/images/2015/12/4-2.png)

想随时随地知道北京空气质量是否超标，却又不想下载广告一大堆的空气质量 App。那我自己动手设一个报警策略吧。

大于或等于 100，就算超标好了。很简单就设置完成了。

![](http://news.oneapm.com/content/images/2015/12/5-2.png)

大功告成，等着邮件提醒吧。顺便秀一下 Kickstarter 买来的 Pebble 手表。舒心啊：没有广告的北京空气质量监测。

![](http://news.oneapm.com/content/images/2015/12/6-1.jpg)
