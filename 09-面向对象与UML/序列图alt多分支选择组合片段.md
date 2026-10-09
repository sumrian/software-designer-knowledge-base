# 序列图 alt 多分支选择组合片段

## 定义
UML 序列图中的 alt（Alternative）组合片段表示按守卫条件选择交互分支。矩形左上角标 alt，各交互操作数之间用水平虚线分隔，守卫条件通常写为 [condition]。

## 要点
- alt 不是顺序、并行或循环；在通常的互斥条件建模中，一次选择一个满足条件的分支。
- [else] 可表示其他守卫条件均不满足的默认分支，并非必须设置。
- alt 可以包含两个以上分支；分支本身没有从上到下的优先级。
- Java if/else-if 按顺序判断，转换为 alt 时应显式构造互斥守卫，避免条件重叠引入不确定选择。
- 所有分支结束后共同执行的消息可画在 alt 框外下方。

## Java/UML 案例
```java
void processRefund(Order order) {
    if (order.isCancelled()) refundService.refund(order);
    else if (order.isCompleted()) auditService.requestReview(order);
    else notificationService.notifyNotEligible(order);
    logService.recordResult(order);
}
```
需求允许 isCancelled 与 isCompleted 同时为 true，Java 优先执行取消分支。UML alt 应使用三个守卫：
- [isCancelled] → refund()
- [!isCancelled && isCompleted] → requestReview()
- [else] → notifyNotEligible()
公共后续消息 recordResult() 放在 alt 外下方，保留原 Java 的优先判断语义。

## 易错点
- 认为 alt 会按上下顺序依次执行或优先匹配。
- 把分支分隔虚线误认为返回消息。
- 认为 alt 最多两个分支或必须有 else。
- 将 Java else-if 的重叠条件原样放入 alt，造成多个守卫同时成立。
- 将公共后续消息只放在某一分支，误以为自动共享。
- 认为任何 if 都必须建模为 alt，而不考虑交互图目的。

## 真题考法
识别 alt 语义与图形、补全守卫条件、Java if/else-if 转换、分支互斥性与公共后续消息位置。本轮未取得可核验年份、场次及完整题干的独立考生回忆版真题；合规真题校准 0/2，原创题不计入。

## 本轮验收（2026-10-09）
- 讲解与问答完成，用户确认无疑问。
- 基础自适应 5/5 全对，答案 B、A、B、A、B；即时理解 2/2、变式应用 2/2、综合迁移 1/1。
- 无知识性错误，无新增活动错题，无需补强。
- 状态：🟦 教学完成，待真题校准。

## 我的笔记
- 用户确认没有额外个人笔记。
