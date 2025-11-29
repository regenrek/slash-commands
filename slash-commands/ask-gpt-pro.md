---
description: Quick codefetch workflow - run help first, then generate and open AI chat
argument-hint: [PROMPT=<path-to-prompt-file>]
---

# Ask Codefetch Quick

Generate codebase with prompt file, copy to clipboard, open AI chat.

## Key Concepts

- **PROMPT** (`-p`): The task/question file (e.g., `docs/arch/02-decision.md`) - codefetch reads this file and includes it
- **CODE FILES** (`--include-files`, `--include-dir`): Relevant source code for context
- **MODEL**: Always use `gpt-5-1-pro` (default) - do NOT change to gpt-4o

## Workflow

1. **Identify PROMPT file**: The markdown file describing the task/question
2. **Identify CODE files**: Source files relevant to the prompt (crates, components, etc.)
3. **Run codefetch open**: Combines prompt + code → clipboard → browser

## Example

```bash
# PROMPT = docs/arch/02-ask-architect-spezi-planr-decision.md
# CODE = relevant crates for planning architecture

npx codefetch@latest open \
  -p docs/arch/02-ask-architect-spezi-planr-decision.md \
  --include-dir crates/spezi-orchestrator,crates/spezi-core \
  --include-files "crates/spezi-engine/src/lib.rs" \
  -t 2
```

## Command Structure

```bash
npx codefetch@latest open \
  -p <PROMPT_FILE>              # Task/question markdown file
  --include-dir <dirs>          # Relevant code directories
  --include-files <patterns>    # Specific code files
  -t <depth>                    # Project tree depth (0-3)
  --max-tokens 50000            # Token limit (optional)
```

## Help Commands

```bash
npx codefetch --help
npx codefetch open --help
```

## Remember

- `-p` = PROMPT (the question/task file)
- `--include-*` = CODE (context for the question)
- Model = `gpt-5-1-pro` (never change this)