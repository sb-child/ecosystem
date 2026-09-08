# reed-solomon-rs

纠错算法 [Reed-Solomon](https://en.wikipedia.org/wiki/Reed%E2%80%93Solomon_error_correction) 的编解码器实现

上游活跃度: 最后一次更新于 2018 年，已停止维护。

- 链接: <https://github.com/sb-child/reed-solomon-rs/tree/patch-1>
- 上游链接: <https://github.com/mersinvald/reed-solomon-rs>
- 编程语言: ![rs](https://img.shields.io/badge/-rust-black?logo=Rust)
- 在 Cargo.toml 安装: `reed-solomon = { git = "https://github.com/sb-child/reed-solomon-rs", rev = "015e46e2ea4291846a367927dbfba229612f2202" }`

---

## 简介

GF(2^8) 上的 Reed-Solomon 纠错码编解码器：`Encoder`/`Decoder` 以运行时参数指定 ECC 字节数，解码支持纠错并可选擦除（erase）位置提示，no_std、零依赖，附带宽测试示例。

## 相对于上游的改动

- 新增编译期定长 API：`FixedEncoder<const ECCLEN>` / `FixedDecoder<const ECCLEN>`（const 泛型），ECC 长度由类型参数决定、无需运行时开销
- 升级 edition 到 2024，修复弃用警告
- 清理早期 rustc 的 plugin/clippy 残留（`#![feature(plugin)]` 等），示例换用 serde crate，修复与新版 rustc 的兼容错误
- 更新文档
