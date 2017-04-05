---
title: docker filesystem
categories:
- docker
tags:
- core
- filesystem
---

# about


# AuFS

- about
    
    http://aufs.sourceforge.net/
    
    layered file system
    
    AuFS is a layered file system, so you can have a read only part, and a write part, and merge those together. So you could have the common parts of the operating system as read only, which are shared amongst all of your containers, and then give each container its own mount for writing.So let's say you have a container image that is 1GB in size. If you wanted to use a Full VM, you would need to have 1GB times x number of VMs you want. With LXC and AuFS you can share the bulk of the 1GB and if you have 1000 containers you still might only have a little over 1GB of space for the containers OS, assuming they are all running the same OS image.

# ceph

- about

    一个 Linux PB 级分布式文件系统
    Ceph is a distributed object store and file system designed to provide excellent performance, reliability and scalability.

- feature

    - 以时间复杂度为O(1)的方式提供消息持久化能力，即使对TB级以上数据也能保证常数时间复杂度的访问性能。
    - 高吞吐率。即使在非常廉价的商用机器上也能做到单机支持每秒100K条以上消息的传输。
    - 支持Kafka Server间的消息分区，及分布式消费，同时保证每个Partition内的消息顺序传输。
    - 同时支持离线数据处理和实时数据处理。
    - Scale out：支持在线水平扩展。

- resource

    link： http://ceph.com/
    doc： http://docs.openfans.org/ceph

- core

- components
    
    cluster monitors
    clients
    metadata server cluster
    object storage cluster

# overlayfs

- link： https://www.kernel.org/doc/Documentation/filesystems/overlayfs.txt