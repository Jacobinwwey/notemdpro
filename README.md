# NoteMD Pro: Agentic Skills Library

[![Platform: Agnostic](https://img.shields.io/badge/Platform-Agnostic-blue.svg)](https://github.com/Jacobinwwey/notemdpro)
[![Primary Agent: Any](https://img.shields.io/badge/Agent-Any-green.svg)](https://github.com/Jacobinwwey/notemdpro)

**NoteMD Pro** is not a traditional Obsidian plugin. It is a highly portable **Agentic Skills Framework** designed strictly for Large Language Models (LLMs) interacting with Markdown files and Knowledge Graphs.

Inspired by [obra/superpowers](https://github.com/obra/superpowers), this repository equips any CLI Agent (Cursor, Claude Code, OpenCode, Aider) with hardened reasoning workflows ("Skills") to autonomously research, generate, heal, and interconnect Markdown vaults.

## 🚀 How NoteMD Pro Works

It starts the moment you fire up your coding agent on a Markdown vault. Instead of writing blindly, the Agent relies on a standardized set of **Skills** (located in the `skills/` directory) that dictate exact methodologies for extracting concepts, building dual-links (`[[Wiki-Link]]`), performing rigorous web research, and healing complex broken `mermaid` diagrams or `LaTeX` equations without relying on hallucinated environment APIs.

### The Basic Workflow

1. **`web-researcher`**: Gather context on technical topics using Tavily/DDG.
2. **`content-generator`**: Generate comprehensive markdown from an initial title/context.
3. **`mermaid-healer` & `formula-healer`**: Immediately validate the generated syntax. Uses proven regex heuristics first, falling back to LLM-in-the-loop.
4. **`concept-extractor`**: Analyze the finished text to map out core knowledge nodes.
5. **`link-analyzer`**: Automate Wikipedia-style `[[Dual-Link]]` creation across the entire folder to build a knowledge graph.
6. **`batch-processor`**: Scales out any of the above operations logically across 100+ files while intelligently dodging OOMs (by stripping raw Base64 image data) and handling HTTP 429 back-off signals.

## 📦 Installation

This Library is framework-agnostic. Pick your Agent:

### Cursor

Cursor reads the `.cursorrules` file automatically upon instantiation.
Simply open your Vault folder in Cursor, ensure the `.cursorrules` file is cloned into the root, and tell it:
_"Extract concepts from this note"_ or _"Analyze the links across this folder"_.

### Claude Code / Generic CLI

Wait for MCP Support or run directly pointing to the instruction URL.

```bash
claude "Fetch and follow instructions from https://raw.githubusercontent.com/Jacobinwwey/notemdpro/refs/heads/main/.codex/INSTALL.md"
```

### OpenCode

```bash
opencode "Fetch and follow instructions from https://raw.githubusercontent.com/Jacobinwwey/notemdpro/refs/heads/main/.opencode/INSTALL.md"
```

## 🧠 Philosophy

- **Standardized I/O**: Strictly generic Node.js/Python FileSystem primitives. Obsidian APIs (`app.vault`) are strictly forbidden.
- **Defensive Chunking**: Safely splits LLM context around Code Fences and LaTeX to avoid damaging syntax boundaries.
- **Fail-Safe Heuristics**: LLMs natively struggle to resolve broken Mermaid structural flow-graphs. The `mermaid-healer` skill explicitly prioritizes 37 battle-tested Regex heuristic steps _before_ escalating to the LLM.

## 🛡️ Sandbox Playground

Before unleashing NoteMD Pro on your 5000-note Zettelkasten, test your Agent's behavior on the `dummy-vault/test-file.md`. Ask it to:

> _"Run the formula-healer and mermaid-healer on the dummy-vault."_

## License

MIT License. See [LICENSE](LICENSE) for details.
