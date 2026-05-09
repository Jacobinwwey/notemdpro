# NoteMD Pro: Agentic Skills Library (智能体技能库)

[![Platform: Agnostic](https://img.shields.io/badge/Platform-Agnostic-blue.svg)](https://github.com/Jacobinwwey/notemdpro)
[![Primary Agent: Any](https://img.shields.io/badge/Agent-Any-green.svg)](https://github.com/Jacobinwwey/notemdpro)
[![Marketplace: skills.sh](https://img.shields.io/badge/Marketplace-skills.sh-violet.svg)](https://skills.sh)

[English](#english-version) | [中文版](#chinese-version)

---

## <a name="english-version"></a> English Version

**NoteMD Pro** is not a traditional Obsidian plugin. It is a highly portable **Agentic Skills Framework** designed strictly for Large Language Models (LLMs) interacting with Markdown files and Knowledge Graphs.

Inspired by [obra/superpowers](https://github.com/obra/superpowers), this repository equips any CLI Agent (Cursor, Claude Code, OpenCode, Aider) with hardened reasoning workflows ("Skills") to autonomously research, generate, heal, and interconnect Markdown vaults.

### 🌟 Publishing to the `skills.sh` Marketplace

This repository is fully compatible with the [skills.sh](https://skills.sh) Open Agent Skills ecosystem.
Because the skills are standardized with `SKILL.md` frontmatter, other users can discover these tools globally using `npx skills find notemdpro`, or install them directly via the Skills CLI.

### 🚀 How NoteMD Pro Works

It starts the moment you fire up your coding agent on a Markdown vault. Instead of writing blindly, the Agent relies on a standardized set of **Skills** (located in the `skills/` directory) that dictate exact methodologies for extracting concepts, building dual-links (`[[Wiki-Link]]`), performing rigorous web research, and healing complex broken `mermaid` diagrams or `LaTeX` equations.

#### The Basic Workflow

1. **`web-researcher`**: Gather context on technical topics using Tavily/DDG.
2. **`content-generator`**: Generate comprehensive markdown from an initial title/context.
3. **`mermaid-healer` & `formula-healer`**: Immediately validate the generated syntax. Uses proven regex heuristics first, falling back to LLM-in-the-loop.
4. **`concept-extractor`**: Analyze the finished text to map out core knowledge nodes.
5. **`link-analyzer`**: Automate Wikipedia-style `[[Dual-Link]]` creation across the entire folder to build a knowledge graph.
6. **`batch-processor`**: Scales out any of the above operations logically across 100+ files while intelligently dodging OOMs (by stripping raw Base64 image data) and handling HTTP 429 back-off signals.

Modern NotEMD parity note:
- Canonical diagram action naming is `generate-diagram` / `preview-diagram`.
- Legacy `generate-experimental-diagram` should be treated as a compatibility alias that normalizes to `generate-diagram`.

### 📦 Installation

This Library is framework-agnostic. Pick your Agent:

#### Method 1: The Skills CLI (Recommended)

You can inject NoteMD Pro directly into your favorite AI agent via the generalized `skills` CLI.

```bash
npx skills add Jacobinwwey/notemdpro@mermaid-healer
npx skills add Jacobinwwey/notemdpro@concept-extractor
```

_Note: If you want to browse all available skills from this repo globally, use `npx skills find Jacobinwwey/notemdpro`._

#### Method 2: Cursor

Cursor reads the `.cursorrules` file automatically upon instantiation.
Simply open your Vault folder in Cursor, ensure the `.cursorrules` file is cloned into the root, and tell it:
_"Extract concepts from this note"_ or _"Analyze the links across this folder"_.

#### Method 3: Claude Code

Run directly pointing to the instruction URL:

```bash
claude "Fetch and follow instructions from https://raw.githubusercontent.com/Jacobinwwey/notemdpro/refs/heads/main/.codex/INSTALL.md"
```

#### Method 4: OpenCode

```bash
opencode "Fetch and follow instructions from https://raw.githubusercontent.com/Jacobinwwey/notemdpro/refs/heads/main/.opencode/INSTALL.md"
```

### 🧠 Philosophy

- **Standardized I/O**: Strictly generic Node.js/Python FileSystem primitives. Obsidian APIs (`app.vault`) are strictly forbidden.
- **Fail-Safe Heuristics**: LLMs natively struggle to resolve broken Mermaid structural flow-graphs. The `mermaid-healer` skill explicitly prioritizes 37 battle-tested Regex heuristic steps _before_ escalating to the LLM.

### 🛡️ Sandbox Playground

Before unleashing NoteMD Pro on your 5000-note Zettelkasten, test your Agent's behavior on the `dummy-vault/test-file.md`. Ask it to:

> _"Run the formula-healer and mermaid-healer on the dummy-vault."_

---

## <a name="chinese-version"></a> 中文版 (Chinese Version)

**NoteMD Pro** 并不是一个传统的 Obsidian 插件。它是一个高度可移植的**智能体技能框架 (Agentic Skills Framework)**，专门为那些需要处理 Markdown 文件和构建知识图谱的大型语言模型 (LLM) 量身定制。

受到 [obra/superpowers](https://github.com/obra/superpowers) 的启发，本项目赋予了任何 CLI CLI 代理 (如 Cursor, Claude Code, OpenCode, Aider) 经过硬化的推理工作流（即“技能”），使它们能自主研究、生成、修复并串联 Markdown 笔记库。

### 🌟 发布至 `skills.sh` 技能市场

本仓库完美兼容 [skills.sh](https://skills.sh) 开放智能体生态系统。
由于所有技能都封装了标准化的 `SKILL.md` 配置头（Frontmatter），全球用户均可通过 `npx skills find notemdpro` 搜索发现，或通过 CLI 直接安装这些强大的 Markdown 技能。

### 🚀 NoteMD Pro 工作原理

当您在 Markdown 笔记库中启动编程代理时，此框架就会介入工作。代理不再只是盲目生成文本，而是依赖于一套标准化的**技能库**（存放在 `skills/` 目录下）。这套技能库为概念提取、建立双链 (`[[Wiki-Link]]`)、执行严谨的网页研究、以及修复复杂的 `mermaid` 图表和 `LaTeX` 公式定义了高精度的标准化流程。

#### 核心工作流

1. **网络研究员 (`web-researcher`)**: 动笔前，基于特定话题进行全网上下文信息检索。
2. **内容生成器 (`content-generator`)**: 基于标题与上下文，生成具备硬核学术气息的 Markdown 文稿。
3. **图表/公式修复师 (`mermaid-healer` / `formula-healer`)**: 作为强逻辑后置校验，自动修复生成的语法错误。它优先依赖 37 条实战验证的正则启发式规则，仅在必要时才回退给大模型 (LLM-in-the-loop)。
4. **概念提取器 (`concept-extractor`)**: 对完稿文本进行切片分析，提取核心概念节点。
5. **链接分析师 (`link-analyzer`)**: 对提取出的概念进行全库维度的 `[[维基双链]]` 自动化构建。
6. **批处理引擎 (`batch-processor`)**: 负责将上述任何单线操作扩展至成百上千个文件：通过主动清理超长 Base64 图片来杜绝内存溢出 (OOM) 并应对 HTTP 429 的指数退避。

与新版 NotEMD 对齐说明：
- 图表 canonical action 命名为 `generate-diagram` / `preview-diagram`。
- 旧命名 `generate-experimental-diagram` 作为兼容别名，应归一化到 `generate-diagram`。

### 📦 安装方式

此技能库支持任何底层框架，您可以根据自己使用的智能体自由选择：

#### 方法 1: 使用 Skills CLI (推荐推荐)

您可以利用通用的 `skills` 包管理器，直接将 NoteMD Pro 注入到各种 AI 开发工具内部。

```bash
npx skills add Jacobinwwey/notemdpro@mermaid-healer
npx skills add Jacobinwwey/notemdpro@concept-extractor
```

_提示: 想要全局检索此仓库中的可用技能，请执行: `npx skills find Jacobinwwey/notemdpro`._

#### 方法 2: Cursor

Cursor 在启动时可自动读取项目根目录的 `.cursorrules` 配置文件。
只需在 Cursor 中打开您的笔记目录，并且确保根目录下存在下载好的 `.cursorrules` 文件。然后向 AI 发出指令：
_“提取这篇笔记的概念”_ 或 _“分析该文件夹下所有的双向链接”_。

#### 方法 3: Claude Code

可以直接运行并指向远程指令链接：

```bash
claude "Fetch and follow instructions from https://raw.githubusercontent.com/Jacobinwwey/notemdpro/refs/heads/main/.codex/INSTALL.md"
```

#### 方法 4: OpenCode

```bash
opencode "Fetch and follow instructions from https://raw.githubusercontent.com/Jacobinwwey/notemdpro/refs/heads/main/.opencode/INSTALL.md"
```

### 🧠 设计理念

- **标准化输入/输出 (I/O)**: 严格依赖通用型 Node.js/Python 的本地文件系统原语，彻底废弃对 Obsidian 原生 API (`app.vault`) 的私有依赖。
- **失效保护启发式引擎**: 考虑到当前 LLM 先天不擅长直接修复 Mermaid 等结构化的制图语法逻辑图，`mermaid-healer` 强制在大模型介入**之前**，优先使用 37 种通过了压力测试的正则表达式启发式修复步骤。

### 🛡️ 安全沙盒游乐场

在让 NoteMD Pro 代理操作您珍贵的拥有近 5000 篇笔记的 Zettelkasten 核心库之前，请先将代理放进自带的 `dummy-vault/test-file.md` 沙盒中跑一跑。让它执行如下指令以验证它对错误代码和 OOM（内存溢出）的保护能力：

> _"Run the formula-healer and mermaid-healer on the dummy-vault."_

## License

MIT License. See [LICENSE](LICENSE) for details.
