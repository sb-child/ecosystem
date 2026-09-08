# opus-rs

音频编解码器 [libopus](https://opus-codec.org/) 的 rust binding

上游活跃度: 活跃维护中。

- 链接: <https://github.com/sb-child/opus-rs/tree/sbchild-patch-1>
- 上游链接: <https://github.com/SpaceManiac/opus-rs>
- 编程语言: ![rs](https://img.shields.io/badge/-rust-black?logo=Rust)
- 在 Cargo.toml 安装: `opus = { git = "https://github.com/sb-child/opus-rs", rev = "13a954dc58483d93e04a7ecf2a56808e591d035e" }`

---

## 简介

libopus 的安全 Rust 绑定：把 PCM 音频帧编码为 opus 包、解码回 PCM，可调码率、复杂度、VBR、声道等参数，适合实时语音场景。底层 sys crate 是 opusic-sys（原 audiopus-sys 的继任）。

## 相对于上游的改动

- 升级依赖 opusic-sys 0.7.3 -> 0.7.5（上游锁定 0.7.3）
- 升级 edition 2015 -> 2024
