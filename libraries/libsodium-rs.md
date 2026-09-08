# libsodium-rs

密码学套件 [libsodium](https://github.com/jedisct1/libsodium) 的 rust binding

上游活跃度: 活跃维护中。

- 链接: <https://github.com/sb-child/libsodium-rs/tree/sbchild-patch2>
- 上游链接: <https://github.com/jedisct1/libsodium-rs>
- 编程语言: ![rs](https://img.shields.io/badge/-rust-black?logo=Rust)
- 在 Cargo.toml 安装: `libsodium-rs = { git = "https://github.com/sb-child/libsodium-rs", rev = "9f7592aa1710b3e15ab3f4175a0684462967e5fa" }`

---

## 简介

libsodium 的现代惯用 Rust 封装（作者就是 libsodium 作者 jedisct1，底层用活跃维护的 libsodium-sys-stable）：覆盖 AEAD（ChaCha20-Poly1305 / XChaCha20 / AEGIS 等）、box/secretbox、签名、通用哈希、KDF 等领域的安全 API。

## 相对于上游的改动

- 默认 feature 从空改为 `fetch-latest`，并删除 build-dependencies 里的 pkg-config：默认直接构建捆绑的最新版 libsodium 源码，不再依赖系统库
- 升级 edition 2021 -> 2024
- 升级依赖 thiserror 1 -> 2
- 升级依赖 zeroize 1.8 -> 1.9
