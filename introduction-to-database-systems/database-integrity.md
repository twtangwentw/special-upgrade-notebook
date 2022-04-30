---
description: 掌握完整性约束条件及完整性控制
cover: ../.gitbook/assets/2022年4月30日07点46分.jpg
coverY: 0
---

# 第五章\~数据库完整性

{% hint style="info" %}
数据库的完整性是指数据的<mark style="color:blue;">正确性</mark>和<mark style="color:purple;">相容性</mark>

<mark style="background-color:red;">**正确性：**</mark>指数据是符合现实世界语义、反映当前实际状况的

<mark style="background-color:purple;">**相容性：**</mark>数据库同一对象在不同关系表中的数据是符合逻辑的
{% endhint %}

## 5.1 实体完整性 <a href="#5.1-entity-integrity" id="5.1-entity-integrity"></a>

### 5.1.1 定义实体完整性 <a href="#5.1.1" id="5.1.1"></a>

```sql
// 在列级定义主码
CREATE TABLE Student (
    Sno CHAR(9) PRIMARY KEY,
    ···
);

// 在表级定义主码
CREATE TABLE Student (
    Sno CHAR(9),
    ···,
    PRIMARY KEY(Sno)
);

// 使用多个主码构成主码
CREATE TABLE SC (
    Sno CHAR(9),
    Cno CHAR(4),
    ···,
    PRIMARY KEY(Sno, Cno)
);
```

### 5.1.2 实体完整性检查和违约处理 <a href="#5.1.2" id="5.1.2"></a>

1. 检查主码值是否唯一，如果不唯一则拒绝插入或修改
2. 检查主码的各个属性是否为空，只要有一个为空就拒绝插入或修改

## 5.3 参照完整性 <a href="#5.3-referential-integrity" id="5.3-referential-integrity"></a>

### 5.2.1 定义参照完整性 <a href="#5.2.1" id="5.2.1"></a>

```sql
// 在表级上使用
FOREIGN KEY (<属性>) REFERENCES <表名>(<属性>)
```

### 5.2.2 参照完整性检查和违约处理 <a href="#5.2.2" id="5.2.2"></a>

> 参照完整性，将两个表相应元组联系起来了

```sql
FOREIGN KEY (<属性>) REFERENCES <表名>(<属性>) ON UPDATE CASCADE ON DELETE CASCADE
// 当更新或删除被参照表时，参照表也更新

// 除了 CASCADE 还有 NO ACTION, SET NULL
// NO ACTION 为默认，直接拒绝操作
```

## 5.3 用户自定义完整性 <a href="#5.3-user-defined-integrity" id="5.3-user-defined-integrity"></a>

### 5.3.1 属性上的约束条件 <a href="#5.3.1" id="5.3.1"></a>

* **列值非空:** `NOT NULL`
* **列值唯一:** `UNIQUE`
* **检查列值是否满足一个条件表达式:** `CHECK`

{% hint style="info" %}
当插入元组或修改属性值时，关系数据库关系统统将检查属性上的约束条件是否满足，如果不满住则操作被拒绝执行
{% endhint %}

### 5.3.2 元组上的约束条件 <a href="#5.3.2" id="5.3.2"></a>

```sql
// 在表级上定义
CREATE TABLE Student (
    ···,
    CHECK (Ssex = '女' OR Sname NOT LIKE 'Ms.%')
);
```

{% hint style="info" %}
当插入元组或修改属性值时，关系数据库关系统统将检查表上的约束条件是否满足，如果不满住则操作被拒绝执行
{% endhint %}

## 5.4 完整性约束命名子句 <a href="#5.4-integrity-constraint-named-clause" id="5.4-integrity-constraint-named-clause"></a>

_<mark style="color:purple;">**`CONSTRAINT`**</mark><mark style="color:orange;">**`<完整性约束名>`**</mark><mark style="color:red;">**`<完整性约束条件>`**</mark>_

**1. 根据名字删除完整性约束:**

```sql
ALTER TABLE <表名> DROP CONSTRAINT <完整性约束名>;
```

2\. 修改完整性约束：先删除后添加

```sql
ALTER TABLE <表名> DROP CONSTRAINT <完整性约束名>;
ALTER TABLE <表名> ADD CONSTRAINT <完整性约束名> <完整性约束条件>;
```

## 5.5 断言 <a href="#5.5-assertion" id="5.5-assertion"></a>

```sql
// 创建断言
CREATE ASSERTION <断言名> <CHECK 子句>;
// 删除断言
DROP ASSERTION <断言名>;
```

## 5.6 触发器 <a href="#5.6-trigger" id="5.6-trigger"></a>

```sql
// 触发器格式
CREATE TRIGGER <触发器名>
{BEFORE | AFTER} <触发事件> ON <表名> // INSERT, DELETE, UPDATE, UPDATE OF (列···), * OR * ···
REFERENCING NEW | OLD ROW AS <变量>
FOR EACH {ROW | STATEMENT}
[WITH <触发条件>] <触发动作体>;


// eg:
// 当对表 SC 的 Grade 属性修改时，若分数增加了 10% ，则将此此操作记录到另一个表
// SC_U(Sno, Cno, OldGrade, NewGrade)
CREATE TRIGGER SC_T
AFTER UPDATE OF Grade ON SC
REFERENCING 
    OLDROW AS OldTuple
    NEWROW AS NewTuple
FOR EACH ROW
WITH (NewTuple.Grade >= 1.1 * OldTuple.Grade)
    INSERT INTO SC_U(Sno, Cno, OldTuple.Grade, NewTuple.Grade)
    VALUES (OldTuple.Sno, OldTuple.Cno, OldTupel.Grade, NewTuple.Grade);
    
    
// 删除触发器
DROP TRIGGER <触发器名> ON <表名>;
```
