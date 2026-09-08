# proc-macro-error-2

在过程宏中向用户报告错误的解决方案

上游活跃度: 最后一次提交于 2024 年 9 月，之后仓库已被归档，停止维护。

- 链接: <https://github.com/sb-child/proc-macro-error-2/tree/patch-1>
- 上游链接: <https://github.com/GnomedDev/proc-macro-error-2>
- 编程语言: ![rs](https://img.shields.io/badge/-rust-black?logo=Rust)
- 在 Cargo.toml 安装: `proc-macro-error2 = { git = "https://github.com/sb-child/proc-macro-error-2", rev = "fa51d84e4162cc33bdb17e45a6f52fa87ad9c639" }`

---

## 简介

过程宏错误报告工具（[proc-macro-error](https://github.com/CreepySkeleton/proc-macro-error) 的社区延续版）：通过 `#[proc_macro_error]` 属性宏把宏体内的 panic / 错误转成带 span 定位的精美诊断，并提供 dummy 模块机制——宏展开失败时自动换成占位实现，避免下游产生连串的级联类型错误。

也许你可以看看别人维护的版本: https://github.com/gamma0987/proc-macro-error3

## 相对于上游的改动

- 升级依赖 syn 2 -> 3
- 升级依赖 trybuild 1.0.99 -> 1.0.121
- 升级 edition 到 2024：`extern crate proc_macro` 改 `pub extern crate`、去掉 match 中的 `ref` 绑定模式等迁移
- 移除 `rust-version = "1.61"`
