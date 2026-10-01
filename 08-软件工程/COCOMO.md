# COCOMO

> 状态：🟦 待真题校准

## 定义
COCOMO（Constructive Cost Model）是经典的软件成本与工作量估算模型，常以 KLOC（千行代码）作为软件规模输入之一。

## 要点
- 基本工作量形式可表示为 E = a × (KLOC)^b，E 通常以人月表示。
- 项目模式：Organic（有机型）、Semi-detached（半独立型）、Embedded（嵌入型）。一般可按约束和复杂度理解为 Organic < Semi-detached < Embedded。
- Organic：规模较小、团队和领域熟悉、约束较少。
- Embedded：实时性、软硬件、可靠性等约束严格。
- Basic：基础规模估算。
- Intermediate：进一步考虑成本驱动因素，并可通过 EAF 调整工作量。
- Detailed：在 Intermediate 基础上进一步细化到生命周期阶段。

## 易错点
- KLOC 表示千行代码。
- 人月是工作量，不等于工期。
- Embedded 的关键是严格约束，不是仅看是否运行在嵌入式芯片。
- 项目模式 Organic/Semi-detached/Embedded 与模型层次 Basic/Intermediate/Detailed 是两套分类。

## 真题考法
常考项目模式辨析、KLOC 含义、Intermediate 的成本驱动因素以及工作量单位。

## 本轮结果
- 基础题组 5/5；外部校准 2/2；无知识性错误。
- 缺合规回忆版真题，保持 🟦 待真题校准。

## 我的笔记
- 无新增。
