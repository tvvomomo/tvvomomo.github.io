---
layout: post
title:  "不支持 OpenStack 的系统监控工具，不是好监控"
date:   2016-08-01 18:45:37
categories: Cloudinsight
tags: cloudinsight, openstack, monitoring
image: /images/openstack.png
---

Cloudinsight 在 15 年年底的时候，成为了国内首家支持 Docker 监控的监控工具。Cloudinsight 一直跟随架构栈的技术变迁，满足不同企业和开发者的监控需求，所以在这炎热和雷雨交替的夏天，Cloudinsight 又一次开启了国内 OpenStack 监控的先河。

## 什么是 OpenStack

OpenStack 是一个美国国家航空航天局和 Rackspace 合作研发的 IaaS 软件，让任何人都可以自行建立和提供云端运算服务。此外，OpenStack 也用作建立防火墙内的私有云，提供机构或企业内各部门共享资源。

简单来说，OpenStack 就是一个提供私有化部署的 Amazon Web Services。经历 5 年的蓬勃发展，加入 OpenStack 阵营的已经包括 Google、惠普、IBM 和 Intel。

在 [Cloudinsight Agent 4.7.0](http://docs-ci.oneapm.com/services_example/openstack.html) 版本中，已经加入了 OpenStack 监控。其中包括计算模块 Nova 的监控。

![](/images/openstack_dash.png)

## OpenStack 模块构成

既然提到 Cloudinsight 支持 Nova 的监控，那么就有必要说说 OpenStack 的模块构成。OpenStack 由以下 5 个重要模块构成：

1. Nova - 计算服务
2. Keysyone - 认证服务
3. Glance - 镜像服务
4. Neutron - 虚拟网络服务
5. Cinder - 存储服务
6. Horizon - UI 组件

总的来说，OpenStack 就像 AWS 提供虚拟计算单元 EC2，虚拟存储 S3，虚拟网段 VPC等。而这些相互独立的模块组成了一套完整的云计算服务平台；如果加上面向对象的存储 Swfit，资源监控 Ceilometer 和云系统部署 Heat，那么这个云计算服务平台会更加完整。

![](/images/openstack_havana_conceptual_arch.png)

下面我们来介绍与 Cloudinsight 监控相关的 3 个模块：Nova、Keystone 和 Neutron，方便您更好地使用 Cloudinsight Agent。

### Nova

Nova 是 OpenStack 计算的弹性控制器。OpenStack 云实例生命期所需的各种动作都将由 Nova 进行处理和支撑，这就意味着 Nova 以管理平台的身份登场，负责管理整个云的计算资源、网络、授权及测度。虽然Nova 本身并不提供任何虚拟能力，但是它将使用 libvirt API 与虚拟机的宿主机进行交互。Nova 通过 Web服务 API 来对外提供处理接口，而且这些接口与 Amazon 的 Web 服务接口是兼容的。

Nova 功能及特点：

* 实例生命周期管理
* 计算资源管理
* 网络与授权管理
* 基于 REST 的 API
* 异步连续通信
* 支持各种宿主：Xen、XenServer/XCP、KVM、UML、VMware vSphere 及 Hyper-V

基于 Nova 的功能，Cloudinsight OpenStack 监控提供以下数据：

* `openstack.nova.current_workload`：当前 Nova 的 Workload，包括 build, snapshot, migration, resize 各种动作的负载。
* `openstack.nova.running_vms`：当前 Nova 在运行的虚拟机和实例（instance）的数量。
* `openstack.nova.hypervisor_load.1`：hypervisor 相关指标；除了一分钟内系统负载外，还包括 disk, ram, cpu 等相关指标。
* `openstack.nova.limits.max_personality`：和 project 和租户相关的指标；包括除 personality 外的 image, security 相关指标。

更全面的监控指标和指标涵义，请参考 [Cloudinsight OpenStack 监控 • 文档](http://docs-ci.oneapm.com/services_example/openstack.html)。

OpenStack 内部在遵循 AMQP（高级消息队列协议）的基础上，基于 Rabbit MQ 作为其消息队列进行通信。Nova 对请求应答进行异步调用，当请求接收后便则立即触发一个回调。由于使用了异步通信，不会有用户的动作被长置于等待状态。例如，启动一个实例或上传一份镜像的过程较为耗时，API调用就将等待返回结果而不影响其它操作，在此异步通信起到了很大作用，使整个系统变得更加高效。

Cloudinsight 支持 RabbitMQ 相关监控，在配置 OpenStack 监控时，可以[开启 RabbitMQ 相关监控](http://docs-ci.oneapm.com/services_example/rabbitmq.html)。

### Neutron & Keystone

Neutron 为 OpenStack 提供虚拟网络管理服务，来使得网络配置更为简单。

Keystone 为所有的 OpenStack 组件提供认证和访问策略服务，它依赖自身 REST（基于 Identity API）系统进行工作，主要对（但不限于）Swift、Glance、Nova 等进行认证与授权。事实上，授权通过对动作消息来源者请求的合法性进行鉴定。

目前 Cloudinsight Agent 可以获知这两个模块的 UP 或 DOWN 的状态，更多指标的监控，敬请期待吧。

## 如何使用 Cloudinsight 监控 OpenStack

相对于 Cloudinsight 其他监控来说，配置 OpenStack 还是稍微有些复杂的。大体上分为几步：

1. 为 Cloudinsight 创建角色；
2. 为该角色赋予 Nova, Neutron, Keystone 相关权限；
3. 开启 Cloudinsight Agent 中 `openstack.yaml` 配置文件，告知 OpenStack 相关配置；
4. 配置 RabbitMQ 监控；
5. 重启 Cloudinsight Agent。

为了您更多地了解 OpenStack 相关信息和监控基础知识，我们就快速地介绍一下如何配置。更为详细的配置，请前往 [Cloudinsight OpenStack 监控 • 文档](http://docs-ci.oneapm.com/services_example/openstack.html)。

### 创建角色

使用 Cloudinsight 监控 Openstak 前，需要在 Openstack 为 Cloudinsight Agent 创建单独的角色，确保 Keystone 模块可以让 Agent 访问指标数据。

```
openstack role create cloudinsight_monitoring
openstack user create cloudinsight --password my_password --project my_project_name
openstack role add cloudinsight_monitoring --project my_project_name --user cloudinsight
```

### 配置权限

需要编辑 Openstack 3 个模块 Nova, Neutron, Keystone 来让 `role:cloudinsight_monitoring` 获取相应权限。

一般来说，Nova 的权限文件会在 /etc/nova/policy.json 路径下。打开并新增如下权限。

```
    - "compute_extension:aggregates",
    - "compute_extension:hypervisors",
    - "compute_extension:server_diagnostics",
    - "compute_extension:v3:os-hypervisors",
    - "compute_extension:v3:os-server-diagnostics",
    - "compute_extension:availability_zone:detail",
    - "compute_extension:v3:availability_zone:detail",
    - "compute_extension:used_limits_for_admin",
    - "os_compute_api:os-aggregates:index",
    - "os_compute_api:os-aggregates:show",
    - "os_compute_api:os-hypervisors",
    - "os_compute_api:os-hypervisors:discoverable",
    - "os_compute_api:os-server-diagnostics",
    - "os_compute_api:os-used-limits"

```

### 告知 Cloudinsight

接下来，需要前往 Cloudinsight Agent 的平台服务配置文件中，新增 Openstack 相关配置。

```
# 修改 keystone_server_url 配置项
# 通常来说，默认的端口号为 5000
init_config:
  keystone_server_url: "https://my-keystone-server.com:port/"

# 除此之外，您还需要前往 <yourHorizonserver>/identity 查询您的 Project ID 来修改配置项 id
# 以下是示例
instances:
    - name: instance_1
      auth_scope:
          project:
              id: b9d363ac9a5b4cceae228e03639357ae
	  # 前往配置项的 User credentials 部分，修改角色的账户和密码
      user:
          password: my_password
          name: cloudinsight
          domain:
              id: default

```

### 配置 RabbitMQ

想要快速开启 RabbitMQ，可以使用以下指令安装 RabbitMQ 监控插件：

```
rabbitmq-plugins enable rabbitmq_management
```

重启 Rabbit 使插件生效。安装成功后，插件为您创建 URL: `http://localhost:15672/api/` 来展示指标数据，而 Cloudinsight 也是通过该地址采集指标数据。

```
service rabbitmq-server restart
```

在 `etc/conf.d/rabbitmq.yaml` 中添加该地址：

```
instances:
    -  rabbitmq_api_url: http://localhost:15672/api/
       rabbitmq_user: guest # defaults to 'guest'
       rabbitmq_pass: guest # defaults to 'guest'
```

### 重启 Agent

重启 Cloudinsight Agent，使配置生效 。

```
/etc/init.d/cloudinsight-agent restart
```


至此，Cloudinsight Openstack 配置已经完成。为什么需要使用 Cloudinsight 来监控 OpenStack 呢？何必使用 Ceilometer 呢？

我们先来看一下 Ceilometer 提供的数据：

```
| Period	| Period Start			| Period End			| Max | Min | Avg | Sum | Count | Duration | Duration Start      | Duration End        |
| 10000000	| 2015-03-29T05:56:38	| 2046-12-05T07:43:18	| 0.0 | 0.0 | 0.0 | 0.0 | 17    | 11273.0  | 2015-03-29T05:56:38 | 2015-03-29T09:04:31 |

```

再来看看 Cloudinsight 提供的 OpenStack 默认监控样式：

![](/images/openstack_pl.png)

Cloudinsight 不仅拥有更好的易读性，还提供指标数据的多维度聚合，能够帮助您更好地定位问题。具体的操作可以进一步了解 Cloudinsight 数据聚合的功能。

## 这些 OpenStack 指标值得关注

OpenStack 复杂程度让 Gartner 在面对企业咨询 OpenStack 搭建私有云时，会询问 3 个问题：

* 你的业务需要搭建在一个 IaaS 私有云平台上吗?
* 你有技术和资源来支持这么复杂的项目吗?
* 像 OpenStack 这样的开源框架合适你的工作环境吗?

可见 OpenStack 并不是一个很容易上手的工程。好在国内出现了 [OpenStack 中国社区](http://www.openstack.cn/)，和 Cloudinsight 这样的组织，帮助企业和开发者更好地使用 OpenStack。

![](/images/openstack_logic_arch.png)

以下，Cloudinsight 会帮助您更好地监控 OpenStack，通过相关提取出的指标和图例，告诉您哪些指标值得关注，从而快速上手 OpenStack 监控。

如上文所述，Cloudinsight 支持 OpenStack 以下几类指标的监控：

* Hypervisor 指标 - 运行的虚拟机数量，hypervisor 自身负载等；
* Nova Server 指标 - 磁盘的读写速率，RAM 相关指标等；
* Tenant 指标 - 资源使用情况，CPU 核数等；
* Message Queue 指标 - MQ 的大小等。

![](/images/arch_over.png)

Nova 以管理平台的身份登场，负责管理整个云的计算资源、网络、授权及测度。它原生支持 KVM, QEMU 宿主，也提供 Xen, VMware vSphere 及 Hyper-V 的支持。Nova 兼容 AWS 的 EC2 和 S3 API，就像 AWS 通过 Nova 我们可以方便地移植应用，减少应用的部署时间。

![](/images/nova-high-level.png)

Nova 指标就包括了 Hypervisor, Nova Server, Tenant 指标。通过 Hypervisor 指标可以了解到系统的整体运行情况和负载，而 Nova Server 指标可以了解某个虚拟机的运行情况，Tenant 指标则是可以查看相关租户的资源使用情况。

### Hypervisor 指标

![](/images/hypervisor.png)

Cloudinsight 为您提取了以下 7 个指标，来查看 Hypervisor 的性能。由上图可知，Hypervisor 主导 Nova 模块中与计算相关的部分。

|指标|描述|
|---|
|hypervisor_load|系统负载|
|current_workload|当前 hypervisor 运行任务的数量|
|hypervisor_up|启停状态|
|running_vms|运行的虚拟机数量|
|vcpus_available|当前可用的 CPU 数量|
|free_disk_gb|当前可用的磁盘空间，默认单位 GB|
|free_ram_mb|当前可用的 RAM 大小，默认单位 MB|

`hypervisor_load` - 该指标表征系统负载，有点类似操作系统的 `system.load.1` 指标，标识过去 1 分钟内系统负载。如果系统负载上升，一般来说相关的虚拟机的指标也会上升，此时需要针对性能进行优化。

`current_workload` - 一般来说，hypervisor 任务包括：[build, snapshot, migrate, resize](http://docs.openstack.org/developer/nova/support-matrix.html)；该指标显示当前任务数量，由于 hypervisor 和虚拟机共用 IO 资源，若当前任务任务数量过高，一般来说会导致 IO 瓶颈问题。

`running_vms` - 当前运行的虚拟机数量。通过数据聚合功能，您可以了解整个平台的虚拟机数量，加上系统负载和任务数量，您可以了解到 OpenStack 整体的运行情况。

![](/images/vcpus.png)

`vcpus_available` - 当前 CPU 使用情况在生产环境下一般是保持不变的。您可以通过设置该指标的报警，来监控该指标的变化，从而预测异常。而在开发环境中，该指标无显著的监控意义。越高的 CPU 数量代表您有越多的资源提供给用于计算的主机，若该指标出现陡然降低的情况，那代表您需要增设机器了。

![](/images/disk.png)

`free_disk_gb` - 磁盘的空闲情况，直接影响是否可以新建虚拟机。关注该指标可以了解到何时需要对占用磁盘空间较大的实例进行迁移。

`free_ram_mb` - 和磁盘的空闲情况相似，RAM 的空闲情况也是一个非常值得关注的情况，属于关键资源。

### Nova Server 指标

关注 Nova Server 指标可以了解主要节点的信息，如每个节点向 Nova 发送请求的数量，从而预测是否存在 [Noisy Neighbor Problem](http://searchcloudcomputing.techtarget.com/definition/noisy-neighbor-cloud-computing-performance)。

若想要了解每个实例（虚拟机）的具体指标，如 CPU 利用率、内存、网络等指标，您可以将 Cloudinsight Agent 安装至每个虚拟机内部。通过标签来进行数据聚合和分组，可以达到更牛掰的监控程度。

|指标|描述|
|---|
|hdd_read_req|每秒读请求的数量|


`hdd_read_req` - 在虚拟环境中，每个进程的 RAM 使用量是十分有限的。查看 HDD 的读请求数量，就意味着监控 Nova 节点的虚拟机的基本性能。举例来说，若该指标出现峰值，就意味着虚拟机的 RAM 太小了，从而导致较小的分页，继而使得对 HDD 的读请求数量激增。此时您就需要对 Nova 集群进行性能排查啦。

### Tenant 指标

Tenant 这个概念在一年前被讨论得蛮火的，特别是国内 SaaS 厂商兴起了以后。租户简单来说，就是一组用户啦。给不同组的用户分配不一样的资源，可以达到资源的合理分配，从而达到利益的最大化。

监控租户相关指标，也就意味着在监控与业务相关的指标。

|指标|描述|
|---|
|total_cores_used|租户当前使用 core 数量|
|max_total_cores|分配给租户最大 core 数量|
|total_instances_used|租户当前使用 instance 数量|
|max_total_instances|分配给租户最大 instance 数量|

`total_cores_used` & `max_total_cores` - Core 作为重要的资源，了解其使用情况，来辅助决策是否需要在激增的情况下，多分配资源；是否需要在持续较低使用率的情况下，减少资源分配。

`total_instances_used` & `max_total_instances` - Instance 数量也就是虚拟机数量，和 core 一样是重要的资源。监控租户的当前使用量，和最大分配量，可以帮助决策是否需要为该租户增添资源。

### RabbitMQ 指标

![](/images/rabbitmq-3.png)

RabbitMQ 指标又是什么鬼啦？正如上文所说，OpenStack 内部在遵循 AMQP（高级消息队列协议）的基础上，基于 Rabbit MQ 作为其消息队列进行通信。

也就是说，监控 RabbitMQ 指标意味着监控 OpenStack 环境的整体情况，毕竟通讯与中断还是蛮重要的一件事情的啦。

|指标|描述|
|---|
|consumer_utilisation|接收新消息的时间占比|
|memory|当前队列的大小|
|count|当前有效队列的数量|
|consumers|每个队列的消费者数量|

`consumer_utilisation` - 理想情况下，该指标保持在 100% 数值下是最好的，这意味着每个队列都可以及时地处理新消息。网络拥堵和 [Comsumer Prefetch](https://www.rabbitmq.com/consumer-prefetch.html) 都会降低该指标的数值。SO SAD！

![](/images/rabbitmq.png)

`memory` - 和大多数 MQ 一样，RabbitMQ 会在内存不够用的情况下调用磁盘。加上磁盘分页而导致的吞吐量增加，很容易使得内存使用量超过系统 RAM 的 40%（默认值），RabbitMQ 会优先对消息生产者进行节流。Duang！性能问题就产生了。

`count` - 建议为该指标设置指标报警策略，当数值为 0 时，触发报警。毕竟有效队列的数量是 0，那是相当可怕的事情呢。

`consumers` - 和 count 一样啦，设置个报警比较好。该指标为 0 是一件犹如伏地魔重生的事情。若您不幸遇到该情况，我们推荐以下两种方式：[Aliveness Testing](http://docs.openstack.org/ops-guide/index.html) 和 [StackTach 工具](https://github.com/openstack/stacktach)。



在此，我们也希望[获取已经使用 OpenStack 的企业和开发者](mailto:lizhe@oneapm.com)，来试用 Cloudinsight OpenStack 监控 Beta 版本；帮助 Cloudinsight 能够更好地优化 OpenStack 监控的功能。
