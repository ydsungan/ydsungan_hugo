---

title: "Snooping-based Protocol"
date: 2022-04-16T15:15:12+08:00
draft: false
description: "计算机体系结构之Cache Coherence的Snooping-based Protocol"
categories:
  - Architecture
---

# Snooping-based Protocol

参考：https://www.youtube.com/watch?v=YNpaELJZm2c



![img](post/snooping1.png)



如上图所示：有 4 个 processor，P1-P4，每个 processor 下面的方框表示私有的 Cache。此时，P1要读主存中的 A，而 P1 的 Cache（方框）为空，表示 Miss，所以它向共享的 Bus 发送一条请求，这个请求在 Bus 上广播出去，因为每个 processor 的 Cache都在监听着总线，所以每个 processor 的Cache都会知道和理解这个请求。由于其他的 processor （P2-P4）的Cache里面都没有A，所以其他的 processor 都忽略这个请求。 



![img](post/snooping2.png)



 如上图所示，P1的Cache已经把A从主存请求过来了，这时候的P1的Cache中的A为shared状态。现在，P3也要请求主存中的A，经过同样的向总线发送请求的过程，将 A 请求到 P3 的Cache中，如下图所示，A的状态也是 Shared状态。



![img](post/snooping3.png)



现在，P1修改了A的值，将A修改为8，此时，P1的Cache的A的状态为Modified，【无图】 

- write invalidate 策略：当P1的Cache的A的状态修改为M后，会像总线发送一条 invalidation 信号让总线上的所有的processor都将他们自己cache内的A改为invalid状态。所以P3中的A的状态修改为 I （Invalid）

  - Write Through：马上把Cache中的A = 8写回主存，如下图所示。

  - Write Back：会等到下一次Cache置换的时候才把A=8写回主存。

    

 ![img](post/snooping4.png)



然而现在，P3的Cache中的A是一个无效值，认为这是一个Cache Miss，叫做 **Coherence Miss**. 

- write update策略：不会像write invalidate策略那样向总线发送一条无效信号，而是发送一条更新信号，通知其他的processor去更新他们本地的Cache Copy。当所有其他的processor更新自己的cache中的A后，所有的Cache中的A的状态改为Shared，包括P1.
  - write through
  - write back：均和上述相同 



目前AMD和Intel的CPU都是采用的Snooping-based Protocol。