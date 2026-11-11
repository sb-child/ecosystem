# photon

高性能 [Rust/WebAssembly 图像处理库](https://github.com/silvia-odwyer/photon)

上游活跃度: 非常活跃，最后一次提交于 2026 年 9 月（持续合并社区 PR）。

- 链接: <https://github.com/sb-child/photon/tree/patch-remove-wasm-bindgen>
- 上游链接: <https://github.com/silvia-odwyer/photon>
- 编程语言: ![rs](https://img.shields.io/badge/-rust-black?logo=Rust)
- 在 Cargo.toml 安装: `photon-rs = { git = "https://github.com/sb-child/photon", rev = "6904cc0576e1eb834da14159631cd42eba262588" }`

---

## 简介

纯 Rust 编写的图像处理库（crates.io 上叫 `photon-rs`），既可原生用于命令行/桌面程序，也可编译成 WebAssembly 在浏览器或 Node 中运行。提供调色、滤镜、噪声、模糊、锐化、色彩空间转换、图像缩放裁剪、文字绘制等常见操作，API 按 `PhotonImage` / `Rgb` / `Rgba` 等类型组织。

## 相对于上游的改动

- 分支 `patch-remove-wasm-bindgen`（基于上游 master `e913bc4`，2026-09-18）：移除全部 `wasm_bindgen` 宏 —— 删掉所有 `#[cfg_attr(feature = "enable_wasm", wasm_bindgen)]` 属性及其相关导出代码（14 个文件，+171/-218），使其作为纯原生 Rust 库使用，不再导出 JS/WASM 绑定
