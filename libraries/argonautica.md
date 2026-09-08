# argonautica

密码哈希函数 [Argon2](https://tools.ietf.org/html/draft-irtf-cfrg-argon2-03) 的 rust binding

上游活跃度: 最后一次提交于 2020 年 3 月，已停止维护。本 fork 实际派生自中间层 fork mysilkway/argonautica（该层也已于 2023 年停更）。

- 链接: <https://github.com/sb-child/argonautica/tree/fixed>
- 上游链接: <https://github.com/bcmyers/argonautica>
- 编程语言: ![rs](https://img.shields.io/badge/-rust-black?logo=Rust)
- 在 Cargo.toml 安装: `argonautica = { git = "https://github.com/sb-child/argonautica", rev = "085f97c6ec9873c3a9b2805a41e904eba2098684" }`

---

## 简介

Argon2 密码哈希的 Rust 封装：构建时自动编译内置的 C 实现（vendored [phc-winner-argon2](https://github.com/P-H-C/phc-winner-argon2)），无需系统库依赖。支持 argon2d / argon2i / argon2id 三种变体与 v1.0/v1.3 版本，可调内存大小、迭代次数与并行度，提供 Hasher / Verifier 高层 API 和多种输入类型（密码、盐、密钥、附加数据）。

## 相对于上游的改动

注：19 个独有提交里有 7 个继承自中间 fork mysilkway/argonautica（bindgen 升级、futures 0.3、macOS Catalina 编译 workaround 等），其余为我的维护工作。

- 把内置 argon2 C 实现（submodule 指针）更新到上游最新
- 将 decode.rs 的 nom 旧宏解析代码迁移到新版函数式 API，修复与新版 rustc 的编译错误
- 持续升级依赖（2025-05、2026-09 仍有 update deps），保证 crate 在现代工具链下可构建
