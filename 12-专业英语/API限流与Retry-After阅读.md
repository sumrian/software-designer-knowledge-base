# 专业英语：API 限流与 Retry-After 阅读

## 原创模拟短文

“Many APIs enforce rate limits to protect shared services. When a client exceeds the allowed request rate, the server may return HTTP 429 and include a Retry-After header. A well-behaved client should wait as instructed and use exponential backoff for repeated failures, rather than immediately retrying every request.”

## 关键词
- **rate limit**：请求速率限制。
- **exceed the allowed request rate**：超过允许的请求速率。
- **HTTP 429**：Too Many Requests，服务器拒绝当前请求通常与请求过于频繁有关。
- **Retry-After header**：提示客户端重试前等待的时间或重试时点。
- **exponential backoff**：指数退避；连续失败时逐步增加等待时间，避免立即反复重试。
- **as instructed**：按照指示。

## 阅读判据
- 原文的核心建议是：收到限流响应后遵循 Retry-After 指示，并在反复失败时采用指数退避。
- 不要把 HTTP 429 与资源永久删除混淆；不要把“有速率限制”理解成“请求永远不会失败”。
- 做专业英语题时，先定位原文直接支持的陈述，不因常识扩写出原文没有保证的结论。

## 原创75题记录（2026-10-20）
- 第41题用户选择 B，正确 B，1/1。原文支持 Retry-After 告知客户端重试前等待多久。
- 本题为原创模拟；单题正确不代表专业英语模块完成正式教学或真题校准。
