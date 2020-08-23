# consul

* 官方文档：[https://www.consul.io/docs/index.html](https://links.jianshu.com/go?to=https%3A%2F%2Fwww.consul.io%2Fdocs%2Findex.html)

* [Consul](https://www.jianshu.com/p/ce7c7b9dcf14)
* [服务发现--初识Consul](https://www.jianshu.com/p/32dc52f28a14)
* [Consul 快速入门 - Kong最佳实践](https://www.jianshu.com/p/7d20dc58c9fc)
* []()
* []()

### 前言

> 服务注册、服务发现作为构建微服务架构得基础设施环节，重要性不言而喻。在当下，比较热门用于做服务注册和发现的开源项目包括zookeeper、etcd、euerka和consul。今天在这里对近期学习consul的一些知识继续浓缩和汇总，作为自己学习过程中的一个总结。

### Consul简介

Consul是基于GO语言开发的开源工具，主要面向分布式，服务化的系统提供服务注册、服务发现和配置管理的功能。Consul的功能都很实用，其中包括：服务注册/发现、健康检查、Key/Value存储、多数据中心和分布式一致性保证等特性。Consul本身只是一个二进制的可执行文件，所以安装和部署都非常简单，只需要从官网下载后，在执行对应的启动脚本即可。

### Consul特性

#### 基础特性

**1.服务注册/发现**
 为什么微服务架构下就需要做服务注册和服务发现呢？微服务的目标就是要将原来大一统的系统架构，拆分成细粒度的按功能职责分成的小系统，这样就会出现很多小的系统，部署的节点也会随之增加。试想一下，如果没有一个统一的服务组件来管理各系统间的列表，微服务架构是很难落地实现的。
 Consul提供的服务注册/发现功能在数据强一致性和分区容错性上都有非常好的保证，但在集群可用性下就会稍微差一些（相比Euerka来说）。

**2.数据强一致性保证**
 Consul采用了一致性算法Raft来保证服务列表数据在数据中心中各Server下的强一致性，这样能保证同一个数据中心下不管某一台Server Down了，请求从其他Server中同样也能获取的最新的服务列表数据。数据强一致性带来的副作用是当数据在同步或者Server在选举Leader过程中，会出现集群不可用。

**3.多数据中心**
 Consul支持多数据中心(Data Center),多个数据中心之间通过Gossip协议进行数据同步。多数据中心的好处是当某个数据中心出现故障时，其他数据中心可以继续提供服务，提升了可用性。

**4.健康检查**
 Consul支持基本硬件资源方面的检查，如：CPU、内存、硬盘等

**5.Key/Value存储**
 Consul支持Key/Value存储功能，可以将Consul作为配置中心使用，可以将一些公共配置信息配置到Consul，然后通过Consul提供的 HTTP API来获取对应Key的Value。

#### 高级特性

**1.HTTP API**
 **2.ACL**

### Consul工作模式

![img](https:////upload-images.jianshu.io/upload_images/9073146-80d310cf9a4e85b3.png?imageMogr2/auto-orient/strip|imageView2/2/w/1200/format/webp)

Consul工作模式

从上图可以看到，Consul中包括的3种不同的角色：Client、Server、Server-Leader。还有一个在图上没有标出来的角色Agent，一共4个角色，下面会逐一介绍它们的作用。

- Agent
   1.是一个守护线程
   2.跟随Consul应用启动而启动
   3.负责检查、维护节点同步
- Client
   1.转发所有请求给Server
   2.无状态，不持久化数据
   3.参与LAN Gossip的健康检查
- Server
   1.持久化数据
   2.转发请求给Server-Leader
   3.参与Server-Leader选举
   4.通过WAN Gossip，与其他数据中心交换数据
- Server-Leader
   1.响应RPC请求
   2.服务列表数据同步给Server

### Consul工作原理

##### 服务注册的方式

Consul服务注册有两种方式：HTTP API & JSON配置文件
 方式一：HTTP API

> http://{ip}:8500/v1/agent/service/register/:service

方式二：JSON 配置文件



```json
{
    "services": [
            {
                    "id": "serverId",
                    "name": "serverName",
                    "tags": [
                            "primary"
                    ],
                    "address": "127.0.0.1",
                    "port": 9003,
                    "checks": [
                            {
                                    "id": "api-servie",
                                    "name": "Service 'xx' check",
                                    "http": "http://127.0.0.1:9003/public/health",
                                    "method": "GET",
                                    "interval": "10s",
                                    "timeout": "1s"
                            }
                    ]
            }
    ]
```

}

启动Consul增加启动参数`-config-dir`

> nohup ./consul agent -dev -config-dir=/consul-conf/service.json &

##### 服务发现的方式

服务发现的方式同时有两种：HTTP API & DNS Agent
 方式一：HTTP API

> 获取某个service下健康的服务列表信息
>  http://{ip}:8500/v1/health/service/:service

方式二：DNS Agent

##### 工作流程

![img](https:////upload-images.jianshu.io/upload_images/9073146-6e96f88cbc612da3.jpg?imageMogr2/auto-orient/strip|imageView2/2/w/526/format/webp)

Consul工作流程.jpg