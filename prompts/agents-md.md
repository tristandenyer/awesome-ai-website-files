# Prompt: `AGENTS.md`

**File:** `/AGENTS.md` (repo root) · **Status:** ✅ Adopted · **Time to ship:** 30 min

The standard that won. 60,000+ repos. Read by Claude Code, Cursor, GitHub Copilot, OpenAI Codex, Gemini CLI, Jules, Amp, Factory, Zed.

---

## Copy this prompt

```
Generate an AGENTS.md for my repository following https://agents.md/.

Project: [DESCRIBE: what does it do, who's it for?]
Primary language(s) and framework(s): [e.g., "TypeScript + Next.js 15"]
Package manager: [npm / pnpm / yarn / pip / poetry / bundler]

Commands:
- Install dependencies: [command]
- Run tests: [command]
- Run a single test file: [command, if different]
- Lint / format: [command]
- Dev server: [command]
- Build for production: [command]

Code style: [DESCRIBE or paste examples, e.g., "TypeScript strict mode, 
single quotes, no semicolons, 100-char line limit"]

Files agents must NEVER modify: [e.g., "secrets/, migrations/, vendor/, 
generated/*"]

PR / commit conventions: [DESCRIBE: title format, required checks, etc.]

Domain gotchas, non-obvious patterns, vocabulary: [DESCRIBE anything weird]

Format:
- "Core Commands" section at the top with executable commands
- Three-tier rules: "Always do" / "Ask first" / "Never do"
- At least one real code example showing my style (not abstract description)
- Under ~150 lines (this gets read on every task; token budget matters)
- Plain Markdown, no required frontmatter

Return as one code block.
```

## Heads up

- **Commands first.** [GitHub's analysis of 2,500+ repos](https://github.blog/ai-and-ml/github-copilot/how-to-write-a-great-agents-md-lessons-from-over-2500-repositories/) found this is the highest-impact section. Put exact build/test/lint commands at the top.
- **Examples beat descriptions.** "Use single quotes, 2-space indentation" is weak. Pasting a real 10-line snippet from your codebase is strong.
- **Nested AGENTS.md works.** Monorepo? Drop one in each package directory; the agent reads the closest one.
- **AGENTS.override.md** lets you override inherited instructions in subdirectories.

## Verify it's working

Open your repo in Cursor or run Claude Code in it. Ask it to make a small change. Watch whether it follows your code style and uses your test commands. If it doesn't, your AGENTS.md isn't specific enough; add more concrete examples.

## Migrating from older files

If you have any of these, AGENTS.md replaces them:

- `CLAUDE.md` → keep as a symlink or merge into `AGENTS.md`
- `.cursorrules` → migrate content to `AGENTS.md`
- `.windsurfrules` → migrate content to `AGENTS.md`
- `.github/copilot-instructions.md` → keep both, or symlink

Many tools still read the old files for backwards compatibility, but new tools assume AGENTS.md.

## See also

- [Real examples in production repos](../examples/agents-md/)
- [Spec: agents.md](https://agents.md/)
- [GitHub's writeup on what makes a good AGENTS.md](https://github.blog/ai-and-ml/github-copilot/how-to-write-a-great-agents-md-lessons-from-over-2500-repositories/)
