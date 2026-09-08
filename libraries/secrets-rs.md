# secrets-rs

在内存中安全存储密钥的解决方案

上游活跃度: 活跃维护中。

- 链接: <https://github.com/sb-child/secrets-rs/tree/sbchild-patch2>
- 上游链接: <https://github.com/stouset/secrets>
- 编程语言: ![rs](https://img.shields.io/badge/-rust-black?logo=Rust)
- 在 Cargo.toml 安装: `secrets = { git = "https://github.com/sb-child/secrets-rs", rev = "a06d204dc6ec0054568dd0c866b650bf0cc0552d" }`

---

## 简介

stouset 的 secrets：密码学密钥的内存安全容器。通过 mlock 锁页防止换出到磁盘、mprotect 动态收放页权限防止被越权读写、zeroize 用后清零，并提供 fork 继承防护等；类型包括 Secret / SecretBox / SecretVec / 自定义分配器。

## 相对于上游的改动

- 依赖 libsodium-sys 0.2 -> libsodium-sys-stable 1.24
- 把 use-libsodium-sys 提升为默认 feature
