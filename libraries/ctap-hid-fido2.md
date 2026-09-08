# ctap-hid-fido2

安全密钥 [FIDO2](https://fidoalliance.org/passkeys/) [CTAP](https://fidoalliance.org/specs/fido-v2.0-id-20180227/fido-client-to-authenticator-protocol-v2.0-id-20180227.html) 客户端实现

上游活跃度: 活跃维护中。

- 链接: <https://github.com/sb-child/ctap-hid-fido2/tree/master>
- 上游链接: <https://github.com/gebogebogebo/ctap-hid-fido2>
- 编程语言: ![rs](https://img.shields.io/badge/-rust-black?logo=Rust)
- 在 Cargo.toml 安装: `ctap-hid-fido2 = { git = "https://github.com/sb-child/ctap-hid-fido2", rev = "f05589b3e842a47c9df3080cf4fe2248a2e44821" }`

---

## 简介

通过 USB HID 与 FIDO2 安全密钥（如 YubiKey）通信的 CTAP 客户端库：枚举 HID 设备并取得厂商/产品信息，支持 make_credential、get_assertion、PIN、指纹（bio）、largeBlobs、credential management 等 FIDO2 能力，附若干命令行示例（ctapcli 等）。

注意：上游很活跃，本 fork 的 master 分支已经落后于上游 develop。建议直接用上游的版本。

## 相对于上游的改动

- `HidInfo` 新增 `serial_number` 字段：把 hidapi 读到的设备序列号暴露给调用方
- 调试/类型名打印从 stdout 改到 stderr，避免污染正常输出
- 全仓 rustfmt 整理与示例程序更新，早期通过 merge 同步过上游的进展
