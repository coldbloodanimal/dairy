# MySQLI

# InnoDB 引擎简介

InnoDB是一个一般目的的存储引擎，均衡了高可用和高性能，8.0中，InnoDB是MySQL的默认存储引擎。除非你设置了不同的存储引擎,建表语句使用InnoDB当不携带引擎语句时。

# InnoDB架构

所有图片皆是官方地址图片

![](https://dev.mysql.com/doc/refman/8.0/en/images/innodb-architecture.png)

## 内存结构：

缓存池（Buffer Pool）：缓冲池是主内存一块区域，缓存表、索引、专用服务器会使用80%的内存给这个区域

修改缓冲区（Change Pool）: