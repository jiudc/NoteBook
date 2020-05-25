# Spark

## Spark组成

Spark(BDAS)：全称伯克利数据分析栈，通过大规模集成算法、机器、人之间展现大数据应用的平台。

- SparkCore
- SparkSQL
- SparkStreaming
- MLib
- GraphX
- BlinkDB
- Tachyon

## RDD

Resilient Distributed DataSet，弹性分布式数据集。代表一个不可变、可分区、里面的元素可并行计算的集合。

### RDD属性

- 一组分片(Partition)，数据集的基本组成单位。每个分片会被一个计算任务处理，并决定并行计算的粒度。可在创建RDD时指定RDD的分片个数，没有指定则会采用默认值。默认值为程序所分配到的CPU Core的数目。
- 一个计算每个分区的函数。Spark中RDD的计算是以分片为单位的，每个RDD都会实现computer函数已达到这个目的。
- RDD之间的依赖关系。
- 一个Partitioner，即RDD的分片函数。只有基于key-value的RDD，才会有Partitioner，非key-value的RDD的 的值是None。Partitioner不但决定了RDD本身的分片数量，也决定了parent RDD Shuffle输出时的分片数量。
	- 基于哈希的HashPartitioner
	- 基于范围的RangePartitioner
- 一个列表，存储存取每个Partition的有限位置。对于一个HDFS文件来说，这个列表保存的是每个Partition所在块的位置。Spark“移动数据不如移动计算”的理念，在进行任务调度时，会尽可能将计算任务分配到其所要处理数据块的存储位置。

### RDD创建方式

- 通过读取文件生成：sc.texeFile(PATH)
- 通过并行化的方式创建RDD：sc.parallelize(array)
- 读取数据库等其他操作

### API

- Transformation：
	- map(func): 返回一个新的RDD，该RDD由每一个输入元素经过func函数转换后组成
	- filter(func): 返回一个新的RDD，该RDD由经过func函数计算后返回值为true的输入元素组成
	- flatMap(func): 类似于map，但是每一个输入元素可以被映射为0或多个输出元素（所以func应该返回一个序列，而不是单一元素）
	- mapPartitions(func): 类似于map，但独立地在RDD的每一个分片上运行，因此在类型为T的RDD上运行时，func的函数类型必须是Iterator[T] => Iterator[U]
	- mapPartitionsWithIndex(func): 类似于mapPartitions，但func带有一个整数参数表示分片的索引值，因此在类型为T的RDD上运行时，func的函数类型必须是(Int, Interator[T]) => Iterator[U]
	- sample(withReplacement, fraction, seed): 根据fraction指定的比例对数据进行采样，可以选择是否使用随机数进行替换，seed用于指定随机数生成器种子
	- union(otherDataset): 对源RDD和参数RDD求并集后返回一个新的RDD
	- intersection(otherDataset): 对源RDD和参数RDD求交集后返回一个新的RDD
	- distinct([numTasks])): 对源RDD进行去重后返回一个新的RDD
	- groupByKey([numTasks]): 在一个(K,V)的RDD上调用，返回一个(K, Iterator[V])的RDD
	- reduceByKey(func, [numTasks])：在一个(K,V)的RDD上调用，返回一个(K,V)的RDD，使用指定的reduce函数，将相同key的值聚合到一起，与groupByKey类似，reduce任务的个数可以通过第二个可选的参数来设置
	- aggregateByKey(zeroValue,seqOp, combOp, [numTasks])：先按分区聚合，再总聚合 。每次要跟初始值交流 例如：aggregateByKey(0)(_+_,_+_) 对k/y的RDD进行操作。
		- zeroValue:初始值
		- seqOp：combine的聚合逻辑，针对每个分区的操作
		- combOp：reduce端大聚合的逻辑
	- sortByKey([ascending], [numTasks])：在一个(K,V)的RDD上调用，K必须实现Ordered接口，返回一个按照key进行排序的(K,V)的RDD
	- sortBy(func,[ascending], [numTasks]):与sortByKey类似，但是更灵活。第一个参数是根据什么排序 ，第二个是怎么排序（false倒序）。第三个排序后分区数 ，默认与原RDD一样
	- join(otherDataset, [numTasks])：在类型为(K,V)和(K,W)的RDD上调用，返回一个相同key对应的所有元素对在一起的(K,(V,W))的RDD 相当于内连接（求交集）
	- cogroup(otherDataset, [numTasks])：在类型为(K,V)和(K,W)的RDD上调用，返回一个(K,(Iterable<V>,Iterable<W>))类型的RDD
	- cartesian(otherDataset)：两个RDD的笛卡尔积 的成很多个K/V
	- pipe(command, [envVars])：调用外部程序
	- coalesce(numPartitions,shuffle): 重新分区。第一个参数是要分多少区，第二个参数是否shuffle，默认false 。少分区变多分区true，多分区变少分区false。
	- coalesce(shuffle=true)==repartition()
		- 不经过shuffle不可能使分区增多
	- repartition(numPartitions): 重新分区必须shuffle。参数是要分多少区、少变多
	- repartitionAndSortWithinPartitions(partitioner)：重新分区+排序 。比先分区再排序效率高，对K/V的RDD进行操作
	- foldByKey(zeroValue,seqOp)：该函数用于K/V做折叠，合并处理 ，与aggregate类似。第一个括号的参数应用于每个V值 第二括号函数是聚合例如：_+_
	- combineByKey(createCombiner, mergeValue, mergeCombiners)：合并相同的key的值 rdd1.combineByKey(x => x, (a: Int, b: Int) => a + b, (m: Int, n: Int) => m + n)
		- createCombiner:给定一个初始值，用函数生成初始值。作用于每一个组的第一个元素上
		- combine:combine端聚合逻辑，在每个分区类进行
		- reduce:reduce端聚合逻辑，会把相同的key的数据拉到一个节点，并进行分组，在不同分区间进行
	- partitionBy(partitioner): 对RDD进行分区 partitioner是分区器
	- cache\persist: RDD缓存，可以避免重复计算从而减少时间，区别：cache内部调用了persist算子，cache默认就一个缓存级别MEMORY-ONLY，而persist则可以选择缓存级别
	- subtract（rdd）: 返回前rdd元素不在后rdd的rdd
	- leftOuterJoin: leftOuterJoin类似于SQL中的左外关联left outer join，返回结果以前面的RDD为主，关联不上的记录为空。只能用于两个RDD之间的关联，如果要多个RDD关联，多关联几次即可。
	- rightOuterJoin: rightOuterJoin类似于SQL中的有外关联right outer join，返回结果以参数中的RDD为主，关联不上的记录为空。只能用于两个RDD之间的关联，如果要多个RDD关联，多关联几次即可
	- subtractByKey: substractByKey和基本转换操作中的subtract类似只不过这里是针对K的，返回在主RDD中出现，并且不在otherRDD中出现的元素
- Action:
	- reduce(func): 通过func函数聚集RDD中的所有元素，这个功能必须是可交换且可并联的
	- collect()：在驱动程序中，以数组的形式返回数据集的所有元素
	- count()：返回RDD的元素个数
	- first()：返回RDD的第一个元素（类似于take(1)）
	- take(n)：返回一个由数据集的前n个元素组成的数组
	- takeSample(withReplacement,num, [seed])：返回一个数组，该数组由从数据集中随机采样的num个元素组成，可以选择是否用随机数替换不足的部分，seed用于指定随机数生成器种子
	- takeOrdered(n, [ordering])：按自然顺序或者指定的排序规则返回前n个元素
	- saveAsTextFile(path)：将数据集的元素以textfile的形式保存到HDFS文件系统或者其他支持的文件系统，对于每个元素，Spark将会调用toString方法，将它装换为文件中的文本
	- saveAsSequenceFile(path) ：将数据集中的元素以Hadoop sequencefile的格式保存到指定的目录下，可以使HDFS或者其他Hadoop支持的文件系统。
	- saveAsObjectFile(path) ：用于将RDD中的元素序列化成对象，存储到文件中。 
	- countByKey()：针对(K,V)类型的RDD，返回一个(K,Int)的map，表示每一个key对应的元素个数。
	- foreach(func)：在数据集的每一个元素上，运行函数func进行更新。
	- aggregate
	- reduceByKeyLocally：该函数将RDD[K,V]中每个K对应的V值根据映射函数来运算，运算结果映射到一个Map[K,V]中，而不是RDD[K,V]
	- lookup(k)：作用于K-V类型的RDD上，返回指定K的所有V值
	- top(n)：按默认或者指定的排序规则返回前n个元素，默认按降序输出
	- fold
	-  foreachPartition：这个函数也是根据传入的function进行处理，但是不同之处再有这里function传入的参数是一个partition对应数据的iterator
- 补充说明：
	- map按条顺次处理，mapPartitions按分区处理

## 基本概念

- Application：应用程序
- Driver：main()函数，创建SparkContext。由SparkContext负责与ClusterManager通信，进行资源申请，任务分配和监控。程序执行完毕后关闭SparkContext
- Executor：某个Application运行在Worker节点上的一个进程，该进行负责运行某些task，并且负责将数据存在内存或者磁盘上。在Spark on Yarn模式下，其进程名称为CoarseGrainedExecutor Backend，有且仅有一个executor对象，它负责将Task包装成taskRunner，并从线程池中抽取一个空闲线程运行Task，并行运行Task的数据就取决于分配给它的CPU个数
- Worker：集群中可以运行Application代码的节点。Standalone模式中指通过slave文件配置的worker节点，在Spark on Yarn模式中指的是NodeManager节点
- Task：在Executor进程中执行任务的工作单元，多个Task组成一个Stage
  - ShuffleMapTask：
  - ResultTask：
- Stage：每个Job会被拆分为很多组Task，作为一个TaskSet，名称为Stage
- DAGSchedule：根据job构建基于Stage的DAG，并提交Stage给TaskScheduler，其划分Stage的依赖是RDD之间的依赖关系
- TaskScheduler：将TaskSet提交给Worker（集群）运行，每个Executor运行什么Task就是在此处分配的

## 运行流程

- 构建Spark Application运行环境（启动SparkContext），SparkContext向资源管理器注册并申请运行Executor资源
- 资源管理器分配Executor资源并启动StandaloneExecutorBackend，Executor运行情况将随着心跳发送到资源管理器上
- SparkContext构建DAG，将DAG分解为Stage，并把Taskset发送给TaskScheduler。Executor向SparkContext申请Task
- TaskScheduler将Task发放给Executor运行同时SparkContext将应用程序程序发放给Executor
- Task在Executor上运行，运行完毕释放所有资源                       

## 运行特点

- 每个Application获取专属的executor进程，该进程在Application期间一直驻留，并以多线程方式运行tasks。Spark Application不能跨应用共享数据。
- Spark与资源管理器无关，只要能够获取executor进程，并能保持互相通信即可
- 提交SparkContext的Client应该尽量靠近Worker节点，最好在同一个Rack里，因为Spark Application运行过程中SparkContext和Executor之间有大量的信息交换；如果想在远程集群中运行，最好使用RPC将SparkContext提交给集群
- Task采用了数据本地性和推测执行的优化机制

## 监控

- 修改spark/conf/spark-defalut.conf的eventLog.enabled和dir
- 配置spark-env.sh的SPARK_HISTORY_OPTS的相关参数
- 启动sbin/start-history-server.sh

## DAG

​	在Spark中，会根据RDD之间的依赖关系将DAG划分为不同的阶段。

- 对于窄依赖，由于partition依赖关系的确定性，partition的转换处理就可以在同一个线程中完成，窄依赖就被spark划分到同一个stage中。
- 对于宽依赖，只能等父RDD shuffle处理完成后，下一个stage才开始接下来的计算。

### DAGScheduler

Job=多个stage，Stage=多个同种的task。Task分为ShuffleMapTask和ResultTask，Dependency分为ShuffleMapDependency和NarrowDependency。

- 接收提交Job的主入口，submitJob(rdd,…)或runJob(rdd,…)
	- 生成一个Stage并提交，判断是否有父Stage未完成，若有，提交并等待父Stage。结果是：DAGScheduler里增加了一些waiting stage和一个running stage。
	- running stage提交后，分析stage里Task的类型，生成一个Task描述，即TaskSet
	- 调用TaskScheduler.submitTask(taskset,…)方法，把Task描述提交给TaskScheduler。TaskScheduler依据资源量和触发分配条件，会为这个TaskSet分配资源并触发执行。
	- DAGScheduler提交job后，异步返回JobWaiter对象，能够返回job运行状态，能够cancel job，执行成功后处理并返回结果
- 处理TaskCompletionEvent 
	- 如果task执行成功，对应的stage里减去这个task
		- 如果task是ResultTask，计数器Accumulator加一，在job里为该task置true，job finish总数加一。加完后如果finish数目与partition数目相等，说明这个stage完成了，标记stage完成，从running stages里减去这个stage，做一些stage移除的清理工作
		- 如果task是ShuffleMapTask，计数器Accumulator加一，在stage里加上一个output location，里面是一个MapStatus类。MapStatus是ShuffleMapTask执行完成的返回，包含location信息和block size(可以选择压缩或未压缩)。同时检查该stage完成，向MapOutputTracker注册本stage里的shuffleId和location信息。然后检查stage的output location里是否存在空，若存在空，说明一些task失败了，整个stage重新提交；否则，继续从waiting stages里提交下一个需要做的stage
	- 如果task是重提交，对应的stage里增加这个task
	- 如果task是fetch失败，马上标记对应的stage完成，从running stages里减去。如果不允许retry，abort整个stage；否则，重新提交整个stage。另外，把这个fetch相关的location和map任务信息，从stage里剔除，从MapOutputTracker注销掉。最后，如果这次fetch的blockManagerId对象不为空，做一次ExecutorLost处理，下次shuffle会换在另一个executor上去执行。
	- 其他task状态会由TaskScheduler处理，如Exception, TaskResultLost, commitDenied等。

### TaskScheduler

​	维护task和executor对应关系、executor和物理资源对应关系、在排队的task和正在跑的task。内部维护一个任务队列，FIFO或Fair策略，调度任务。

### 划分stage

​	从后往前推，遇到宽依赖就断开，划分为一个stage；遇到窄依赖就将这个RDD加入该stage中。

​	Task的类型分为：ShuffleMapTask和ResultTask。

​	DAG的最后一个阶段会为每个结果的partition生成一个ResultTask，即每个stage里面的Task数量是由该Stage中最后一个RDD的Partition数量所决定的。其余所有阶段都会生成ShuffleMapTask；被称为ShuffleMapTask是因为它需要将自己的计算结果通过shuffle到下一个stage中。

- submitTasks(taskSet)，接收DAGScheduler提交来的tasks
	-  为tasks创建一个TaskSetManager，添加到任务队列里。TaskSetManager跟踪每个task的执行状况，维护了task的许多具体信息。
	- 触发一次资源的索要。 
	- 首先，TaskScheduler对照手头的可用资源和Task队列，进行executor分配(考虑优先级、本地化等策略)，符合条件的executor会被分配给TaskSetManager。
	- 然后，得到的Task描述交给SchedulerBackend，调用launchTask(tasks)，触发executor上task的执行。task描述被序列化后发给executor，executor提取task信息，调用task的run()方法执行计算。
- cancelTasks(stageId)，取消一个stage的tasks 
	- 调用SchedulerBackend的killTask(taskId, executorId, ...)方法。taskId和executorId在TaskScheduler里一直维护着。
- resourceOffer(offers: Seq[Workers])，这是非常重要的一个方法，调用者是SchedulerBacnend，用途是底层资源SchedulerBackend把空余的workers资源交给TaskScheduler，让其根据调度策略为排队的任务分配合理的cpu和内存资源，然后把任务描述列表传回给SchedulerBackend 
	- 从worker offers里，搜集executor和host的对应关系、active executors、机架信息等等
	- worker offers资源列表进行随机洗牌，任务队列里的任务列表依据调度策略进行一次排序
	- 遍历每个taskSet，按照进程本地化、worker本地化、机器本地化、机架本地化的优先级顺序，为每个taskSet提供可用的cpu核数，看是否满足 
		- 默认一个task需要一个cpu，设置参数为"spark.task.cpus=1"
		- 为taskSet分配资源，校验是否满足的逻辑，最终在TaskSetManager的resourceOffer(execId, host, maxLocality)方法
		-  满足的话，会生成最终的任务描述，并且调用DAGScheduler的taskStarted(task, info)方法，通知DAGScheduler，这时候每次会触发DAGScheduler做一次submitMissingStage的尝试，即stage的tasks都分配到了资源的话，马上会被提交执行
- statusUpdate(taskId, taskState, data),另一个非常重要的方法，调用者是SchedulerBacnend，用途是SchedulerBacnend会将task执行的状态汇报给TaskScheduler做一些决定 
	- 若TaskLost，找到该task对应的executor，从active executor里移除，避免这个executor被分配到其他task继续失败下去。
	- task finish包括四种状态：finished, killed, failed, lost。只有finished是成功执行完成了。其他三种是失败。
	- task成功执行完，调用TaskResultGetter.enqueueSuccessfulTask(taskSet, tid, data)，否则调用TaskResultGetter.enqueueFailedTask(taskSet, tid, state, data)。TaskResultGetter内部维护了一个线程池，负责异步fetch task执行结果并反序列化。默认开四个线程做这件事，可配参数"spark.resultGetter.threads"=4
- TaskResultGetter取task result的逻辑
	- 对于success task，如果taskResult里的数据是直接结果数据，直接把data反序列出来得到结果；如果不是，会调用blockManager.getRemoteBytes(blockId)从远程获取。如果远程取回的数据是空的，那么会调用TaskScheduler.handleFailedTask，告诉它这个任务是完成了的但是数据是丢失的。否则，取到数据之后会通知BlockManagerMaster移除这个block信息，调用TaskScheduler.handleSuccessfulTask，告诉它这个任务是执行成功的，并且把result data传回去。
	- 对于failed task，从data里解析出fail的理由，调用TaskScheduler.handleFailedTask，告诉它这个任务失败了，理由是什么。

### SchedulerBackend

### 宽依赖(Wide Dependencies,Shuffle Dependencies)

重新Shuffle，1:n或者m:n。

- groupByKey
- join(父RDD不是hash-partitioned)
- partitionBy

### 窄依赖（Narrow Dependencies）

- map
- filter
- union
- join(父RDD是hash-partitioned)
- mapPartitions
- mapValues

## 广播变量

​	如果分布式计算里面分发的大对象不是广播变量，那么每个task就会分发一份，这在task数目十分多的情况下Driver的带宽会成为系统的瓶颈，而且会大量消耗task服务商的资源。如果申明为广播变量，那么每个executor拥有一份，这个executor启动的task会共享这个变量。

### 注意事项

- 广播变量只能读，不能修改
- RDD不能广播，因为RDD不存储数据，可以将RDD的结构广播出去
- 广播变量只能在Driver端定义，不能再Executor端定义
- 在Driver端可以修改广播变量的值，在Executor端无法修改
- Executor端用到了Drvier的变量，如果不使用广播变量在Executor中有多少个task就有多少Driver端的变量副本
- Executor端用到了Drvier的变量，如果使用广播变量在Executor中有一个Driver端的变量副本

### 累加器

​	累加器在Driver端定义赋初始值，累加器只能在Driver端读取最后的值，在executor

端更新。

## RDD容错

### lineage

​	每个RDD都包含了其是如何由其他RDD变换过来的以及如何重建某一块数据的信息。

​	Lineage容错原理：在容错机制中，如果一个节点宕机，针对于窄依赖，只要把丢失的父RDD分区重算，不存在冗余计算；宽依赖会有冗余计算开销。

### checkpoint

​	针对以下两种情况，RDD加上检查点：

- DAG的Lineage过长，如果重算，开销太
-  在宽依赖上做checkpoint获得的收益更大

​	RDD的doCheckPoint方法相当于通过冗余数据来缓存数据。实际CheckPoint使用方法：

```scala
sc.setCheckPointDir(“/tmp/spark/checkpoint”)
data.checkpoint
```

checkpoint写流程：Initialized->marked for checkpointiong->checkpointing in progress->checkpointed

## 数据倾斜

### 原因分析

- 程序上distinct、group by、join会触发shuffle，若某个key对应的数据量特别大就会发生数据倾斜
- 数据上，若表中若出现一张表的key存在大量的null值或相同一个数据，则会数据分布的时候倾斜
- 业务上，业务发展不平衡

### 现象

- 大多数Task执行会很快，当发生数据倾斜后的task会执行很长时间
- 有时候直接报OOM：JVM Out of Memory

### 定位

​	数据倾斜多发生在shuffle阶段，需要留意会触发shuffle的算子：distinct、groupByKe、reduceByKey、aggregateByKey、join、cogroup、repartition

- 某个task执行特别慢
	- 确定哪个stage导致，通过log或者Spark Web UI查看stage中task分配的数据量
	- 确定缓慢的stage，根据stage划分原理推断代码
- 某个task内存溢出
	- 从log中找到异常栈，异常栈一般会标明程序出错的位置
	- 在内存溢出代码位置附近找会触发shuffle的操作

## 调优

### 开发调优

- 设置并行度

  - 在会产生shuffle的操作函数内设置并行度参数
  - 读取文件增加并行度参数

- DAG设计

- 避免创建重复的RDD

- 尽可能复用同一个RDD

- 对多次使用的RDD进行持久化：调用cache()或persist()，建议选择顺序为MEMORY_ONLY>MEMORY_ONLY_SER>MEMORY_AND_DISK_SER

  - rdd.persist(StorageLevel.MEMORY_ONLY)
  - 可能出现OOM

- 尽量避免使用shuffle算子

  - 重分区算子
    - repartition
    - coalesce
  - ByKey算子
    - groupByKey
    - reduceByKey
    - aggregateByKey
    - combineByKey
    - sortByKey
    - sortBy
  - Join算子
    - cogroup
    - join
    - leftOuterJoin
    - intersection
    - subtract
    - subtractByKey
  - 去重算子
    - distinct
  - 举例
    - 使用Broadcast与map代替join操作，针对数据集在到1、2G大小

- 使用map-site预聚合的shuffle操作：尽量使用map-side预聚合算子（如reduceByKey或aggregateByKey代替groupByKey），map-side预聚合使

- 使用高性能的算子
  - 使用aggregateByKey/reduceByKey代替groupByKey
  - 使用mapPartitions代替map
    - 一次函数调用处理一个partition所有数据，可能OOM
  - 使用foreachPartitions代替foreach
  - 使用filter之后进行coalesce
    - 增加分区：coalesce(number,true)和reparation(num)
    - 较少分区：coalesce(number,false)
  - 使用repartitionAndSortWithinPartitions代替repartition和sort

- 广播大变量：可保证每个executor内存中只保留一份变量，否则每个task都有

  - 使用set/map(O(1))代替数组，Iterator（O(n)）

- 使用Kryo优化序列化性能

  - ```scala
    // 创建SparkConf对象。
    val conf = new SparkConf().setMaster(...).setAppName(...)
    // 设置序列化器为KryoSerializer。
    conf.set("spark.serializer", "org.apache.spark.serializer.KryoSerializer")
    // 注册要序列化的自定义类型。
    conf.registerKryoClasses(Array(classOf[MyClass1], classOf[MyClass2]))
    ```

### 集群资源调优

- Driver
- Executor
  - task执行代码，20%
  - 通过shuffle拉去上一个stage的task输出，20%
  - 持久化，60%
- 参数
  - num-executors:spark.executor.instance(50~100)
  - execute-memory:spark.executor.memory
  - executor-cores:spark.executor.cores(5)
  - driver-memory:dpark.driver.memory
  - spark.default.parallelism(500~1000)
  - spark.storage.memoryFraction:0.6,持久化数据内存占比。频繁gc可降低该值
  - spark.shuffle.memoryFraction:0.2，shuffle聚合操作的内存占比
- 配置内存
  - spark.driver.extraJavaOptions和spark.executor.extraJavaOptions选项中增加参数：
    - "-verbose:gc -XX:+PrintGCDetails -XX:+PrintGCTimeStamps"
    - 若频繁出现full gc则需要优化
      - 优化GC，调整老年代和新生代的大小和比例。在客户端的conf/spark-
        default.conf配置文件中，在spark.driver.extraJavaOptions和
        spark.executor.extraJavaOptions配置项中添加参数：-XX:NewRatio。如，" -
        XX:NewRatio=2"，则新生代占整个堆空间的1/3，老年代占2/3。
      - 开发Spark应用程序时，优化RDD的数据结构。
        – 使用原始类型数组替代集合类，如可使用fastutil库。
        – 避免嵌套结构。
        – Key尽量不要使用String。
      -  开发Spark应用程序时，建议序列化RDD。
        RDD做cache时默认是不序列化数据的，可以通过设置存储级别来序列化RDD减小
        内存。例如：
        testRDD.persist(StorageLevel.MEMORY_ONLY_SER)
- Yarn平台动态资源调度
  - 配置External shuffle service
  - spark.dynamicAllocation.enabled修改为ture
  - 

### 数据倾斜

1. 个别task执行极慢
2. 正常的作业，突然报出OOM

- 采样拆分
  - 采样将数据倾斜的键值的数据生成新的RDD

### Shuffle优化

1. shuffle wirte:一个stage结束计算后，为下一个stage可以执行shuffle类的算子，而将每个task处理的数据按key进行分类。数据写入内存缓存中，当内存缓冲满之后溢出写到磁盘文件中。每个task写入磁盘文件数由下一个任务的task决定。
2. shuffle read:将上一个stage中计算结果中的所有相同key，从各个节点通过网络拉取到自己所在的节点。一边拉取一边聚合。

### 建议

- 想要获取更好的表达式查询速度，可以将spark.sql.codegen设置为Ture；
- 对于大数据集的计算结果，不要使用collect() ,collect()就结果返回给driver，很容易撑爆driver的内存；一般直接输出到分布式文件系统中；
- 对于Worker倾斜，设置spark.speculation=true 将持续不给力的节点去掉；
- 对于数据倾斜，采用加入部分中间步骤，如聚合后cache，具体情况具体分析；
- 适当的使用序化方案以及压缩方案；、
- 善于利用集群监控系统，将集群的运行状况维持在一个合理的、平稳的状态；
- 善于解决重点矛盾，多观察Stage中的Task，查看最耗时的Task，查找原因并改善

## Spark Streaming

### Spark Streaming定义

​	用于流式数据处理。支持的输入源很多，例如：Kafka、Flume、Twitter、ZeroMQ和简单的CP套接字等等。

使用离散化流作为抽象表示，DStream。

### Spark Streeam特点

1. 易用
2. 容错
3. 易整合到Spark体系

### Spark Stream架构

#### 背压机制

​	Spark1.5以前版本，限制Receiver的数据接收速率，可通过惊天配置参数"spark.streaming.reveiver.maxRate"来是实现，可能回造成资源利用率下降。

​	Saprk1.5之后使用背压机制，可动态调节数据接收速率。在Drvier的ReceiverRateController模块实现。

​	通过属性"spark.streaming.backpresure.enabled"来空盒子是否开启，默认为false，不开启。

### Dstream入门

#### scala示例

```scala
val sparkConf:SparkConf = new SparkConf().setMaster("local[*]"").setAppName("WordCount")
val ssc = new StreamingContex(sparkConf, Seconds(3))
val lineDStream : ReveiverInputDStream[String] = ssc.socketTextStream("hostname",port)
val wordDStream : DStream[String] = lineDStream.flatMap(_.split(" "))
wordDStream.map((_,1)).reduceByKey(_+_).print
ssc.start()
ssc.awaitTermination()
```

#### WordCount数据流解析

Reciver

Executor

### Dstream创建

#### Kafka数据源

1. 用法及说明：两个核心类KafkaUtils、KafkaCluster

2. 代码示例

   ```scala
   //读取上一次所消费数据位置
   def getOffset(kafkaCluster:KafkaCluster,group:String,topic:String)={
       //获取分区信息
       val partitions:[Either[Err,Set[TopicAndPartition]] = KafkaCluster.getPartisions(Set(topic))
       //判断是否有分区信息
       if(partitions.isRight){
       	//分区信息
           val to = partions.right.get
           val 
       }
       KafkaCluster.getConsumerOffsets(group,)
   }
   def main(args:Array[String]):Unit={
       val sparkConf:SparkConf = new SparkConf().setMaster("local[*]").setAppName("KafkaStreaming")
       cal ssc = new StreamingContext(sparkConf, Second(3))
       //卡夫卡参数定义
       var brokers = ""
       var topic = ""
       var group = ""
       var deserialization = ""
       //卡夫卡参数
       val kafakaPara = Map(ConsumerConfig.BOOTRAT_->brokers,Group,Key,Value)
       //获取KafkaCluster
       val kafkaCluster = new KafkaCluster(kafkaPara)
       //读取Kafka数据创建DStream
       KafkaUtils.createDirectStream(ssc,kafkaPara,)
       }
   ```

### Dstream转换

#### 无状态转换

Transform

#### 有状态装换

1. UpdateStateByKey
2. Window Operations

### 自定义数据源

## 模块设计

### Spark Core

​	SparkContext的初始化、部署模式、部署体系、任务提交与执行、计算引擎

- SparkContext：正式提交Application需要初始化SparkContext
  - DAGScheduler负责创建Job。
  - TaskScheduler负责资源的申请、任务的提交及集群对任务的调度
- 存储体系：优先使用各节点的内存作为存储
- 计算引擎：有DAGScheduler、RDD及具体节点上的Executor负责执行
- 部署模式   

### Spark SQL

​	提供SQL处理能力，便于熟悉关系型数据库操作的工程师进行交互查询

### Spark Streaming

​	提供流式计算处理能力

## SparkContext初始化

### SparkConf

​	ConcurrentHashMap，"spark."

1. 创建Spark执行环境SparkEnv
2. 创建RDD清理器metedataClearner
3. 创建并初始化Spark UI
4. Hadoop相关配置及Executor环境变量的设置
5. 创建任务调度TaskScheduler
6. 创建和启动DAGScheduler
7. TaskScheduler启动
8. 初始化块管理器BlockManager
9. 启动测量系统MetricsSystem
10. 创建和启动Executor分配管理器ExecutorAllocationManager
11. ContextCleaner的创建和启动
12. Spark环境更新
13. 创建DAGSchedulerSource和BlockManagerSource
14. 将SparkContext编辑为激活

