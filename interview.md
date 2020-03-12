
### 目录
```
第一部分 研发通用;    
第二部分 Java面试;    
第三部分 安全面试;    
第四部分 测试面试;    
第五部分 前端;    
第六部分 运维
```

# 第一部分 研发通用
#### （1）你认为你的代码提测的标准是什么?
#### （2）你常用的Linux命令及工具有什么? 
#### （3）对于团队成员，你是如何打KPI的，标准是什么?(同级之间，上下级，不同部门) 
#### （4）SQL-请说下左连接与右链接?区别?
#### （5）MySQL的myisam与innodb区别
#### （6）了解 OKR 吗
#### （7）怎么看待996和007
#### （8）你能接受什么样的加班

# 第二部分 Java面试

### 一、五轮阿里面试题及答案
https://www.jianshu.com/p/c8a271448dcd

#### 五轮阿里面试题及答案（摘录）
#### （1）了解过JVM调优没，基本思路是什么
#### （2）JVM自动内存管理，Minor GC与Full GC的触发机制
#### （3）你觉得你适合从事哪个方向的开发
#### （4）缓存的实现原理，设计缓存要注意什么
`将热点数据放在内存中，用户查询时命中内存中的数据而不用到数据库中查询,注意缓存的一致性，缓存雪崩、击穿、穿透的问题`


### 二、看过Spring源码没，说说Ioc容器的加载过程吧
```
简单概括：
1.刷新预处理
2.将配置信息解析，注册到BeanFactory
3.设置bean的类加载器
4.如果有第三方想再bean加载注册完成后，初始化前做点什么(例如修改属性的值，修改bean的scope为单例或者多例。)，提供了相应的模板方法，后面还调用了这个方法的实现，并且把这些个实现类注册到对应的容器中
5.初始化当前的事件广播器
6.初始化所有的bean
7.广播applicationcontext初始化完成。
```

### 三、如何设计存储海量数据的存储系统（海量数据的解决方案）
```
=== 页面上：
使用缓存；页面静态化技术；
=== 数据库层面：
分离数据库中活跃的数据；批量读取和延迟修改；读写分离；使用NoSQL和Hadoop等技术；分布式部署数据库；应用服务和数据服务分离；
=== 其他方面：
使用搜索引擎搜索数据库中的数据；进行业务的拆分；
=== 高并发情况下的解决方案：
应用程序和静态资源文件进行分离，静态资源可以使用CDN；
=== 集群与分布式；
使用Nginx反向代理；`
```

### 四、MQ：
#### （1）你知不知道你们系统里为什么要用消息队列这个东西?
#### （2）MQ的顺序消息?MQ的消息消费方式有几种?用于什么场景? 
#### （3）如何保证消息的可靠性传输（如何处理消息丢失的问题）？
#### （4）既然你用了 MQ，可能是某一种 MQ，那么你当时做没做过调研？
#### （5）消息队列比较:ActiveMQ，RabbitMQ，ZeroMQ，Kafka，MetaMQ，RocketMQ

### 五、事务与锁：
#### （1）简述分布式事务的实现方式有几种?在工作中是怎么实现的? 
#### （2）事务的级别有几种?工作中用到哪些?场景?
#### （3）MySQL 的悲观锁与乐观锁是指什么?开发具体是如何实现？分别适合什么场景？高并发下使用哪种？
#### （4）如何才能产生死锁?什么场景遇到过?怎么处理? 
#### （5）共享锁&排他锁&意向锁&间隙锁
#### （6）并发编程，那么你说说Java锁有哪些种类，以及区别
```
=== 公平锁/非公平锁
这个是在ReentrankLock中实现的，synchronized没有，是用一个队列实现的，在公平锁好理解，就是先进这个队列的，也先出队列获得资源，而非公平锁的话，则是还没有进队列之前可以与队列中的线程竞争尝试获得锁，如果获取失败，则进队列，此时也是要乖乖等前面出队才行
=== 可重入锁
如果一个线程获得过该锁，可以再次获得，主要是用途就是在递归方面，还有就是防止死锁，比如在一个同步方法块中调用了另一个相同锁对象的同步方法块
=== 独享锁/共享锁
共享锁可以由多个线程获取使用，而独享锁只能由一个线程获取。
对ReentrantReadWriteLock其读锁是共享锁，其写锁是独占锁
读锁的共享锁可保证并发读是非常高效的，读写，写读，写写的过程是互斥的。其中获得写锁的线程还能同时获得读锁，然后通过释放写锁来降级。读锁则不能升级
=== 互斥锁/读写锁
上面讲的独享锁/共享锁就是一种广义的说法，互斥锁/读写锁就是具体的实现。
互斥锁在Java中的具体实现就是ReentrantLock
读写锁在Java中的具体实现就是ReadWriteLock
=== 乐观锁/悲观锁
乐观锁就是乐观的认为不会发生冲突，用cas和版本号实现
悲观锁就是认为一定会发生冲突，对操作上锁
=== 分段锁
在1.7的concurrenthashmap中有分段锁的实现，具体为默认16个的segement数组，其中segement继承自reentranklock，每个线程过来获取一个锁，然后操作这个锁下连着的map。
=== 偏向锁/轻量级锁/重量级锁
在jdk1.6中做了第synchronized的优化，
偏向锁指的是当前只有这个线程获得，没有发生争抢，此时将方法头的markword设置成0，然后每次过来都cas一下就好，不用重复的获取锁
轻量级锁：在偏向锁的基础上，有线程来争抢，此时膨胀为轻量级锁，多个线程获取锁时用cas自旋获取，而不是阻塞状态
重量级锁：轻量级锁自旋一定次数后，膨胀为重量级锁，其他线程阻塞，当获取锁线程释放锁后唤醒其他线程。（线程阻塞和唤醒比上下文切换的时间影响大的多，涉及到用户态和内核态的切换）
=== 自旋锁：在没有获取锁的时候，不挂起而是不断轮询锁的状态
```

### 六、探索Mysql锁机制(二)——共享锁&排他锁&意向锁&间隙锁
https://www.jianshu.com/p/904f52bde904

### 七、探索Mysql锁机制(一)——乐观锁&悲观锁
https://www.jianshu.com/p/ed896335b3b4

### 八、相关
#### (1)HashMap实现原理
#### (2)HashTable实现原理?区别 
#### (3)常用的队列有哪些几，详细介绍下工作中用到的场景? 
#### (4)股票的盘口如何用Java来实现
#### (5)多线程的实现方式有几种
#### (6)多线程如何进行信息交互
#### (7)sleep、wait的区别 
#### (8)Java内存管理及回收算法
#### (9)如何自己实现一个双向链表? 
#### (10)让你做一个内存缓存方案?如何做? 
#### (11)简述缓存的技术有哪些?用于哪些场景?如何使用? 
#### (12)简述秒杀的实现方式? 
#### (13)简述Dubbo机制原理?工作有用到吗?

# 第三部分 安全面试
#### （1）讲述一下你最近3年内的所在公司发生的最大的安全事故是什么？讲一下经过；
#### （2）讲述一下你最近3年最少三个由安全部分发现，未发生的安全问题？
#### （3）Linux 熟悉程度？讲述一下你所知道的阿里或亚马逊上的公有镜像存在的哪些安全漏洞？（之前的也可以）
#### （4）去年阿里开源的fastjson的漏洞大家都知道，你具体介绍下。
#### （5）线上运行的服务的安全你会从哪几个方面去着手进行安全规范。（研发、运维角度）
#### （6）如何对运维部门进行安全规范或安全监控。（安全事故，大部分还是人的问题）
#### （7）Java 有多熟悉?
#### （8） FCoin宣称是羊毛党撸死的？你从安全角度下怎么看待FCoin事件？
#### （9）BTC、ETH，你们的安全方面是怎么做的？
#### （10）0day 漏洞（知道几个）

# 第四部分 测试面试
#### (1)请描述下你上个公司的测试发布流程?你认为有什么改善的地方? 
#### (2)白盒和黑盒的区别，你是怎么运用的?
#### (3)你是如何做测试分析的?
#### (4)什么样的测试用例是好的用例? 
#### (5)提测的标准是什么?你认为? 
#### (6)你是如何做应用兼容性测试的? 
#### (7)请问移动应用的灰度是怎么做的? 
#### (8)App应用闪退的原因是什么原因造成的? 
#### (9)如果一个线上问题无法测试复现，怎么处理? 
#### (10)你上个公司自动化测试投入产出比怎样?哪些可以做自动化测试? 
#### (11)自动测试的工具和语言?
#### (12)一个微信登陆页面，你会从哪几个角度去测试? 
#### (13)Web元素的八种定位是哪八种? 
#### (14)测试分类哪些种类?分别适合于什么?
#### (15)你是怎么做安全性测试的?
#### (16)GET和POST请求有什么区别? 
#### (17)一般有哪些状态返回?哪些情况会导致405? 
#### (18)页面碎裂图，有哪些情况导致?

# 第五部分 前端
#### (1)基于VUE的二次封装框架有哪些?用过哪些? 
#### (2)VUE动态绑定参数有哪几种方法? 
#### (3)mounted和created谁先执行，区别是什么? 
#### (4)对于父子组件如何通信? 
#### (5)详细介绍下Vue的生命周期 
#### (6)VUEX是什么?怎么使用?哪种功能场景使用它? 
#### (7)简述VUE响应原理
#### (8)vue本地存储数据方式?区别与场景 
#### (9)localstorage、sessionstorage的区别

# 第六部分 运维
#### (1)你做过你认为最安全的Linux安全是怎么做的?思路?(防攻击) 
#### (2)数据库索引怎么建是最合理最高效的?越多越好还是? 
#### (3)怎么做日志自动分割?
#### (4)服务器监控监控哪指标，如何做? 
#### (5)你是怎么实现自动化运维的?详细的解决方案? 
#### (6)简述一下一台新的服务器，你是如何优化的?(用于高并发高负载高可用) 
#### (7)你是怎么实现自动化备份的?文件?数据库? 
#### (8)基于docker的自动化运维是怎么做的?用于什么场景? 
#### (9)从哪几个方面可以查出一台服务器是否被入侵?快速?10分钟? 
#### (10)BTC、ETH网络节点你是怎么配置的?同步需要多久?不同步怎么处理?
#### (11)VUE的跨域怎么解决
#### (12)CDN你了解多少，原理是什么？
#### (13)简述你是怎么实现自动化布署的？
