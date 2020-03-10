# JDBC

​	JDBC（Java Data Base Connectivity,java数据库连接）是一种用于执行SQL语句的Java API，可以为多种关系数据库提供统一访问，它由一组用Java语言编写的类和接口组成。JDBC提供了一种基准，据此可以构建更高级的工具和接口，使数据库开发人员能够编写数据库应用程序。

## 常用接口

### Driver

​	Driver接口由数据库厂家提供，作为java开发人员，只需要使用Driver接口就可以了。在编程中要连接数据库，必须先装载特定厂商的数据库驱动程序，不同的数据库有不同的装载方法。

### DriverManager

​	用于管理JDBC驱动的服务类，程序中使用该类主要功能是获取Connection对象。

```java
Public static synchronized Connection getConnetcion(String url, String user, String pass) throws SQLException
```

### Connection

​	Connection与特定数据库的连接（会话），在连接上下文中执行sql语句并返回结果。常用方法：

- createStatement()：创建向数据库发送sql的statement对象。
- prepareStatement(sql) ：创建向数据库发送预编译sql的
- PrepareSatement对象。会将SQL语句提交到数据库进行预编译。需要为预编译的SQL语句传入参数，比Statement多了如下方法void setXXX(int paramenterIndex, XXX value)
- prepareCall(sql)：创建执行存储过程的callableStatement对象。
- setAutoCommit(boolean autoCommit)：设置事务是否自动提交。
- commit() ：在链接上提交事务。
- rollback() ：在此链接上回滚事务。
- Savepoint setsavepoint(String name)：创建一个保存点
- Void setTransactionIsolation(int level):设置事务的隔离级别

### Statement

- ResultSet executeQuery(String sql) throws SQLException
- Int executeUpdate(String sql) throws SQLException
- Boolean execute(String sql) throws SQLException
- closeOnCompletion(),当所有依赖于statement的resultset关闭时，该statement会自动关闭。
- isCloseOnCompletion()，用于判断Statement是否打开了CloseOnCompletion
- executeLargeUpdate(),DML语句影响超过Integer.MAX_VALUE时使用

### ResultSet

- Void close():释放
- Boolean absolute(int row):结果集指针移到row行，若为负数，则倒数
- Void beforeFirst():初始化
- Boolean first()
- Boolean previous()
- Boolean next()
- Boolean last()
- Void afterLast()

## JDBC编程步骤

### 加载数据库驱动

Class.forName(driverClass)

### 获取数据库连接

DriverManager.getConnection(String url, String user,String pass)

URL遵循如下写法,jdbc：subprotocol：other stuff

Jdbc:mysql://hostname:port/databasename

### 创建Statement对象

通过Connection对象创建Statement

1. createStatement()
2. prepareStatement(String sql)
3. prepareCall(String sql)

### 执行SQL

使用Statement执行SQL

1. execute()-返回boolean，使用getResultSet()、getUpdateCount()
2. executeUpdate()
3. executeQuery()

### 操作结果集

ResultSet

### 回收数据库资源

关闭ResultSet、Statement、Connection

## Statement与PreparedStatement对比

1. PreparedStatement预编译SQL，性能更好
2. PreparedStatement无须拼接SQL，编程更简单
3. PreparedStatement可以防止SQL注入，安全性更好

## 结果集

​	默认方式打开的ResultSet是不可更新的，如希望更新，可在创建Statement或PrepareStatement加入额外参数。

1. ResultSetType：
	1. Result.TYPE_FORWARD_ONLY：指针只能向前移动；
	2. ResultSet.TYPE_SCROLL_INSENSITIVE:自由，底层数据不影响ResultSet内容
	3. ResultSet.TYPE_SCROLL_SENSITIVE: 自由，底层数据影响ResultSet内容
2. ResultSetConcurrency：控制并发类型
	1. ResultSet.CONCUR_READ_ONLY:只读的并发模式
	2. ResultSet.CONCUR_UPDATEBLE:可更新的并发模式

```java
Pstmt = conn.prepareStatement(sql, ResultSet.TYPE_SCROLL_INSENSITVIE, ResultSet.CONCUR_UPDATABLE)
```

​    若定义为可更新结果集，需要满足

1. 所有数据集必须来自一个表
2. 选出数据集必须包含主键列

​	可使用ResultSet的upateXxx(int columnIndex,Xxx value)修改，最后通过调用ResultSet的updateRow()提交修改

​	使用结果集更新、删除、新增记录。Rs.insertRow()，Rs.updateRow()，Rs.deleteRow()

## Blob类型数据

​	Blob（binary long Object）常用于存储大文件，图片，声音等。储存Blob数据需要使用PreparedStatement，该对象有一个方法setBinaryStream(int paremeterIndex,INputStream x)

## 示例

### 连接MySql

1. Class.forName(“com.mysql.jdbc.Driver”)
2. Connection con = DriverManager.getConnection(“jdbc:mysql://localhost:8080/datename,user,password”);

## Batch Insert

```
String [] queries = {

​    "insert into employee (name, city, phone) values ('A', 'X', '123')",

​    "insert into employee (name, city, phone) values ('B', 'Y', '234')",

​    "insert into employee (name, city, phone) values ('C', 'Z', '345')",

};

 

Connection connection = new getConnection();

Statement statement = connection.createStatement();

For(String query: queries){

​    Statement.addBatch(query);

}

Statement.executeBatch();

Statement.close();

Connection.close();
```





有可能导致数据库注入或者OutOfMemory

```java
String sql = "insert into employee (name, city, phone) values (?, ?, ?)";
Connection connection = new getConnection();
PreparedStatement ps = connection.prepareStatement(sql);
 
final int batchSize = 1000;
int count = 0;
 
for (Employee employee: employees) {
 
        ps.setString(1, employee.getName());
        ps.setString(2, employee.getCity());
        ps.setString(3, employee.getPhone());
        ps.addBatch();
        
        if(++count % batchSize == 0) {
               ps.executeBatch();
               ps.commit();
        }
}
ps.executeBatch(); // insert remaining records
ps.close();
connection.close();
 
catch (DataTruncation ex) {           getLogger(FNM01I01LS2000Dao.class.getName()).log(Level.SEVERE, null, ex);
        }
        catch (SQLException | ArrayIndexOutOfBoundsException ex) {           getLogger(FNM01I01LS2000Dao.class.getName()).log(Level.SEVERE, null, ex);
        }
```

## 事务处理

ACID(Atomicity,Consistency,Isolation,Durability)

### MySql对事务的支持

1. SET AUTOCOMMIT = 0，取消自动提交处理，开启事务处理
2. SET AUTOCOMMIT = 1，打开自动处理，关闭事务处理
3. START TRANSACTION，启动事务
4. BEGIN，启动事务，相当于执行START TRANSACTION
5. COMMIT，提交事务
6. ROLLBACK，回滚全部操作
7. SAVEPOINT pointName，设置事务保存点
8. ROLLBACK TO SAVEPOINT pointName，回滚操作到保存点

## 使用元数据分析数据库

### DatebaseMetaData

​	可以得到数据库的一些基本信息，比如数据库的名称、版本

1. String getDatabaseProdctName() throws SQLException,数据库名称
2. Int getDriverMajorVersion(),主版本号
3. Int getDriverMinorVersion(),次版本号
4. ResultSet getPrimaryKeys(String catalog, String schema, String table)throws SQLException
	1. TABLE_CAT String:表类别
	2. TABLE_SCHEM String:表模式
	3. TABLE_NAME String:表名称
	4. COLUMN_NAME String:列名称
	5. KEY_SEQ short:主键中的序列号
	6. PK_NAME String:主键的名称

### ResultSetMetaData

- Int getColumnCount() throws SQLException
- Boolean isAUtoIncrement(int column) throws SQLException:判断指定列是否是自动编号
- String getColumnName(int column) throws SQLException:返回列的名称