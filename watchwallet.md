# watchwallet

监听 EVM 钱包活动，把收款、转账和授权事件打印成小票。

- 链接: <https://github.com/sb-child/watchwallet>
- 编程语言: ![rs](https://img.shields.io/badge/-rust-black?logo=Rust)
- 许可证: MIT

---

## 简介

通过 WebSocket 订阅 EVM 链上的 `Transfer` / `Approval` 日志，并在每个新区块里扫描原生币转账；事件按被监听地址的方向渲染成 Typst 小票，提交给 [dz-print](./dz-print.md) 的 `dzserver` 打印。不配置 `dzserver` 时只渲染打印包，不提交打印。

- 配置文件: `watchwallet.toml`（示例见仓库的 `watchwallet.example.toml`），每条链一份 `[[evm]]`，地址用 `[[evm.address]]` 起名
- 监听范围：ERC-20 `Transfer` / `Approval`，以及可选的顶层原生币转账（`native_transfer` 默认开启；出块快的链可以关掉，省 RPC）
- 原生币转账按区块拉完整交易列表扫描，只覆盖顶层交易，合约内部转账暂不支持
- 查询交易回执确认执行成功，失败的交易不会通知；手续费和出块时间也会填进模板
- 模板：`evm-erc20-send` / `evm-erc20-recv` / `evm-erc20-approve` / `evm-erc20-approve-recv` / `evm-native-send` / `evm-native-recv`

## 结构

- `evm/`：每条链一个 WebSocket 订阅和事件通道；`log.rs` 解析 Transfer / Approval，`native.rs` 逐块扫描原生币转账，`cache.rs` 缓存代币元数据、账户类型和回执
- `output/evm.rs`：EVM 事件 → 模板名和变量表，金额按 decimals 格式化，地址 / 哈希按 EIP-55 混合大小写
- `template/`：Typst 模板（内嵌进二进制）和渲染器，占位符是独占字符串字面量的 `"{{key}}"`
- `printer/`：dzserver gRPC 客户端、全局打印队列和串行打印 worker；只在连接 / 等打印机阶段自动重试（最多 2 次，间隔 30 秒），进入打印阶段后不重发

## 使用注意

- 启动：`watchwallet --config watchwallet.toml`；日志走 `RUST_LOG`（默认 `info`）
- dzserver 地址写在 `[dzserver] grpc_url`；不配置时只渲染并打日志，不提交打印
- 打印队列容量 64，满了会丢弃新任务，避免打印机离线拖住监听；单个任务整体 15 分钟超时
- 每个被监听地址都要在对应链的 `[[evm.address]]` 里列出，同一地址在多条链上要分别配置
- dzserver 端需要准备模板用到的字体，示例见仓库 README
- 目前只有 EVM 后端；`template/` 和 `printer/` 与链无关，后续可以接 SVM 等

## 相关项目

- [dz-print](./dz-print.md) ![rs](https://img.shields.io/badge/-rust-black?logo=Rust) 提供 `dzserver` gRPC 打印服务
- [print-my-mail](./print-my-mail.md) ![py](https://img.shields.io/badge/-python-black?logo=Python) 同样通过 `dzserver` 打印的小票通知
