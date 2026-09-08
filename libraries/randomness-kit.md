# randomness-kit

测量随机性的函数库

- 链接: <https://github.com/sb-child/randomness-kit>
- 编程语言: ![py](https://img.shields.io/badge/-python-black?logo=Python) ![rs](https://img.shields.io/badge/-rust-black?logo=Rust) (python binding)
- 通过 [pypi](https://pypi.org/project/randomness-kit/) 安装: `pip install randomness-kit`

---

## 简介

量化一串数据"有多随机/多可预测"的工具集：提供最小熵（min-entropy）、香农熵（Shannon entropy，带 biased 修正版实现）等统计计算，另附字符串分片等辅助工具。Rust 核心 + maturin Python binding，通过 PyPI 发布（v0.1.x），常用来评估密钥/口令/随机源的不可预测性。
