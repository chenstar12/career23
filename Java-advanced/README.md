涵盖[高并发](#高并发架构)、[分布式](#分布式系统)、[高可用](#高可用架构)、[微服务](#微服务架构)、[海量数据处理](#海量数据处理)；

-   Gitee Pages: https://doocs.gitee.io/advanced-java
-   GitHub Pages: https://doocs.github.io/advanced-java

## 高并发架构

### [消息队列](/docs/high-concurrency/mq-interview.md)

### [搜索引擎](/docs/high-concurrency/es-introduction.md)

### 缓存

### 分库分表

-   [为什么要分库分表（设计高并发系统的时候，数据库层面该如何设计）？用过哪些分库分表中间件？不同的分库分表中间件都有什么优点和缺点？你们具体是如何对数据库如何进行垂直拆分或水平拆分的？](/docs/high-concurrency/database-shard.md)
-   [现在有一个未分库分表的系统，未来要分库分表，如何设计才可以让系统从未分库分表动态切换到分库分表上？](/docs/high-concurrency/database-shard-method.md)
-   [如何设计可以动态扩容缩容的分库分表方案？](/docs/high-concurrency/database-shard-dynamic-expand.md)
-   [分库分表之后，id 主键如何处理？](/docs/high-concurrency/database-shard-global-id-generate.md)

### 读写分离

-   [如何实现 MySQL 的读写分离？MySQL 主从复制原理是啥？如何解决 MySQL 主从同步的延时问题？](/docs/high-concurrency/mysql-read-write-separation.md)

### 高并发系统

-   [如何设计一个高并发系统？](/docs/high-concurrency/high-concurrency-design.md)

## 分布式系统

### [面试连环炮](/docs/distributed-system/distributed-system-interview.md)

### 系统拆分

-   [为什么要进行系统拆分？如何进行系统拆分？拆分后不用 Dubbo 可以吗？](/docs/distributed-system/why-dubbo.md)

### 分布式服务框架

### 分布式锁

-   [Zookeeper 都有哪些应用场景？](/docs/distributed-system/zookeeper-application-scenarios.md)
-   [使用 Redis 如何设计分布式锁？使用 Zookeeper 来设计分布式锁可以吗？以上两种分布式锁的实现方式哪种效率比较高？](/docs/distributed-system/distributed-lock-redis-vs-zookeeper.md)

### 分布式事务

-   [分布式事务了解吗？你们如何解决分布式事务问题的？TCC 如果出现网络连不通怎么办？XA 的一致性如何保证？](/docs/distributed-system/distributed-transaction.md)

### 分布式会话

-   [集群部署时的分布式 Session 如何实现？](/docs/distributed-system/distributed-session.md)

## 高可用架构

### 高可用系统

### 熔断

### 降级

## 微服务架构

---

## Doocs 社区优质项目

| #   | 项目                                                              | 描述                                                                                             | 热度                                                                                                                            |
| --- | ----------------------------------------------------------------- | ------------------------------------------------------------------------------------------------ | ------------------------------------------------------------------------------------------------------------------------------- |
| 1   | [advanced-java](https://github.com/doocs/advanced-java)           | 互联网 Java 工程师进阶知识完全扫盲：涵盖高并发、分布式、高可用、微服务、海量数据处理等领域知识。 | ![](https://badgen.net/github/stars/doocs/advanced-java) <br>![](https://badgen.net/github/forks/doocs/advanced-java)           |
| 2   | [leetcode](https://github.com/doocs/leetcode)                     | 多种编程语言实现 LeetCode、《剑指 Offer（第 2 版）》、《程序员面试金典（第 6 版）》题解。        | ![](https://badgen.net/github/stars/doocs/leetcode) <br>![](https://badgen.net/github/forks/doocs/leetcode)                     |
| 3   | [source-code-hunter](https://github.com/doocs/source-code-hunter) | 互联网常用组件框架源码分析。                                                                     | ![](https://badgen.net/github/stars/doocs/source-code-hunter) <br>![](https://badgen.net/github/forks/doocs/source-code-hunter) |
| 4   | [jvm](https://github.com/doocs/jvm)                               | Java 虚拟机底层原理知识总结。                                                                    | ![](https://badgen.net/github/stars/doocs/jvm) <br>![](https://badgen.net/github/forks/doocs/jvm)                               |
| 5   | [coding-interview](https://github.com/doocs/coding-interview)     | 代码面试题集，包括《剑指 Offer》、《编程之美》等。                                               | ![](https://badgen.net/github/stars/doocs/coding-interview) <br>![](https://badgen.net/github/forks/doocs/coding-interview)     |
| 6   | [md](https://github.com/doocs/md)                                 | 一款高度简洁的微信 Markdown 编辑器。                                                             | ![](https://badgen.net/github/stars/doocs/md) <br>![](https://badgen.net/github/forks/doocs/md)                                 |
| 7   | [technical-books](https://github.com/doocs/technical-books)       | 值得一看的技术书籍列表。                                                                         | ![](https://badgen.net/github/stars/doocs/technical-books) <br>![](https://badgen.net/github/forks/doocs/technical-books)       |
