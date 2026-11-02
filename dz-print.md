# dz-print

德佟印立方（[Detonger](https://detonger.com)）标签打印机的第三方 USB/蓝牙驱动，主板由道臻信息技术有限公司（[DothanTech](https://dothantech.com)）生产。

- 链接: <https://github.com/sb-child/dz-print>
- 编程语言: ![rs](https://img.shields.io/badge/-rust-black?logo=Rust)
- 许可证: MPL-2.0

---

## 简介

官方 SDK 并不开源，这一层的开发完全基于对 SDK 的逆向、抓包、说明文档和上机测试，因此仓库本身就是一份持续修补的协议笔记 —— 除了代码，还带着 `protocol.md`（已分析出的协议与命令）、`print-status.md`（已知打印机状态类型）、`flow-control.md`（buffer 容量感知的分块发送流控）、`cmd-impl-status.md`（命令实现状态）几种文档。

- 通讯方式：USB 已实现；蓝牙（BLE）协议已确认但后端还没写（见 issue #3）
- 已测试机型：DP27P
- 示例：打印一张图片、打印 Typst 文档、`dzcli`（CLI 骨架，功能未接）

## 使用注意

- 依赖 `dbus-devel`
- 需要 udev 规则，例如 `SUBSYSTEM=="usb", ATTRS{idVendor}=="3533", ATTRS{idProduct}=="5c15", MODE="0666"`
- `master` 是逆向初期的实现；`new-standard` 分支正在重写命令层，`src/` 里新老两套代码并存（新实现带 `[新]` 标注）
