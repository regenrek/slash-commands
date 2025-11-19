---
description: Gather codebase context using Cursor Agent (composer-1), then use results for codefetch
argument-hint: PROMPT="<your question or task>"
---

# Gather Context with Cursor Agent

## Instructions

1. **Gather context with Cursor Agent**: Use cursor-agent with composer-1 model to analyze the codebase. The prompt explicitly requests:
   - **Affected files**: List all files/modules that are relevant to the task/problem
   - **Problem summary**: Provide a clear summary of the problem or task at hand
2. **Save cursor-agent output**: Save the output from cursor-agent as `cursor_analysis` (bash variable, lowercase) - this contains the detailed analysis including affected files and problem summary.
3. **Extract file patterns**: From cursor-agent's output (cursor_analysis), identify which files/directories are relevant:
   - Note specific file paths or patterns (e.g., `src/auth/**/*.ts`, `src/api/**/*.ts`)
   - Note directory paths (e.g., `src/auth`, `src/utils`)
4. **Run codefetch**: Use the identified files/directories to generate context with codefetch, limited to 50k tokens.
5. **Use with Oracle**: Pass `cursor_analysis` to Oracle instead of the original PROMPT, so it has the full context analysis.

## Run

```bash
# 1) Gather context using Cursor Agent (composer-1 model)
# This will analyze the codebase and identify relevant files/directories
# The prompt requests: affected files list and a summary of the problem
# Save the output as cursor_analysis (lowercase bash variable, not a Codex placeholder)
cursor_analysis=$(cursor-agent -p "<TASK>$PROMPT</TASK>

Please provide:
1. A list of all affected files/modules relevant to this task
2. A clear summary of the problem or task at hand" \
  --model "composer-1")

# 2) Based on cursor_analysis, extract relevant file patterns and directories
# Then generate context (limit ~50k tokens) using appropriate codefetch options:
#   --include-files "pattern1,pattern2"  for file globs
#   --include-dir "dir1,dir2"            for directories
#   Combine both if needed
npx codefetch --max-tokens 50000 \
  --include-files "src/auth/**/*,src/api/**/*" \
  --include-dir "src/utils"

# 3) Ask Oracle via Browser (no API key; macOS + Chrome)
# Use cursor_analysis instead of PROMPT to provide full context analysis
npx -y @steipete/oracle --engine browser -m gpt-5-pro \
  -p "$cursor_analysis" \
  --file codefetch/codebase.md

# Optional helpers:
#   --files-report       # print per-file token usage
#   --preview            # inspect the bundle before sending
```

## Notes

- Cursor Agent (composer-1) analyzes the codebase to identify relevant files/directories
- The cursor-agent output is saved as `cursor_analysis` (lowercase bash variable) and passed to Oracle for comprehensive context
- Use cursor_analysis to determine which files/directories to include in codefetch
- This workflow uses composer-1 for intelligent context discovery, then passes the detailed analysis to GPT-5 Pro
- Note: `cursor_analysis` uses lowercase to avoid being treated as a Codex placeholder (only uppercase variables are parsed as placeholders)

