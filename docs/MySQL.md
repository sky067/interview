# Mysql

## 一、数据库三范式
1. 第一范式：
    - 原子性：表中字段的数据，不可以再拆分。
    - 通常第一范式可以用主键实现。


## 二、索引

### 1. 索引的类型（五种类型）

1. 普通索引
    - 允许空值和重复值。
2. 唯一索引
    - 唯一索引与普通索引的区别：唯一索引所在列值不允许重复。
    
3. 主键索引（特殊的唯一索引）
    - 特殊的唯一索引，不允许有空值。
    - b+树中，主键索引在叶子节点里，存放的是整行数据。

4. 联合索引
    - 在多个字段上创建索引，使用时遵循最左前缀原则

    - 联合索引的最左前缀原则
        - 最左前缀举例
            - 创建了一个包含三列的联合索引`(col1, col2, col3)`，索引生效于`(col1), (col1, col2), and (col1, col2, col3)`。 


5. 空间索引


### 6. 聚集（簇）索引与非聚集（簇）索引

#### 聚集索引
- InnoDB中，主键索引又被称为聚集索引`（clustered index）`。

#### 回表
什么是回表?

因为`非主键索引`叶子节点存的是`主键`的值。所以在同时存在主键索引与非主键索引的表中，where字句的条件是非主键索引时，会通过该`非主键索引`的索引树（b+树）查找到对应`主键的值`，再
通过`主键的值`查找`主键索引`对应的索引树，从而查出整行数据。该过程称为`回表`。

回表举例：
- 建表
```sql
create table A(
    id int primary key not null,
    name char(4) not null,
)

```

### 4. 


### 2. 索引的底层结构
#### B+树
Innodb使用b+树

- b+树中，主键索引在叶子节点里，存放的是整行数据。
- b+树中，非主键索引叶子节点存的是主键的值。InnoDB中，非主键索引也叫做二级索引（secondary index）。

### 3. 如何创建与删除索引

#### 3.1 创建普通索引（三种方式）

1. 建表时创建索引
```sql
CREATE TABLE `table_name` (  
`id` int(11) NOT NULL AUTO_INCREMENT ,  
`name` char(255) NOT NULL ,  
PRIMARY KEY (`id`),  
INDEX index_name (name)  
) ENGINE=InnoDB  DEFAULT CHARSET=utf8mb4;  
```

2. 使用`create index`直接创建
```sql
CREATE INDEX index_name ON table_name(column_name)
```

3. 使用`alter table`以修改表结构的方式添加索引
```sql
ALTER TABLE table_name ADD INDEX index_name ON (column_name) 
```

#### 3.2 创建唯一索引

- 创建唯一索引时，使用`UNIQUE`参数进行约束
```sql
CREATE TABLE `table_name` (  
`id` int(11) NOT NULL AUTO_INCREMENT primary key,  
`name` char(255) NOT NULL ,  
UNIQUE INDEX index_name (name)  
) ENGINE=InnoDB  DEFAULT CHARSET=utf8mb4;  

mysql> CREATE  table address(
    -> id  int(11)  auto_increment  primary  key  not  null,
    -> name  varchar(50),
    -> address  varchar(200),
    -> UNIQUE  INDEX  address(id  ASC)
    -> );

```

#### 3.2 创建主键索引
#### 3.2 创建联合索引

#### 3.5 如何删除索引  
`DROP INDEX index_name ON table_name; ` 

### 5. 查看索引是否被引用  



## 三、锁（悲观锁、乐观锁）

### 1. 悲观锁（读锁、写锁）
悲观锁认为每次修改数据时，都会有其他线程来修改数据。

- 悲观锁分为`读锁`和`写锁`

- sql语句使用 `for update` 上悲观锁。

    - 使用方法（用在select语句上）： `select * from table where id=1 for update`

    - 注意：`for update` 仅适用于`InnoDB`，并且必须开启**事务**，在`begin与commit`之间才生效。

#### 1.1 读锁（共享锁（shared lock））
- 多个事务共享一个锁，都可以访问数据。
- 只可读，不可修改数据。

#### 1.2 读锁（排他锁（exclusive lock））
- 排他锁被一个事务获取后，该数据行无法被其他事务获取锁。
- 排他锁对数据可读可改。

悲观锁总结：
- 悲观锁先锁定行数据，再进行访问。
- 优点：保证了数据的安全性
- 缺点：降低了并行性，增加了死锁的风险

### 2. 乐观锁

乐观锁在更新数据时，才进行加锁判断。
适用与读多写少的场景。

乐观锁不需要借助数据库的锁机制。

- 乐观锁实现：

    1. 首先查询一遍数据，
    2. 在update之前，再次查询一遍数据，如果数据未改变，则update

    3. 常用方法是在表中增加一个字段version，每次更新使 version + 1，并在更新前后，且在version+1前都读取version来判断是否一致   

乐观锁总结：
- 实现乐观锁两个步骤：
    1. 检测冲突。
    2. 数据更新。

## 锁的级别
锁的级别分为行级锁和表级锁

### 1. 行级锁

- 行级锁都是基于索引的。

- 如果一条 SQL 语句用不到索引是不会使用行级锁的，会使用表级锁把整张表锁住，这点需要注意。

- InnoDB 默认行级锁。

### 2. 表级锁

## 事务

### 1. 事务的隔离级别

4中隔离级别

1. 读已提交
2. 读未提交
    - 也叫脏读
    - 事务可以读取其他事务未提交的数据
3. 可重复读
    - mysql的默认隔离级别
4. 串行化

5. 幻读


慢查询怎么看
一条sql最多可以使用多少索引
mvcc
索引失效的情况
sql慢如何查找
微服务根据什么拆分
服务遇到cpu飙高怎么处理
服务限流和熔断怎么做
redis哨兵机制
redis分布式锁怎么实现
什么是线程池
秒杀会遇到什么问题怎么解决
    大量请求
    解决：
        限流
        削峰
        熔断
        服务降级
        

