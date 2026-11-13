# dz-print

德佟印立方（[Detonger](https://detonger.com) / [DothanTech](https://dothantech.com)）标签打印机的第三方 USB/蓝牙驱动。

- 链接: <https://github.com/sb-child/dz-print>
- 编程语言: ![rs](https://img.shields.io/badge/-rust-black?logo=Rust)
- 许可证: MPL-2.0

---

## 简介

官方 SDK 不开源，协议全部来自逆向、抓包和真机测试，因此仓库本身也是一份持续修补的协议笔记：`protocol.md`（协议和命令）、`print-status.md`（状态码、等待打印完成、卡住后的恢复）、`flow-control.md`（缓冲区感知的分块发送）、`rle.md`（RLE 编码与真机实测结论）、`dzserver.md`（打印服务器的目录、流水线、任务状态）、`cmd-impl-status.md`（命令实现进度）、`TODO.md`（进度与已知限制）。

- 通讯方式：USB 已实现；蓝牙（BLE）协议已确认，后端未实现（见 issue #3）
- 已测试机型：DP27P

## 附带的程序

- `dzprint_image`：图片抖成位图后打印
- `dzprint_typst`：编译 Typst 文档并逐页打印，页内 `#metadata((...)) <print-settings>` 可覆盖纸张间隔 / 浓度 / 速度 / RLE 等参数
- `dzserver`：gRPC 打印服务器，流水线是「入口队列 → N 个渲染 worker → 1 个打印队列 → 1 个打印 worker」，接受图片或 Typst tar；任务有 `SUCCEEDED` / `FAILED` / `CANCELLED` 三个互斥终态，失败带 stage / reason / message，支持查询和取消任务；[print-my-mail](./print-my-mail.md) 就是通过它打印的
- `test_usb`：真机测试平台（`--phase` / `--rle` / `--rows`）
- `probe_end_page`：真机验证「打印机沉默后还发 `EndPage`」的后果，结论已落地到库里，保留作复现和回归验证
- `dzcli`：CLI 骨架，功能未接

每个 bin 对应一个 feature（`bins` 是合集），只要库的话 `cargo build --no-default-features` 即可；`dzprint_typst` 和 `dzserver` 共用 `typst-support`。

## 结构

- `backend/`：`Transport` trait + `rusb` 中断端点读写，USB 外层 `1e <长度>` 切帧
- `command/`：请求/响应对象与 deku 字节模板（按机型分开）、4 种 varint、速度/浓度/间隔枚举
- `planner/`：`Assembler` 编码请求、`FlowPlanner` 做预算感知的分块发送、位图 → 打印计划
- `frontend/`：`Device` 批量发送 / 按类型接收 / 等打印完成与沉默恢复，worker 线程交由 [`mytm`](./libraries/mytm.md) 的 `TaskManager` 托管
- `bin/dzserver/`：`api.rs` 接口、`job.rs` 任务表与失败原因、`pipeline.rs` 渲染 / 打印队列、`render.rs` 渲染、`printer.rs` 设备生命周期与重连
- `bin/common/`：CLI 与服务器共用的参数、打印循环和 Typst 渲染
- `rle/` 与 `bitmap/`：位级 RLE4/5/6 编解码、按 bit 组织的位图

## 相关项目

- [print-my-mail](./print-my-mail.md) ![py](https://img.shields.io/badge/-python-black?logo=Python) 把新邮件和 GitHub 通知打印成小票
- [watchwallet](./watchwallet.md) ![rs](https://img.shields.io/badge/-rust-black?logo=Rust) EVM 钱包活动监听器，通过 `dzserver` 打印收款 / 转账 / 授权通知

## 使用注意

- 依赖 `dbus-devel`；编译 `dzserver` 还需要 `protobuf-compiler`
- 需要 udev 规则，例如 `SUBSYSTEM=="usb", ATTRS{idVendor}=="3533", ATTRS{idProduct}=="5c15", MODE="0666"`
- 行数据默认不压缩：RLE 在 DP27P 上打真实照片会在纸中间插入空白，只建议给简单线稿实验
- `--print-timeout` 是等打印结束的总超时，`--recv-timeout` 是单条响应的等待超时；`RUST_LOG=debug` 开细节日志
- 打印中开盖、缺纸、USB 拔出会作废当前 job；`dzserver` 空闲时会主动重连，打印机长时间回不到可打印状态时按 `--print-timeout` 作废当前 job，避免后面的任务被堵死
- 打印机沉默后不能补 `EndPage`：库会在写页尾前确认设备还在应答；恢复方式是断开连接、静置约 2 秒再重新打开，当前 job 仍然作废
- `dzserver` 渲染时记录打印头点数，排队期间换了打印头宽度不同的机型会让该任务失败，需要重新提交

## 发版

推 `v*` tag 会触发 `.github/workflows/release.yml`：先校验 tag 与 `Cargo.toml` 的版本一致，再在 Ubuntu 22.04 上跑测试并构建 x86_64 的 `dzserver` / `dzprint_image` / `dzprint_typst` 发行包（附 `proto/dzsrv.proto`、`LICENSE` 和 `.sha256`）。`dzcli`、`test_usb`、`probe_end_page` 仍只能从源码构建。

## 分支

`new-standard`（重写命令层）已经合并回 `master`，现在一套实现，不再有新老代码并存。
