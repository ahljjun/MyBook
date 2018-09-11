### 介绍

线程池被广泛使用在各种场景种。线程池+Event Loop的方式可以提高吞吐率，实现任务的异步处理。

本质上是生产者/消费者模型。客户端负责生产task，threads pool 作为消费端不停的去从一个Task Queue中获取task

进行处理。

### 构成

一般由   一个Bounded Queue + 两个condition variable + 互斥量\(mutex\) 来实现





