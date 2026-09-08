# ads1298-rs

生物电信号采集芯片 [ADS1298](https://www.ti.com/product/ADS1298) 的驱动程序

- 链接: <https://github.com/sb-child/ads1298-rs/tree/master>
- 编程语言: ![rs](https://img.shields.io/badge/-rust-black?logo=Rust)
- 在 Cargo.toml 安装: `ads1298-rs = { git = "https://github.com/sb-child/ads1298-rs", rev = "53e59f411196b206f9f0f6548a063c7c0dc91b51" }`

---

## 简介

TI ADS1298（8 通道、24 位 ΔΣ ADC，用于 ECG 等生物电信号采集）的 Rust 驱动，基于 embedded-hal 1.0 编写：按寄存器组（addressable / data / access）封装读写、提供初始化（initialization）、常规操作（operator）与连续数据流读取（stream_reader）等模块。

项目模板来自 Ghislaine Laios 的 ads1293-rs。
