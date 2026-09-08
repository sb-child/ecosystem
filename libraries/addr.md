# addr

域名/邮箱地址解析与校验器实现

上游活跃度: 最后一次提交于 2022 年 8 月，此后基本停更；欠了多年的 idna feature 至今仍只是空实现。

- 链接: <https://github.com/sb-child/addr/tree/main>
- 上游链接: <https://github.com/addr-rs/addr>
- 编程语言: ![rs](https://img.shields.io/badge/-rust-black?logo=Rust)
- 在 Cargo.toml 安装: `addr = { git = "https://github.com/sb-child/addr", rev = "338e14147416bcbd42e299e4b4fdd86b83f2043f" }`

---

## 简介

解析与校验邮箱地址、域名的 Rust 库：把 `user@example.com` 这类输入解析为结构化组件（邮箱本地部分、域名），支持域名解析、格式化与 serde 序列化。上游 addr-rs/addr 还带一个可选的 psl（公共后缀列表）支持，用于把注册域与子域区分开。

## 相对于上游的改动

- 移除 net feature：其依赖的 no-std-net 已不兼容新版 rustc
- 自研 IDNA2008 完整校验（RFC 5890-5893：U-label/A-label 规则、连字符限制、Bidi 规则等；字符数据来自 IANA 注册表与 UCD，附抓取/生成工具脚本），填上了上游长期空置的 idna feature，JSON Schema 测试套件通过
- serde.rs 中 `email::Host` 的 Deserialize 补上 psl feature 的 cfg gate（修复 `--all-features` 下的编译问题）
