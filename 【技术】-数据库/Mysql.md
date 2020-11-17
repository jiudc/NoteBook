# Mysql

## MySQL Client

connector

## MySQL Server

内部包含各种组件，以插件的形式实现各种存储引擎，以InnoDB是使用最广泛的

- Connection Pool
- Paraser
- SQL-Interface
- Optimizer
- Management
- Storage Engine Plugin

### InnoDB

1. 内存结构，在MySql服务进程内

   1. 缓冲池：提升InnoDB性能，加速读请求，避免每次数据访问都进行磁盘IO
   2. 写缓冲：提升InnoDB性能，加速写请求，避免每次写入都进行磁盘IO
   3. 自适应哈希索引：提升InnoDB性能，加速读请求，减少索引查询的寻路路径。
   4. 日志缓冲：提升InnoDB性能，极大优化redo日志性能，并提供了高并发与强一致的折中方案

2. OS cache，内核态内存

3. 磁盘结构

   这三层交互有两种，通过OS Cache落地数据，直接O_Direct落地数据

## File Sys

数据存储和日志

## 缓冲池

### 预读

磁盘读写并不是按需读取，而是按页读取，一次至少读一页数据(4K)，如果未来要读取的数据就在页中，可以省去后续的磁盘IO

### LRU(Least recently used)

预读失效：预读的数据并没有被真正读取，使用新生代和老年代解决，只有预读成功的才会加入到新生代的头部

缓冲池污染：批量扫描大量数据，导致大量热数据被换出。使用老生代停留时间窗口机制。

##  写缓存

应用在非唯一普通索引页不在缓冲池中，对页进行写操作，并不会立即将磁盘页加载到缓冲池，而仅仅记录缓冲变更，等未来数据被读取时，再将数据合并恢复到缓冲池中的技术。

​	只有数据在真正读取时，才会被加载到缓冲池中。读的数据不在缓存中，1）载入索引页；2）从写缓冲读取相关信息；3）恢复索引页，放到缓冲池LRU里。

以下会刷写缓冲中的数据：

1. 有一个后台线程，会认为数据库空闲时
2. 数据库缓冲池不够用时
3. 数据库正常关闭时
4. redo log写满时

## 自适应哈希索引

1. InnoDB用户无法手动创建哈希索引
2. InnDB会自调优，如果判定建立自适应哈希索引能够提升效益，InnoDB自己会建立相关哈希索引

## 事务管理

事务提交后，必须将事务对数据页的修改刷(fsync)写到磁盘上，才能保证事务的ACID特征。

这个刷盘是随机写，随机写性能较低，会造成性能较低。

InnoDB通过1）将随机写改成顺序写；2）将单次写改成批量写的方式提升性能。

redo log落盘步骤：

1. 事务提交时，会调用MySQL自己的函数WriteRedolog写入Log Buffer
2. 只有当MySQL发起系统调用写文件write时，Log Buffer里的数据才会写到OS cache。
3. 操作系统（MYSQL主动flush）将OS cache中的数据fsync到磁盘上。

优点：操作系统将缓冲数据写入到OS cache中，可以提高操作系统性能。缓冲数据到Log Buffer里，可以提高数据库性能

缺点：可能丢失数据

1. 事务提交时，将redo log写入Log Buffer，就会认为成功
2. 如果写入Log Buffer，write入OS cache之前，数据库崩溃，就会出现数据丢失
3. 如果写入OS cache的数据，fsync磁盘之前，操作系统奔溃，也可能出现数据丢失。

MySQL提供参数innodb_flush_log_at_trxcommit

1. 最佳性能（=0），每隔一秒，才将LogBuffer的数据批量write入OS cache，同时MySQL主动fsync
2. 强一致(=1)，每次事务均将LogBuffer的数据批量write入OS cache，同时MySQL主动fsync
3. 折中（=2），每次事务提交将LogBuffer的数据批量write入OS cache，每隔一秒MySQL主动fsync

高并发业务，行业内的最佳实践是2