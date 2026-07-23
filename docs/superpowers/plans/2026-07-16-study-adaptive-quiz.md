# Study Adaptive Quiz Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** 将 `study` 的子考点答题升级为 5 题起步的自适应题组，并建立至少 2 道已核验真题才能转 🟨 的溯源门槛。

**Architecture:** `SKILL.md` 保存答题决策规则；`13-真题与模拟/真题索引.md` 保存渐进式真题元数据与转述模板；`进度跟踪.md` 保存当前剩余题组与校准状态。

**Tech Stack:** Markdown、Ruby YAML 解析、`rg`、Python 本地链接检查。

---

### Task 1: 建立失败基线断言

**Files:**
- Test: `.agents/skills/study/SKILL.md`

- [ ] **Step 1:** 断言当前 skill 缺少「5 题起步」「2 道变式」「至少 2 道已核验真题」和「待真题校准」。
- [ ] **Step 2:** 运行 `rg` 断言并确认失败原因是规则尚未实现。

### Task 2: 更新自适应答题规则

**Files:**
- Modify: `.agents/skills/study/SKILL.md`

- [ ] **Step 1:** 将答题步骤改为 2 道即时 + 2 道变式 + 1 道综合的 5 题起步结构。
- [ ] **Step 2:** 加入 0 错进真题、1 错追加 2 题、2+ 错补讲后追加 3–5 题的分支。
- [ ] **Step 3:** 加入真题转述格式、来源优先级、禁止伪造年份/题号和不足 2 道时保持 🟦 的门槛。
- [ ] **Step 4:** 运行内容断言，预期全部命中。

### Task 3: 建立渐进式真题索引

**Files:**
- Create: `13-真题与模拟/真题索引.md`
- Modify: `13-真题与模拟/README.md`

- [ ] **Step 1:** 建立包含本地编号、年份、场次、科目、原题号、考点、来源、核验日期、转述题干、答案解析和使用记录的模板。
- [ ] **Step 2:** 在真题 README 中链接索引，说明只在核验后追加。
- [ ] **Step 3:** 检查索引模板字段和 README 链接。

### Task 4: 迁移当前学习检查点

**Files:**
- Modify: `00-学习计划/进度跟踪.md`

- [ ] **Step 1:** 保留当前 3 题全对的历史结果。
- [ ] **Step 2:** 将阶段设为答题，待完成改为 2 道变式题 + 至少 2 道已核验真题 + 个人笔记 + 沉淀。
- [ ] **Step 3:** 检查子考点仍为 🟦，未提前升级。

### Task 5: 全量验证

**Files:**
- Verify: `.agents/skills/study/SKILL.md`
- Verify: `13-真题与模拟/真题索引.md`
- Verify: `13-真题与模拟/README.md`
- Verify: `00-学习计划/进度跟踪.md`

- [ ] **Step 1:** 用 Ruby 检查 skill YAML。
- [ ] **Step 2:** 用 `rg` 检查全部行为约束和当前检查点。
- [ ] **Step 3:** 用 Python 检查 fenced code block 之外的 Markdown 本地链接。
- [ ] **Step 4:** 确认原有 8 道错题、学习日志和 3 道当前题结果未丢失。
