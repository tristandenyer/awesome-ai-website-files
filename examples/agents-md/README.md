# `AGENTS.md` examples

Real-world `AGENTS.md` patterns. Used by Claude Code, Cursor, Codex, Copilot, Gemini CLI, Jules, Amp, Factory, Zed.

## What's here

| File | Notes |
|---|---|
| [`AGENTS.md`](AGENTS.md) | Composite example demonstrating GitHub's 2,500-repo best-practice patterns |

## Patterns to copy

Per [GitHub's analysis of 2,500+ repos](https://github.blog/ai-and-ml/github-copilot/how-to-write-a-great-agents-md-lessons-from-over-2500-repositories/), the highest-impact patterns are:

1. **Commands first.** Build, test, lint, run, all at the very top of the file
2. **Three-tier rules:** Always do / Ask first / Never do
3. **Real code examples**, not abstract description ("we use single quotes" → paste a 10-line snippet showing what good code looks like)
4. **Explicit boundaries:** files agents must never touch
5. **PR conventions:** title format, required checks, merge style
6. **Under ~150 lines.** This file is read on every task; token budget matters.

## Public examples to study

These open-source projects publish strong AGENTS.md files worth reading:

- [openai/codex](https://github.com/openai/codex/blob/main/AGENTS.md): from the team that helped define the standard
- [microsoft/vscode](https://github.com/microsoft/vscode): large enterprise codebase
- [anthropics/claude-code](https://github.com/anthropics/claude-code): meta-example from Claude Code itself
- The [agents.md spec repo](https://github.com/agents-md/agents.md): reference implementation

## Adding more examples

PRs welcome. Copy real production files (with attribution + permission if from private repos), add a header comment with the source, and add a row to the table above.

For composite/illustrative examples (like the one in this folder), make it clear in the header that it's not from a single real repo.

See [CONTRIBUTING.md](../../CONTRIBUTING.md) for full rules.
