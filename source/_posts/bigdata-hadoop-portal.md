---
title: hadoop portal
categories:
- bigdata
tags:
- portal
- hadoop
---

# about hadoop

    head first:
        离线计算框架;
        开源分布式系统实现，完整的mapreduce计算框架实现，山寨Google计算系统中的强者;

	usage
	    搭建大型数据仓库，PB数据存储处理分析统计等
	    搜索引擎，商业智能，日志分析，数据挖掘
	advantage
	   高扩展，低成本，成熟的生态圈
       业界大数据平台的首选
	   人才缺口：开发与运维
	version
		1.2 stable
		2.x
	founder
		Doug_Cutting
			https://en.wikipedia.org/wiki/Doug_Cutting
			https://www.linkedin.com/in/cutting

# resource

resource
	link
		http://hadoop.apache.org
		小象学院
			http://www.chinahadoop.cn/
		https://hadoopecosystemtable.github.io/
	awesome
		https://github.com/youngwookim/awesome-hadoop
	doc
		http://hadoop.apache.org/docs/current/
	read
		book
			Hadoop: The Definitive Guide
			Hadoop Operations
			Apache Hadoop Yarn
			HBase: The Definitive Guide
			Programming Pig
			Programming Hive
			Hadoop in Practice, Second Edition
			Hadoop in Action, Second Edition
		blog
			http://zh.hortonworks.com/blog/hadoop-perect-app-for-openstack/
				Savanna/Sahara
			http://hadoop123.com/
			http://dongxicheng.org/
			http://blog.fens.me/hadoop-family-roadmap/
		zhihu
			https://www.zhihu.com/topic/19563390/hot
	project
		modules
			Hadoop Common
				The common utilities that support the other Hadoop modules.
			HDFS
				about
					Hadoop Distributed File System (HDFS™)
					A distributed file system that provides high-throughput access to application data.
					存储海量数据的分布式文件系统，支持MapReduce操作，不支持标准POSIX接口，不操作裸磁盘

					局限性
						不适合低延迟数据访问
						无法高效存储大量小文件
						不支持多用户写入和任意修改
				concept
					block
						块，默认大小 64M，是文件存储的基本逻辑单元
							2.x 默认128M
						hdfs=site.xml
					name node
						管理节点，存放文件元数据：文件与数据块的映射表，数据块与数据节点的映射表

					data node
						工作节点，存放数据块

					数据管理与容错
						每个数据块三份副本，分布在两个机架内的三个节点
						data node定期向 name node 发送心跳消息
						二级name node 定期同步元数据映像文件和修改日志，name node故障时转正
							fsimage, editlog
					文件读取流程


					文件写入流程


					job  and task
						job
							1 job -> many task
						task
							map task
							reduce task
					job tracker
						master 管理节点
							监控TaskTracker状态
							分配任务，监控任务执行进度
							作业调度

					task tracker
						data node 在同一组节点
						执行任务
						汇报任务状态
					作业执行

					容错机制
						重复执行
							四次失败后放弃
						推测执行
							所有map端完成后reduce端才开始；如果个别特别慢，则新建一个task tracker同步执行，认最快算完的tracker
					sample
						link
							http://www.imooc.com/video/7642
						word count


						sort

				feature
					数据冗余，硬件容错
					流式数据访问。一次写入，多次读取。一旦写入无法修改，只能追加写。
					适合存储大文件
					适合数据批量读写，吞吐高。不适合交互式应用，低延迟很难满足
					适合一次写入多次读取，顺序读写。不支持多用户并发读写相同文件
			Hadoop YARN
				 A framework for job scheduling and cluster resource management.
			Hadoop MapReduce
				about
					A YARN-based system for parallel processing of large data sets.
					分布式并行数据处理和计算模型和框架，实现任务分解和调度，可以分析海量数据
					用户只需要实现map() reduce() 两个函数即可
						形参是KV
					高容错，高扩展，编程简单，适合大数据离线批量处理PB+
						实时需求：storm，hbase
				solution
					mapr
						https://www.mapr.com
						The MapR Converged Data Platform integrates the power of Hadoop and Spark with global event streaming, real-time database capabilities, and enterprise storage for developing and running innovative data applications.
The MapR Platform is powered by the industry’s fastest, most reliable, secure, and open data infrastructure that dramatically lowers TCO and enables global real-time data applications.
				core
					分而治之思想
						一个大任务分成多个子任务（map），并行执行后，合并结果（reduce）

						sample: 100G访问日志，找出访问次数最多的IP地址

				map任务处理

				reduce任务处理

				map,reduce KV对格式

				MR流程

				MR角色
					job client
						提交作业
					job tracker
						初始化作业，分配作业，task tracker与其通讯，协调监控整个作业
					task tracker
						定期与job tracker通讯，执行MR任务
					hdfs
						读取和保存作业数据，配置，jar包，结果
				作业提交流程
					准备
						编写mr程序
						配置作业，输入输出路径等
					提交作业
						通过job client提交

						可以提交至YARN，也可以本地运行
					作业初始化

					任务分配

					错误处理
						job tracker失败
							存在单点故障，hadoop2.0解决
						task tracker失败

						task 失败
							任务失败，会想task tracker抛出异常，最后任务挂起
					map 输入


					reduce输入
						框架自动完成

		Hadoop-related projects at Apache
			Ambari
				A web-based tool for provisioning, managing, and monitoring Apache Hadoop clusters which includes support for Hadoop HDFS,
Hadoop MapReduce, Hive, HCatalog, HBase, ZooKeeper, Oozie, Pig and Sqoop. Ambari also provides a dashboard for viewing cluster health
such as heatmaps and ability to view MapReduce, Pig and Hive applications visually alongwith features to diagnose their performance characteristics in a user-friendly manner.
			Avro
				 A data serialization system.
				RPC
			Cassandra
				A scalable multi-master database with no single points of failure.
			Chukwa
				A data collection system for managing large distributed systems.
			HBase
				about
					实时KV存储,放弃事务特性，最求更高的扩展，提供数据随机读写和实时访问，实现对表数据的读写功能
					A scalable, distributed database that supports structured data storage for large tables.
				link
					http://hbase.apache.org/
			Hive
				about
					SQL on Hadoop
					 A data warehouse infrastructure that provides data summarization and ad hoc querying.
					SQL语句->Hadoop任务
				link
					http://hive.apache.org/
			Mahout
				A Scalable machine learning and data mining library.
			Pig
				A high-level data-flow language and execution framework for parallel computation.
			Spark
				A fast and general compute engine for Hadoop data. Spark provides a simple and expressive programming model that supports a wide range of applications, including ETL, machine learning, stream processing, and graph computation.
			Tez
				A generalized data-flow programming framework, built on Hadoop YARN,
which provides a powerful and flexible engine to execute an arbitrary DAG of tasks to process data for both batch and interactive use-cases.
Tez is being adopted by Hive™, Pig™ and other frameworks in the Hadoop ecosystem, and also by other commercial software (e.g. ETL tools), to replace Hadoop™ MapReduce as the underlying execution engine.
			ZooKeeper
				about
					锁服务，管理集群配置，监控每个节点状态，数据一致性
					A high-performance coordination service for distributed applications.
				link
		awesome
			hadoop
				Apache Hadoop - Apache Hadoop
				Apache Tez - A Framework for YARN-based, Data Processing Applications In Hadoop
				SpatialHadoop - SpatialHadoop is a MapReduce extension to Apache Hadoop designed specially to work with spatial data.
				GIS Tools for Hadoop - Big Data Spatial Analytics for the Hadoop Framework
				Elasticsearch Hadoop - Elasticsearch real-time search and analytics natively integrated with Hadoop. Supports Map/Reduce, Cascading, Apache Hive and Apache Pig.
				dumbo - Python module that allows you to easily write and run Hadoop programs.
				hadoopy - Python MapReduce library written in Cython.
				mrjob - mrjob is a Python 2.5+ package that helps you write and run Hadoop Streaming jobs.
				pydoop - Pydoop is a package that provides a Python API for Hadoop.
				hdfs-du - HDFS-DU is an interactive visualization of the Hadoop distributed file system.
				White Elephant - Hadoop log aggregator and dashboard
				Kiji Project
				Genie - Genie provides REST-ful APIs to run Hadoop, Hive and Pig jobs, and to manage multiple Hadoop resources and perform job submissions across them.
				Apache Kylin - Apache Kylin is an open source Distributed Analytics Engine from eBay Inc. that provides SQL interface and multi-dimensional analysis (OLAP) on Hadoop supporting extremely large datasets
				Crunch - Go-based toolkit for ETL and feature extraction on Hadoop
				Apache Ignite - Distributed in-memory platform
			YARN
				Apache Slider - Apache Slider is a project in incubation at the Apache Software Foundation with the goal of making it possible and easy to deploy existing applications onto a YARN cluster.
				Apache Twill - Apache Twill is an abstraction over Apache Hadoop® YARN that reduces the complexity of developing distributed applications, allowing developers to focus more on their application logic.
				mpich2-yarn - Running MPICH2 on Yarn
			nosql
				Apache HBase - Apache HBase
				Apache Phoenix - A SQL skin over HBase supporting secondary indices
				happybase - A developer-friendly Python library to interact with Apache HBase.
				Hannibal - Hannibal is tool to help monitor and maintain HBase-Clusters that are configured for manual splitting.
				Haeinsa - Haeinsa is linearly scalable multi-row, multi-table transaction library for HBase
				hindex - Secondary Index for HBase
				Apache Accumulo - The Apache Accumulo™ sorted, distributed key/value store is a robust, scalable, high performance data storage and retrieval system.
				OpenTSDB - The Scalable Time Series Database
				Apache Cassandra
			data management
				Apache Calcite - A Dynamic Data Management Framework
				Apache Atlas - Metadata tagging & lineage capture suppoting complex business data taxonomies
			SQL on Hadoop
				Apache Hive
				Apache Phoenix A SQL skin over HBase supporting secondary indices
				Pivotal HAWQ - Parallel Postgres on Hadoop
				Lingual - SQL interface for Cascading (MR/Tez job generator)
				Cloudera Impala
				Presto - Distributed SQL Query Engine for Big Data. Open sourced by Facebook.
				Apache Tajo - Data warehouse system for Apache Hadoop
				Apache Drill
			Workflow, Lifecycle and Governance
				Apache Oozie - Apache Oozie
				Azkaban
				Apache Falcon - Data management and processing platform
				Apache NiFi - A dataflow system
				AirFlow - AirFlow is a platform to programmaticaly author, schedule and monitor data pipelines
				Luigi - Python package that helps you build complex pipelines of batch jobs
			Data Ingestion and Integration
				Apache Flume - Apache Flume
				Suro - Netflix's distributed Data Pipeline
				Apache Sqoop - Apache Sqoop
				Apache Kafka - Apache Kafka
				Gobblin from LinkedIn - Universal data ingestion framework for Hadoop
			DSL
				Apache Pig - Apache Pig
				Apache DataFu - A collection of libraries for working with large-scale data in Hadoop
				vahara - Machine learning and natural language processing with Apache Pig
				packetpig - Open Source Big Data Security Analytics
				akela - Mozilla's utility library for Hadoop, HBase, Pig, etc.
				seqpig - Simple and scalable scripting for large sequencing data set(ex: bioinfomation) in Hadoop
				Lipstick - Pig workflow visualization tool. Introducing Lipstick on A(pache) Pig
				PigPen - PigPen is map-reduce for Clojure, or distributed Clojure. It compiles to Apache Pig, but you don't need to
			Libraries and Tools
				Kite Software Development Kit - A set of libraries, tools, examples, and documentation
				gohadoop - Native go clients for Apache Hadoop YARN.
				Hue - A Web interface for analyzing data with Apache Hadoop.
				Apache Zeppelin - A web-based notebook that enables interactive data analytics
				Jumbune - Jumbune is an open-source product built for analyzing Hadoop cluster and MapReduce jobs.
				Apache Thrift
				Apache Avro - Apache Avro is a data serialization system.
				Elephant Bird - Twitter's collection of LZO and Protocol Buffer-related Hadoop, Pig, Hive, and HBase code.
				Spring for Apache Hadoop
				hdfs - A native go client for HDFS
				Oozie Eclipse Plugin - A graphical editor for editing Apache Oozie workflows inside Eclipse.
			Realtime Data Processing
				Apache Storm
				Apache Samza
				Apache Spark
				Apache Flink - Apache Flink is a platform for efficient, distributed, general-purpose data processing. It supports exactly once stream processing.
			Distributed Computing and Programming
				Apache Spark
					Spark Packages - A community index of packages for Apache Spark
					SparkHub - A community site for Apache Spark
				Apache Crunch
				Cascading - Cascading is the proven application development platform for building data applications on Hadoop.
				Apache Flink - Apache Flink is a platform for efficient, distributed, general-purpose data processing.
				Apache Apex (incubating) - Enterprise-grade unified stream and batch processing engine.
			Packaging, Provisioning and Monitoring
				Apache Bigtop - Apache Bigtop: Packaging and tests of the Apache Hadoop ecosystem
				Apache Ambari - Apache Ambari
				Ganglia Monitoring System
				ankush - A big data cluster management tool that creates and manages clusters of different technologies.
				Apache Zookeeper - Apache Zookeeper
				Apache Curator - ZooKeeper client wrapper and rich ZooKeeper framework
				Buildoop - Hadoop Ecosystem Builder
				Deploop - The Hadoop Deploy System
				Jumbune - An open source MapReduce profiling, MapReduce flow debugging, HDFS data quality validation and Hadoop cluster monitoring tool.
				inviso - Inviso is a lightweight tool that provides the ability to search for Hadoop jobs, visualize the performance, and view cluster utilization.
			Search
				ElasticSearch
				Apache Solr
				SenseiDB - Open-source, distributed, realtime, semi-structured database
				Banana - Kibana port for Apache Solr
			Search Engine Framework
				Apache Nutch - Apache Nutch is a highly extensible and scalable open source web crawler software project.
			Security
				Apache Ranger - Ranger is a framework to enable, monitor and manage comprehensive data security across the Hadoop platform.
				Apache Sentry - An authorization module for Hadoop
				Apache Knox Gateway - A REST API Gateway for interacting with Hadoop clusters.
			Benchmark
				Big Data Benchmark
				HiBench
				Big-Bench
				hive-benchmarks
				hive-testbench - Testbench for experimenting with Apache Hive at any data scale.
				YCSB - The Yahoo! Cloud Serving Benchmark (YCSB) is an open-source specification and program suite for evaluating retrieval and maintenance capabilities of computer programs. It is often used to compare relative performance of NoSQL database management systems.
			Machine learning and Big Data analytics
				Apache Mahout
				Oryx 2 - Lambda architecture on Spark, Kafka for real-time large scale machine learning
				MLlib - MLlib is Apache Spark's scalable machine learning library.
				R - R is a free software environment for statistical computing and graphics.
				RHadoop including RHDFS, RHBase, RMR2, plyrmr
				RHive RHive, for launching Hive queries from R
				Apache Lens
		EasyHadoop
			快速部署工具
			https://github.com/xianglei/easyhadoop
	solution
		horton
			http://hortonworks.com/
		星环
			http://www.transwarp.cn/
	conf
		ApacheCon
		Strata + Hadoop World
		Hadoop Summit
			http://hadoopsummit.org/
	course
		http://www.infoq.com/cn/presentations/baidu-open-cloud-big-data-technology-evolution
