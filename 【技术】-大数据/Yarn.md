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
  - 与NM通信以启动/停止任务
  - 监听所有任务运行状态，并在任务运行失败时重新为任务申请资源并重启任务
- NM：NodeManager，负责每一个节点上的维护。是每个节点上的资源和任务管理器。YARN集群每个几点
  - 定时向ResourceManager汇报本节点资源（CPU、内存）的使用情况和Container的运行状态。当RM宕机时NM自动连接到RM备用节点。
  - NM接收并处理来自AM的Container启动、停止各种请求

## 运行流程

![img](https://img-blog.csdn.net/20180315005117211)

- 步骤1　用户向YARN中提交应用程序，其中包括ApplicationMaster程序、启动ApplicationMaster的命令、用户程序等。

- 步骤2　ResourceManager为该应用程序分配第一个Container，并与对应的Node-Manager通信，要求它在这个Container中启动应用程序的ApplicationMaster。

- 步骤3　ApplicationMaster首先向ResourceManager注册，这样用户可以直接通过ResourceManager查看应用程序的运行状态，然后它将为各个任务申请资源，并监控它的运行状态，直到运行结束，即重复步骤4~7。

- 步骤4　ApplicationMaster采用轮询的方式通过RPC协议向ResourceManager申请和领取资源。

- 步骤5　一旦ApplicationMaster申请到资源后，便与对应的NodeManager通信，要求它启动任务。

- 步骤6　NodeManager为任务设置好运行环境（包括环境变量、JAR包、二进制程序等）后，将任务启动命令写到一个脚本中，并通过运行该脚本启动任务。

- 步骤7　各个任务通过某个RPC协议向ApplicationMaster汇报自己的状态和进度，以让ApplicationMaster随时掌握各个任务的运行状态，从而可以在任务失败时重新启动任务。

-   在应用程序运行过程中，用户可随时通过RPC向ApplicationMaster查询应用程序的当前运行状态。

- 步骤8　应用程序运行完成后，ApplicationMaster向ResourceManager注销并关闭自己。

## Yarn调度器Scheduler

### FIFO Scheduler

![img](https://img-blog.csdn.net/20180220212149567?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvcXFfMzM2MjQ5NTI=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

### Capacity Scheduler

![img](https://img-blog.csdn.net/20180220212307463?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvcXFfMzM2MjQ5NTI=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

### Fair Scheduler

![img](https://img-blog.csdn.net/20180220212448121?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvcXFfMzM2MjQ5NTI=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

​	调度器的使用是通过 yarn-site.xml 配置文件中的 yarn.resourcemanager.scheduler.class 参数进行配置的， 默认采用 Capacity Scheduler 调度器。资源调度器是YARN中最核心的组件之一，且是插拔式的，它定义了一整套接口规范以便用户可按照需要实现自己的调度器

​	YARN自带FIFO、Capacity Scheduler和Fair Scheduler三种常用资源调度器，当然，用户可按照接口规范编写一个新的资源调度器，并通过简单的配置使它运行起来

​	YARN的资源管理器实际上是一个事件处理器，它需要处理来自外部的6种Scheduler-EventType类型的事件，并根据事件的具体含义进行相应的处理，6种事件含义如下：

- NODE_REMOVED：表示集群中移除了一个**计算节点**（可能是节点故障或者管理员主动移除），资源调度器收到该事件时需要从可分配资源总量中移除相应的资源
- NODE_ADDED：表示集群中增加了一个**计算节点**，资源调度器收到该事件时需要将新增的资源量添加到可分配资源总量中
- APPLICATION_ADDED：表示ResourceManager收到一个新的Application。通常而言，资源管理器需要为每个Application维护一个独立的数据结构，以便于统一管理和资源分配。资源管理器将该Application添加到相应的数据结构中
- APPLICATION_REMOVED：表示一个Application运行结束（可能成功或失败），资源管理器将该Application从相应的数据结构中清除
- CONTAINER_EXPIRED：当资源调度器将一个Container分配给某个Application-Master后，如果该ApplicationMaster在一定时间内没有使用该Container，则资源调度器会对该Container进行（回收后）再分配
- NODE_UPDATE：ResourceManager收到NodeManager通过心跳机制汇报的信息后，会触发一个NODE_UPDATE事件，由于此时可能有新的Container得到释放，因此该事件会触发资源分配。也就是说，该事件是6个事件中最重要的事件，它会触发资源调度器最核心的资源分配机制

## YARN配置

### ResourceManager

| 参数名称                                                     | 作用                                                         | 默认值                                                       |
| :----------------------------------------------------------- | :----------------------------------------------------------- | :----------------------------------------------------------- |
| yarn.resourcemanager.address                                 | ResourceManager 对客户端暴露的地址。客户端通过该地址向RM提交应用程序，杀死应用程序等。 | ${yarn.resourcemanager.hostname}:8032                        |
| yarn.resourcemanager.scheduler.address                       | ResourceManager 对ApplicationMaster暴露的访问地址。ApplicationMaster通过该地址向RM申请资源、释放资源等。 | ${yarn.resourcemanager.hostname}:8030                        |
| yarn.resourcemanager.resource-tracker.address                | ResourceManager 对NodeManager暴露的地址.。NodeManager通过该地址向RM汇报心跳，领取任务等。 | ${yarn.resourcemanager.hostname}:8031                        |
| yarn.resourcemanager.admin.address                           | ResourceManager 对管理员暴露的访问地址。管理员通过该地址向RM发送管理命令等。 | ${yarn.resourcemanager.hostname}:8033                        |
| yarn.resourcemanager.webapp.address                          | ResourceManager对外web ui地址。用户可通过该地址在浏览器中查看集群各类信息。 | ${yarn.resourcemanager.hostname}:8088                        |
| yarn.resourcemanager.scheduler.class                         | 启用的资源调度器主类。目前可用的有FIFO、Capacity Scheduler和Fair Scheduler。 | org.apache.hadoop.yarn.server.resourcemanager.scheduler.capacity.CapacityScheduler |
| yarn.resourcemanager.resource-tracker.client.thread-count    | 处理来自NodeManager的RPC请求的Handler数目。                  | 50                                                           |
| yarn.resourcemanager.scheduler.client.thread-count           | 处理来自ApplicationMaster的RPC请求的Handler数目。            | 50                                                           |
| yarn.scheduler.minimum-allocation-mb/ yarn.scheduler.maximum-allocation-mb | 单个可申请的最小/最大内存资源量。比如设置为1024和3072，则运行MapRedce作业时，每个Task最少可申请1024MB内存，最多可申请3072MB内存。 | 1024/8192                                                    |
| yarn.scheduler.minimum-allocation-vcores / yarn.scheduler.maximum-allocation-vcores | 单个可申请的最小/最大虚拟CPU个数。比如设置为1和4，则运行MapRedce作业时，每个Task最少可申请1个虚拟CPU，最多可申请4个虚拟CPU。 | 1/32                                                         |
| yarn.resourcemanager.nodes.include-path /yarn.resourcemanager.nodes.exclude-path | NodeManager黑白名单。如果发现若干个NodeManager存在问题，比如故障率很高，任务运行失败率高，则可以将之加入黑名单中。注意，这两个配置参数可以动态生效。（调用一个refresh命令即可） | ""                                                           |
| yarn.resourcemanager.nodemanagers.heartbeat-interval-ms      | NodeManager心跳间隔                                          | 1000（毫秒）                                                 |

## NodeManager

| 参数名称                             | 作用                                                         | 默认值                         |
| :----------------------------------- | :----------------------------------------------------------- | :----------------------------- |
| yarn.nodemanager.resource.memory-mb  | NodeManager总的可用物理内存。注意，该参数是不可修改的，一旦设置，整个运行过程中不可动态修改。另外，该参数的默认值是8192MB，即使你的机器内存不够8192MB，YARN也会按照这些内存来使用（傻不傻？），因此，这个值通过一定要配置。不过，Apache已经正在尝试将该参数做成可动态修改的。 | 8192                           |
| yarn.nodemanager.vmem-pmem-ratio     | 每使用1MB物理内存，最多可用的虚拟内存数。                    | 2.1                            |
| yarn.nodemanager.resource.cpu-vcores | NodeManager总的可用虚拟CPU个数。                             | 8                              |
| yarn.nodemanager.local-dirs          | 中间结果存放位置，类似于1.0中的mapred.local.dir。注意，这个参数通常会配置多个目录，已分摊磁盘IO负载。 | ${hadoop.tmp.dir}/nm-local-dir |
| yarn.nodemanager.log-dirs            | 日志存放地址（可配置多个目录）。                             | ${yarn.log.dir}/userlogs       |
| yarn.nodemanager.log.retain-seconds  | NodeManager上日志最多存放时间（不启用日志聚集功能时有效）。  | 10800（3小时）                 |
| yarn.nodemanager.aux-services        | NodeManager上运行的附属服务。需配置成mapreduce_shuffle，才可运行MapReduce程序 | 默认值：""                     |

## 权限与日志聚集

### 日志聚集

| 参数名称                                           | 作用                                                         | 默认值                                                       |
| :------------------------------------------------- | :----------------------------------------------------------- | :----------------------------------------------------------- |
| yarn.log-aggregation-enable                        | 是否启用日志聚集功能。                                       | false                                                        |
| yarn.log-aggregation.retain-seconds                | 在HDFS上聚集的日志最多保存多长时间。                         | -1                                                           |
| yarn.log-aggregation.retain-check-interval-seconds | 多长时间检查一次日志，并将满足条件的删除，如果是0或者负数，则为上一个值的1/10。 | -1                                                           |
| yarn.nodemanager.remote-app-log-dir                | 当应用程序运行结束后，日志被转移到的HDFS目录（启用日志聚集功能时有效）。 | /tmp/logs                                                    |
| yarn.log-aggregation.retain-seconds                | 远程日志目录子目录名称（启用日志聚集功能时有效）。           | 日志将被转移到目录![{yarn.nodemanager.remote-app-log-dir}/](https://math.jianshu.com/math?formula=%7Byarn.nodemanager.remote-app-log-dir%7D%2F){user}/${thisParam}下 |

## MapReduce

本节所有配置都写在mapred-site.xml中

### MapReduce JobHistory

| 参数名称                                   | 作用                                       | 默认值           |
| :----------------------------------------- | :----------------------------------------- | :--------------- |
| mapreduce.jobhistory.address               | MapReduce JobHistory Server地址。          | 0.0.0.0:10020    |
| mapreduce.jobhistory.webapp.address        | MapReduce JobHistory Server Web UI地址。   | 0.0.0.0:19888    |
| mapreduce.jobhistory.intermediate-done-dir | MapReduce作业产生的日志存放位置。          | /mr-history/tmp  |
| mapreduce.jobhistory.done-dir              | MR JobHistory Server管理的日志的存放位置。 | /mr-history/done |

### MapReduce作业配置

下面的参数可以配置在客户端的mapred-site.xml中，作为MapReduce作业的缺省配置。也可以在提交作业时单独指定这些参数

| 参数名称                                  | 说明                                            | 缺省值  |
| :---------------------------------------- | :---------------------------------------------- | :------ |
| mapreduce.job.name                        | 作业名称                                        |         |
| mapreduce.job.priority                    | 作业优先级                                      | NORMAL  |
| yarn.app.mapreduce.am.resource.mb         | ApplicationMaster占用的内存量                   | 1536    |
| yarn.app.mapreduce.am.resource.cpu-vcores | ApplicationMaster占用的虚拟CPU个数              | 1       |
| mapreduce.am.max-attempts                 | ApplicationMaster最大失败尝试次数               | 2       |
| mapreduce.map.memory.mb                   | 每个Map Task需要的内存量                        | 1024    |
| mapreduce.map.cpu.vcores                  | 每个Map Task需要的虚拟CPU个数                   | 1       |
| mapreduce.map.maxattempts                 | Map Task最大失败尝试次数                        | 4       |
| mapreduce.reduce.memory.mb                | 每个Reduce Task需要的内存量                     | 1024    |
| mapreduce.reduce.cpu.vcores               | 每个Reduce Task需要的虚拟CPU个数                | 1       |
| mapreduce.reduce.maxattempts              | Reduce Task最大失败尝试次数                     | 4       |
| mapreduce.map.speculative                 | 是否对Map Task启用推测执行机制                  | FALSE   |
| mapreduce.reduce.speculative              | 是否对Reduce Task启用推测执行机制               | FALSE   |
| mapreduce.job.queuename                   | 作业提交到的队列                                | default |
| mapreduce.task.io.sort.mb                 | 任务内部排序缓冲区大小                          | 100     |
| mapreduce.map.sort.spill.percent          | Map阶段溢写文件的阈值（排序缓冲区大小的百分比） | 0.8     |
| mapreduce.reduce.shuffle.parallelcopies   | Reduce Task启动的并发拷贝数据的线程数目         | 5       |

## Fair Scheduler相关参数

本节的参数需要在yarn-site.xml中，将配置参数yarn.resourcemanager.scheduler.class设置为org.apache.hadoop.yarn.server.resourcemanager.scheduler.fair.FairScheduler 才会生效。Fair Scheduler的配置选项包括两部分，其中一部分在yarn-site.xml中，主要用于配置调度器级别的参数，另外一部分在一个自定义配置文件（默认是fair-scheduler.xml）中，主要用于配置各个队列的资源量、权重等信息。

### yarn-site.xml

| 参数名称                                    | 说明                                                         | 缺省值                                                       |
| :------------------------------------------ | :----------------------------------------------------------- | :----------------------------------------------------------- |
| yarn.scheduler.fair.allocation.file         | 自定义XML配置文件所在位置，该文件主要用于描述各个队列的属性，比如资源量、权重等，具体配置格式将在后面介绍。 |                                                              |
| yarn.scheduler.fair.user-as-default-queue   | 当应用程序未指定队列名时，是否指定用户名作为应用程序所在的队列名。如果设置为false或者未设置，所有未知队列的应用程序将被提交到default队列中 | true                                                         |
| yarn.scheduler.fair.preemption              | 是否启用抢占机制                                             | false                                                        |
| yarn.scheduler.fair.sizebasedweight         | 在一个队列内部分配资源时，默认情况下，采用公平轮询的方法将资源分配各各个应用程序，而该参数则提供了另外一种资源分配方式：按照应用程序资源需求数目分配资源，即需求资源数量越多，分配的资源越多。 | false                                                        |
| yarn.scheduler.assignmultiple               | 是否启动批量分配功能。当一个节点出现大量资源时，可以一次分配完成，也可以多次分配完成。 | false                                                        |
| yarn.scheduler.fair.max.assign              | 如果开启批量分配功能，可指定一次分配的container数目。        | -1，表示不限制。                                             |
| yarn.scheduler.fair.locality.threshold.node | 当应用程序请求某个节点上资源时，它可以接受的可跳过的最大资源调度机会。当按照分配策略，可将一个节点上的资源分配给某个应用程序时，如果该节点不是应用程序期望的节点，可选择跳过该分配机会暂时将资源分配给其他应用程序，直到出现满足该应用程序需的节点资源出现。通常而言，一次心跳代表一次调度机会，而该参数则表示跳过调度机会占节点总数的比例 | -1.0，表示不跳过任何调度机会                                 |
| yarn.scheduler.fair.locality.threshold.rack | 当应用程序请求某个机架上资源时，它可以接受的可跳过的最大资源调度机会。 |                                                              |
| yarn.scheduler.increment-allocation-mb      | 内存规整化单位                                               | 默认是1024，这意味着，如果一个Container请求资源是1.5GB，则将被调度器规整化为ceiling(1.5 GB / 1GB) * 1G=2GB。 |
| yarn.scheduler.increment-allocation-vcores  | 虚拟CPU规整化单位                                            | 默认是1，含义与内存规整化单位类似。                          |

### 自定义配置文件

Fair Scheduler允许用户将队列信息专门放到一个配置文件（默认是fair-scheduler.xml），对于每个队列，管理员可配置以下几个选项：

| 参数名称                        | 说明                                                         | 缺省值 |
| :------------------------------ | :----------------------------------------------------------- | :----- |
| minResources                    | 最少资源保证量，设置格式为“X mb, Y vcores”，当一个队列的最少资源保证量未满足时，它将优先于其他同级队列获得资源，对于不同的调度策略（后面会详细介绍），最少资源保证量的含义不同，对于fair策略，则只考虑内存资源，即如果一个队列使用的内存资源超过了它的最少资源量，则认为它已得到了满足；对于drf策略，则考虑主资源使用的资源量，即如果一个队列的主资源量超过它的最少资源量，则认为它已得到了满足。 |        |
| maxResources                    | 最多可以使用的资源量，fair scheduler会保证每个队列使用的资源量不会超过该队列的最多可使用资源量。 |        |
| maxRunningApps                  | 最多同时运行的应用程序数目。通过限制该数目，可防止超量Map Task同时运行时产生的中间输出结果撑爆磁盘。 |        |
| minSharePreemptionTimeout       | 最小共享量抢占时间。如果一个资源池在该时间内使用的资源量一直低于最小资源量，则开始抢占资源。 |        |
| schedulingMode/schedulingPolicy | 队列采用的调度模式，可以是fifo、fair或者drf。                |        |
| aclSubmitApps                   | 可向队列中提交应用程序的Linux用户或用户组列表，默认情况下为“*”，表示任何用户均可以向该队列提交应用程序。需要注意的是，该属性具有继承性，即子队列的列表会继承父队列的列表。配置该属性时，用户之间或用户组之间用“，”分割，用户和用户组之间用空格分割，比如“user1, user2 group1,group2”。 |        |
| aclAdministerApps               | 该队列的管理员列表。一个队列的管理员可管理该队列中的资源和应用程序，比如可杀死任意应用程序。 |        |

## Capacity Scheduler相关参数

在Capacity Scheduler的配置文件中，队列queueX的参数Y的配置名称为yarn.scheduler.capacity.queueX.Y，为了简单起见，我们记为Y，则每个队列可以配置的参数如下：

### 资源分配相关参数

| 参数名称                   | 说明                                                         | 缺省值 |
| :------------------------- | :----------------------------------------------------------- | :----- |
| capacity                   | 队列的资源容量（百分比）。 当系统非常繁忙时，应保证每个队列的容量得到满足，而如果每个队列应用程序较少，可将剩余资源共享给其他队列。注意，所有队列的容量之和应小于100。 |        |
| maximum-capacity           | 队列的资源使用上限（百分比）。由于存在资源共享，因此一个队列使用的资源量可能超过其容量，而最多使用资源量可通过该参数限制。 |        |
| minimum-user-limit-percent | 每个用户最低资源保障（百分比）。任何时刻，一个队列中每个用户可使用的资源量均有一定的限制。当一个队列中同时运行多个用户的应用程序时中，每个用户的使用资源量在一个最小值和最大值之间浮动，其中，最小值取决于正在运行的应用程序数目，而最大值则由minimum-user-limit-percent决定。比如，假设minimum-user-limit-percent为25。当两个用户向该队列提交应用程序时，每个用户可使用资源量不能超过50%，如果三个用户提交应用程序，则每个用户可使用资源量不能超多33%，如果四个或者更多用户提交应用程序，则每个用户可用资源量不能超过25%。 |        |
| user-limit-factor          | 每个用户最多可使用的资源量（百分比）。比如，假设该值为30，则任何时刻，每个用户使用的资源量不能超过该队列容量的30%。 |        |

### 现在应用程序数目相关参数

| 参数名称                    | 说明                                                         | 缺省值       |
| :-------------------------- | :----------------------------------------------------------- | :----------- |
| maximum-applications        | 集群或者队列中同时处于等待和运行状态的应用程序数目上限，这是一个强限制，一旦集群中应用程序数目超过该上限，后续提交的应用程序将被拒绝，所有队列的数目上限可通过参数yarn.scheduler.capacity.maximum-applications设置（可看做默认值），而单个队列可通过参数yarn.scheduler.capacity.<queue-path>.maximum-applications设置适合自己的值。 | 10000        |
| maximum-am-resource-percent | 集群中用于运行应用程序ApplicationMaster的资源比例上限，该参数通常用于限制处于活动状态的应用程序数目。该参数类型为浮点型。所有队列的ApplicationMaster资源比例上限可通过参数yarn.scheduler.capacity. maximum-am-resource-percent设置（可看做默认值），而单个队列可通过参数yarn.scheduler.capacity.<queue-path>. maximum-am-resource-percent设置适合自己的值。 | 0.1，表示10% |

### 队列访问和权限控制参数

| 参数名称                | 说明                                                         | 缺省值 |
| :---------------------- | :----------------------------------------------------------- | :----- |
| state                   | 队列状态可以为STOPPED或者RUNNING，如果一个队列处于STOPPED状态，用户不可以将应用程序提交到该队列或者它的子队列中，类似的，如果ROOT队列处于STOPPED状态，用户不可以向集群中提交应用程序，但正在运行的应用程序仍可以正常运行结束，以便队列可以优雅地退出。 |        |
| acl_submit_applications | 限定哪些Linux用户/用户组可向给定队列中提交应用程序。需要注意的是，该属性具有继承性，即如果一个用户可以向某个队列中提交应用程序，则它可以向它的所有子队列中提交应用程序。配置该属性时，用户之间或用户组之间用“，”分割，用户和用户组之间用空格分割，比如“user1, user2 group1,group2”。 |        |
| acl_administer_queue    | 为队列指定一个管理员，该管理员可控制该队列的所有应用程序，比如杀死任意一个应用程序等。同样，该属性具有继承性，如果一个用户可以向某个队列中提交应用程序，则它可以向它的所有子队列中提交应用程序。 |        |