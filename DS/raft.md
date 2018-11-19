# Consesus problem

Several computers \(or nodes\) achieve consensus if they all agree on some value. More formally:

1. Agreement: Every correct process must agree on the same value.
2. Integrity: Every correct process decides at most one value, and if it decides some value, then it must have been proposed by some process.
3. Termination: All processes eventually reach a decision.
4. Validity: If all correct processes propose the same value V, then all correct processes decide V.

##### FLP Impossibility result\(FLP不可能性\)

在分布式系统中，异步网络（消息延迟可能任意大或丢失，消息可能乱序），只要有一个进程失效（进程死亡或者足够长时间不响应），就不可能设计出一个一致性协议。该结论由Fisher、Lynch、Paterson三位分布式系统领域的科学家在1985年在论文Impossibility of Distributed Consensus with One Faulty Process证明



##### CAP

CAP 述说了三个属性：

* Consistency\(一致性\): all nodes see the same data at the same time
* Availability\(可用性\): node failures do not prevent survivors from continue to operate 
* Partition tolerance\(分区容错性\): The system continue to operate despite message loss due to network and/or node failure.

满足分区容错性的分布式系统，只能在一致性和可用性两者中，选择其中一个。也就是说分布式系统不可能同时满足三个特性.这就需要我们在搭建系统时进行取舍了，那么，怎么取舍才是更好的策略呢? \(https://blog.csdn.net/yeyazhishang/article/details/80758354 \)

* CA without P：如果不要求P（不允许分区），则C（强一致性）和A（可用性）是可以保证的。但放弃P的同时也就意味着放弃了系统的扩展性，也就是分布式节点受限，没办法部署子节点，这是违背分布式系统设计的初衷的。
* CP without A：如果不要求A（可用），相当于每个请求都需要在Server之间强一致，而P（分区）会导致同步时间无限延长\(也就是等待数据同步完才能正常访问服务\)，如此CP也是可以保证的。很多传统的数据库分布式事务都属于这种模式。
* AP wihtout C：要高可用并允许分区，则需放弃一致性。一旦分区发生，节点之间可能会失去联系，为了高可用，每个节点只能用本地数据提供服务，而这样会导致全局数据的不一致性。现在众多的NoSQL都属于此类，如redis,mongdb等。

##### Strong  Consistency  vs Other Consistency

* Strong consistency models \(capable of maintaining a single copy\)
  * Linearizable consistency
  * Sequential consistency
* Weak consistency models\(not strong\)
  * Client-centric consistency model
  * Causal consistency: strongest model avaiable 
  * Eventual consistency model

强一致性模型保证了apparent order and visibility of updates is equivalent to a non-replicated system.  弱一致性模型没有这种保证。

##### 

##### Time and Order



Total Order 

Partial Order



Time is a source of order, it allow us to define the order of operations 

















##### 

##### Raft

#### Demo Video

[http://thesecretlivesofdata.com/raft/](http://thesecretlivesofdata.com/raft/)

Reference:

[https://blog.csdn.net/chen77716/article/details/27963079](https://blog.csdn.net/chen77716/article/details/27963079)

[https://www.cnblogs.com/prpl/p/6799638.html](https://www.cnblogs.com/prpl/p/6799638.html)

