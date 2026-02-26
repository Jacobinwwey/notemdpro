# NoteMD Pro: Dummy Test Object

This file is a sandbox for the AI Agent to test the `notemdpro` skills library safely before modifying the user's real vault.

## Objective

The agent should try its `mermaid-healer`, `concept-extractor`, or `formula-healer` skills on this file.

## 1. Messy Content (Test 1: Concept Extraction)

The quick brown fox jumps over the lazy dog. But the real core concept here is Agentic Software Engineering and Test-Driven Development (TDD).

## 2. Broken Mermaid (Test 2: Healer)

```mermaid
graph TD;
    A[Start] --> B(Middle);
    B -> C[End]
```

_(Notice the missing arrow dash on B -> C)_

## 3. Inline Math vs Block Math (Test 3: Formula Healer)

This is an inline formula \(\sqrt{x^2 + y^2}\) that needs fixing.
This is a block formula:
\[
E = mc^2
\]

## 4. OOM Bomb (Test 4: Batch Processor OOM Safety)

_(The AI should strip or ignore the following before token chunking)_
![[data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVR42mP8z8BQDwAEhQGAhKmMIQAAAABJRU5ErkJggg==]]
