# Consesus problem

Several computers \(or nodes\) achieve consensus if they all agree on some value. More formally:

1. Agreement: Every correct process must agree on the same value.
2. Integrity: Every correct process decides at most one value, and if it decides some value, then it must have been proposed by some process.
3. Termination: All processes eventually reach a decision.
4. Validity: If all correct processes propose the same value V, then all correct processes decide V.



##### FLP Impossibility result\(FLP不可能性\)

在分布式系统中，异步网络（消息延迟可能任意大或丢失，消息可能乱序），只要有一个进程失效（进程死亡或者足够长时间不响应），就不可能设计出一个一致性协议。该结论由Fisher、Lynch、Paterson三位分布式系统领域的科学家在1985年在论文Impossibility of Distributed Consensus with One Faulty Process证明



##### CAP



##### Raft

#### Demo Video

[http://thesecretlivesofdata.com/raft/](http://thesecretlivesofdata.com/raft/)







Reference:

https://blog.csdn.net/chen77716/article/details/27963079

https://www.cnblogs.com/prpl/p/6799638.html



