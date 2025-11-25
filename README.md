# Slash Commands Collection

<div align="center">
  <img src="public/banner.png" alt="Slash Commands" width="100%" />
</div>

A curated collection of AI-powered slash commands for code review, problem analysis, and refactoring workflows.

## Quick Reference

📝 `/create-plan` - Create comprehensive task plans  
📋 `/plan-review` - Review implementation plans  
✅ `/finalize-plan` - Finalize completed tasks  
🔍 `/code-review-low` - Fast code review  
🔬 `/code-review-high` - Thorough code review  
🐛 `/problem-analyzer` - Identify bugs and affected files  
♻️ `/refactor-code` - Start refactoring workflows  
🔌 `/kill-port` - Kill processes running on specific ports  
🔎 `/research-better-lib` - Find modern, faster library alternatives  
🤖 `/ask-gpt-pro-browser` - Ask GPT-5 Pro via browser (no API key)  
🌐 `/ask-gpt-pro-api` - Ask GPT-5 Pro via API  
🎬 `/ask-pro-directors-cut` - Use Cursor Agent + codefetch + GPT-5 Pro  
🔮 `/ask-oracle` - Use Oracle CLI to copy code bundle for debugging  
⚛️ `/solidjs-rules` - Comprehensive SolidJS coding rules  
🧹 `/component-cleanup` - SolidJS component size and structure rules  

## Installation

### Cursor
Copy the `.md` files from the `slash-commands/` directory into `.cursor/commands/` directory. Remove the `$1` parameters and XML wrapping tags when using in Cursor.

**Official docs**: [Cursor Slash Commands](https://cursor.com/docs/cli/reference/slash-commands)

### Codex
Copy the `.md` files from the `slash-commands/` directory into `~/.codex/prompts/` directory. Restart Codex after installation for changes to take effect.

**Official docs**: [Codex Prompts](https://github.com/openai/codex/blob/main/docs/prompts.md)

### Claude Code
Claude Code supports both project-specific and personal commands:

**Project commands** (shared with team):
```bash
mkdir -p .claude/commands
# Copy the .md files to .claude/commands/
```

**Personal commands** (available across all projects):
```bash
mkdir -p ~/.claude/commands
# Copy the .md files to ~/.claude/commands/
```

**Official docs**: [Claude Code Slash Commands](https://docs.claude.com/en/docs/claude-code/slash-commands)

## Commands

### 1. `create-plan`
Creates a comprehensive task plan with structured sections for context, success criteria, deliverables, approach, risks, and testing. Ensures plans follow best practices and include all necessary information before implementation.

**Features**:
- Structured plan template with all required sections
- Context and problem statement
- Measurable success criteria
- Clear deliverables and approach
- Risk assessment and testing strategy
- Cross-references to AGENTS.md for rules/constraints

**Usage**: `/create-plan TASK="<task-description-or-goal>"`

**Examples**:
```
/prompts:create-plan TASK="Implement user authentication system"

/prompts:create-plan TASK="Refactor the payment processing module"
```

### 2. `plan-review`
Reviews implementation plans and provides a go/no-go decision for development. Evaluates:
- Codebase alignment and current patterns
- Scope clarity and completeness
- Performance, security, and privacy impact
- Code smells and potential caveats

**Usage**: `/plan-review "<your-plan>"`

**Examples**:
```
/plan-review "We're planning to implement plan @task-x-new-cool-feature.md"

/plan-review "Here is the plan we want to implement: @my-plan.md"
```

### 3. `finalize-plan`
Finalizes a completed task by creating two artifacts: a finished plan file and a lightweight summary for fast AI ingestion. Maintains traceability while keeping summaries token-friendly.

**Features**:
- Moves task to finished directory with status
- Creates lightweight summary file (SOW - Summary of Work)
- Cross-references between plan and summary
- Tracks affected files, tests, and decisions
- Token-optimized for AI consumption

**Usage**: `/finalize-plan TASK_FILE="<path-to-task.md>"`

**Examples**:
```
/prompts:finalize-plan TASK_FILE="docs/tasks/todo/01-user-auth.md"

/prompts:finalize-plan TASK_FILE="docs/tasks/todo/05-payment-refactor.md"
```

### 4. `code-review-low` & `code-review-high`
Comprehensive code review commands for post-implementation analysis.

**`code-review-low`**: Fast review focusing on critical issues
- Code smells and security vulnerabilities
- Performance bottlenecks
- Essential test coverage

**`code-review-high`**: Thorough review with extended analysis
- All `code-review-low` checks plus additional validations
- Deeper security and performance analysis
- Comprehensive test recommendations

**Usage**: `/code-review-low "<files/code/description to review>"`

**Examples**:

```
/code-review-low "my plan @task-x-cool-feature has been implemented check unstaged files for review"

/code-review-low "@auth.server.ts @auth.rs @auth.connector.ts"
```

### 5. `problem-analyzer`
Identifies bugs and maps affected files across the codebase. Provides:
- Root cause analysis
- File impact assessment
- Minimal safe fix proposals
- Documentation gap identification

**Usage**: `/problem-analyzer "<problem-description>"`

**Examples:**
The Problem analyzer prompt works best when you add logs and a short problem description.

I ran this often with cheeta model to get a quick analysis and move over to gpt codex.

```
/problem-analyzer "the app won't compile. here are the logs <paste-logs>"

/problem-analyzer "we have a serious blocking bug in @user-form.ts and @user.service.ts"
```

### 6. `refactor-code`
Initiates refactoring workflows with clear scope and isolation:
- Feature-specific refactoring
- Commit isolation guidelines
- Documentation of unrelated issues (without fixing)
- No backward compatibility or feature flags

**Usage**: `/refactor-code "<refactoring-goal>"`

```
/refactor-code "here is our implementation plan @impl-plan lets start"
```

### 7. `kill-port`
Kills processes running on specified ports with OS-specific commands and safety checks:
- Cross-platform support (macOS, Linux, Windows)
- Process identification and verification
- Safety warnings for system processes
- Graceful shutdown recommendations

**Usage**: `/kill-port "<port-number(s)>"`

**Examples**:
```
/kill-port "3000"

/kill-port "3000 8080 9000"
```

### 8. `research-better-lib`
A comprehensive template for evaluating alternatives to baseline libraries. Helps find modern, faster JavaScript/TypeScript libraries that outperform existing solutions in latency and bundle size while maintaining or improving relevance/quality.

**Features**:
- Structured problem statement and success metrics
- Search query templates for benchmarking
- Candidate library shortlist evaluation
- Minimal benchmark design guidelines
- Decision rubric and deliverables checklist

**Usage**: `/research-better-lib "<baseline-library> <domain/use-case> <candidate-libraries>"`

**Examples**:
```
/research-better-lib "Fuse.js fuzzy search fuzzysort quick-score FlexSearch MiniSearch"

/research-better-lib "lodash utility functions ramda remeda"
```

### 9. `ask-gpt-pro-browser` & `ask-gpt-pro-api`
Query GPT-5 Pro with codebase context using codefetch. These commands help you ask complex questions about your codebase with full context.

**`ask-gpt-pro-browser`**: Uses browser automation (no API key required, macOS + Chrome)
- Research codebase to identify relevant files/directories
- Generate context with codefetch (50k token limit)
- Query GPT-5 Pro via browser engine

**`ask-gpt-pro-api`**: Uses OpenAI API (requires OPENAI_API_KEY)
- Same workflow as browser version
- Requires API key for authentication

**Usage**: `/ask-gpt-pro-browser PROMPT="<your-question>"` or `/ask-gpt-pro-api PROMPT="<your-question>"`

**Examples**:
```
/prompts:ask-gpt-pro-browser PROMPT="Review the authentication logic for security issues"

/prompts:ask-gpt-pro-api PROMPT="Explain how the caching layer works"
```

### 10. `ask-pro-directors-cut`
Advanced workflow that combines Cursor Agent (composer-1), codefetch, and GPT-5 Pro for comprehensive codebase analysis. Uses intelligent context discovery before querying GPT-5 Pro.

**Workflow**:
1. Cursor Agent analyzes codebase and identifies affected files
2. Extract file patterns from analysis
3. Generate context with codefetch using identified files
4. Query GPT-5 Pro with detailed analysis

**Features**:
- Intelligent file discovery via Cursor Agent
- Detailed problem summary and affected files
- Full context passed to GPT-5 Pro
- No API key required (browser engine)

**Usage**: `/ask-pro-directors-cut PROMPT="<your-question>"`

**Examples**:
```
/prompts:ask-pro-directors-cut PROMPT="Analyze the performance bottlenecks in the authentication system"

/prompts:ask-pro-directors-cut PROMPT="Review the error handling patterns across the codebase"
```

### 11. `ask-oracle`
Use Oracle CLI to copy code bundle for debugging in ChatGPT. Helps gather relevant files for debug sessions including error locations, stack traces, and recently edited files.

**Features**:
- Copy code bundle to clipboard for pasting into ChatGPT
- Support for multiple file patterns and globs
- Include test files and related components/utils
- Render and copy bundle in one command

**Usage**: `/ask-oracle [PROMPT="<debug-message>"] [FILES="<file-patterns>"]`

**Examples**:
```
/prompts:ask-oracle PROMPT="Debug authentication error" FILES="src/auth/**/*.ts,src/**/*.test.ts"

/prompts:ask-oracle FILES="src/components/Button.tsx,src/utils/helpers.ts"
```

### 12. `solidjs-rules`
Comprehensive SolidJS coding rules and best practices covering reactivity, control flow, async patterns, props, styling, file layout, and tooling.

**Features**:
- Reactivity basics and signal usage
- Control flow helpers (Show, For, Switch)
- Async and side effect patterns
- Component API and prop guidelines
- Styling conventions
- File and folder organization
- ESLint configuration examples

**Usage**: `/solidjs-rules CODEFILES="<code-files-to-review>"`

**Examples**:
```
/prompts:solidjs-rules CODEFILES="@Button.tsx @TodoList.tsx"

/prompts:solidjs-rules CODEFILES="src/components/**/*.tsx"
```

### 13. `component-cleanup`
SolidJS component size and structure rules for code cleanup. Helps maintain clean, maintainable components by enforcing size limits and structure guidelines.

**Features**:
- Component size guidelines (150-250 LOC target, 600 LOC hard cap)
- File naming conventions
- Component splitting strategies
- Logic extraction to hooks/utilities
- JSX nesting limits

**Usage**: `/component-cleanup CODEFILES="<component-files-to-review>"`

**Examples**:
```
/prompts:component-cleanup CODEFILES="@UserProfile.tsx"

/prompts:component-cleanup CODEFILES="src/components/**/*.tsx"
```

## Workflow Tips

- Use `create-plan` at the start of new tasks to ensure comprehensive planning
- Use `plan-review` before starting complex implementations
- Use `finalize-plan` when tasks are completed to maintain organized documentation
- Use `code-review-low` after implementing features for quick validation
- Use `code-review-high` for major changes or before releases
- Combine `problem-analyzer` with debugging sessions for faster issue resolution
- Use `ask-pro-directors-cut` for deep codebase analysis with intelligent context gathering
- Use `ask-gpt-pro-browser` for quick questions without API keys (macOS + Chrome)
- Use `ask-gpt-pro-api` when you need API-based access to GPT-5 Pro
- Use `ask-oracle` to quickly gather code bundles for debugging sessions
- Use `solidjs-rules` and `component-cleanup` when working with SolidJS codebases

## Links

- **X/Twitter**: [@kregenrek](https://x.com/kregenrek)
- **Bluesky**: [@kevinkern.dev](https://bsky.app/profile/kevinkern.dev)
- **Website**: Learn to build software with AI: [instructa.ai](https://www.instructa.ai)

## My other projects
* OpenAI Codex Tools [codex-1up](https://github.com/regenrek/codex-1up)
* [AI Prompts](https://github.com/instructa/ai-prompts/blob/main/README.md) - Curated AI Prompts for Cursor AI, Cline, Windsurf and Github Copilot
* [codefetch](https://github.com/regenrek/codefetch) - Turn code into Markdown for LLMs with one simple terminal command
* [aidex](https://github.com/regenrek/aidex) A CLI tool that provides detailed information about AI language models, helping developers choose the right model for their needs.# tool-starter