---
name: notemdpro
description: "Obsidian NoteMD Pro - AI-powered note enhancement plugin. When Claude needs to work with Obsidian notes for: (1) Adding wiki-links between concepts, (2) Generating content from titles, (3) Web research and summarization, (4) Translation, (5) Concept extraction and management, (6) Mermaid diagram fixing, or (7) LaTeX formula fixing"
---

# NoteMD Pro - Obsidian Plugin Skills

## Overview

NoteMD Pro is an Obsidian plugin that enhances your note-taking with AI capabilities. It provides multiple workflows for note management, content generation, and syntax fixing.

## Workflow Decision Tree

### Content Enhancement
- **Add wiki-links** → Use "notemd-links" skill
- **Generate content from title** → Use "notemd-generate" skill
- **Extract concepts** → Use "notemd-concepts" skill
- **Summarize as Mermaid** → Use "notemd-summarize-as-mermaid" skill

### Research & Translation
- **Web research** → Use "notemd-research" skill
- **Translate notes** → Use "notemd-translate" skill

### Data Extraction
- **Q&A extraction** → Use "notemd-extraction" skill

### Syntax Fixing
- **Fix Mermaid diagrams** → Use "notemd-mermaid-fix" skill
- **Fix LaTeX formulas** → Use "notemd-formula-fix" skill

### Processing
- **Batch operations** → Use "notemd-batch" skill
- **Auto link refactoring** → Use "notemd-architecture" skill (Vault listeners)

### Architecture
- **Understand system design** → Use "notemd-architecture" skill

## Skills Index

| Skill | Description |
|-------|-------------|
| [notemd-links](links/SKILL.md) | Add wiki-links between concepts in markdown files |
| [notemd-generate](generate/SKILL.md) | Generate comprehensive content from note titles |
| [notemd-research](research/SKILL.md) | Research topics using web search and summarize |
| [notemd-translate](translate/SKILL.md) | Translate notes to different languages |
| [notemd-concepts](concepts/SKILL.md) | Extract concepts and create interconnected notes |
| [notemd-extraction](extraction/SKILL.md) | Extract Q&A from text based on questions |
| [notemd-summarize-as-mermaid](summarize-as-mermaid/SKILL.md) | Convert documents to Mermaid mindmaps |
| [notemd-mermaid-fix](mermaid-fix/SKILL.md) | Fix Mermaid diagram syntax errors |
| [notemd-formula-fix](formula-fix/SKILL.md) | Fix LaTeX formula formatting |
| [notemd-batch](batch/SKILL.md) | Batch processing with concurrency control |
| [notemd-architecture](architecture/SKILL.md) | System architecture and dependencies |

## Supported LLM Providers

The plugin supports 9 LLM providers:
- OpenAI
- DeepSeek
- Anthropic
- Google
- Mistral
- Azure OpenAI
- LMStudio
- Ollama
- OpenRouter

## Key Features

1. **Batch Processing**: Process multiple files with concurrency control
2. **Custom Prompts**: Configure prompts for each task type
3. **Multi-language Support**: Generate content in different languages
4. **Auto-fix**: Automatic Mermaid and formula fixing after generation
5. **Progress Reporting**: Real-time progress with cancellation support

## Dynamic Prompt Injection

Prompts can be customized dynamically without editing skill files:

### Prompt Variables

| Variable | Description | Example |
|----------|-------------|---------|
| `{TITLE}` | Note title for generation | "Neural Networks" |
| `{LANGUAGE}` | Target language | "Spanish" |
| `{TEXT}` | Content to process | File content |
| `{REFERENCE_CONTENT}` | Context for extraction | Full document |
| `{USER_INPUT}` | User question | "What is..." |
| `{TOPIC}` | Research topic | "Quantum Computing" |
| `{SEARCH_RESULTS_CONTEXT}` | Web search results | Combined snippets |

### Customizing Prompts

#### Method 1: Settings Configuration
```typescript
// In settings, customize prompts:
settings.customPromptAddLinks = `Your custom prompt here with {VARIABLES}`;
settings.customPromptGenerateTitle = `Another custom prompt`;
```

#### Method 2: Runtime Injection
```typescript
function getDynamicPrompt(basePrompt: string, variables: Record<string, string>): string {
    let prompt = basePrompt;
    for (const [key, value] of Object.entries(variables)) {
        prompt = prompt.replace(new RegExp(`{${key}}`, 'g'), value);
    }
    return prompt;
}

// Usage
const prompt = getDynamicPrompt(DEFAULT_PROMPTS.generateTitle, {
    TITLE: "Machine Learning",
    RESEARCH_CONTEXT_SECTION: context
});
```

#### Method 3: LLM-Guided Adaptation
```
User: "Generate a simpler explanation"
AI: Adjusts prompt to reduce technical depth

User: "Make it more academic"
AI: Adjusts prompt for formal tone
```

### Prompt Templates by Task

| Task | Default Variables |
|------|------------------|
| addLinks | content chunk |
| generateTitle | TITLE, RESEARCH_CONTEXT_SECTION |
| translate | LANGUAGE, TEXT |
| researchSummarize | TOPIC, SEARCH_RESULTS_CONTEXT, LANGUAGE |
| extractConcepts | REFERENCE_CONTENT |
| extractOriginalText | REFERENCE_CONTENT, USER_INPUT |
| summarizeToMermaid | document content |

### Best Practices

1. **Keep variables in braces**: `{VARIABLE}`
2. **Provide fallback**: Handle missing variables
3. **Validate prompts**: Test before production
4. **Version control**: Track prompt changes
