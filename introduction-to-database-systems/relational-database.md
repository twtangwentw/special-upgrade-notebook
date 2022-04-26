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

### 2.2.1 基本的关系操作 <a href="#2.2.1" id="2.2.1"></a>

> 关系模型中常用的关系操作包括<mark style="color:blue;">查询(query)</mark>和<mark style="color:purple;">插入(insert)</mark>、<mark style="color:purple;">删除(delete)</mark>、<mark style="color:purple;">更新(update)</mark>两大类操作

{% hint style="info" %}
查询操作可分为：<mark style="color:blue;">选择(select)、投影(project)、连接(join)、除(div)、并(union)、差(except)、交(intersection)、笛卡尔积</mark>等

其中<mark style="color:blue;">选择</mark>、<mark style="color:purple;">投影</mark>、<mark style="color:orange;">并</mark>、<mark style="color:red;">差</mark>、<mark style="color:green;">笛卡尔积</mark>是5种基本操作

关系的操作特点是集合操作方式，即操作的对象和结构都是集合
{% endhint %}

## 2.3 关系完整性 <a href="#2.3-relationship-integrity" id="2.3-relationship-integrity"></a>

{% hint style="info" %}
关系模型中又<mark style="color:blue;">三类</mark>完整性约束条件：<mark style="color:orange;">实体完整性(entity integrity)</mark>、<mark style="color:red;">参照完整性(referential integrity)</mark>、<mark style="color:green;">用户自定义完整性(user-defined integrity)</mark>
{% endhint %}

### 2.3.1 实体完整性 <a href="#2.3.1" id="2.3.1"></a>

{% hint style="info" %}
若属性(指一个或一组属性) _<mark style="color:purple;">A</mark>_ 是基本关系 _<mark style="color:purple;">R</mark>_ 的主属性，则 _<mark style="color:purple;">A</mark>_ 不能取空值(null)。
{% endhint %}

实体完整性规则：

1. 实体完整性针对于于基本关系
2. 以主码作为唯一性标识
3. 主码的属性不能为空

### 2.3.2 参照完整性 <a href="#2.3.2" id="2.3.2"></a>

$$F$$ 是<mark style="color:green;">基本关系</mark> $$R$$ 的一个或一组<mark style="color:red;">属性</mark>，但不是关系 $$R$$ 的码，$$K_s$$ 是基本关系 $$S$$ 的<mark style="color:red;">主码</mark>。如果 $$F$$ 与 $$K_s$$ 相对应，则称 $$F$$ 是 $$R$$ 的外码(foreign key)，并称基本关系 $$R$$ 为<mark style="color:blue;">参照关系</mark>(referencing relation)，基本关系 $$S$$ 为被<mark style="color:blue;">参照关系</mark>(referenced relation)或<mark style="color:blue;">目标关系</mark>(target relation)。且关系 $$R$$ 与 $$S$$ 可以是相同的关系

<img src="../.gitbook/assets/file.drawing (2).svg" alt="参照完整性示意图" class="gitbook-drawing">

参照完整性规则：

1. 外码可以取空值
2. 外面可以取被参照关系某个元组的主码值

### 2.3.3 用户自定义完整性 <a href="#2.3.3" id="2.3.3"></a>

{% hint style="info" %}
用户自定义完整性就是针对某一具体关系数据库的约束条件，它反映某一具体值应用所涉及的数据必须满足的予以要求
{% endhint %}

## 2.4 关系代数 <a href="#2.4-relational-algebra" id="2.4-relational-algebra"></a>

{% hint style="info" %}
关系代数是一种<mark style="color:orange;">抽象的查询语言</mark>，它用<mark style="color:blue;">对关系</mark>的<mark style="color:red;">运算</mark>来表达查询

传统集合运算：<mark style="background-color:purple;">并、差、交、笛卡尔积</mark>

专门的关系运算：<mark style="background-color:red;">选择、投影、连接、除</mark>
{% endhint %}

| 运算符        | 含义   |
| ---------- | ---- |
| $$\cup$$   | 并    |
| $$-$$      | 差    |
| $$\cap$$   | 交    |
| $$\times$$ | 笛卡尔积 |
| $$\sigma$$ | 选择   |
| $$\Pi$$    | 投影   |
| $$\Join$$  | 连接   |
| $$\div$$   | 除    |

### 2.4.1 传统的集合运算 <a href="#2.4.1" id="2.4.1"></a>

1.  并(union)

    $$R \cup S = \text{\{} t | t \in R \lor t \in S \text{\}}$$
2.  差(except)

    $$R \cup S = \text{\{} t | t \in R \land t \notin S \text{\}}$$
3.  交(intersection)

    $$R \cup S = \text{\{} t | t \in R \land t \in S \text{\}}$$
4.  笛卡尔积(cartesian product)

    $$R \cup S = \text{\{} \overgroup{t_rt_s} | t_r \in R \land t_s \in S \text{\}}$$

{% hint style="info" %}
$$R(X, Z)$$，_X_ 和 _Z_ 是属性组。当 t\[_X_] = x 时，x 在 R 中的<mark style="color:red;">象集</mark>定义为

$$Z_x =  \text{\{} t[Z] | t \in R, t[X] = x \text{\}}$$
{% endhint %}

|    X    |    Z    |
| :-----: | :-----: |
| $$x_1$$ | $$z_1$$ |
| $$x_1$$ | $$z_1$$ |
| $$x_1$$ | $$z_2$$ |
| $$x_2$$ | $$z_3$$ |
| $$x_2$$ | $$z_4$$ |
| $$x_3$$ | $$z_5$$ |
| $$x_3$$ | $$z_6$$ |

$$x_1$$在 R 上的象集 $$Z_{x1} =  \text{\{} Z_1,Z_2 \text{\}}$$

$$x_2$$在 R 上的象集 $$Z_{x1} =  \text{\{} Z_3,Z_ 4\text{\}}$$

$$x_3$$在 R 上的象集 $$Z_{x1} =  \text{\{} Z_5,Z_ 6\text{\}}$$

### 2.4.2 专门的关系运算 <a href="#2.4.2" id="2.4.2"></a>

**一、选择：**

{% hint style="info" %}
选择又称限制。它是在关系 _R_ 选择满足给定条件的各元组，记作：$$\color{red} \sigma_F(R) = \text{\{} t | t \in R \land F(t) \text{\}}$$****
{% endhint %}

F表示选择条件，它是一个逻辑表达式，取逻辑值 `true` 或 `false`

逻辑表达式的基本形式为：$$\color{red} X_1 \theta Y_1$$

当 $$X_1 , Y_1$$ 是一个属性值时，$$\theta$$ 是比较运算符：<mark style="color:blue;">大于</mark>($$\gt$$)、<mark style="color:purple;">大于等于</mark>($$\ge$$)、<mark style="color:orange;">小于</mark>($$\lt$$)、<mark style="color:red;">小于等于</mark>($$\le$$)、<mark style="color:yellow;">等于</mark>($$=$$)、<mark style="color:green;">不等于</mark>($$\lt\gt$$)

当 $$X_1 , Y_1$$ 是一个逻辑值是， $$\theta$$ 是逻辑运算符：<mark style="color:green;">与</mark>($$\land$$)、<mark style="color:blue;">或</mark>($$\lor$$)

或者形式为：$$\color{red} \theta Y_1$$，则 $$\theta$$ 为 <mark style="color:purple;">非</mark>($$┐$$)

eg：$$\sigma_{Sdept = 'IS'}(Student)$$

**二、投影：**

{% hint style="info" %}
关系 _R_ 上的投影是从 _R_ 中<mark style="color:purple;">选择出若干属性列</mark>组成新的关系。记住：

$$\color{red} \Pi_A(R) = \text{\{} t[A] | t < R \text{\}}$$
{% endhint %}

A 是属性属性组，R 是关系

eg：$$\Pi_{Sdept}(Student)$$，$$\Pi_{Sname, Sdept}(Student)$$

**三、连接：**

{% hint style="info" %}
从两关系的笛卡尔积中选取属性间满足一定条件的元组。记作：

$$\color{red}{ R \underset{A \theta B}{\Join} S = \text{\{} \widehat{t_rt_s} | t_r \in R \land t_s \in S \land t_r[A] \theta t_s[B] \text{\}}}$$
{% endhint %}

A 和 B 分别为关系 R 和 S 上的数列相等且可比的属性组，θ 是比较运算符

θ 为 = 时的连接为等值连接：$$R \underset{A =B}{\Join} S = \text{\{} \widehat{t_rt_s} | t_r \in R \land t_s \in S \land t_r[A] = t_s[B] \text{\}}$$

<mark style="background-color:purple;">自然连接是一种特殊的等值连接</mark>：要求进行连接的两个关系进行比较的分量是同名的属性组，并在结果在去掉相同的属性列，即 $$R \Join S = \text{\{} \widehat{t_rt_s}[U-B] | t_r \in R \land t_s \in S \land t_r[A] = t_s[B] \text{\}}$$ _U_ 是 _R_ 和 _S_ 的全体属性集合，_B_ 是 _R_ 和 _S_ 共有的属性

外连接：保留自然连接的悬浮元组(共有属性值没有匹配的值的元组)

左连接：只保留S的属性组为空的悬浮元组

右连接：只保留R的属性组为空的悬浮元组

**四、除：**

{% hint style="info" %}
$$R(X, Y)$$，$$S(Y, Z)$$

$$\color {red} R \div S =  \text {\{} t_r[X] \in R \land \Pi_Y(S) \sube Y_x \text {\}} = T$$\
T的属性 包含在 R 但不包含在 S 中，<mark style="color:red;">且T 和 S\[Y] 的笛卡尔积是 R 的子集</mark>
{% endhint %}

### 2.4.3 关系代数总结 <a href="#2.4.3" id="2.4.3"></a>

<mark style="color:purple;">eg：查询所有选修了课程的学生的学号和姓名</mark>

<mark style="color:purple;"></mark>$$\Pi_{Sno, Cno}(SC) \div \Pi_{Cno}(Course) \Join \Pi_{Sno, Sname}(Student)$$<mark style="color:purple;"></mark>

<mark style="background-color:purple;">关系代数经过有限次复合后形成的形成的表达式称为</mark><mark style="background-color:purple;"><mark style="color:red;">**关系代数表达式**<mark style="color:red;"></mark>
