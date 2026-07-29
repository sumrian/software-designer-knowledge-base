# PV 经典问题：读者—写者

> 模块：02 操作系统 ｜ 状态：🟦
>
> 读者优先、写者优先与公平方案均已完成自适应训练和针对性补强；尚缺至少 2 道题源明确的软件设计师考生回忆版真题校准。

## 定义

读者只读取共享数据，写者会修改共享数据。多个读者可以并发读取；写者访问时，不能同时存在其他读者或写者。

## 一、读者优先基础模型

```text
int readcount = 0;
semaphore mutex = 1;  // 保护 readcount
semaphore rw = 1;     // 协调读者群体与写者
```

### 读者

```text
P(mutex)
readcount++
if readcount == 1:
    P(rw)
V(mutex)

读取数据

P(mutex)
readcount--
if readcount == 0:
    V(rw)
V(mutex)
```

### 写者

```text
P(rw)
写入数据
V(rw)
```

所有并发读者可视为一个整体：第一个读者执行 `P(rw)`，最后一个读者执行 `V(rw)`。新读者可以直接加入现有读者群体，因此读者连续到达时，写者可能饥饿。

## 二、写者优先方案

```text
int readcount = 0;
int writecount = 0;

semaphore rmutex = 1;  // 保护 readcount
semaphore wmutex = 1;  // 保护 writecount
semaphore readTry = 1; // 新读者入口
semaphore rw = 1;      // 共享数据访问权
```

### 读者

```text
P(readTry)
P(rmutex)
readcount++
if readcount == 1:
    P(rw)
V(rmutex)
V(readTry)

读取数据

P(rmutex)
readcount--
if readcount == 0:
    V(rw)
V(rmutex)
```

### 写者

```text
P(wmutex)
writecount++
if writecount == 1:
    P(readTry)
V(wmutex)

P(rw)
写入数据
V(rw)

P(wmutex)
writecount--
if writecount == 0:
    V(readTry)
V(wmutex)
```

第一个写者关闭新读者入口 `readTry`，最后一个写者重新开放入口。已有读者仍可正常退出，但后来读者不能插队。若写者持续到达，读者可能饥饿。

## 三、公平排队方案

```text
int readcount = 0;

semaphore serviceQueue = 1; // 读者和写者统一排队
semaphore rmutex = 1;       // 保护 readcount
semaphore rw = 1;           // 共享数据访问权
```

### 读者

```text
P(serviceQueue)
P(rmutex)
readcount++
if readcount == 1:
    P(rw)
V(serviceQueue)
V(rmutex)

读取数据

P(rmutex)
readcount--
if readcount == 0:
    V(rw)
V(rmutex)
```

### 写者

```text
P(serviceQueue)
P(rw)
V(serviceQueue)

写入数据

V(rw)
```

`serviceQueue` 是统一排队闸机，`rw` 才是真正的共享数据访问锁。进程必须取得 `rw` 后才能释放 `serviceQueue`，否则后来者可能越过统一闸机加入 `rw` 的竞争。严格公平还依赖等待队列采用公平或 FIFO 唤醒策略。

## 四、三种策略对比

| 策略 | 入口规则 | 可能饥饿者 |
| --- | --- | --- |
| 读者优先 | 新读者可加入已有读者群体 | 写者 |
| 写者优先 | 有写者等待时关闭新读者入口 | 读者 |
| 公平排队 | 读者和写者统一经过 `serviceQueue` | 公平/FIFO 前提下通常无 |

## 易错点

- `mutex/rmutex` 只保护 `readcount`；`wmutex` 只保护 `writecount`；`rw` 保护真正的共享数据访问。
- 只有第一个读者执行 `P(rw)`，只有最后一个读者执行 `V(rw)`。
- 后续读者能并发，不是因为 `rw` 空闲，而是因为 `readcount>1` 时不再执行 `P(rw)`。
- 第一个读者阻塞在公平方案的 `P(rw)` 时，后面的 V 尚未执行，因此仍持有 `serviceQueue` 和 `rmutex`。
- 读者开始读取后已释放 `serviceQueue` 和 `rmutex`，但 `rw` 仍由整个读者群体持有，直到最后一个读者退出。
- `serviceQueue` 管排队而不保护共享数据；进程释放闸机后仍需依靠 `rw` 保证读写互斥。
- 信号量值相同不代表进程状态相同，必须结合刚执行的是 P 还是 V，以及进程原来的状态判断。
- 被 V 唤醒的进程，其此前阻塞的 P 已经完成，不需要醒来后重新执行 P。
- V 唤醒后，进程通常先由阻塞态转为就绪态，获得 CPU 后才进入运行态。
- `readcount` 包含所有当前读者，包括即将退出的读者；推演前必须明确进程身份。

## 真题校准

目前能确认公开题库列有软件设计师考生或学员回忆版试卷，也能找到写者优先与公平方案的教学资料，但未找到同时具备明确回忆版声明、完整题面和题号的对应题目。因此本考点保持待真题校准，不编造年份或题号。

## 我的笔记

- [#N73] 判据 ｜ P 操作先减一；若结果 `S<0`，执行 P 的进程阻塞。V 操作先加一；若结果 `S<=0`，从等待队列唤醒一个进程，并把本次许可直接交给它；被唤醒的进程不需要重新执行 P。不能只根据 S 的当前值判断进程能否运行，还要结合刚执行的是 P 还是 V，以及进程原本的状态。
- [#N74] 口诀 ｜ 第一个读者关门，最后一个读者开门；每个读者都改计数，只有首尾操作 `rw`。
- [#N75] 职责 ｜ `mutex` 保护 `readcount` 的修改与判断；`rw` 协调读者群体与写者访问共享数据。
- [#N76] 易错 ｜ 非最后读者退出只减少 `readcount`，不执行 `V(rw)`，因此不会改变 `rw`。
- [#N77] 状态 ｜ V 唤醒后，进程通常由阻塞态转为就绪态；获得 CPU 调度后才进入运行态。
- [#N78] 风险 ｜ 读者优先允许新读者不断加入现有读者群体，使 `readcount` 可能长期无法归零，等待的写者可能发生饥饿。
- [#N79] 区别 ｜ 读者优先可能饿死写者；写者优先可能饿死读者；公平方案让双方经过同一个排队闸机。
- [#N80] 职责 ｜ `serviceQueue` 管排队，`rw` 管共享数据访问，`rmutex` 管 `readcount`。
- [#N81] 判据 ｜ 公平方案中，进程必须取得 `rw` 后才能释放 `serviceQueue`，否则后来者可能越过统一闸机加入 `rw` 竞争。
- [#N82] 状态 ｜ 第一个读者阻塞在 `P(rw)` 时，尚未执行后面的 V，因此仍持有 `serviceQueue` 和 `rmutex`。
- [#N83] 易错 ｜ 读者开始读取后只释放 `serviceQueue` 和 `rmutex`；`rw` 仍由整个读者群体持有，直到最后一个读者退出。
- [#N84] 并发 ｜ 后续读者能并发，不是因为 `rw` 空闲，而是因为 `readcount>1` 时不再执行 `P(rw)`。
- [#N85] 口诀 ｜ 排队锁管先后，资源锁管读写；首读拿资源，末读还资源。

### #N73 对比例

```text
P：S 从 0 变为 -1
→ 执行 P 的当前进程阻塞

V：S 从 -2 变为 -1
→ 唤醒一个等待进程，许可直接交给它
→ S=-1 表示仍有一个进程等待
```

两种情况最终都可能得到 `S=-1`，但前者是当前进程新近阻塞，后者是已有等待者被唤醒，因此不能脱离操作过程只看数值。

## 当前待巩固

- 换题复测 `serviceQueue`、`rmutex`、`rw` 的持有与释放时点。
- 使用两道合格的软件设计师考生回忆版真题完成校准。
