---
title: CQRS
date: 2019-09-01 03:13:17
tags: DDD,软件架构,API设计,事件架构
---

# CQRS

## 什么是CQRS？

CQRS 是指将读模型和写模型分离，从而可以分而治之。减少局部系统的复杂性和负载能力。但是会提高软件的全局复杂度。

## 传统信息系统模型架构

### 初期架构

![cqrs-single-Model](https://i.postimg.cc/FKXNSTCP/cqrs-single-Model.png)



如图，我们的model 会提供CRUD功能。DB提供信息的持久化，UI负责交互(model内容聚合展示，传入更新指令)。

### 分层架构下的复杂度解决方案

当展示业务展示越来越负复杂。而我们的模型还是单一的话，在分层结构中我们的model可能如下：

单model 模型的决定可能根据**Command model**(负责写入) + **Intergretion point Model**(一个可以较中立的满足所有展示层的Model)

![cqrs-complex-Representation](https://i.postimg.cc/0yw9rTnM/cqrs-complex-Representation.png)

#### 带来的问题

- 很难定义Single Mode,由于尝试着将多个复杂而相关度较低的的模型做统一抽象，这样恰到好处的抽象存在难度。
- 展示业务较为复杂。如果single model 偏向于command Model ，这会造成query视图的衍生逻辑相当复杂。
- 不适合高性能的实时展示模型。由于展示业务较为复杂，所以不适合高性能的实时展示模型

## CQRS架构下的解决方案(模型分离)

![cqrs-split-Model](https://i.postimg.cc/GpPrxvZb/cqrs-split-Model.png)



### 优点

- 解决了Model抽象难的问题，当一个问题无法较好地达成统一时，或许分而治之才是解决之道。
- 两个model的模式，可简化业务代码的复杂性，因为不同的model都是针对于不同的场景抽象的，他们各自更贴近他们的目标场景，以任务报表系统为例，Command Model 的抽象关注于任务具体长什么样，而报表端关注于报表指标的抽象。
- 可满足高性能的实时展示。由于启动数据是离线算好。我们只需让Query Model关注Command Model的更新，就可以实现高性能的查询。

### 带来的问题

- 增加了系统的复杂度。
- 数据一致性延迟问题，会存在Command Model到Query Model交互，和 Query Model 应用Command Model change 的延迟。造成展示视图的延迟一致。

原文链接[CQRS](https://martinfowler.com/bliki/CQRS.html)



