# `unsafe { ... }` 系列

> 这个系列的名字灵感源自 Rust 的 `unsafe` 关键字

顾名思义，这个系列的产品包括各种安全硬件(secure hardware)。

为什么 `unsafe` 呢？因为在理念上，绝对的安全是不存在的，安全永远是相对的，因为威胁模型是未知的，双方的博弈永远不会结束。

不出意外，这些项目主要的编程语言会是 Rust。

## 多用途密钥 `unsafe { key }`

这是嵌入式项目。

尺寸类似 U 盘，但是多一个用于确认操作的触摸按键。支持 FIDO，PGP Smartcard 等协议标准。这个项目的灵感来自 CanoKeys。

- https://github.com/sb-child/unsafe-key
- https://github.com/sb-child/unsafe-key-source (早期版本)

项目状态：其实有几版早期的软硬件设计了。之后我应该先把 `unsafe { wallet }` 做出来，因为这两个项目在计划内会使用大致相同的硬件架构。

## 加密货币钱包 `unsafe { wallet }`

这是嵌入式项目。

长得像银行 K 宝，但是里面保存着绝密的私钥。用途很简单，就是签署交易。其实 SafePal X1 的外观设计，和一些流行的钱包的架构都给我了很多灵感和 knowledge。

- https://github.com/sb-child/unsafe-wallet

项目状态：还在评估可实施性，我前期计划做多个版本，从大尺寸到小尺寸。

## 核心组件 `unsafe { core }`

这是嵌入式项目。

构成安全产品的通用核心组件。

- https://github.com/sb-child/unsafe-core

项目状态：在选型以及考虑制造和规范问题。

## UnsafePal

> SafePal 嘴上说着开源，但是其实它的固件源码是半开源的... [软件不透明](https://github.com/SafePalWallet/safepal-x1/issues/6) [硬件不透明](https://github.com/SafePalWallet/safepal-x1/issues/9)

这个项目的目标是实现一个能够与 SafePal X1 钱包进行蓝牙通信的 SDK，这个钱包只支持蓝牙。

- https://github.com/sb-child/UnsafePal

项目状态：源代码非常残缺不全，在思考怎么下手。
