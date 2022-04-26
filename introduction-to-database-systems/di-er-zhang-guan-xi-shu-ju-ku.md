---
description: 关系数据库应用数学方法来处理数据库中的数据
cover: ../.gitbook/assets/1634536200366.jpg
coverY: 0
---

# 第二章\~关系数据库

## 2.1 关系数据结构及形式化定义 <a href="#2.1-relational-data-structure-and-formal-definition" id="2.1-relational-data-structure-and-formal-definition"></a>

关系数据库系统是支持关系模型的数据库系统

### 2.1.1 关系 <a href="#2.1.1" id="2.1.1"></a>

{% hint style="info" %}
关系模型的数据结构<mark style="color:orange;">非常简单</mark>,只包含单一的数据结构——关系
{% endhint %}

关系模型的基本概念：

1.  **域(domain)**

    域是一组具有<mark style="color:blue;">相同数据类型</mark>的<mark style="color:red;">值</mark>的集合\
    eg：自然数、整数、{0，1} 等
2.  **笛卡尔积(cartesian product)**

    给定一组域 $$D_1, D_2, ···, D_n$$，允许其中某些域是相同的，$$D_1, D_2, ···, D_n$$ 的<mark style="background-color:red;">笛卡尔积</mark>为:\
    $$D_1 \times D_2 \times \cdots \times D_n = \text{\textbraceleft}(d_1, d_2, \cdots, d_n) | d_i \in D_i, i = 1, 2, \cdots, n \text{\textbraceright}$$\
    每一个<mark style="color:blue;">元素</mark>$$(d_1, d_2, \cdots, d_n)$$叫做一个<mark style="color:purple;">n元组</mark>(n-tuple)，或者简称<mark style="color:orange;">元组</mark>(tuple)。元组的每一个<mark style="color:red;">值</mark>$$d_i$$叫做元组的一个<mark style="color:green;">分量</mark>(component)
3.  **关系(relation)**

    $$D_1 \times D_2 \times \cdots \times D_n$$的子集叫做在域 $$D_1, D_2, ···, D_n$$上的关系，表示为 $$R(D_1, D_2, ···, D_n)$$\
    R 表示关系的<mark style="color:orange;">名字</mark>， n 表示关系的<mark style="color:red;">目或度</mark>，关系中的每个元素是关系的<mark style="color:red;">元组</mark>，通常用t表示\
    当 $$n = 1$$ 时是单元关系或一元关系，当 $$n = 2$$ 时是二元关系\
    关系每一列的名字称为属性，n 目关系，必有 n 个属性

{% hint style="info" %}
若关系中的某一属性组能<mark style="background-color:red;">唯一标识</mark>一个元组，而其子集不能，则称该属性组为<mark style="color:red;">候选码(candidate key)</mark>
{% endhint %}

* 若一个关系有多个候选码，则选定<mark style="background-color:red;">其中一个</mark>为<mark style="color:red;">主码(primary key)</mark>
* 候选码的属性称为<mark style="color:red;">主属性</mark>，<mark style="background-color:blue;">不包含在任何候选码</mark>的属性称为<mark style="color:green;">非主属性</mark>或<mark style="color:green;">非码属性</mark>
* 关系属性的<mark style="background-color:purple;">所有</mark>属性是这个关系的候选码，则称为<mark style="color:red;">全码(all-key)</mark>

{% hint style="info" %}
关系可以有三种类型：<mark style="color:blue;">基本关系(通常又称为基本表或基表)</mark>、<mark style="color:green;">查询表</mark>、<mark style="color:red;">视图表</mark>
{% endhint %}

基本表的六条性质

1. 列是<mark style="color:blue;">同质的</mark>，即每一列的分量是同一类型的数据
2. 不同的列可出自同一个域，称其中的每一列为一个属性，不同的属性要给予<mark style="color:purple;">不同的属性名</mark>
3. <mark style="color:orange;">列的顺序</mark>无所谓
4. 任意两个元组的候选码<mark style="color:red;">不能取相同的值</mark>
5. <mark style="color:yellow;">行的顺序</mark>无所谓
6. 分量<mark style="color:green;">必须取原子值</mark><mark style="background-color:red;">(是关系模型规范的最基本的一条)</mark>

### 2.1.2 关系模式 <a href="#2.1.2" id="2.1.2"></a>

在关系数据库中，关系模式是型，关系是值

{% hint style="info" %}
<mark style="background-color:red;">关系的描述称为关系模式(relation schema)</mark>，它可以形式化地表示为：

$$R(U, D, DOM, F)$$\
_R_：关系名

_U_：组成该关系的属性名集合

_D_：为 _U_ 中属性所来自的域

_DOM_：为属性向域的映射集合

_F_：为属性间数的依赖关系集合
{% endhint %}

### 2.1.3 关系数据库 <a href="#2.1.3" id="2.1.3"></a>

在一个给定的应用领域中，所有关系的集合构成一个关系数据库。<mark style="color:red;">关系数据库的型</mark>也称为关系数据库模式，<mark style="color:blue;">是对关系数据库的描述</mark>。关系数据库模式包括若干域的定义，以及在这些域上定义的若干关系模式。<mark style="color:red;">关系数据库的值</mark>是这些关系模式在<mark style="color:blue;">某一时刻对应的关系的集合</mark>，通常就称为关系数据库

## 2.2 关系操作 <a href="#2.2-relational-operations" id="2.2-relational-operations"></a>

{% hint style="info" %}
关系模型<mark style="color:blue;">给出</mark>了关系操作的<mark style="color:red;">能力说明</mark>，但<mark style="color:blue;">不</mark>对关系数据库管理系统语言<mark style="color:blue;">给出具体</mark>的<mark style="color:red;">语法要求</mark>
{% endhint %}

#### 2.2.1 基本的关系操作 <a href="#2.2.1" id="2.2.1"></a>

> 关系模型中常用的关系操作包括<mark style="color:blue;">查询(query)</mark>和<mark style="color:purple;">插入(insert)</mark>、<mark style="color:purple;">删除(delete)</mark>、<mark style="color:purple;">更新(update)</mark>两大类操作

{% hint style="info" %}
查询操作可分为：<mark style="color:blue;">选择(select)、投影(project)、连接(join)、除(div)、并(union)、差(except)、交(intersection)、笛卡尔积</mark>等

其中<mark style="color:blue;">选择</mark>、<mark style="color:purple;">投影</mark>、<mark style="color:orange;">并</mark>、<mark style="color:red;">差</mark>、<mark style="color:green;">笛卡尔积</mark>是5种基本操作

关系的操作特点是集合操作方式，即操作的对象和结构都是集合
{% endhint %}

### 2.3 关系完整性 <a href="#2.3-relationship-integrity" id="2.3-relationship-integrity"></a>

