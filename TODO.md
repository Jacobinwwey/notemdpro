# NoteMD Pro: 技能补全与独立性提升计划 (Independent Processing Update Plan)

本计划基于新的 `analysis.md`，旨在将 `notemdpro` 彻底打造成一个脱离 Obsidian 的、独立且鲁棒的轻量级文本处理 AI 技能。
This plan addresses the gaps identified in the new `analysis.md`, focusing on elevating `notemdpro` into a completely independent, platform-agnostic, and robust lightweight text processing AI skill.

---

## 🌐 1. 彻底剿灭 Obsidian 耦合 (Total Eradication of Obsidian Coupling)

### 跨技能更新 (Cross-Skill Updates)

- [x] **目标 (Target)**: `architecture/SKILL.md`, `generate/SKILL.md`, `extraction/SKILL.md`, `translate/SKILL.md`, `batch/SKILL.md`, `concepts/SKILL.md`, `links/SKILL.md`, `research/SKILL.md`, `summarize-as-mermaid/SKILL.md`.
- [x] **动作 (Action)**: 清除伪代码与说明中所有的 `App`, `TFile`, `TFolder`, `app.vault.read()` 等 Obsidian 专有 API。替换为通用的文件系统抽象（如 `fs.readFileSync`，输入字符串，输出路径）。
- [x] **Action (English)**: Scrub all traces of Obsidian-native APIs (`App`, `TFile`, `app.vault`). Replace them with generic File System / I/O concepts suitable for any backend (Python, Node.js).

---

## 🧩 2. 智能分块与 OOM 规避 (Smart Chunking & OOM Avoidance)

### 更新 `batch/SKILL.md` & `architecture/SKILL.md` (Update Batch & Architecture)

- [x] **Token 安全切分 (Token-Safe Chunking)**: 明确写出切分 Markdown 的边界规则——绝不能从代码块 (` ``` `) 或 LaTeX 区块中间截断文本。
- [x] **Base64 图片清洗 (Base64 Sanitization)**: 教会 AI 在计算词数前或执行批处理前，主动清洗或剥离 Base64 编码的超长图片字符串 (`![[data:image/pxx;base64,...]]`)，从根本上杜绝内存溢出 (OOM)。

---

## 🛑 3. 独立遥测与强异常中断 (Telemetry & Abort Paradigms)

### 更新 `batch/SKILL.md` (Update Batch)

- [x] **HTTP 429 与致命错误 (HTTP 429 & Fatal Errors)**: 补充独立的重试与熔断机制。当 LLM 接口返回 429 或非 JSON 的网关宕机错误时，指导 AI 执行指数退避并记录通用日志，而非依赖原插件的 UI 弹窗。

---

## ⚕️ 4. 启发式语法引擎的重塑 (Syntax Healing Paradigm)

### 更新 `formula-fix/SKILL.md` (Update Formula Fix)

- [x] **废弃脆弱正则 (Deprecate Fragile Regex)**: 将现有的单行正则标记为缺陷。教导 AI 必须使用上下文感知的正则或模拟 AST (抽象语法树) 解析，以准确区分 `$行内公式$` 与 `$$块级展示公式$$`。

### 更新 `mermaid-fix/SKILL.md` (Update Mermaid Fix)

- [x] **重申正则的主导地位 (Reaffirm Regex Supremacy)**: 根据实际应用中的经验证明，即使是当今强大的 LLM 也往往无法直接修复复杂的 Mermaid 结构幻觉。因此，**必须恢复 37+ 启发式正则作为绝对主导引擎的地位**。LLM-in-the-loop 仅作为最后的备选项。
- [x] **Action (English)**: Explicitly note that the 37+ heuristic regexes are proven to be the most robust method in practice, as LLMs currently struggle to directly fix diagram structural hallucinations. Prioritize Regex over LLM-in-the-loop.

---

## 🚀 5. 跨平台技能框架发布 (Cross-Platform Framework Publish)

### 参考 `obra/superpowers` 模型 (Adopt Superpowers Model)

- [x] **目录标准化 (Directory Standardization)**: 将所有技能文件夹汇聚至 `skills/` 根目录。重命名为标准的 `-processor` / `-healer` 风格。
- [x] **安装指令分发 (Installation Guides)**: 建立 `.codex/INSTALL.md`、`.opencode/INSTALL.md` 与全局 `.cursorrules`，指导各类 Agent 自动读取环境。
- [x] **安全沙盒验证 (Sandbox Setup)**: 建立 `dummy-vault/` 测试区域，提供破碎图表与危险 Base64 测试桩，训练 AI 在真实运行前先测试安全边界。
