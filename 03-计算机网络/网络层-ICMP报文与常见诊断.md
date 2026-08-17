# 网络层：ICMP 报文与常见诊断

## 定义

ICMP（Internet Control Message Protocol）与网络层密切相关，用于差错报告、状态反馈和网络诊断。ICMP 报文封装在 IP 数据报中；IPv4 首部中 `Protocol = 1` 表示 ICMP。

## 高频报文与用途

- **Echo Request / Echo Reply**：`ping` 的基础，用于测试目标是否可达并观察往返时延。
- **Destination Unreachable**：报告目的网络、主机或相关目标不可达。
- **Time Exceeded**：当 IPv4 数据报 TTL 在路由器处减到 0 时，路由器丢弃数据报，并可向源主机返回 ICMP 超时报文。

## ping 与 traceroute / tracert

### ping

典型利用 ICMP Echo Request / Echo Reply 测试目标连通性。

注意：`ping` 成功只说明 ICMP 连通性基本正常，不代表某个 TCP/UDP 应用端口、应用服务或防火墙策略一定正常。

### traceroute / tracert

经典原理是逐步增大探测报文的 TTL：`1、2、3...`。不同跳的路由器依次因 TTL 归零而返回 ICMP Time Exceeded，从而暴露沿途路径。

可记：**Ping 看终点，Trace 看路径；TTL 归零，ICMP 报超时。**

## 易错点

- ICMP 不是 TCP/UDP 那样的传输层协议，不使用端口号区分应用进程。
- ICMP 封装在 IP 中，但仍属于网络层相关控制协议。
- TTL 归零后的核心动作是丢弃原 IP 数据报；随后可向源主机返回 ICMP Time Exceeded。
- `ping` 通不等于 Web、SSH 等应用服务一定可用。

## 常见考法

1. 判断 `ping` 使用的协议或报文类型。
2. 判断 TTL 归零时的处理及对应 ICMP 报文。
3. 判断 `traceroute / tracert` 如何利用 TTL 和 ICMP 探测路径。
4. 区分 ICMP 与 TCP/UDP 的层次和作用。
5. 判断 `ping` 成功与应用服务可用之间的关系。

## 答题与真题校准

- 自适应题组：5/5。
- 真题校准 1：2008 年上半年软件设计师上午综合知识第 7 题，考查禁止其他主机通过 `ping` 测试服务器连通性与 ICMP 的关系，作答正确。
- 真题校准 2：2024 年下半年软件设计师综合知识回忆版第 1 题，考查测试网络连通性使用 `ping` 命令，作答正确。
- 两道可追溯真题校准均通过，本子点状态提升为 🟨“初步掌握，待延迟复测”。
- 本子点无新增错题。

## 我的笔记

本次无额外个人记忆点。
