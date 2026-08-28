# SQL：DCL

> 模块：04 数据库系统 ｜ 状态：🟦

## 定义
DCL（Data Control Language）用于管理数据库对象的访问权限，核心命令是 `GRANT` 和 `REVOKE`。

## 要点
- `GRANT ... ON 对象 TO 用户/角色` 授予权限。
- `REVOKE ... ON 对象 FROM 用户/角色` 收回权限。
- 常见对象权限包括 `SELECT`、`INSERT`、`UPDATE`、`DELETE`，彼此独立。
- `WITH GRANT OPTION` 允许被授权者继续转授该权限。

## 易错点
- `GRANT` 是授予，`REVOKE` 是收回；`ON` 后写对象，`TO/FROM` 后写用户或角色。
- `UPDATE` 不自动包含 `SELECT`；`WITH GRANT OPTION` 是转授权限，不是数据操作权限。

## 真题考法
- 判断授权/收权语句作用，补写 `ON`、`TO`、`FROM`，分析连续授权与权限独立性。

## 我的笔记
- 无。

## 相关错题
- 无。
