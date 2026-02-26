# 2026-02-26 v2.0

# Evaluation of "notemdpro": Towards Independent Lightweight Text Processing

(English Version)

## 1. Overview and Evaluation Premise

The `notemdpro` skill directory is designed to distill the capabilities of the complex `obsidian-NoteMD_new` plugin into an **independent, standalone AI skill set**. The goal of this skill is not to act as a developer guide for modifying the Obsidian source code, but rather to empower a Large Language Model (LLM) to achieve the same high-quality text processing, knowledge graph extraction, and syntax healing workflows independently, effectively achieving "lightweight text processing" outside the heavy constraints of the Obsidian architecture.

This re-evaluation analyzes how successfully the core logic, prompts, and orchestration flows have been extracted from the plugin environment and packaged into a portable, robust agentic capability.

---

## 2. Functional Migration Integrity (Abstraction Analysis)

### High-Fidelity Knowledge Extraction (Successfully Migrated)

- **Prompt Isolation**: The skill successfully extracts the highly engineered prompt templates from the original `promptUtils.ts`. Complex prompting strategies for generating Mermaid mindmaps, extracting Q&A, and formatting technical documentation have been cleanly documented, allowing any LLM to replicate the plugin's cognitive outcomes.
- **Workflow Decoupling**: The sophisticated multi-step workflows (e.g., Split Content $\rightarrow$ Call LLM $\rightarrow$ Clean Delimiters $\rightarrow$ Refine Mermaid) have been abstracted into logical flowcharts (like in `generate/SKILL.md`). This allows an autonomous AI agent to manually orchestrate these steps in any environment (e.g., an independent Python script, a VSCode extension, or a standalone MCP server).
- **Heuristic Rule Capture**: The `notemd-mermaid-fix` skill impressively captures the 30+ specific heuristic regexes used to repair LLM Mermaid hallucinations. By documenting the exact failure modes (e.g., unquoted labels, inverted arrows), it teaches the AI how to act as a self-healing pipeline.

### Migration Gaps (Barriers to True Independence)

While the cognitive logic has been migrated, the skills still suffer from "environmental memory" of the Obsidian platform, hindering true independence:

1. **Residual Obsidian API Coupling**: Across almost all skill documents (`generate`, `extraction`, `translate`, `batch`), the function signatures and pseudo-code heavily reference Obsidian-native types like `app: App`, `file: TFile`, `TFolder`, and `app.vault`. For a skill meant to be independent, these references induce LLM hallucinations where the AI might attempt to use `app.vault.read()` in an environment where only standard Node.js `fs` or Python `os` exists.
2. **Abstracted Chunking Logic**: The skill emphasizes "batch processing" and "splitting content", but heavily glosses over the actual mechanics of `splitContent`. An independent AI attempting to process a 50,000-word markdown file needs precise token-estimation rules and boundary-safe splitting logic (e.g., not splitting in the middle of a code block), which the skill does not currently provide enough detail to replicate independently.
3. **Missing Telemetry & Abort Paradigms**: While UI concepts like `ProgressReporter` were safely left behind (which is correct for an independent skill), the skill lacks an independent paradigm for handling fatal AI errors, such as HTTP 429 Rate Limits from the LLM provider, relying instead on vague "retry" suggestions.

---

## 3. Robustness Analysis (Standalone Capabilities)

### Cognitive & Syntax Healing Edge Cases

- **Formula Regex Fragility**: Inherited directly from the source project, the `formula-fix/SKILL.md` relies on a highly brittle line-based regex (`^(\s*)\$(\s*)$/gm`). If an AI agent attempts to use this regex independently in a generic text processing pipeline, it will fail to fix inline math (`$x=2$`) and multi-line scalar equations. This significantly reduces the robustness of the standalone math-fixing capability.
- **Mermaid "Regex vs. Reasoning" Conflict**: The `mermaid-fix` skill provides both 30+ regex rules and an "LLM-in-the-Loop" concept. In an independent, lightweight text processing setting run by a powerful LLM, running a waterfall of 30 regexes is inefficient and fragile compared to feeding a parser error directly back into the LLM's reasoning engine.

### Operational Robustness

- **Standalone OOM (Out Of Memory) Risks**: The batch processing skill references splitting by word count. In a lightweight processing pipeline, parsing Markdown files swollen with massive Base64-encoded image strings (`![[data:image/png;base64,...]]`) will break word-count heuristics and cause the AI workspace to crash due to memory exhaustion.

---

## 4. Areas for In-Depth Improvement and Outlook

To elevate `notemdpro` into a truly independent, universal text-processing engine powered by LLMs, the following evolutionary steps are recommended:

### A. Total Eradication of Obsidian Types (True Platform Agnosticism)

- **Action**: Scrub all instances of `TFile`, `App`, and `app.vault` from the `SKILL.md` files.
- **Replacement**: Replace them with universally understood I/O concepts: `Input String`, `Output File Path`, `Read Stream`, and `Write Stream`. The AI should be taught to operate purely on raw text strings and standard POSIX file system abstractions, making the skill instantly portable to any coding language or agent framework.

### B. Upgrade to Context-Aware Syntax Parsing

- **Action**: Deprecate the naive regex in `formula-fix`.
- **Replacement**: Teach the AI to simulate an Abstract Syntax Tree (AST) approach, or provide it with advanced state-tracking regexes that can intelligently distinguish between `$inline math$` and `$$display math$$` based on surrounding markdown structures (like code fences and tables).

### C. Elevate LLM-in-the-Loop as the Primary Engine

- **Action**: In `mermaid-fix`, demote the 30+ regexes to a "Legacy Fallback" section.
- **Replacement**: Establish "Parser-Feedback Reasoning" as the primary heuristic. Teach the AI to attempt attempting to render/parse the Mermaid block locally; if it fails, instruct the AI to ingest the syntax error and self-correct using its native reasoning capabilities (e.g., via DeepSeek-Reasoner), which guarantees much higher robustness for complex diagram topologies.

### D. Advanced Token-Safe Chunking Guidelines

- **Action**: Add a new sub-skill or heavily expand the architecture document detailing exactly _how_ to split markdown safely for an LLM.
- **Replacement**: Provide precise algorithms for "Smart Splitting"—ensuring the AI knows how to chunk at `##` headers, avoiding splits inside ` ``` ` code fences, LaTeX blocks, or frontmatter, and handling Base64 image stripping to prevent token-overflow and memory crashes during lightweight processing.

---

---

# “notemdpro”技能：面向独立轻量级文本处理引擎的评估

(Chinese Version)

## 1. 概述与评估前提

`notemdpro` 技能目录的设计初衷，是提取庞大复杂的 `obsidian-NoteMD_new` 插件的核心能力，将其提炼为一套**独立的、纯净的 AI 技能集**。该技能的目的绝非作为修改 Obsidian 源码的开发指南，而是为了赋能大语言模型（LLM），使其能够在脱离 Obsidian 沉重架构的外部环境中独立运作，以“轻量级文本处理”的模式，达成与原版插件完全一致的高质量内容生成、知识提取和语法修复效果。

本次重新评估，旨在分析原项目中的核心逻辑、提示词工程与系统编排流程，是否被成功地剥离并封装成了高度便携、鲁棒的智能体能力。

---

## 2. 功能迁移完整性（抽象化分析）

### 高保真认知逻辑提取（成功迁移的部分）

- **提示词工程的完美剥离**：该技能成功从原代码中提取了高度定制化的提示词模板。无论是生成 Mermaid 思维导图、从长文本中精准抽取 Q&A，还是排版专业的学术文档，其控制指令都被清晰地记录，使得任何 LLM 都能直接复刻插件的“认知大脑”。
- **工作流的巧妙解耦**：原始复杂的长链条操作（例如：切分文本 $\rightarrow$ 调用大模型 $\rightarrow$ 清洗 LaTeX 占位符 $\rightarrow$ 修复 Mermaid 结构）被成功抽象为了标准的逻辑流程图（如 `generate/SKILL.md` 中所示）。这使得自动化的 AI 智能体可以利用 Python 脚本、VSCode 插件乃至独立的 MCP 服务器，在任意环境中复现这套管线。
- **启发式规则的捕获**：`notemd-mermaid-fix` 技能非常惊艳地捕获了源码中为了对付大模型 Mermaid 幻觉而硬编码的 30 余种正则修复规则。通过记录具体的失败模式（如未加引号的标签、方向反转的箭头），它教会了 AI 如何构建一个自我修复的管线。

### 迁移差距（阻碍真正独立的盲点）

尽管“认知逻辑”已经迁移，但这些技能文档中依然残留着大量的 Obsidian 平台“环境记忆”，严重阻碍了其轻量化独立的初衷：

1. **残留的 Obsidian API 强耦合**：在几乎所有的技能文档（包括生成、提取、翻译等）中，伪代码和函数签名里充斥着 `app: App`、`file: TFile`、`TFolder` 和 `app.vault` 等 Obsidian 专有环境对象。对于一个旨在独立的系统，这些信息会造成 LLM 的严重幻觉——导致 AI 在只有原生 Node.js `fs` 或 Python `os` 的环境中，依然尝试去调用根本不存在的 `app.vault.read()`。
2. **文本分块（Chunking）逻辑过度抽象**：技能频繁强调“批处理”和“拆分内容”，却对 `splitContent` 的具体实现细节一笔带过。一个独立的 AI 在处理 5 万字的 Markdown 文档时，需要精确的 Token 估算规则和安全边界拆分逻辑（例如绝对不能将一个代码块从中间切断），而目前的技能设定并未提供足够的细节。
3. **缺乏独立的遥测与中断范式**：虽然技能正确地抛弃了原插件中的 UI 组件（如 `ProgressModal`，这符合“轻量独立”的诉求），但它却没有建立一套独立的、适用于无头 AI 的致命错误（如 LLM 返回 HTTP 429 频次超限）熔断处理机制，仅仅给了模糊的“重试”建议。

---

## 3. 鲁棒性分析（基于独立运作场景）

### 认知与语法自愈的边缘情况

- **公式正则的极度脆弱性**：`formula-fix/SKILL.md` 直接继承了原项目极其脆弱的基于行的单美元符正则（`^(\s*)\$(\s*)$/gm`）。如果 AI 智能体在通用的外部文本处理管线中强行应用此正则，将完全无法处理行内公式（`$x=2$`）或夹杂文本的公式区块。这大幅削弱了其作为独立数学修复引擎的鲁棒性。
- **Mermaid “正则暴力 vs. 模型推理”的冲突**：在面对极其灵活的 Mermaid 语法修复时，让一个拥有强大推理能力的 LLM（如 DeepSeek-Reasoner 或 GPT-4o）去按顺序执行 30 多道僵硬的正则表达式，不仅低效且容易误伤。相比之下，技能中提到的“闭环利用大模型重写修复”才是在独立运作时真正鲁邦的手段。

### 运维级鲁棒性

- **独立环境下的 OOM（内存溢出）危机**：批处理技能中提到按“词数”进行文件切分。在轻量级处理环境中，如果读取的 Markdown 文档内嵌了海量的 Base64 格式的图片编码文字，基于词数的探针将彻底失效，极易导致 AI 的执行环境因内存耗尽而瞬间崩溃。

---

## 4. 深入改进和未来发展的方向（展望）

为了将 `notemdpro` 真正升格为由大语言模型驱动的、宇宙通用的轻量级文本处理引擎，建议采取以下深度进化方案：

### A. 彻底剿灭 Obsidian 数据类型（实现纯正的平台无关性）

- **行动**：使用文本清理工具，在所有的 `SKILL.md` 中彻底抹除 `TFile`、`App` 和 `app.vault` 的所有痕迹。
- **替代方案**：将其全面替换为计算机科学界通用的 I/O 概念：`Input String`（输入字符串）、`Output File Path`（输出路径）、`Read Stream`（读流）和 `Write Stream`（写流）。必须教会 AI 纯粹在文本字符串和标准的 POSIX 文件系统抽象之上进行操作，使该技能包瞬间具备跨语言、跨框架的无限可移植性。

### B. 升级至上下文感知的语法解析器

- **行动**：废弃 `formula-fix` 中原始落后的正则表达式。
- **替代方案**：教导 AI 在处理公式时，应当模拟抽象语法树（AST）的行为机制，或者提供具备状态记忆的高级上下文正则规则，使其能够根据 Markdown 的上下文（如是否处于代码块或表格内）安全且智能地分辨 `$行内数学$` 与 `$$块级展示数学$$` 的区别。

### C. 确立“模型纠错闭环”为一线主力引擎

- **行动**：在 `mermaid-fix` 技能内，将那 30 多条死板的正则规则降级为“兜底遗留方案”。
- **替代方案**：确立“解析器报错反馈推理（Parser-Feedback Reasoning）”为首要启发式规则。教会 AI 尝试在本地对图表进行渲染或静态分析，一旦失败，直接抓取 Parser 的报错特征抛回给自身的深度思考能力核心，依靠原生推理完成对复杂图表拓扑结构的语法自愈，此举将带来碾压级的修补成功率。

### D. 建立“Token 安全”的文档智能分块算法体系

- **行动**：在架构指南中新增或重写有关“如何安全拆分 Markdown 让 LLM 读取”的核心子技能。
- **替代方案**：提供极其精准的“智能拆解算法（Smart Splitting）”规范——强制规定切分动作只能在 `##` 或 `###` 标题级发生，必须具备前瞻性地避开 ` ``` ` 代码块、LaTeX 区块以及元数据区的硬切割；并且必须引入 Base64 图片清洗预处理逻辑，从根本上杜绝独立轻量级处理时的 Token 额度溢出与内存崩溃。
