# 序列图 opt 可选执行组合片段

## 定义
UML 序列图中的 opt（Optional）组合片段表示守卫条件成立时执行片段中的交互，条件不成立时跳过整个片段；只有一个显式交互操作数区域。

## 要点
- opt 常对应 Java 无 else 的 if，守卫通常用方括号 [condition] 表示。
- 守卫为假时，片段内所有消息均跳过，但不会因此结束整个交互；片段外的公共消息继续执行。
- 一个 opt 可以包含多条有顺序的消息，例如生成发票后发送邮件。
- 两个独立条件可以使用两个独立 opt，允许两者都执行、都跳过或只执行一个。
- 只有条件性消息放入 opt，公共前置和后续消息放在框外且保持原顺序。
- opt 不是 loop，也不要求 else；与 alt 多分支选择不同。

## Java/UML 案例
```java
void completeOrder(Order order) {
    orderService.saveOrder(order);
    if (order.isNeedInvoice()) {
        invoiceService.generateInvoice(order);
        emailService.sendInvoice(order);
    }
    if (order.isVip()) {
        pointsService.addBonusPoints(order);
    }
    notificationService.notifySuccess(order);
}
```
建模：saveOrder() 在 opt 框外上方；第一个 opt [needInvoice] 内先 generateInvoice() 再 sendInvoice()；第二个 opt [isVip] 内 addBonusPoints()；notifySuccess() 在两个 opt 外下方。两个条件相互独立，不能用 [needInvoice || isVip] 包裹所有可选消息。

## 易错点
- 认为 opt 条件为假时只跳过第一条消息，或结束整个序列图。
- 将 opt 当作循环，或要求必须包含 else 分支。
- 将无条件保存、通知等公共消息误放入 opt。
- 将两个独立可选条件错误合并成互斥 alt 分支。
- 将多个条件合并为逻辑与/或后错误执行全部消息。

## 真题考法
辨识 opt 语义和单操作数结构；判断守卫为假时执行路径；从 Java if 提取可选消息；校验多个独立 opt 与公共消息位置。本轮未取得可核验年份、场次及完整题干的独立考生回忆版真题；合规真题校准 0/2，原创题不计入。

## 本轮验收（2026-10-09）
- 讲解与问答完成，用户确认无疑问。
- 基础自适应 5/5 全对，答案 B、A、C、C、C；即时理解 2/2、变式应用 2/2、综合迁移 1/1。
- 无知识性错误，无新增活动错题，无需补强。
- 状态：🟦 教学完成，待真题校准。

## 我的笔记
- 用户确认没有额外个人笔记。
