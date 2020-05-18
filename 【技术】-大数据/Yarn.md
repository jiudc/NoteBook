# YARN

## 定义

​	Apache Hadoop Yarn(Yet Another Resource Negotiator)

## 基本框架

![img](https://img-blog.csdn.net/20180220205009314?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvcXFfMzM2MjQ5NTI=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

- RM：ResourceManager，负责所有资源的监控、分配和管理
  - NM以心跳的方式向RM汇报资源使用情况，主要是CPU和内存使用情况
  - Yarn Scheduler根据application的请求为其分配资源
- AM：ApplicationMaster，负责每一个具体程序的调度和协调
  - 用户提交的每个应用程序均包含一个AM，运行在RM以外的机器
  - 负责与RM获取资源（Container）
  - 将得到的资源进一步分配给内部的任务
  - 与NM
- NM：NodeManager，负责每一个节点上的维护。是每个节点上的资源和任务管理器。YARN集群每个几点
  - 定时向ResourceManager汇报本节点资源（CPU、内存）的使用情况和Container的运行状态。当RM宕机时NM自动连接到RM备用节点。
  - NM接收并处理来自AM的Container启动、停止各种请求

