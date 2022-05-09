[TOC]

# MySQL简介



MySQL存储引擎架构

![MySQL存储引擎架构](https://dev.mysql.com/doc/refman/8.0/en/images/mysql-architecture.png)

MySQL可插拔存储引擎架构允许按照需求指定引擎，同时不用管理应用编码需求。在存储层的底层实现细节上，MySQL服务架构隔离了应用开发者和DBA，提供了简单一致的模型和接口。因此即使不同存储殷勤有不同的能力，应用感知不到它们的差异.

**MySQL由以下基本分组成**：

- 连接器
- NoSQL接口组件
- SQL接口组件
- 分析器
- 优化器
- 缓存
- 可插拔存储引擎
- 文件

MySQL存储引擎：

```
show engines;
```

| 存储引擎名称 | 特点                                                         |
| ------------ | ------------------------------------------------------------ |
| InnoDB       | 默认引擎（从5.5.8版本开始），支持事务。                      |
| MyISAM       | 不支持事务，从5.5.8之前为默认引擎（除windows版本外）         |
| NDB Cluster  | 集群存储引擎                                                 |
| Memory       | 内存存储引擎，拥有极高的插入，更新和查询效率。但是会占用和数据量成正比的内存空间。只在内存上保存数据，意味着数据可能会丢失 |
| ……           |                                                              |

# InnoDB 引擎简介

InnoDB是一个一般目的的存储引擎，均衡了高可用和高性能，8.0中，InnoDB是MySQL的默认存储引擎。除非你设置了不同的存储引擎。

## InnoDB架构

![InnoDB架构图](https://dev.mysql.com/doc/refman/8.0/en/images/innodb-architecture.png)

## 内存结构：

缓存池（Buffer Pool）：缓冲池是主内存一块区域，缓存表、索引、专用服务器会使用80%的内存给这个区域。

修改缓冲区（Change Pool）:一个数据结构用来存储次级缓存页当它们不在缓存池的时候，会在DML语句时候变更，当这些页被读的时候会合并到缓存池中。推测这样设计的目的:聚簇索引数据和索引是在一起的，次级索引是另外存储的。

自适应哈希索引(Adaptive Hash Index)：自适应哈希索引允许InnoDB像内存数据库一样执行，在系统上总起工作量适当和缓冲池内存重组时候，不会牺牲事务特性以及可靠性的时候。可通过参数[`innodb_adaptive_hash_index`](https://dev.mysql.com/doc/refman/8.0/en/innodb-parameters.html#sysvar_innodb_adaptive_hash_index) 开启关闭。未深究。

日志缓存（Log Buffer）：日志缓存是一块内存区域来保留数据那些将被写到磁盘日志文件的。默认16M。日志缓存会被周期性的写道磁盘。大的日志缓存可以避免大事务提交前，写redo日志到磁盘。

![Change Buffer](https://dev.mysql.com/doc/refman/8.0/en/images/innodb-change-buffer.png)



# mysql文件结构

mysql是通过文件系统对数据和索引进行存储。

mysql从物理结构上可以分为日志文件和数据索引文件。

mysql文件通常放在**/var/lib/mysql**

## 常用的日志文件：

错误日志、二进制日志、查询日志、慢查询日志、Redo日志、中继日志。

查看日志使用清空

```mysql
show variables like 'log_%';
```

| Variable_name                          | Value                                  |
| -------------------------------------- | -------------------------------------- |
| log_bin                                | ON                                     |
| log_bin_basename                       | /var/lib/mysql/binlog                  |
| log_bin_index                          | /var/lib/mysql/binlog.index            |
| log_bin_trust_function_creators        | OFF                                    |
| log_bin_use_v1_row_events              | OFF                                    |
| log_error                              | /var/log/mysqld.log                    |
| log_error_services                     | log_filter_internal; log_sink_internal |
| log_error_suppression_list             |                                        |
| log_error_verbosity                    | 2                                      |
| log_output                             | FILE                                   |
| log_queries_not_using_indexes          | OFF                                    |
| log_raw                                | OFF                                    |
| log_replica_updates                    | ON                                     |
| log_slave_updates                      | ON                                     |
| log_slow_admin_statements              | OFF                                    |
| log_slow_extra                         | OFF                                    |
| log_slow_replica_statements            | OFF                                    |
| log_slow_slave_statements              | OFF                                    |
| log_statements_unsafe_for_binlog       | ON                                     |
| log_throttle_queries_not_using_indexes | 0                                      |
| log_timestamps                         | UTC                                    |

### 错误日志

错误日志记录服务启动停止、错误和警告日志。

### 二进制日志（The Binary Log）

二进制日志记录那些描述数据库变更如表创建，数据变更等事件。同时会记录事件可能会造成改变（举例，如一个没有匹配到记录的删除），除非配置了基于行的日志。同时记录了语句执行所用时长。二进制日志主要用于：**恢复、主从复制、审计**。

### 查询日志

默认情况下是关闭的。查询日志会记录用户的所有操作，其中还包含增删查改等信息，在并发操作大的环境下会产生大量的信息从而导致不必要的磁盘IO，会影响mysql的性能的。如若不是为了调试数据库的目的建议**不要开启**查询日志。

### **慢查询日志**

默认关闭。记录执行时间超过**long_query_time**秒的所有查询，便于收集查询时间比较长的SQL语句
