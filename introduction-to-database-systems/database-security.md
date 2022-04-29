---
description: 掌握计算机系统的三类安全性问题以及数据库安全性控制的基本技术
cover: ../.gitbook/assets/1634536211255.jpg
coverY: 0
---

# 第四章\~数据库安全性

## 4.1 数据库安全性概述 <a href="#4.1-overview-of-database-securit" id="4.1-overview-of-database-securit"></a>

{% hint style="info" %}
数据库的安全性是指<mark style="color:purple;">保护数据库</mark>以防止不合法使用所造成的<mark style="color:red;">数据泄露、更改或破坏</mark>
{% endhint %}

### 4.1.1 数据库的不安全因素(即三类安全性问题) <a href="#4.1.1" id="4.1.1"></a>

1.  **非授权用户对数据库的恶意存取和破坏**

    假冒用户偷取、修改甚至破坏用户数据
2.  **数据库中重要或敏感的数据被泄露**

    数据库中的重要数据被盗取、机密信息被暴露
3.  **安全环境脆弱性**

    操作系统安全性脆弱，网络协议安全保障的不足等都会造成数据库安全性的破解

## 4.2 数据库安全性控制 <a href="#4.2-database-security-control" id="4.2-database-security-control"></a>

数据库安全技术主要包括：用户身份鉴定、多层存取控制、审计、视图和数据加密等

> 数据库安全按保护的存取控制流程：
>
> 1. 数据库管理系统对提出 SQL 访问请求的数据库用户进行身份鉴定、防止不可信用户是使用系统
> 2. 在 SQL 处理层进行自主存取控制和强制存取控制，进一步还可以进行推理控制
> 3. 为监控恶意访问，可根据具体安全需求配置审计规则，对用户访问行为和系统关键操作进行审计
> 4. 通过设置简单入侵检测规则，对异常用户行为检测和处理
> 5. 在数据层，对数据进行有效备份

### 4.2.1 用户身份鉴定 <a href="#4.2.1" id="4.2.1"></a>

用户是否鉴定的方法：

1. 静态口令鉴定：固定不变的密钥
2. 动态口令鉴定：一次一密
3. 生物特定鉴定：如指纹
4. 智能卡鉴定：不可复制的硬件

### 4.2.2 存取控制 <a href="#4.2.2" id="4.2.2"></a>

<mark style="background-color:red;">存取控制机制主要包括定义用户权限和合法权限检查两部分</mark>

1.  定义用户权限

    将用户的操作权限记录在数据字典中
2.  合法权限检查

    用户发出存取请求后，数据库管理系统查找数据字典，根据安全规则进行合法权限检查，若用户操作请求超出了定义的权限，系统将拒绝执行此操作

{% hint style="info" %}
定义用户权限和用户权限检查机制一起组成了数据库管理系统的存取控制子系统

C2 级的数据库管理系统支持<mark style="background-color:blue;">自主存取控制（DAC）</mark>

B1 级的数据库管理系统支持<mark style="background-color:red;">强制存取控制（MAC）</mark>
{% endhint %}

**自主存取控制：**

用户对不同数据库有不同的存取权限、不同用户对同一数据库也有不同的存取权限，而且用户还可以将其拥有的存取权限转授给其它用户

**强制存取控制：**

每一个对象被标记为一个密级，每一个用户也被标记为拥有某一个级别的许可证。对于任意一个对象，只有具有合法许可证的用户才可以存取。强制存取控制相对比较严格

### 4.2.3 自主存取控制方法 <a href="#4.2.3" id="4.2.3"></a>

<mark style="background-color:red;">用户权限的两个要素：</mark><mark style="background-color:red;">**数据库对象**</mark><mark style="background-color:red;">和</mark><mark style="background-color:red;">**操作类型**</mark>，<mark style="background-color:purple;">定义存取权限称为</mark><mark style="background-color:purple;">**授权（authorization）**</mark>

{% tabs %}
{% tab title="数据库模式存取权限" %}
| 对象  | 操作类型                     |
| --- | ------------------------ |
| 模式  | CREATE SCHEMA            |
| 基本表 | CREATE TABLE、ALTER TABLE |
| 视图  | CREATE VIEW              |
| 索引  | CREATE INDEX             |
{% endtab %}

{% tab title="数据存取权限" %}
|        |                                                           |
| ------ | --------------------------------------------------------- |
| 基本表和视图 | SELECT、INSERT、UPDATE、DELETE、REFERENCES、**ALL PRIVILEGES** |
| 属性列    | SELECT、INSERT、UPDATE、REFERENCES、**ALL PRIVILEGES**        |
{% endtab %}
{% endtabs %}

### 4.2.4 授权：授予和回收 <a href="#4.2.4" id="4.2.4"></a>

<mark style="color:purple;">**GRANT**</mark>：授予权限

<mark style="color:red;">**REVOKE**</mark>：回收权限

{% hint style="info" %}
GRANT 和 REVOKE 用户授予或回收用户对数据的操作权限
{% endhint %}

```sql
// =============================
// GRANT 语句的一般格式
GRANT <权限>[,<权限>] ···
ON <对象类型><对象名>[,<对象名> ···][,<对象类型><对象名>[,<对象名> ···]] ···
TO <用户>[,<用户>] ···                           // 如果用户是 PUBLIC ，则代表全体用户
[WITH GRANT OPTION];                            // 允许用户对其它用户授权，但是不能循环

// 把查询 Student 表的权限授给用户 U1
GRANT SELECT
ON TABLE Student
TO U1;

// 把对 Student 和 Course 表的全部操作权限授予用户 U2 和 U3
GRANT ALL PRIVILEGES
ON TABLE Studnet, Course
TO U2, U3;

// 把对表 SC 的查询权限授予所有用户
GRANT SELECT
ON TABLE SC
TO PUBLIC;

// 把查询 Student 表，和修改学生学号的权限授予用户 U4
GRANT UPDATE(Sno), SELECT
ON TABLE Studetn
TO U4;

// =============================
// REVOKE 语句的一把格式
REVOKE <权限>[,<权限>] ···
ON <对象类型><对象名>[,<对象名> ···][,<对象类型><对象名>[,<对象名> ···]] ···
FROM <用户>[,<用户>] ··· [CASCADE | RESTRICT]; // 使用 CASCADE 当移除用户权限时会移除用户授权给其它用户的权限，如果使用了 RESTRICT 则但有授权给其它用户时，则会移除失败

// 把用户 U4 修改学生学号的权限回收
REVOKE UPDATE(Sno)
ON TABLE Student
FROM U4;

// 回收所有用户对表 SC 的查询权限
REVOKE SELECT
ON TABLE SC
FROM PUBLIC;
```

<mark style="color:red;">**CREATE USER**</mark>：用户创建用户，并在创建用户时对用户的数据库模式的权限进行限制

格式为：_`CREATE USER <用户名> [WITH] [DBA | RESOURCE | CONNECT]`_

* 只有系统超级用户(DBA)才能有创建一个新的数据库用户的能力
* 创建用户的默认权限为 CONNECT

_**用户默认的权限表：**_

| 角色       | CREATE USER | CREATE SCHEMA | CREATE TABLE | 登录数据库，执行数据查询和操纵   |
| -------- | ----------- | ------------- | ------------ | ----------------- |
| _DBA_    | 可以          | 可以            | 可以           | 可以                |
| RESOURCE | 不可以         | 不可以           | 可以           | 可以                |
| CONNECT  | 不可以         | 不可以           | 不可以          | 可以，但是需要GRANT 授予权限 |

### 4.2.5 数据库角色 <a href="#4.2.5" id="4.2.5"></a>

> <mark style="background-color:purple;">数据库角色是被命名的一组与数据库相关的权限，角色是权限的集合</mark>

```sql
// 角色的创建
CREATE ROLE <角色名>;

// 给角色授权
GRANT <权限>[,<权限>] ···
ON <对象类型><对象名>
TO <角色名>[,<角色名>] ···;

// 将一个角色授予其它的角色或用户
GRANT <角色>[,<角色>] ···
TO <角色>[,<角色>] ···;
[WITH ADMIN OPTION];

// 角色权限的回收
REVOKE <权限>[,<权限>] ···
ON <对象类型><对象名>
FROM <角色>[,<角色>] ···;


// ===================
// 创建角色 R1
CREATE ROLE R1;

// 授予角色 Student 表的查询、更新、插入权限
GRANT SELECT, UPDATE, INSERT
ON TABLE Student
TO R1;

// 将角色 R1 授予 WANG、ZHANG、ZHAO
GRANT R1
TO WANG, ZHANG, ZHAO;

// 回收 WANG 的 R1 角色
REVOKE R1
FROM WANG

// 给角色 R1 添加删除权限
GRANT DELETE
ON TABLE Student
TO R1;

// 移除角色 R1 的查询权限
GRANT SELECT
ON TABLE Student
FROM R1;
```

### 4.2.6 强制存取控制 <a href="#4.2.6" id="4.2.6"></a>

在强制存取控制中，数据库管理系统所管理的全部实体分为主题和客体两大类

<mark style="color:purple;">**主体**</mark>：系统中的活动实体，包括数据库管理系统所管理的实际用户，也包括代表用户的个进程

<mark style="color:purple;">**客体**</mark>：系统中的被动实体，受主题操纵，包括文件、基本表、索引、视图等

对于主体和客体，数据库管理系统为他们每个实例指派一个<mark style="background-color:red;">**敏感度标记**</mark>

敏感度标记包括：

1. 绝密（Top Secrt，TS）
2. 机密（Secret，S）
3. 可信（Confidential，C）
4. 公开（Public，P）

密级的次序是 <mark style="color:blue;">**TS >= S >= C >= P**</mark>

## 4.3 视图机制 <a href="#4.2-view-control" id="4.2-view-control"></a>

{% hint style="info" %}
为不同的用户定义不同的视图，把数据对象限制在一定的范围内。也就是通过视图机制把要保密的数据隐藏对无权存取的用户隐藏起来，从而自动对数据提供一定程度的安全保护
{% endhint %}

<mark style="background-color:red;">视图机制简介地实现支持存取谓词地用户权限定义</mark>

## 4.4 审计 <a href="#4.4-audit" id="4.4-audit"></a>

{% hint style="info" %}
审计功能把用户对数据库地所有操作自动记录下来放入到审计日志中，审计员可以通过审计日志监控数据库中地各种行为，重现导致数据库现有状态改变地一系列事件，找出非法存取地数据地人、时间和内容等
{% endhint %}

## 4.5 数据加密 <a href="#4.5-data-encryption" id="4.5-data-encryption"></a>

1. 存储加密
2. 传输加密

## 4.6 其它安全性保护 <a href="#4.6-other-security-protection" id="4.6-other-security-protection"></a>

1. 推理控制
2. 隐藏信道
3. 数据隐私保护
