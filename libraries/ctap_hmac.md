# ctap_hmac

安全密钥 [FIDO2](https://fidoalliance.org/passkeys/) [CTAP](https://fidoalliance.org/specs/fido-v2.0-id-20180227/fido-client-to-authenticator-protocol-v2.0-id-20180227.html) 客户端实现

上游活跃度: 最后一次提交于 2021 年 8 月，已停止维护。

- 链接: <https://github.com/sb-child/ctap_hmac/tree/master>
- 上游链接: <https://github.com/shimunn/ctap>
- 编程语言: ![rs](https://img.shields.io/badge/-rust-black?logo=Rust)
- 在 Cargo.toml 安装: `ctap_hmac = { git = "https://github.com/sb-child/ctap_hmac", rev = "72fc9c304cca79c45f05d66afcf937022d3ff54c" }`

---

## 简介

通过 HID 与 FIDO2 安全密钥通信的 CTAP 客户端库（shimunn/ctap 的分支），围绕 hmac-secret 扩展场景使用（见 examples/hmac.rs）：枚举设备、请求断言、并用 CTAP 的 HMAC 能力对固定数据派生 HMAC，可做无服务器口令/本地加密之类的用途。

## 相对于上游的改动

- 手动合入两个上游一直未合的开源 PR：#1 robin-nitrokey/usage-shimunn（修复 usage 参数检查）、#2 HeroicKatora/master（给 request builder 结构体补文档等）
- 升级 ring 等依赖并适配其新 API（`EphemeralPrivateKey::compute_public_key()`、`hmac::Key`、`UnparsedPublicKey` 等，涉及 crypto.rs、error.rs、lib.rs），使 crate 能在 2024 年的工具链下编译
