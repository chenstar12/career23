## 必看专栏

- **[《Java 面试指北》](./zhuanlan/java-mian-shi-zhi-bei.md)** : 专为面试打造，与 JavaGuide 开源版的内容互补！
- **[《手写 RPC 框架》](./zhuanlan/handwritten-rpc-framework.md)** : 从零开始基于 Netty+Kyro+Zookeeper 实现一个简易的 RPC 框架。

## 推荐阅读 

- [Java学习路线](https://zhuanlan.zhihu.com/p/379041500) : 一份涵盖 Java 后端开发必备技能的学习路线！全面且清晰！
- [Java开源项目精选](./open-source-project/readme.md) ：收集整理了 Gitee/Github 上非常棒的 Java 开源项目集合。Java 开发必备！
- [Java技术文章精选](/high-quality-technical-articles/) : 精选一些和 Java 相关的优质技术文章，每一篇都值得你阅读 3 遍以上！
- [Java书单精选](https://gitee.com/SnailClimb/awesome-cs) : Java 后端开发值得一读的一些书籍。

## PDF

- [《JavaGuide 面试突击版》](https://mp.weixin.qq.com/s?__biz=Mzg2OTA0Njk0OA==&mid=100029614&idx=1&sn=62993c5cf10265cb7018db7f1ec67250&chksm=4ea1fb6579d67273499b7243641d4ef372decd08047bfbb6dfb5843ef81c7ccba209086cf345#rd)
- [《消息队列常见知识点&面试题总结》](https://t.1yb.co/Fy0u)
- [《Java 工程师进阶知识完全扫盲》](https://t.1yb.co/GXLF)
- [《分布式相关面试题汇总》](https://t.1yb.co/GXLF)
- [《图解计算机基础》](https://mp.weixin.qq.com/s?__biz=Mzg2OTA0Njk0OA==&mid=100021725&idx=1&sn=2db9664ca25363139a81691043e9fd8f&chksm=4ea19a1679d61300d8990f7e43bfc7f476577a81b712cf0f9c6f6552a8b219bc081efddb5c54#rd)


## 项目相关

- [项目介绍](./javaguide/intro.md)
- [贡献指南](./javaguide/contribution-guideline.md)

# 部分目录

## 安全

#### 认证授权

**[《认证授权基础》](docs/system-design/security/basis-of-authority-certification.md)** 这篇文章中我会介绍认证授权常见概念： **Authentication**, **Authorization** 以及 **Cookie**、**Session**、Token、**OAuth 2**、**SSO** 。如果你不清楚这些概念的话，建议好好阅读一下这篇文章。

* **JWT** ：JWT（JSON Web Token）是一种身份认证的方式，JWT 本质上就一段签名的 JSON 格式的数据。由于它是带有签名的，因此接收者便可以验证它的真实性。相关阅读：
  + [JWT 优缺点分析以及常见问题解决方案](docs/system-design/security/advantages&disadvantages-of-jwt.md)
  + [适合初学者入门 Spring Security With JWT 的 Demo](https://github.com/Snailclimb/spring-security-jwt-guide)

* **SSO(单点登录)** ：**SSO(Single Sign On)** 即单点登录说的是用户登陆多个子系统的其中一个就有权访问与其相关的其他系统。举个例子我们在登陆了京东金融之后，我们同时也成功登陆京东的京东超市、京东家电等子系统。相关阅读：[**SSO 单点登录看这篇就够了！**](docs/system-design/security/sso-intro.md)

#### 数据脱敏

数据脱敏说的就是我们根据特定的规则对敏感信息数据进行变形，比如我们把手机号、身份证号某些位数使用 * 来代替。

#### 敏感词过滤

系统需要对用户输入的文本进行敏感词过滤如色情、政治、暴力相关的词汇。

相关阅读：[《敏感词过滤》](./docs/system-design/security/sentive-words-filter.md)

### 定时任务

定时任务的一些概念以及一些常见的定时任务技术选型：[《Java定时任务大揭秘》](./docs/system-design/schedule-task.md)

## 分布式

### CAP 理论和 BASE 理论

CAP 也就是 Consistency（一致性）、Availability（可用性）、Partition Tolerance（分区容错性） 这三个单词首字母组合。

**BASE** 是 **Basically Available（基本可用）** 、**Soft-state（软状态）** 和 **Eventually Consistent（最终一致性）** 三个短语的缩写。BASE 理论是对 CAP 中一致性和可用性权衡的结果，其来源于对大规模互联网系统分布式实践的总结，是基于 CAP 定理逐步演化而来的，它大大降低了我们对系统的要求。

相关阅读：[CAP 理论和 BASE 理论解读](docs/distributed-system/theorem&algorithm&protocol/cap&base-theorem.md)

### Paxos 算法和 Raft 算法

**Paxos 算法** 诞生于 1990 年，这是一种解决分布式系统一致性的经典算法 。但是，由于 Paxos 算法非常难以理解和实现，不断有人尝试简化这一算法。到了2013 年才诞生了一个比 Paxos 算法更易理解和实现的分布式一致性算法—**Raft 算法**。

相关阅读：

* [Paxos 算法解读](docs/distributed-system/theorem&algorithm&protocol/paxos-algorithm.md)
* [Raft 算法解读](docs/distributed-system/theorem&algorithm&protocol/raft-algorithm.md)

### RPC

RPC 让调用远程服务调用像调用本地方法那样简单。

Dubbo 是一款国产的 RPC 框架，由阿里开源。相关阅读：

* [Dubbo 常见问题总结](docs/distributed-system/rpc/dubbo.md)
* [服务之间的调用为啥不直接用 HTTP 而用 RPC？](docs/distributed-system/rpc/why-use-rpc.md)

### API 网关

网关主要用于请求转发、安全认证、协议转换、容灾。

相关阅读：

* [为什么要网关？你知道有哪些常见的网关系统？](docs/distributed-system/api-gateway.md)
* [百亿规模API网关服务Shepherd的设计与实现](https://tech.meituan.com/2021/05/20/shepherd-api-gateway.html)

### 分布式 id

在复杂分布式系统中，往往需要对大量的数据和消息进行唯一标识。比如数据量太大之后，往往需要对数据进行分库分表，分库分表后需要有一个唯一 ID 来标识一条数据或消息，数据库的自增 ID 显然不能满足需求。相关阅读：[为什么要分布式 id ？分布式 id 生成方案有哪些？](docs/distributed-system/distributed-id.md)

### 分布式事务

**分布式事务就是指事务的参与者、支持事务的服务器、资源服务器以及事务管理器分别位于不同的分布式系统的不同节点之上。**

简单的说，就是一次大的操作由不同的小操作组成，这些小的操作分布在不同的服务器上，且属于不同的应用，分布式事务需要保证这些小操作要么全部成功，要么全部失败。本质上来说，分布式事务就是为了保证不同数据库的数据一致性。

## 高性能

### 消息队列

消息队列在分布式系统中主要是为了解耦和削峰。相关阅读： [消息队列常见问题总结](docs/high-performance/message-queue/message-queue.md)。

1. **RabbitMQ** : [RabbitMQ 入门](docs/high-performance/message-queue/rabbitmq-intro.md)
2. **RocketMQ** : [RocketMQ 入门](docs/high-performance/message-queue/rocketmq-intro)、[RocketMQ 的几个简单问题与答案](docs/high-performance/message-queue/rocketmq-questions.md)
3. **Kafka** ：[Kafka 常见问题总结](docs/high-performance/message-queue/kafka-questions-01.md)

### 读写分离&分库分表

读写分离主要是为了将数据库的读和写操作分不到不同的数据库节点上。主服务器负责写，从服务器负责读。另外，一主一从或者一主多从都可以。

读写分离可以大幅提高读性能，小幅提高写的性能。因此，读写分离更适合单机并发读请求比较多的场景。

分库分表是为了解决由于库、表数据量过大，而导致数据库性能持续下降的问题。

常见的分库分表工具有： `sharding-jdbc` （当当）、 `TSharding` （蘑菇街）、 `MyCAT` （基于 Cobar）、 `Cobar` （阿里巴巴）...。 推荐使用 `sharding-jdbc` 。 因为， `sharding-jdbc` 是一款轻量级 `Java` 框架，以 `jar` 包形式提供服务，不要我们做额外的运维工作，并且兼容性也很好。

相关阅读： [读写分离&分库分表常见问题总结](docs/high-performance/read-and-write-separation-and-library-subtable.md)

### 负载均衡

负载均衡系统通常用于将任务比如用户请求处理分配到多个服务器处理以提高网站、应用或者数据库的性能和可靠性。

常见的负载均衡系统包括 3 种：

1. **DNS 负载均衡** ：一般用来实现地理级别的均衡。
2. **硬件负载均衡** ： 通过单独的硬件设备比如 F5 来实现负载均衡功能（硬件的价格一般很贵）。
3. **软件负载均衡** ：通过负载均衡软件比如 Nginx 来实现负载均衡功能。

## 高可用

高可用描述的是一个系统在大部分时间都是可用的，可以为我们提供服务的。高可用代表系统即使在发生硬件故障或者系统升级的时候，服务仍然是可用的 。

相关阅读： **《[如何设计一个高可用系统？要考虑哪些地方？](docs/high-availability/high-availability-system-design.md)》** 。

### 限流

限流是从用户访问压力的角度来考虑如何应对系统故障。

限流为了对服务端的接口接受请求的频率进行限制，防止服务挂掉。比如某一接口的请求限制为 100 个每秒, 对超过限制的请求放弃处理或者放到队列中等待处理。限流可以有效应对突发请求过多。相关阅读：[何为限流？限流算法有哪些？](docs/high-availability/limit-request.md)

### 降级

降级是从系统功能优先级的角度考虑如何应对系统故障。

服务降级指的是当服务器压力剧增的情况下，根据当前业务情况及流量对一些服务和页面有策略的降级，以此释放服务器资源以保证核心任务的正常运行。

### 熔断

熔断和降级是两个比较容易混淆的概念，两者的含义并不相同。

降级的目的在于应对系统自身的故障，而熔断的目的在于应对当前系统依赖的外部系统或者第三方系统的故障。

### 排队

另类的一种限流，类比于现实世界的排队。玩过英雄联盟的小伙伴应该有体会，每次一有活动，就要经历一波排队才能进入游戏。

### 集群

相同的服务部署多份，避免单点故障。

### 超时和重试机制

**一旦用户的请求超过某个时间得不到响应就结束此次请求并抛出异常。** 如果不进行超时设置可能会导致请求响应速度慢，甚至导致请求堆积进而让系统无法再处理请求。

重试的次数一般设为 3 次，再多的重试次数没有好处，反而会加重服务器压力（部分场景使用失败重试机制会不太适合）。在一次重试失败之后通常会加上一个时间间隔 delay 再进行下一次重试，时间间隔 delay 通常建议是随机的。

并且，为了更好地保护下游，我们还可以结合断路器。

### 灾备设计和异地多活

**灾备**  = 容灾+备份。

* **备份** ： 将系统所产生的的所有重要数据多备份几份。
* **容灾** ： 在异地建立两个完全相同的系统。当某个地方的系统突然挂掉，整个应用系统可以切换到另一个，这样系统就可以正常提供服务了。

**异地多活** 描述的是将服务部署在异地并且服务同时对外提供服务。和传统的灾备设计的最主要区别在于“多活”，即所有站点都是同时在对外提供服务的。异地多活是为了应对突发状况比如火灾、地震等自然或者人为灾害。

相关阅读：

* [搞懂异地多活，看这篇就够了](https://mp.weixin.qq.com/s/T6mMDdtTfBuIiEowCpqu6Q)
* [四步构建异地多活](https://mp.weixin.qq.com/s/hMD-IS__4JE5_nQhYPYSTg)
* [《从零开始学架构》— 28 | 业务高可用的保障：异地多活架构](http://gk.link/a/10pKZ)
