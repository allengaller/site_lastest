---
title: microservice portal
categories:
- microservice
tags:
- portal
---

# about

- current

    - 多层架构

            表现层，业务逻辑层，数据层
            分层设计与优化，合理设计接口，每层可再细分，功能模块可重用
    
    - 单体模式

            monolith，是目前主流打包方式
            一个单独的java war文件，rails或node中一个单独的目录
            优势：业界熟练使用，生态健全，外围工具丰富。易于开发，测试，部署
            开始项目最简单快捷的方式，充分利用已有代码和工具，不必担心分布式部署。
            但是应用工程变得负责，敏捷和部署举步维艰，启动时间长，难以采用新技术。可靠性差。

- 微服务架构

    - 优势

            由多个独立运行的微小服务构成
            使用轻量级通讯机制
                独立构建部署
            每个服务保持独立性
                构建，部署，扩容，容错，数据管理
            敏捷最大化
                代码运行速度更高，更短的反馈周期，更简单的使用方法，快速应对变化
            可以使用不同技术
                每个服务可以使用独立技术栈
                易于重构，分散式管理
            高效团队
                小规模团队
                责任明晰，便捷清晰
                围绕业务功能进行组织，非常灵活

    - 不足

            过度关注服务大小，可能过度拆分
            分布式系统的构建与部署问题
            分布式的数据架构
            测试的复杂度
            改动带来的沟通成本

# link

    main: http://microservices.io/patterns/microservices.html
    http://microservices.io/

    conf:
        http://2017.qconbeijing.com/track/58

# project

    iron.io: Microservices For The Enterprise
    http://www.iron.io/ 

# clusterup

    about
        life cycle management GUI for docker microservices
        Real-time monitoring of Docker containers and applications
        Manage and monitor your app pre-production. We provide app analytics
    link
        https://clusterup.io/
        