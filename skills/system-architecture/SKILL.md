---
name: system-architecture
description: "Use when understanding module dependencies, function call chains, or the FileSystemPort abstraction layer of the NoteMD Pro pipeline"
---

# NoteMD Pro - Architecture

## Module Structure

### Source Files

| Module                 | File                                 | Responsibilities                                                             |
| ---------------------- | ------------------------------------ | ---------------------------------------------------------------------------- |
| **Main Entry**         | `main.ts`                            | Plugin lifecycle, command registration, settings management, UI coordination |
| **File Operations**    | `fileUtils.ts`                       | File processing, concept extraction, backlink management, duplicate handling |
| **LLM Integration**    | `llmUtils.ts`                        | API calls to 9 providers, error handling, retry logic                        |
| **Prompts**            | `promptUtils.ts`                     | Default prompts for all tasks                                                |
| **Mermaid Processing** | `mermaidProcessor.ts`                | Syntax validation and fixing                                                 |
| **Formula Fixing**     | `formulaFixer.ts`                    | LaTeX delimiter fixes                                                        |
| **Translation**        | `translate.ts`                       | File/folder translation                                                      |
| **Search**             | `searchUtils.ts`, `SearchManager.ts` | Web research                                                                 |
| **Text Extraction**    | `extractOriginalText.ts`             | Q&A extraction from text                                                     |
| **Utilities**          | `utils.ts`                           | Content chunking, token estimation                                           |
| **UI Components**      | `ui/` directory                      | Settings tab, progress modals, sidebars                                      |

## Vault Event Listeners (Auto Link Refactoring)

The plugin automatically listens to Vault events to maintain link integrity:

### File Rename Handler

When a file is renamed, independently resolve backlinks:

```
Watch File System / Event Hook
├── Extract old and new filename
├── Search all files for [[oldName]] links
├── Replace with [[newName]]
└── Log updated count
```

### File Delete Handler

When a file is deleted, automatically remove backlinks:

```
Watch File System / Event Hook
├── Extract deleted filename
├── Search all files for [[deletedName]] links
├── Remove links and clean empty list items
└── Clean extra blank lines
```

### Core Functions (fileUtils.ts)

````typescript
### Core Functions (fileUtils.ts)

```typescript
export async function handleFileRename(
  oldPath: string,
  newPath: string,
);
export async function handleFileDelete(path: string);
````

```

## Function Call Chains

### 1. Add Wiki-Links (processFile)

```

processFile
├── splitContent
├── getProviderForTask('addLinks')
├── call[Provider]API (callDeepSeekAPI, callOpenAIApi, etc.)
├── createConceptNotes
�?└── normalizeNameForFilePath
├── handleDuplicates
�?└── findDuplicates
├── cleanupLatexDelimiters
├── refineMermaidBlocks (AUTO-FIX)
└── saveOrMoveProcessedFile

```

### 2. Generate Content from Title (generateContentForTitle)

```

generateContentForTitle
├── \_performResearch (optional, if enabled)
�?├── SearchManager.getProvider
�?├── TavilyProvider.search / DuckDuckGoProvider.search
�?└── fetchContentFromUrl
├── getProviderForTask('generateTitle')
├── call[Provider]API
├── cleanupLatexDelimiters
├── refineMermaidBlocks
└── fixMermaidSyntaxInFile (AUTO-RUN after LLM)

```

### 3. Extract Concepts (extractConceptsFromFile)

```

extractConceptsFromFile
├── splitContent
├── getProviderForTask('extractConcepts')
├── call[Provider]API
└── Parse CONCEPT: lines from response

```

### 4. Create Concept Notes (createConceptNotes)

```

createConceptNotes
├── normalizeNameForFilePath
├── mkdir_p (if not exists)
├── write_file (for new notes)
└── append_file (for existing notes)

```

### 5. Research (\_performResearch)

```

\_performResearch
├── SearchManager.getProvider
�?├── TavilyProvider (if searchProvider='tavily')
�?└── DuckDuckGoProvider (if searchProvider='duckduckgo')
├── provider.search
├── fetchContentFromUrl (for DDG)
└── Combine research context

```

### 6. Extract Original Text (Q&A)

```

extractOriginalText
├── read_file(inputPath)
├── settings.extractQuestions (split by newline)
├── IF mergedMode
�?└── callLLM with all questions combined
└── ELSE
└── for each question, callLLM individually
└── save to output file

```

### 7. Summarize as Mermaid

```

summarizeToMermaidCommand
├── read_file(inputPath)
├── getProviderAndModelForTask('summarizeToMermaid')
├── call[Provider]API with summarizeToMermaid prompt
└── saveMermaidSummaryFile

```

### 8. Check Duplicate Concepts

```

checkAndRemoveDuplicateConceptNotes
├── Get concept folder files
├── Group by normalized name
├── Find potential duplicates
├── Show confirmation modal
└── Merge or remove duplicates

```

## Interdependency Diagram

```

main.ts
├── fileUtils.ts
�?├── promptUtils.ts (getSystemPrompt)
�?├── llmUtils.ts (call\*API functions)
�?├── mermaidProcessor.ts (refineMermaidBlocks)
�?└── searchUtils.ts (\_performResearch)
├── translate.ts
�?├── llmUtils.ts
�?└── promptUtils.ts
├── searchUtils.ts
�?├── llmUtils.ts
�?└── SearchManager �?TavilyProvider/DuckDuckGoProvider
├── formulaFixer.ts
├── mermaidProcessor.ts
└── utils.ts (splitContent, getProviderForTask)

````

## Settings Architecture

### NotemdSettings (types.ts)

- `providers`: Array of LLMProviderConfig
- `activeProvider`: Currently selected provider
- `useMultiModelSettings`: Enable per-task model selection
- Task-specific providers: `addLinksProvider`, `researchProvider`, `generateTitleProvider`, `translateProvider`, `summarizeToMermaidProvider`, `extractConceptsProvider`
- Task-specific models: `addLinksModel`, `researchModel`, etc.
- Batch settings: `batchConcurrency`, `batchSize`, `batchInterDelayMs`, `apiCallIntervalMs`
- Custom prompts: `useCustomPromptFor*`, `customPrompt*`

### Constants Default Parity (constants.ts)

The `constants.ts` file holds `DEFAULT_SETTINGS` which act as fallback configurations.
**CRITICAL**: When the AI agent interacts with the codebase, it must refer to these defaults to avoid hallucinating configuration states that violate the plugin's established norms.

## Error Handling

### Error Types (types.ts)

- `NotemdError`: Base error class
- `NetworkError`: API and network issues
- `FileOperationError`: File access issues
- `ValidationError`: Invalid input/configuration

### Persistent Error Logging (saveErrorLog)

The source project actively maintains an `error_processing_filename.log` file in the Vault root using the `saveErrorLog(app, reporter, error, settings)` utility.
**Agent Instruction**: When debugging failed batch processes or unexpected API errors, the AI agent MUST prioritize reading this log file to instantly access complete, persistent stack traces without waiting for user UI screenshots.

### Progress Reporting & UI Abstraction

- **Interface**: `ProgressReporter` (Bridges backend logic with frontend UI)
- **Implementations**:
  - `NotemdSidebarView` (Persistent sidebar tracking)
  - `ProgressModal` (Blocking modal for intensive tasks)
- **Methods**: `log()`, `updateStatus()`, `requestCancel()`, `clearDisplay()`
- **Properties**: `cancelled`, `abortController`, `activeTasks`

## Batch Processing

### Concurrency Control

```typescript
createConcurrentProcessor(concurrency, delayMs, progressReporter)
├── Promise.all with limit
├── Chunk array processing
└── Delay between batches
````

### Flow

```
batchGenerateContentForTitles
├── filter eligible files
├── create complete folder
├── chunkArray(files, batchSize)
└── processor(tasks) for each batch
```

### Generic Independent Interface

Create a platform-agnostic interface to ensure the logic isn't tied to any client runtime:

```typescript
interface FileSystemPort {
  read(path: string): Promise<string>;
  write(path: string, content: string): Promise<void>;
  exists(path: string): Promise<boolean>;
  listFiles(pattern: string): Promise<string[]>;
  createDir(path: string): Promise<void>;
}
```

By adhering to this, the `notemdpro` skills can be instantiated inside generic Python or Node.js Docker containers without requiring Obsidian to exist on the host machine.

### Platform-Specific Implementations
