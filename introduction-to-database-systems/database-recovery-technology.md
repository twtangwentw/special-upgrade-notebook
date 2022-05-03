---
description: 掌握事务的概念及特征，了解数据库恢复的基本原则和恢复的具体实现方法
---

# 第八章\~数据库恢复技术

## 8.1 事务的基本概念 <a href="#8.1-basic-concepts-of-transaction" id="8.1-basic-concepts-of-transaction"></a>

### 8.1.1 事务 <a href="#8.1.1" id="8.1.1"></a>

{% hint style="info" %}
是用户定义的一个数据库的一系列操作序列，这些操作要么全部做，要么全部不做，是一个不可分割的工作单位
{% endhint %}

事务相关的三条 SQl 语句：

* BEGIN TRANSACTION：开启事务
* COMMINT：提交事务的所有操作
* ROLLBACK：回滚，撤销事务中已进行了的操作

### 8.1.2 事务的 ACID 性 <a href="#8.1.2" id="8.1.2"></a>

事务具有的是个特性：<mark style="color:blue;">原子性(Atomicity)</mark>、<mark style="color:purple;">一致性(Consistency)</mark>、<mark style="color:orange;">隔离性(Isolation)</mark>、<mark style="color:red;">持续性(Durability)</mark>

破坏事务 ACID 性的因素有：

* 多个事务同时运行，不同事物的操作交叉执行
* 事物再运行过程中被强行终止

## 8.2 数据库恢复概述 <a href="#8.2-overview-of-database-recovery" id="8.2-overview-of-database-recovery"></a>

数据库管理系统将数据库从错误恢复到某一已知的正确状态。恢复子系统是数据库管理系统的一个重要组成部分，且十分庞大

## 8.3 故障的种类 <a href="#8.3-type-of-fault" id="8.3-type-of-fault"></a>

### 8.3.1 事务内部的故障 <a href="#8.3.1" id="8.3.1"></a>

事务内部的故障有的是可以通过事务程序本身发现，但是更多的是非预期的，是不能有应用程序处理的

{% hint style="info" %}
恢复程序再<mark style="color:purple;">不影响其它事务运行</mark>的情况下，强行回滚该事务，即撤销该事务已经作出的任何数据库的修改，使得该事务好像没有启动一样，这类恢复操作称为<mark style="color:green;">**事务撤销**</mark>
{% endhint %}

### 8.3.2 系统故障 <a href="#8.3.2" id="8.3.2"></a>

是指造成系统停止运行的任何事件，使得系统要重新启动

{% hint style="info" %}
这类故障，恢复子系统需要进行<mark style="color:purple;">**重做**</mark>操作，即撤销所有没有完成的事务提交，还需要重做已经提交的事务，将数据库恢复到一致状态
{% endhint %}

### 8.3.3 介质故障 <a href="#8.3.3" id="8.3.3"></a>

指外存故障，即磁盘损坏、磁头碰撞等

### 8.3.4 计算机病毒 <a href="#8.3.4" id="8.3.4"></a>

## 8.4 数据恢复的基本原理 <a href="#8.4-basic-principles-of-data-recovery" id="8.4-basic-principles-of-data-recovery"></a>

用一个词概括: <mark style="color:red;">**冗余**</mark>

> 数据库中的任何一部分破坏或不正确的数据可以根据系统再别处冗余数据来重建

## 8.5 恢复技术的实现 <a href="#8.5-implementation-of-recovery-technology" id="8.5-implementation-of-recovery-technology"></a>

### 8.5.1 数据转储 <a href="#8.5.1" id="8.5.1"></a>

数据转储是数据恢复中采用的<mark style="color:purple;">基本技术</mark>

由数据库管理员定期将整个数据库复制到磁盘或其它存储介质上保存，这个保存起来的数据称为**后备副本**

当数据库遭到破坏后可以将后备副本重新装入，但<mark style="background-color:red;">重装后备副本只能将数据库恢复到转储时的状态</mark>

数据转储分类：静态转储和动态转储、增量转储和海量转储

### 8.5.2 登记日志文件 <a href="#8.5.2" id="8.5.2"></a>

**1.日志文件的格式和内容：**

日志文件是用来记录事务对数据库的更新操作的文件，日志文件的两种主要格式：<mark style="color:purple;">以记录为单位的日志文件</mark>和<mark style="color:blue;">以数据库为单位的日志文件</mark>

{% tabs %}
{% tab title="对于以记录为单位的日志文件" %}


日志文件中需要登记的内容包括：

* 各个事务的开始（BEGIN TRANSACTION）标记
* 各个事务的结束（COMMIT 和 ROLLBACK）标记
* 各个事务的所有更新操作

每一个日志记录的内容主要包括

* 事务标识（标明是哪个事务的操作）
* 操作的类型（插入、删除、修改）
* 操作对象（记录内部标识）
* 更新前数据的旧值（对插入而言这项为空）
* 更新后数据的新值（对删除而言这项为空）
{% endtab %}

{% tab title="对于以数据块为单位的日志文件" %}
日志记录的内容包括事务标识和被更新的数据块
{% endtab %}
{% endtabs %}

**2.日志文件的作用**

1. 事务故障恢复和系统故障恢复必须用日志文件
2. 再动态转储方式中必须建立日志文件，后备副本和日志文件结合起来才能有效的恢复数据库
3. 再静态转储方式中也可以建立数据库文件，当数据库被毁坏后可以重新插入后备副本把数据库恢复到转储结束时刻的正确状态，然后利用日志文件把已完成的事务进行重做处理，对故障发生时尚未完成的事务进行撤销处理

**3.登记日志文件**

为保证数据库是可恢复的，登记日志文件时必须遵守两条原则：

* 登记的次序严格按并发事务执行的事件次序
* 必须先写日志文件，后写数据库
