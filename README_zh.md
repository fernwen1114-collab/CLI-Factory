<div align="center">
  <img src="assets/hero.png" alt="CLI-Factory Hero Banner" width="800">
  <br/>
  <h1>🏭 CLI-Factory</h1>
  <strong>Agent-native CLI 的规范与工厂。</strong>
  <br/><br/>
  
  [![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](https://opensource.org/licenses/MIT)
  [![Status: Early Stage](https://img.shields.io/badge/Status-Early_Stage-orange.svg)]()
  
  <br/>
  <a href="README.md">English</a> | 中文
</div>

---

## 🏭 CLI-Factory 是什么

**一份规范**，定义 agent-native CLI 必须提供什么——结构化 JSON 输出、任务生命周期、预算控制、错误分类、幂等性、漂移检测。

**一套工具**，帮助开发者构建新的 CLI——或升级现有的 CLI——使其符合这份规范。

```
  你的配置                规范               测试套件
       │                    │                     │
       ▼                    ▼                     ▼
  ┌──────────────────────────────────────────────────┐
  │               CLI-Factory  🔥                    │
  │          燃烧 token，vibe-code 你的 CLI           │
  │                                                  │
  │            任意 CLI 生成后端                       │
  └────────────────────────┬─────────────────────────┘
                           │
                           ▼
                   Agent-Native CLI
```

## 🚨 问题

AI agent 通过 CLI 命令与世界交互。现在已经有很多优秀的工具可以生成 CLI——把网站变成命令、为桌面应用生成 CLI 壳、包装内部工具。每周都有新的工具出现。

这些工具解决了**接入层**的问题。CLI-Factory 解决的是**可靠性层**的问题——通过一个共享的标准。

举个实际的例子。bb-browser 可以调用一个视频生成平台：

```bash
bb-browser site xiaoyunque/generate-video "prompt" "ref" "thread" "model" "720p"
```

接入没问题。但对于 agent 来说，真正的难题在调用返回之后：

- ❌ 任务完成了吗？——没有生命周期追踪
- ❌ 花了多少钱？——没有预算感知
- ❌ 产出在哪？——没有产物获取
- ❌ 超时了能重试吗？——没有幂等性
- ❌ 平台变了？——没有漂移检测
- ❌ 结构化的错误？——没有错误分类

这就是可靠性缺口。CLI-Factory 填补它。

## ⏳ 为什么是现在

世界正在被生成的 CLI 快速填满。浏览器变成了命令，桌面应用变成了 CLI 壳，内部工具一夜之间被包装好。

瓶颈变了。

我们不再只是需要*更多*的 CLI。我们需要 agent 在真实执行中可以信赖的 CLI：长时间运行的任务、不稳定的平台、预算约束、重试、产物收集、运行时漂移。

如果没有一个共享标准，每一次 agent 集成都要重新发明同样的答案：

- 成功的返回长什么样？
- 怎么区分排队中、运行中、已完成？
- 重试安全吗？
- 这次运行花了多少？
- 产出在哪里？
- 是平台挂了，还是我的输入有问题？

CLI-Factory 就是为了让这些答案标准化。

## 🛡️ 核心保证

CLI-Factory 对 agent 应该能依赖什么有明确的主张：

- 🧩 **结构化输出**：每个命令返回机器可读的 JSON，而不是需要猜测的文本。
- 🚦 **生命周期可见**：异步工作暴露清晰的状态——排队、运行中、成功、失败、超时。
- 💰 **预算感知**：估算和执行可以被明确的花费上限保护。
- 🔄 **幂等执行**：重试不会意外地重复昂贵的操作。
- ⚠️ **类型化错误**：agent 能区分用户错误、平台故障、可重试的瞬态问题和漂移。
- 📦 **产物获取**：产出是可发现的、可下载的、被明确报告的。
- 📡 **漂移检测**：当目标平台发生变化时，CLI 清晰地暴露而不是静默退化。

## 🗺️ 路线图

我们分层构建 CLI-Factory：

1. 定义规范：命令契约、生命周期语义、错误分类、预算模型。
2. 交付参考实现：一个端到端的 CLI，在实践中验证规范。
3. 构建工具集：脚手架、适配器、可复用组件，用于升级现有 CLI。
4. 增加验证：合规测试，让开发者证明自己的 CLI 是 agent-reliable 的。
5. 扩大后端覆盖：支持更多 web、桌面和内部工具集成。

## 🚧 当前状态

> **早期阶段——正在定义规范。**
>
> 我们正在编写规范并构建第一个参考实现。愿景清晰，代码正在编写中。

## 🤝 加入我们

我们在寻找相信 CLI 应该成为 agent 生态一等公民的开发者。

如果你对规范设计、CLI 开发或集成新后端感兴趣——[提一个 issue](https://github.com/fernwen1114-collab/CLI-Factory/issues) 或 [发起讨论](https://github.com/fernwen1114-collab/CLI-Factory/discussions)。
