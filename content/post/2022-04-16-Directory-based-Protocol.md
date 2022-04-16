---
title: "Directory-based Protocol"
date: 2022-04-16T16:07:12+08:00
draft: false
description: "计算机体系结构之Cache Coherence的Directory-based Protocol"
categories:
  - Architecture
image: "post/directory1.png"
---

## Snooping-based Protocol缺点

如果有很多processor，那么总线共享，请求传播会带来总线争用，因为所有的processor都会把请求往总线上广播。这种策略适合processor较少的情况下。

## Directory-based Protocol

 Directory-based 协议比 Snooping-based协议早产生。

 Directory-based 协议下，每个processor有自己的Cache、Memory、以及Directory。每一个processor可以进行点对点通信（就是直接通信），这里没有广播，没有共享的总线，没有总线争用。



![img](post/directory2.png)



如上图所示：P1想要访问A，而A正在P3的Memory中，这时P1直接发生与P3的点对点通信，请求获取P3 Memory中的A。那么P3吧A的Copy发送给P1，P1存储在Cache中，而这时P3在自己的Directory中记下A的状态：Shared with P1。



 如果此时，P3也想要请求A，那么P3也会在自己的Directory中记下P3作为A的共享者。当P3想要修改A的时候，P3先检查自己的Directory，查看还有谁拥有A，它会向P1发送一条请求，让P1把Cache中的A无效（Invalidate）。而P1也会向P3发送一条确认请求，告诉P3自己已经把A无效化。这时候P3就可以放心修改A了，P3会现在自己的Directory中把 A:S_P1删除掉（因为P1中已经标记为Invalid）。 



当P3修改了A之后，P3将Directory中的A:S_P3修改为：A:M_P3，这个时候，P3可以使用write through和write back。 



然而，现在，P1中的A为无效状态，被认为是Cache Miss，称为 **Coverage Miss**。