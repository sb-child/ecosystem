# mytm

基于 Tokio 的轻量异步任务管理器，可以用在你的下一个项目里。

- 链接: <https://github.com/sb-child/mytm>
- 编程语言: ![rs](https://img.shields.io/badge/-rust-black?logo=Rust)
- 许可证: MIT
- 安装（未发布到 crates.io）: `cargo add mytm --git https://github.com/sb-child/mytm`

---

## 简介

把「启动一个后台任务 → 上报进度 → 等它结束 → 中途取消」这套东西抽象成 `TaskManager` / `TaskHandle`。进度、返回值、错误类型都由泛型固定，取回结果不需要手工 downcast；同时保留一套 `TaskId` + `TaskState` 的无类型 API 给动态场景用。

`TaskManager` 可以低成本克隆，在任务之间共享后继续派生子任务——[dz-print](./dz-print.md) 的 `frontend` worker 线程就是交给它的 `TaskManager` 托管的。

## 特性

- **异步 / 阻塞任务**：`spawn_typed` 跑异步任务，`spawn_blocking_typed` 跑阻塞任务
- **类型安全**：进度、返回值、错误类型由泛型固定
- **进度上报**：任务通过 `ProgressUpdater` 更新进度，首次更新把任务从 `Pending` 推进到 `Running`
- **协作式取消**：取消信号走 `CancellationToken`，超时未退出会 `abort`（仅异步任务）
- **不崩溃**：任务内的 panic 会被捕获并转成 `Panicked` 状态
- **事件驱动等待**：`wait_terminal` / `wait_for` 由通知唤醒，不轮询
- **自动清理**：终态保留 60 秒后自动删除，避免无限增长

## 任务闭包

每个任务都拿到三个参数：

```rust
|tm: TaskManager, progress: ProgressUpdater<Status>, ct: CancellationToken| async move {
    // ...
    Ok::<Ret, Err>(value)
}
```

`TaskManager` 用来继续派生子任务，`ProgressUpdater` 可 `Clone` 后传给子任务。`spawn_blocking_*` 的闭包是同步的，直接返回 `Result`，参数相同。

## API 一览

### 启动任务

| 方法 | 说明 | 返回 |
| --- | --- | --- |
| `TaskManager::spawn_typed(f)` | 启动 Tokio 异步任务 | `TaskHandle<Status, Ret, Err>` |
| `TaskManager::spawn_blocking_typed(f)` | 在线程池启动阻塞任务 | `TaskHandle<Status, Ret, Err>` |
| `TaskManager::spawn(f)` | 启动异步任务 | `TaskId` |
| `TaskManager::spawn_blocking(f)` | 启动阻塞任务 | `TaskId` |

### 查询与等待

| 方法 | 说明 |
| --- | --- |
| `handle.status()` | 获取当前类型化状态 |
| `tm.get_status(id)` | 获取无类型状态 |
| `handle.wait_terminal()` / `tm.wait_terminal(id)` | 等待进入终态 |
| `handle.wait_for(pred)` / `tm.wait_for(id, pred)` | 等待状态满足谓词；只在状态切换时检查，任务进入终态时兜底返回 |

### 取消与关闭

| 方法 | 说明 |
| --- | --- |
| `handle.cancel()` / `tm.cancel_task(id)` | 立即请求取消 |
| `handle.cancel_at(ct)` / `tm.cancel_task_at(id, ct)` | 当另一个 token 触发时取消 |
| `tm.close()` | 拒绝新任务并取消所有运行中的任务 |
| `tm.is_closed()` | 是否已关闭 |

## 状态机

```
Pending ──progress.update──▶ Running ──Ok ──▶ Completed
   │                           │
   │                           ├──Err──────▶ Failed
   │                           └──panic────▶ Panicked
   │
   └──────── cancel_task ─────▶ Cancelling ──▶ Cancelled
```

- 首次 `progress.update(x)` 把任务从 `Pending` 切到 `Running(x)`；后续更新只替换进度值，**不会**唤醒 `wait_for` 的等待者
- 终态是粘性的：进入终态后再调用 `cancel` 不会改变结果
- 取消期间 `ProgressUpdater::update` 会被忽略，不会把任务从 `Cancelling` 拉回 `Running`
- 类型不匹配或任务已被清理时，类型化状态为 `Invalid`

## 取消语义

`cancel` 不会立刻置为 `Cancelled`：

1. 状态置为 `Cancelling`，并触发闭包收到的 `CancellationToken`
2. 最多等 5 秒让任务自行响应 token 退出
3. 超时后对异步任务 `abort()`；阻塞任务不能 abort，会继续等闭包返回
4. 退出后状态置为 `Cancelled`，60 秒后清理

所以任务里要检查 token，循环里用 `select!` 或 `while !ct.is_cancelled()` 都行。

## 生命周期

- 终态（`Completed` / `Failed` / `Cancelled` / `Panicked`）保留 60 秒后自动删除，之后 `get_status` 返回 `None`，类型化句柄返回 `Invalid`
- `close()` 之后新任务直接得到 `Cancelled`
- `TaskManager` 被 drop **不会**自动取消任务，要显式 `close()`

## 模块组织

| 文件 | 内容 |
| --- | --- |
| `src/lib.rs` | `TaskManager`、`TaskHandle`、状态类型、取消与等待逻辑 |
| `src/util.rs` | `use_id!` 宏与 serde 重导出 |
| `src/tests.rs` | 行为测试 |

`TaskId` 由 `use_id!` 宏生成，支持 `Debug` / `Clone` / `Copy` / `Hash` / `PartialEq` / `Eq`，实现了 serde 序列化，也可以和 `u64` 互相转换。
