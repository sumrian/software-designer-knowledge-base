# UML 类图：成员可见性

## 定义
UML 类图中的成员可见性描述属性和操作可被哪些元素访问。属性写作 `可见性 名称: 类型`，操作写作 `可见性 名称(参数): 返回类型`。

## 核心要点
| UML 标记 | 含义 | Java 对应 |
|---|---|---|
| `+` | public 公有 | `public` |
| `-` | private 私有 | `private` |
| `#` | protected 受保护 | `protected` |
| `~` | package 包可见 | 省略访问修饰符 |

示例：
```java
package service;
public class PaymentService {
    private String apiKey;
    protected int retryCount;
    boolean enabled;
    public void pay() {}
}
```
对应 UML：`- apiKey: String`、`# retryCount: int`、`~ enabled: boolean`、`+ pay(): void`。

## 易错点
- `#` 是 protected，`-` 才是 private；`~` 是包可见，Java 不写访问修饰符，并非成员访问修饰符 `default`。
- Java 跨包子类可通过 `this.owner` 或子类类型的接收者访问继承的 protected 实例成员，但不可直接通过父类类型接收者访问；与同包访问区别。
- 可见性符号与下划线（静态）、斜体（抽象）是不同维度。

## 真题考法
- 识别 UML 类图成员的可见性。
- Java 成员声明与 UML 标记互相转换。
- 结合包与继承关系判断访问合法性。
- 截至 2026-10-08，本考点尚无已核验到年份/场次的合规考生回忆版真题；不得将原创验收题当作真题。

## 验收记录
- 2026-10-08：基础自适应 5/5 全对（即时理解 2/2、变式 2/2、综合 1/1）。
- 答案依次为 B、C、C、A、B；第 3 题用户确认选择 C，先前误记 A 已纠正，不产生错题 #126。
- 无知识性错误；真题校准 0/2，状态 🟦 待真题校准。

## 我的笔记
- 2026-10-08：用户确认没有额外个人记忆点。
