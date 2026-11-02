# dz-print

德佟印立方（[Detonger](https://detonger.com) / [DothanTech](https://dothantech.com)）标签打印机的第三方 USB/蓝牙驱动。

- 链接: <https://github.com/sb-child/dz-print>
- 编程语言: ![rs](https://img.shields.io/badge/-rust-black?logo=Rust)
- 许可证: MPL-2.0

---

## 简介

官方 SDK 不开源，协议全部来自逆向、抓包和真机测试，因此仓库本身也是一份持续修补的协议笔记：`protocol.md`（协议和命令）、`print-status.md`（状态类型）、`flow-control.md`（缓冲区感知的分块发送）、`rle.md`（RLE 编码与真机实测结论）、`TODO.md`（进度与已知限制）。

- 通讯方式：USB 已实现；蓝牙（BLE）协议已确认，后端未实现（见 issue #3）
- 已测试机型：DP27P
- 示例 bin：`dzprint_image`（图片抖动成位图）、`dzprint_typst`（编译 Typst 并逐页打印，页内 `#metadata` 可覆盖纸张间隔/浓度等参数）、`test_usb`（真机测试平台）、`dzcli`（CLI 骨架，功能未接）

## 结构

- `backend/`：`Transport` trait + `rusb` 中断端点读写，USB 外层 `1e <长度>` 切帧
- `command/`：请求/响应对象与 deku 字节模板（按机型分开）、4 种 varint、速度/浓度/间隔枚举
- `planner/`：`Assembler` 编码请求、`FlowPlanner` 做预算感知的分块发送、位图 → 打印计划
- `frontend/`：`Device` 批量发送 / 按类型接收 / 等打印完成，worker 线程交由 [`mytm`](./libraries/mytm.md) 的 `TaskManager` 托管
- `rle/` 与 `bitmap/`：位级 RLE4/5/6 编解码、按 bit 组织的位图

## 使用注意

- 依赖 `dbus-devel`
- 需要 udev 规则，例如 `SUBSYSTEM=="usb", ATTRS{idVendor}=="3533", ATTRS{idProduct}=="5c15", MODE="0666"`
- 行数据默认不压缩：RLE 在 DP27P 上打真实照片会在纸中间插入空白，只建议给简单线稿实验
- `--print-timeout` 是等打印结束的总超时，`--recv-timeout` 是单条响应的等待超时；`RUST_LOG=debug` 开细节日志

## 分支

`new-standard`（重写命令层）已经合并回 `master`，现在一套实现，不再有新老代码并存。
