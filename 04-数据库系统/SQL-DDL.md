# SQL：DDL

> 模块：04 数据库系统 ｜ 状态：🟦 待真题校准

## 定义

DDL（Data Definition Language，数据定义语言）用于创建、修改和删除数据库对象的结构。

常见命令：

- `CREATE`：创建对象。
- `ALTER`：修改对象结构。
- `DROP`：删除对象。
- `TRUNCATE`：清空表中全部数据并保留表结构，考试中通常归入 DDL。

## 要点

### 1. 创建表

```sql
CREATE TABLE Student (
    student_id   CHAR(10) PRIMARY KEY,
    student_name VARCHAR(50) NOT NULL,
    email        VARCHAR(100) UNIQUE,
    credits      INT DEFAULT 0,
    CHECK (credits >= 0)
);
```

常见约束：

- `PRIMARY KEY`：唯一且非空，一张表只有一个主键，但主键可由多列组成。
- `FOREIGN KEY`：保证非空引用值在被引用关系中存在。
- `UNIQUE`：限制列或列组合不得重复。
- `NOT NULL`：要求必须填写该列。
- `CHECK`：限制值满足指定条件。
- `DEFAULT`：未提供列值时使用默认值，不等于非空约束。

### 2. 组合主键

```sql
CREATE TABLE Enrollment (
    student_id CHAR(10),
    course_id  CHAR(10),
    score      DECIMAL(5,2),
    PRIMARY KEY (student_id, course_id),
    CHECK (score BETWEEN 0 AND 100)
);
```

`student_id＋course_id`是一个由两列组成的主键，不是两个独立主键。

### 3. 外键、非空与唯一的职责

```sql
department_id CHAR(10) NOT NULL,
FOREIGN KEY (department_id)
    REFERENCES Department(department_id)
```

- `NOT NULL`：每名员工必须填写部门号。
- `FOREIGN KEY`：部门号必须引用已存在的部门。
- 不添加 `UNIQUE`：允许多名员工属于同一部门。

外键不会自动唯一，也不会自动非空。若要表达 1:1 联系，通常需根据业务规则额外添加 `UNIQUE`；若要求必须参与，再添加 `NOT NULL`。

### 4. 修改表结构

增加列：

```sql
ALTER TABLE Product
ADD stock INT DEFAULT 0;
```

增加约束：

```sql
ALTER TABLE Student
ADD CONSTRAINT uk_student_email
UNIQUE (email);
```

删除列的常见写法：

```sql
ALTER TABLE Student
DROP COLUMN phone;
```

修改列类型和删除约束的具体语法可能因 DBMS 不同而变化；未指定产品时，重点判断修改结构应使用 `ALTER TABLE`。

### 5. DELETE、TRUNCATE、DROP

| 命令 | 操作对象 | 可用 WHERE | 保留表结构 | 分类 |
|---|---|---:|---:|---|
| `DELETE` | 数据行 | 是 | 是 | DML |
| `TRUNCATE` | 全部数据行 | 否 | 是 | 通常归 DDL |
| `DROP` | 整张表对象 | 否 | 否 | DDL |

示例：

```sql
DELETE FROM Product WHERE price < 10;
TRUNCATE TABLE Product;
DROP TABLE Product;
```

## 易错点

- 把操作数据的 DML 与操作结构的 DDL 混淆。
- 认为外键自动唯一或自动非空。
- 用 `UNIQUE`约束 1:N 联系中的外键，错误地禁止多个子表元组引用同一父表元组。
- 把 `DEFAULT`当成 `NOT NULL`。
- 认为组合主键是多个主键。
- 混淆 `DROP`与 `TRUNCATE`：前者删除表，后者清空数据但保留表。
- 尝试给 `TRUNCATE`添加 `WHERE`。
- 理由正确但选项字母不对应；作答后应根据理由反查选项原文。
- 忽略 SQL 方言差异，机械套用某个 DBMS 修改列或约束的专有语法。

口诀：

> 建结构 CREATE，改结构 ALTER，删结构 DROP；删部分行 DELETE，清空留表 TRUNCATE；主键唯一非空，外键只管引用合法。

## 真题考法

- 判断 `CREATE/ALTER/DROP`所属语言类别。
- 补全 `CREATE TABLE`中的主键、外键、非空、唯一和检查约束。
- 根据业务规则选择组合主键。
- 区分 `NOT NULL`、`FOREIGN KEY`、`UNIQUE`的职责。
- 使用 `ALTER TABLE`增加列或约束。
- 比较 `DELETE`、`TRUNCATE`和 `DROP`的效果。
- 根据多条业务约束选择完整建表语句。

当前未核验到两道题面、场次和题号均完整可追溯的软件设计师考生回忆版题；已用仿真题完成命题模型校准，状态保持 🟦。

## 答题小结

- 基础题组：4/5。
- 针对性补强：2/2。
- 仿真校准：2/2。
- 新增错题：#错51。
- 个人记忆点：无。

## 我的笔记

- 暂无。

## 相关错题

- [#错51：选项与正确理由不对应](../14-错题本/错51-SQL-DDL-选项与理由不对应.md)
