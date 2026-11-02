# print-my-mail

通过打印一张小票提醒你有新邮件或 GitHub 通知。

- 链接: <https://github.com/sb-child/print-my-mail>
- 编程语言: ![py](https://img.shields.io/badge/-python-black?logo=Python)
- 许可证: MIT

---

## 简介

轮询配置好的 IMAP 邮箱和 GitHub 账户，把未读内容渲染成小票，交给 [dz-print](./dz-print.md) 的 gRPC 打印服务打印，并且只有在确认打印成功之后才把邮件 / 通知标记为已读。可用 OpenAI 兼容接口对邮件做摘要。

- 依赖 `dzserver`（dz-print 的 gRPC 打印服务器），通过本地地址提交图片或 Typst 文档
- 配置在 `config.toml`（邮箱应用专用密码、GitHub PAT），仓库里有 `config.toml.example`
- 子命令：默认常驻监听，`test` 打印示例页，`once` 只轮询一轮，`folders` 列出真实 IMAP 文件夹名
- 启动时只处理最近 1 分钟内到达的邮件，更早的未读邮件会被跳过，不会被打印或标记已读
