# whirlwind

异步线程安全 [HashMap](https://doc.rust-lang.org/beta/std/collections/struct.HashMap.html) 实现

上游活跃度: 最后一次发版于 2025 年，有几个 pr 至今没合。

- 链接: <https://github.com/sb-child/whirlwind/tree/sbchild-patch>
- 上游链接: <https://github.com/willothy/whirlwind>
- 编程语言: ![rs](https://img.shields.io/badge/-rust-black?logo=Rust)
- 在 Cargo.toml 安装: `whirlwind = { git = "https://github.com/sb-child/whirlwind", rev = "96b5370e2dc7b4cfca3f09008bcdba2972966339" }`

---

## 简介

面向高并发异步 Rust 的线程安全集合（ShardMap / ShardSet）：分片（sharding）布局、每分片独立锁，基于 hashbrown 的 raw 表实现，写操作以异步方式等待分片锁、读路径开销小。上游来自 fortress-build（willothy，560+ stars）。

## 相对于上游的改动

- 新增迭代器 API：`iter` / `iter_mut` / `keys` / `values` / `values_mut`（上游缺失，相关 PR 一直没合），并补了对应测试；为支持迭代引入 futures 依赖
- 升级 edition 2021 -> 2024
- 升级依赖 hashbrown 0.15 -> 0.17
- 升级依赖 tokio 1.41 -> 1.53
- 升级依赖 crossbeam-utils 0.8.20 -> 0.8.23
