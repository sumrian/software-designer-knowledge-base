# BCNF：决定因素必须是候选码

> 模块：04 数据库系统 ｜ 状态：🟦

## 定义
BCNF 要求：对每个非平凡函数依赖 X → Y，决定因素 X 必须是超码（因而至少包含某个候选码）。

## 要点
- 判定前先求候选码，再逐条检查函数依赖左部是否为超码。
- 例：选课(Student, Course, Instructor)，依赖 (Student, Course) → Instructor、Instructor → Course；候选码为 (Student, Course) 和 (Student, Instructor)，但 Instructor 不是超码却决定 Course，因此不满足 BCNF。
- 按 Instructor → Course 分解为 教师课程(Instructor, Course) 与 学生教师(Student, Instructor)，两个子关系的决定因素均为候选码。
- BCNF 比 3NF 严格：3NF 允许决定因素不是超码但右部为主属性；BCNF 不允许这一例外。
- BCNF 一定满足 3NF，但 3NF 不一定满足 BCNF。

## 易错点
- 不能因为关系有一个候选码，就停止检查其他依赖；必须逐条检查所有非平凡依赖。
- “决定因素”指函数依赖左部 X；右部属性属于候选码，并不能让 X 自动成为超码。
- 外键或候选码的一部分不等于超码。
- 分解应围绕违反 BCNF 的 X → Y，拆出包含 X 与 Y 的关系，并保留关联属性以维持连接语义。

## 真题考法
- 给出候选码和函数依赖集，判断是否满足 BCNF。
- 给出满足 3NF 但不满足 BCNF 的关系，找出左部不是超码的依赖并写出分解。
- 本轮未核验到两道明确标注年份、场次且完整考查 BCNF 的软件设计师回忆版真题，使用两道仿真题完成校准，状态暂保持 🟦。

## 我的笔记
- 无。

## 相关错题
- [#错63](../14-错题本/README.md)
- [#错64](../14-错题本/README.md)
