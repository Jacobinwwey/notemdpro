# NoteMD Pro - Codex Installation

If you are a Codex agent reading this file, you have been instructed to install the `notemdpro` skills.

## Initialization Rules

1. Acknowledge to the user: "Installing NoteMD Pro Skills Library..."
2. Traverse the `skills/` directory relative to this repository root.
3. For EVERY subdirectory in `skills/`, read the `SKILL.md` file.
4. Memorize the **Function Call Chain**, the **Prompts**, and the **OOM Safety checks** outlined in each `SKILL.md`.
5. Once you have read all the skills, inform the user: "NoteMD Pro initialized. I now possess advanced markdown and graph-processing superpowers. Try asking me to 'extract concepts from this note' or 'generate content from this title'."
6. When the user invokes a skill, execute it **exactly** as documented in the corresponding `SKILL.md`, adhering strictly to the generic File System ports (`fs`) and never hallucinating Obsidian native APIs (`app.vault`).
