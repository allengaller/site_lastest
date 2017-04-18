---
title: cloud portal
categories:
- cloud
tags:
- portal
- cloud
---

# aws
https://github.com/donnemartin/saws

# top resource
infoq: http://2017.qconbeijing.com/tracks

https://www.oschina.net/p/dubbo
https://github.com/alibaba/dubbo


## zookeeper

- about

        ZooKeeper is a centralized service for maintaining configuration information, naming, providing distributed synchronization, and providing group services. All of these kinds of services are used in some form or another by distributed applications. Each time they are implemented there is a lot of work that goes into fixing the bugs and race conditions that are inevitable. Because of the difficulty of implementing these kinds of services, applications initially usually skimp on them ,which make them brittle in the presence of change and difficult to manage. Even when done correctly, different implementations of these services lead to management complexity when the applications are deployed.

        考虑一下有多个服务器的分布式系统，每台服务器都负责保存数据，在数据上执行操作。这样的潜在例子包括分布式搜索引擎、分布式构建系统或者已知的系统如Apache Hadoop。所有这些分布式系统的一个常见问题是，你如何在任一时间点确定哪些服务器活着并且在工作中。最重要的是，当面对这些分布式计算的难题，例如网络失败、带宽限制、可变延迟连接、安全问题以及任何网络环境，甚至跨多个数据中心时可能发生的错误时，你如何可靠地做这些事。这些正是Apache ZooKeeper所关注的问题，它是一个快速、高可用、容错、分布式的协调服务。你可以使用ZooKeeper构建可靠的、分布式的数据结构，用于群组成员、领导人选举、协同工作流和配置服务，以及广义的分布式数据结构如锁、队列、屏障（Barrier）和锁存器（Latch）。许多知名且成功的项目依赖于ZooKeeper，其中包括HBase、Hadoop 2.0、Solr Cloud、Neo4J、Apache Blur（Incubating）和Accumulo。

        ZooKeeper是一个分布式的、分层级的文件系统，能促进客户端间的松耦合，并提供最终一致的，类似于传统文件系统中文件和目录的Znode视图。它提供了基本的操作，例如创建、删除和检查Znode是否存在。它提供了事件驱动模型，客户端能观察特定Znode的变化，例如现有Znode增加了一个新的子节点。ZooKeeper运行多个ZooKeeper服务器，称为Ensemble，以获得高可用性。每个服务器都持有分布式文件系统的内存复本，为客户端的读取请求提供服务。

- usage

        所有分布式的协商和一致都可以利用zk实现。可以理解为一个分布式的带有订阅功能的小型元数据库。
        使用方式自然就是通过上面这句话而定。比如你需要一个订阅功能的数据库，发布你的信息给其他客户端，享受订阅功能。又比如你有很多信息是由很多客户端同时竞争写入zk,但只允许第一个到达的写入，就可以享受zk的一致性。

        现在流行的分布式系统已经完全无法脱离zookeeper。列举几个简单的例子：
        storm中用zookeeper来协调同步集群中机器的状态（并不传递消息）。基本不会有负载，对机器性能没什么要求。jstorm也是用zk来做一致性服务。

        dubbo中采用zookeeper来作注册中心，在阿里内部采用的是基于数据库的注册中心，我们自己用dubbo的时候采用了zookeeper，配置起来非常方便。当然如果采用redis或者自己写个简单的基于内存的注册中心也是可以的。从稳定性来讲，个人觉得zookeeper是比较好的选择，毕竟zookeeper集群中的机器只要不是半数以上宕掉，服务就是可用的。dubbo服务的消费者和提供者都需要用到zookeeper，然而消费者和提供者之间的长连接建立后，zookeeper参与程度就比较弱了(仅需要接受一些心跳包)，除非此时有新的提供者消费者加入或者离开(需要更新节点数据)。因此负载也是非常低的，基本不用考虑性能问题。zookeeper还同时起到了监控各个服务的作用。

        HBase要用到zk，这个已经有答主提到，就不用细说了。Hadoop也是需要用到zookeeper的，用来管理集群中的NameNode。差点忘了，Kafka集群依赖于ZooKeeper。kafka通过zookeeper实现生产者在消费端的负载均衡，动态的集群扩展等等。


resource:
    http://zookeeper.apache.org/
    http://www.ibm.com/developerworks/cn/opensource/os-cn-zookeeper/

# distributed messaging

## kafka

about

    Apache Kafka是分布式发布-订阅消息系统。它最初由LinkedIn公司开发，之后成为Apache项目的一部分。Kafka是一种快速、可扩展的、设计内在就是分布式的，分区的和可复制的提交日志服务。Kafka是linkedin开源的MQ系统，主要特点是基于Pull的模式来处理消息消费，追求高吞吐量，一开始的目的就是用于日志收集和传输，0.8开始支持复制，不支持事务，适合产生大量数据的互联网服务的数据收集业务。

    Apache Kafka与传统消息系统相比，有以下不同：
    它被设计为一个分布式系统，易于向外扩展；
    它同时为发布和订阅提供高吞吐量；
    它支持多订阅者，当失败时能自动平衡消费者；
    它将消息持久化到磁盘，因此可用于批量消费，例如ETL，以及实时应用程序。

    Kafka 使用自己的协议。Kafka 自身服务和消费者都需要依赖 Zookeeper。RabbitMQ 在有大量消息堆积的情况下性能会下降，Kafka不会。毕竟AMQP设计的初衷不是用来持久化海量消息的，而Kafka一开始是用来处理海量日志的。


resource：
    http://kafka.apache.org/
    https://github.com/apache/kafka

    http://www.infoq.com/cn/articles/apache-kafka
    http://www.infoq.com/cn/presentations/use-apache-kafka-to-transfer-key-business-message?utm_source=infoq&utm_medium=related_content_link&utm_campaign=relatedContent_articles_clk

    must read:
        https://github.com/oldratlee/translations/blob/master/log-what-every-software-engineer-should-know-about-real-time-datas-unifying/README.md
        http://www.cnblogs.com/foreach-break/p/notes_about_distributed_system_and_The_log.html

    faq：https://www.zhihu.com/question/22480085

## rabbitmq
RabbitMQ 支持 AMQP（二进制），STOMP（文本），MQTT（二进制），HTTP（里面包装其他协议）等协议。

## rocketmq
about
    RocketMQ 是一款分布式、队列模型的消息中间件，具有以下特点：

    能够保证严格的消息顺序
    提供丰富的消息拉取模式
    高效的订阅者水平扩展能力
    实时的消息订阅机制
    亿级消息堆积能力
    Metaq3.0 版本改名，产品名称改为RocketMQ

    RMQ的结构分为四个部分：生产者、消费者、nameserver、brokerserver
    nameserver：nameserver接收broker的请求注册broker路由信息。收client的请求根据某个topic获取所有到broker的路由信息。
    brokerserver：消息的接收和推送，
    生产者：发送消息，将消息推送给brokerserver。
    消费者：接收消息，从brokerserver上获取消息。

resource:
    https://www.aliyun.com/product/ons
    https://github.com/apache/incubator-rocketmq
    https://rocketmq.incubator.apache.org/
    http://www.jianshu.com/p/453c6e7ff81c
    http://www.jialeens.com/archives/681.html?utm_source=tuicool&utm_medium=referral

## zeromq
ZeroMQ 和 RabbitMQ/Kafka 不同，它只是一个异步消息库，在套接字的基础上提供了类似于消息代理的机制。使用 ZeroMQ 的话，需要对自己的业务代码进行改造，不利于服务解耦。


# distribute kv

## etcd
about
        etcd 是一个应用在分布式环境下的 key/value 存储服务。利用 etcd 的特性，应用程序可以在集群中共享信息、配置或作服务发现，etcd 会在集群的各个节点中复制这些数据并保证这些数据始终正确。etcd 无论是在 CoreOS 还是 Kubernetes 体系中都是不可或缺的一环。

        2. 规范词汇表
        etcd 0.5.0 版首次对 etcd 代码、文档及 CLI 中使用的术语进行了定义。

        2.1. node

        node 指一个 raft 状态机实例。每个 node 都具有唯一的标识，并在处于 leader 状态时记录其它节点的步进数。

        2.2. member

        member 指一个 etcd 实例。member 运行在每个 node 上，并向这一 node 上的其它应用程序提供服务。

        2.3. Cluster

        Cluster 由多个 member 组成。每个 member 中的 node 遵循 raft 共识协议来复制日志。Cluster 接收来自 member 的提案消息，将其提交并存储于本地磁盘。

        2.4. Peer

        同一 Cluster 中的其它 member。

        2.5. Client

        Client 指调用 Cluster API 的对象。

        3. Raft 共识算法
        etcd 集群的工作原理基于 raft 共识算法 (The Raft Consensus Algorithm)。etcd 在 0.5.0 版本中重新实现了 raft 算法，而非像之前那样依赖于第三方库 go-raft 。raft 共识算法的优点在于可以在高效的解决分布式系统中各个节点日志内容一致性问题的同时，也使得集群具备一定的容错能力。即使集群中出现部分节点故障、网络故障等问题，仍可保证其余大多数节点正确的步进。甚至当更多的节点（一般来说超过集群节点总数的一半）出现故障而导致集群不可用时，依然可以保证节点中的数据不会出现错误的结果。


https://github.com/coreos/etcd
http://www.infoq.com/cn/articles/coreos-analyse-etcd/


# IOE resource

IBM doc:
http://www.ibm.com/analytics/cn/zh/

Oracle doc: http://docs.oracle.com/en/
SAP doc: http://www.sap.com/developer.html
SAP case: http://www.bestsapchina.com/ResourceCenter/i-t-s.html
