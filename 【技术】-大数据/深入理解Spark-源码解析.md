# 核心设计篇

## SparkContext初始化

​	Sparkcontext的初始化是Driver任务提交的前提。配置参数由SparkConf负责。SparkConf主要通过ConcurrentHashMap来维护各种Spark的配置属性。

​	SparkContext的主构造器参数为SparkConf。1）Utils.getCallSite()-callSite中存储最靠近栈顶的用户及最靠近栈底的Scala或Spark核心类信息；2）使用markPartiallyConstructed来确保实例的唯一性；3）对sparkconf进行复制并对各种配置信息进行配置

### 初始化步骤

#### 创建Spark执行环境SparkEnv

SparkEnv是Spark执行的环境对象，会出现在Driver或者CoarseGrainedExecutorBackend进程中，创建SparkEnv主要使用SparkEnv的createDriverEnv(conf,isLocal,listenrBus)方法，最终会调用create创建SparkEnv，其构造步骤为：

1. 创建安全管理器SecurityManager：针对权限、账号进行设置
2. 创建基于Akka的分布式消息系统ActorSystem：spark使用它发送分布式消息，又实现并发编程（自1.6.0版本Netty已经完全取代）
3. 创建Map任务输出跟踪器mapOutputTracker：MapoutputTrackerMaster内部使用mapStatuses:TImeStampedHashMap[Int,Array[MapStatus]]来维护跟踪各个map任务的输出状态。key对应ShuffleId。MapStatus中维护map输出Block的地址BlockManagerId。还使用cachedSerializedStatus：TimeStampedHashMap[Int,Array[Byte]]维护序列化之后的各个map任务的输出状态。
4. 实例化ShuffleManager：负责管理本地及远程的block数据的shuffle操作，默认通过反射方法生成SortShuffleManager实例。通过持有的IndexShuffleBlockManager间接操作BlockManager中的DiskBlockManager将map结果写入本地，并根据shuffleId、mapId写入索引文件，也能通过MapOutputTrackerMaster中维护的mapStatuses从本地或其他远程节点读取文件。
5. 创建ShuffleMemoryManager：负责shuffle线程战友内存的分配与释放。并通过threadMemory:mutable.HashMap[Long,Long]缓存每个线程的内存字节数。shuffle所有线程占用的最大内存的计算公式为：Java运行时最大内存*Spark的shuffle最大内存占比\*Spark的安全内存占比。可以修改spark.shuffle.memoryFraction修改Spark的shuffle最大内存占比，spark.shuffle.safeFraction修改安全内存占比。
6. 创建块传输服务BlockTransferService：默认为NettyBlockTransferService，使用Netty提供的异步事件驱动的网络应用框架，提供web服务及客户端，获取远程节点上Block的集合。
7. 创建BlockManagerMaster：负责对Block的管理和协调。具体依赖于BlockManagerMasterActor。
8. 创建块管理器BlockManager：负责对Block的管理
9. 创建广播管理器BroadcastManager：用于将配置信息和序列化的RDD、Job及ShuffleDependency等信息在本地存储。
10. 创建缓存管理器CacheManager：用于缓存RDD某个分区计算后的中间结果，缓存计算结果发生在迭代计算的时候。
11. 创建HTTP文件服务器HttpFileServer：主要提供对jar以及其他文件的http访问。
12. 创建测量系统MetricsSystem
13. 创建SparkEnv

#### 创建metadataCleaner



#### 创建RDD清理器metadataCleaner

#### 创建并初始化Spark UI

#### Hadoop相关配置及Executor环境变量的设置

#### 创建任务调度TaskScheduler

#### 创建和启动DAGScheduler

#### TaskScheduler的启动

#### 初始化块管理器BlockManager

#### 启动测量系统MetricsSystem

#### 创建和启动Executor分配管理器ExecutorAllocationManager

#### ContextCleaner的创建与启动

#### Spark环境更新

#### 创建DAGSchedulerSource和BlockManagerSource

#### 将SparkContext标记为激活



## 存储体系

## 任务提交和执行

## 计算引擎

## 部署模式

