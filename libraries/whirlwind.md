# whirlwind

异步线程安全 [HashMap](https://doc.rust-lang.org/beta/std/collections/struct.HashMap.html) 实现

上游活跃度: 最后一次发版于 2024 年 11 月（v0.1.1），最后提交于 2025 年 1 月，几个 PR 至今没合。

- 链接: <https://github.com/sb-child/whirlwind/tree/sbchild-patch>
- 上游链接: <https://github.com/willothy/whirlwind>
- 编程语言: ![rs](https://img.shields.io/badge/-rust-black?logo=Rust)
- 在 Cargo.toml 安装: `whirlwind = { git = "https://github.com/sb-child/whirlwind", rev = "dec605442523e0fadc30fbe6490aea9681ab8f1c" }`

---

## 简介

面向高并发异步 Rust 的线程安全集合（ShardMap / ShardSet）：分片（sharding）布局、每分片独立锁，基于 hashbrown 的 raw 表实现，写操作以异步方式等待分片锁、读路径开销小。上游来自 fortress-build（willothy，560+ stars）。

## 相对于上游的改动

- 新增迭代器 API：`iter` / `iter_mut` / `keys` / `values` / `values_mut`（上游缺失，相关 PR 一直没合），并补了对应测试；为支持迭代引入 futures 依赖
- `ShardMap` / `ShardSet` 实现 `FromIterator`，支持 `.collect()` 直接构造
- `ShardMap` 新增 `remove_entry`；`get` / `get_mut` 的 key 生命周期不再与返回的引用绑定；`is_empty` 改为遇到非空分片即返回
- `ShardSet` API 扩充：`get`（返回 `SetRef`）/ `replace` / `take` / `iter`，`insert` 改为返回是否为新值，实现 `Clone`，构造函数命名与 `ShardMap` 对齐（`new_with_*` -> `with_*`）
- 性能：读写锁增加 `try_read` / `try_write` 快路径，避免无竞争时进入公平等待队列；默认分片数从 CPU 核数 x4 提升到 x8
- 健壮性：分片数校验从 `debug_assert` 改为 `assert`（release 下同样生效），容量计算改用饱和运算
- 升级 edition 2021 -> 2024
- 升级依赖 hashbrown 0.15.1 -> 0.17.1
- 升级依赖 tokio 1.41.0 -> 1.54.1
- 升级依赖 crossbeam-utils 0.8.20 -> 0.8.23
