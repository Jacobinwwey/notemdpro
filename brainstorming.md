# NoteMD Pro: GitHub Publishing Brainstorm (Cross-Platform Skills Framework)

## 1. Project Positioning & Philosophy

Inspired by the `obra/superpowers` framework, `notemdpro` should not just be a script, but an **Agentic Skills Library for Markdown/Knowledge Graphs**. It should be platform-agnostic, meaning any major LLM Agent (Cursor, Claude Code, GitHub Copilot Workspace, OpenCode) can load and execute these skills.

## 2. Directory Structure Standardization

To ensure maximum compatibility, the repository should adopt a strict, standardized directory structure out-of-the-box:

```text
notemdpro/
├── .cursor/                 # Cursor-specific rules/prompts (if applicable)
├── .codex/                  # Codex specific installation instructions
│   └── INSTALL.md
├── .opencode/               # OpenCode specific installation instructions
│   └── INSTALL.md
├── docs/                    # Global documentation and API paradigms
├── skills/                  # The core independent skills library
│   ├── batch-processor/     # Formerly "batch"
│   ├── concept-extractor/   # Formerly "concepts"
│   ├── content-generator/   # Formerly "generate"
│   ├── formula-healer/      # Formerly "formula-fix"
│   ├── link-analyzer/       # Formerly "links"
│   ├── mermaid-healer/      # Formerly "mermaid-fix"
│   ├── web-researcher/      # Formerly "research"
│   └── text-translator/     # Formerly "translate"
├── README.md                # The master landing page
└── LICENSE
```

## 3. Cross-Platform Supported IDEs & Agents

### 3.1 Claude Code (CLI) & Cursor

- These platforms heavily utilize `.md` files as prompt context.
- We should package `notemdpro` into an MCP server or a "Plugin Marketplace" structure (similar to `obra/superpowers-marketplace`), allowing users to run `/plugin install notemdpro`.
- Alternatively, we provide a `.cursorrules` or generic system prompt that simply points the AI agent to read `skills/*/SKILL.md` when triggered by keywords (e.g., "Extract concepts from this note").

### 3.2 Generic CLI Tools (OpenCode, Codex, Aider)

- Provide simple `INSTALL.md` instructions. The user tells their agent: `Fetch and follow instructions from https://raw.githubusercontent.com/.../.opencode/INSTALL.md`.
- The instructions teach the AI to traverse the `skills/` directory and memorize the standardized workflows.

## 4. Establishing the "Basic Workflow" for NoteMD Pro

Just like `superpowers` has a mandatory workflow spanning from `brainstorming` to `writing-plans`, `notemdpro` should chain its skills logically:

1. **web-researcher**: Gathers context on a topic before writing.
2. **content-generator**: Drafts the initial Markdown based on the research.
3. **mermaid-healer / formula-healer**: Immediately acts as a post-generation validation step to ensure syntactical perfection.
4. **concept-extractor**: After writing, the AI runs this to identify core nodes.
5. **link-analyzer**: Wraps the identified concepts in `[[Wiki-Links]]` and updates the vault graph.

## 5. Security and Abort Paradigms (Best Practices)

- Every `SKILL.md` must explicitly warn the Agent never to blindly overwrite files without `OOM` (Out of Memory) checks on Base64 images.
- Provide a `dummy-vault/` in the repo for users/agents to run dry-run tests (TDD approach) before touching a user's real knowledge base.
- State clearly that the user must define standard I/O (like Node `fs`) rather than Obsidian APIs.

---

# NoteMD Pro: GitHub 发布方案头脑风暴 (跨平台技能框架)

## 1. 项目定位与核心理念

受到 `obra/superpowers` 框架的启发，`notemdpro` 不应仅仅被视为一个分离的脚本代码库，而应定位为 **“面向 Markdown 与知识图谱的 Agent 专属技能库” (Agentic Skills Library)**。它必须是平台无关的，这意味着任何主流的 LLM Agent (如 Cursor, Claude Code, OpenCode) 都可以直接加载并执行这些“技能”。

## 2. 目录结构的绝对标准化

为了保证最大化的 IDE/Agent 兼容性，仓库必须采用类似于超级能力框架的严格标准目录：

```text
notemdpro/
├── .cursor/                 # Cursor 专属规则或启动上下文
├── .codex/                  # Codex 专属安装指引
│   └── INSTALL.md
├── .opencode/               # OpenCode 专属安装指引
│   └── INSTALL.md
├── docs/                    # 全局文档与 API 范式说明
├── skills/                  # 核心技能库 (独立解耦)
│   ├── batch-processor/     # 批处理引擎 (原 batch)
│   ├── concept-extractor/   # 概念提取器 (原 concepts)
│   ├── content-generator/   # 内容生成器 (原 generate)
│   ├── formula-healer/      # 公式修复师 (原 formula-fix)
│   ├── link-analyzer/       # 链接分析师 (原 links)
│   ├── mermaid-healer/      # 图表修复师 (原 mermaid-fix)
│   ├── web-researcher/      # 网络研究员 (原 research)
│   └── text-translator/     # 文本翻译官 (原 translate)
├── README.md                # 统一的着陆页与使用说明
└── LICENSE
```

## 3. 面向各大 IDE 与 Agent 的跨平台兼容方案

### 3.1 Claude Code (CLI) 与 Cursor

- 这类平台高度依赖本地 `.md` 文件作为提示词上下文。
- 我们可以将 `notemdpro` 打包为一个轻量级 MCP Server，或者采用“插件市场”模式（学习 `obra/superpowers-marketplace`），允许用户通过类似 `/plugin install notemdpro` 的指令一键配置。
- 基础方案：提供包含全局指令的 `.cursorrules` 文件，指导 AI 代理通过监控特定关键词（例如“帮我提取这篇笔记的概念”），自动去读取对应的 `skills/*/SKILL.md` 执行逻辑。

### 3.2 通用 CLI 代理 (如 OpenCode, Codex, Aider等)

- 效仿参考项目，提供各平台的 `INSTALL.md`。使用方式是将一句话发给 AI：“请获取并遵循该链接的指令 `https://raw.githubusercontent.com/.../.opencode/INSTALL.md`”。
- 该安装文件会指导 AI 遍历本地或远端的 `skills/` 目录，并将结构化的工作流“刻入”它的短期记忆。

## 4. 确立 NoteMD Pro 的“标配工作流”

正如 `superpowers` 拥有一套从设计到测试的强制工作流，`notemdpro` 也应该为 AI 定义一套符合逻辑的“组合技”：

1. **网络研究员 (web-researcher)**：在动笔前，进行全网前沿知识检索。
2. **内容生成器 (content-generator)**：基于检索上下文，生成硬核且富有学术气息的 Markdown 文档。
3. **图表/公式修复师 (mermaid-healer / formula-healer)**：作为后置校验流，每次生成 Markdown 后**必须**自动闭环调用，确保语法完美。
4. **概念提取器 (concept-extractor)**：内容稳定后，切片提取核心知识点。
5. **链接分析师 (link-analyzer)**：根据知识点进行全库维度的 `[[双链]]` 构建。

## 5. 安全性与熔断机制 (最佳实践)

- 在发布时，每个技能文档 (`SKILL.md`) 开头不仅要有触发条件，还要有强制的**熔断告警**：要求 AI 在处理含大体积 Base64 图片的文档前必须清洗，防止 OOM。
- 在仓库中提供一个小型 `dummy-vault/`（测试桩），鼓励 AI 在用户的真实笔记库乱动前，先在测试环境跑通 Test-Driven Development 流程。
- 明确声明剥离 Obsidian API，任何文件操作必须基于通用标准 I/O 环境（如 Node `fs` 抽象层）。
