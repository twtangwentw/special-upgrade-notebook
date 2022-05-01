---
description: >-
  掌握关系数据库理论提出的背景，对给定的数据如何改造数据模型；掌握函数依赖的定义：函数依赖中的部分函数依赖、完全函数依赖、传递函数依赖；对给定的实际问题可以确定函数依赖；掌握码的定义，对实际问题可以确定码；掌握1NF，2NF，3NF，BCNF的定义，对给定的关系模式可以确定属于什么级别的范式
cover: ../.gitbook/assets/2022年5月1日08点47分.jpg
coverY: 0
---

# 第六章\~关系数据理论

## 6.1 问题的提出 <a href="#6.1-statement-of-problem" id="6.1-statement-of-problem"></a>

{% hint style="info" %}
以关系模型为背景，形成了数据库逻辑设计工具——关系数据库规范化理论
{% endhint %}

数据依赖是一个关系内部属性与属性之间的一种关系约束关系。这种约束关系是通过属性间值的相等与否体现出来的数据间相关联系

<mark style="background-color:red;">数据依赖中最重要的是</mark><mark style="background-color:red;">**函数依赖**</mark><mark style="background-color:red;">和</mark><mark style="background-color:red;">**多值依赖**</mark>

## 6.2 规范化 <a href="#6.2-normalize" id="6.2-normalize"></a>

> 关系模式根据规范化程度可以分为第一范式、第二范式、第三范式、第四范式等

### 6.2.1 函数依赖 <a href="#6.2.1" id="6.2.1"></a>

定义：设关系模式 **R(U)** 是属性集 **U** 上的关系模式，**X**，**Y** 是 **U** 的子集。若对与 **R(U)** 的任意一个的可能关系 **r**，**r** 中不可能存在两个元组再 **X** 上的属性值相等，而再 **Y** 上的属性值不相等，则称为 <mark style="color:red;"></mark> <mark style="color:red;"></mark><mark style="color:red;">**X**</mark> <mark style="color:red;"></mark><mark style="color:red;">函数确定</mark> <mark style="color:red;"></mark><mark style="color:red;">**Y**</mark> 或 <mark style="color:purple;">**Y**</mark> <mark style="color:purple;"></mark><mark style="color:purple;">函数依赖于</mark> <mark style="color:purple;"></mark><mark style="color:purple;">**X**</mark>，**记作 X -> Y**

> 函数依赖和别的数据依赖一样是语义规范的概念，只能根据语义来确定一个函数依赖

函数依赖的一些术语和记号：

* $$X \to Y$$，但 $$Y \nsubseteq X$$，则称 $$X \to Y$$ 是**非平凡函数依赖**
* $$X \to Y$$ 但 $$Y \subseteq  X$$，则称 $$X \to Y$$ 是**平凡函数依赖**
* 若 $$X \to Y$$，则 $$X$$ 称为这个函数依赖的**决定属性组**，也称**决定因素**
* 如 $$X \to Y,Y \to X$$，则记作 $$X \gets\to Y$$
* 如果 $$Y$$ 不函数依赖于 $$X$$，则记作 $$X \nrightarrow Y$$

若 X 函数确定 Y，且 X 的任意一个子集都不函数确定 Y，则称 Y 对 X **完成函数依赖**： $$X\overset{F}{\to} Y$$

若 $$X \to Y$$但 $$Y$$ 不完全函数依赖于 $$X$$ ，则称 $$Y$$ 对 $$X$$ **部分函数依赖**：$$X \overset{P}{\to} Y$$

在 $$R(U)$$中如果 $$X\to Y(Y \nsubseteq X),Y \nrightarrow  X,Y \to Z,Z \nsubseteq Y$$，则称 $$Z$$对 $$X$$**传递函数依赖**：$$X\overset{传递}{\to} Z$$

### 6.2.2 码 <a href="#6.2.2" id="6.2.2"></a>

{% hint style="success" %}
码是关系模式中一个重要概念
{% endhint %}

**候选码(K)**根据函数依赖定义为：$$K$$ 为 $$R(U)$$ 中的属性或属性集合，则 $$K \overset{F}{\to} U$$（即完全函数依赖）

超码是函数依赖，是候选码的超集

在任何候选好的属性称为<mark style="color:purple;">**主属性**</mark>，其它为<mark style="color:red;">**非主属性**</mark>

### 6.2.3 范式 <a href="#6.2.3" id="6.2.3"></a>

{% hint style="info" %}
<mark style="color:purple;">关系数据库中的关系是要满足一定要求的，满足不同程度要求的为不同的范式</mark>

各范式之间的关系

$$5NF \sub 4NF \sub BCNF \sub 3NF \sub 2NF \sub 1NF$$
{% endhint %}

### 6.2.4 \~ 1NF <a href="#6.2.4" id="6.2.4"></a>

<mark style="color:red;background-color:blue;">符合一个最基本的条件：每一个分量必须是一个不可分的数据项</mark>

### 6.2.5 \~ 2NF <a href="#6.2.5" id="6.2.5"></a>

**定义**：若 $$R \in 1NF$$ 且每一个非主属性完全函数依赖于任何一个候选码,$$R \in 2NF$$

<mark style="background-color:purple;">**即不存在候选码中的部分主属性能函数确定部分非主属性**</mark>

### 6.2.6 \~ 3NF <a href="#6.2.6" id="6.2.6"></a>

**定义**：设关系模式 $$R(U, F) \in 1NF$$，若 $$R$$ 中不存在这样分码 $$X$$，属性组 $$Y$$，即非属性组 $$Z(Z \nsubseteq Y)$$，使得 $$X \to Y，Y \to Z$$ 成立 $$Y \nrightarrow X$$，则称 $$R(U, F) \in 3NF$$

<mark style="background-color:green;">**即关系模式中没有任何非主属性函数依赖于非主属性**</mark>

### 6.2.7 \~ BCNF <a href="#6.2.7" id="6.2.7"></a>

**定义**：关系模式 $$R(U, F) \in 1NF$$，若 $$X \to Y$$ 且 $$Y \nsubseteq X$$ 时 $$X$$ 必须含有码，则 $$R(U, F) \in BCNF$$

<mark style="background-color:red;">**即非主属性的函数依赖属性必须是码**</mark>

### 6.2.8 \~ 多值依赖 <a href="#6.2.8" id="6.2.8"></a>
