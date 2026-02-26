---
name: concept-extractor
description: "Use when extracting core knowledge concepts from a document to build a wiki-linked knowledge graph without modifying the original file"
---

# NoteMD Pro - Concept Extraction

## Overview

This feature extracts key concepts from notes and creates interconnected concept notes. It builds a knowledge graph by generating new notes for each concept and adding backlinks to source files.

## When to Use

- **Build knowledge base**: Create interconnected notes
- **Extract key terms**: Identify important concepts in documents
- **Organize research**: Map relationships between ideas
- **Create index notes**: Generate overview notes for topics

## Function Call Chain

```
extractConceptsFromFile (fileUtils.ts)
├── read_file(inputPath)
├── splitContent(content, settings)
├── getProviderForTask('extractConcepts', settings)
├── call[Provider]API(prompt, chunk)
├── Parse CONCEPT: lines from response
└── Return Set<string> of concepts

createConceptNotes (fileUtils.ts)
├── for each concept
�?  ├── normalizeNameForFilePath(concept)
�?  └── write_file(notePath, content)
└── generateConceptLog (if enabled)

generateConceptLog
├── mkdir_p(logFolderPath)
└── write_file(logFilePath, logContent)
```

## Key Functions (from fileUtils.ts)

### extractConceptsFromFile

Extracts concepts from a file using LLM.

```typescript
export async function extractConceptsFromFile(
  plugin: NotemdPlugin,
  inputPath: string,
  progressReporter: ProgressReporter,
): Promise<Set<string>>;
```

### createConceptNotes

Creates concept notes with backlinks.

```typescript
export async function createConceptNotes(
  settings: NotemdSettings,
  concepts: Set<string>,
  currentProcessingFileBasename: string | null,
  options?: { disableBacklink?: boolean; minimalTemplate?: boolean },
);
```

### generateConceptLog

Generates a log file of created concepts.

```typescript
async function generateConceptLog(
  settings: NotemdSettings,
  createdConcepts: string[],
);
```

### findDuplicates

Finds duplicate words in content.

```typescript
export function findDuplicates(content: string): Set<string>;
```

### handleDuplicates

Handles duplicate detection and reporting.

```typescript
export async function handleDuplicates(
  content: string,
  settings: NotemdSettings,
);
```

## Duplicate Concept Management

### checkAndRemoveDuplicateConceptNotes

Checks for and removes duplicate concept notes based on normalized names.

```typescript
export async function checkAndRemoveDuplicateConceptNotes(
  settings: NotemdSettings,
  progressReporter: ProgressReporter,
);
```

### Process Flow

```
checkAndRemoveDuplicateConceptNotes
├── Get all concept folder files
├── Normalize filenames (remove special chars)
├── Group files by normalized name
├── Find duplicates (case variations, etc.)
├── Show deletion confirmation modal
├── Merge or remove duplicates
└── Update backlinks
```

### Duplicate Detection Modes

| Mode                  | Description               |
| --------------------- | ------------------------- |
| `vault`               | Check entire vault        |
| `include`             | Check specified paths     |
| `exclude`             | Exclude specified paths   |
| `concept_folder_only` | Check only concept folder |

### Settings

| Setting                    | Description                     |
| -------------------------- | ------------------------------- |
| `duplicateCheckScopeMode`  | Detection mode                  |
| `duplicateCheckScopePaths` | Paths for include/exclude modes |

## Settings

### Concept Note Settings

| Setting                      | Description                  |
| ---------------------------- | ---------------------------- |
| `useCustomConceptNoteFolder` | Use custom concept folder    |
| `conceptNoteFolder`          | Path to concept notes folder |

### Log Settings

| Setting                       | Description                      |
| ----------------------------- | -------------------------------- |
| `generateConceptLogFile`      | Generate log of created concepts |
| `useCustomConceptLogFolder`   | Use custom log folder            |
| `conceptLogFolderPath`        | Path to log folder               |
| `useCustomConceptLogFileName` | Custom log filename              |
| `conceptLogFileName`          | Log filename                     |

### Duplicate Detection

| Setting                    | Description                                          |
| -------------------------- | ---------------------------------------------------- |
| `enableDuplicateDetection` | Enable duplicate word detection                      |
| `duplicateCheckScopeMode`  | 'vault', 'include', 'exclude', 'concept_folder_only' |
| `duplicateCheckScopePaths` | Paths for include/exclude modes                      |

### Extract Options

| Setting                          | Description                     |
| -------------------------------- | ------------------------------- |
| `extractConceptsMinimalTemplate` | Use minimal template            |
| `extractConceptsAddBacklink`     | Add backlinks to concept notes  |
| `extractConceptsProvider`        | Provider for concept extraction |
| `extractConceptsModel`           | Model for extraction            |

## Prompt Template (extractConcepts)

From `promptUtils.ts`:

```
You are an AI assistant specializing in knowledge extraction.
Your task is to Completely decompose and structure the knowledge points
in this markdown document, analyze the markdown document and identify
all core concepts and keywords.

CRITICAL: Your output must be a simple list of core concepts.
Each concept must be on a new line and prefixed with CONCEPT:.
Do not include the original text, explanations, or any other formatting.

Concept Identification Criteria:
- Identify Core Concepts: Extract nouns or noun phrases central to the topic
- Prioritize Specificity: Extract most specific concept available
- Technical & Scientific Terms: Focus on technical terms, principles
- Contextual Relevance: Only extract if it's a core concept

Rules:
1. Normalize to singular form
2. Multi-word concepts take priority over single-word
3. Singular/plural: extract singular only
4. Avoid common nouns, proper nouns unless core concept
5. Ignore References/Bibliography sections
6. Extract unique concepts only
7. Output empty list if no concepts found

Example Output:
CONCEPT: Dielectric Relaxation
CONCEPT: Polymer Physics
CONCEPT: Turing Machine
```

## Concept Note Template

### New Note

```markdown
# {Concept Name}

## Linked From

- [[Source Note]]
```

### Existing Note (backlink added)

```markdown
# {Concept Name}

## Linked From

- [[Source Note]]
- [[Another Source]]
```

## Error Handling

### Common Errors

1. **No concepts found**
   - Warning: "No concepts found in LLM output"
   - Solution: Check content quality, adjust prompt

2. **Invalid folder path**
   - Error: "Concept note output path exists but is not a folder"
   - Solution: Check settings, create folder manually

3. **File name conflicts**
   - Error: Multiple concepts map to same filename
   - Solution: normalizeNameForFilePath handles special characters
