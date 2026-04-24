# Claude Commands

Custom slash commands for [Claude Code](https://docs.anthropic.com/en/docs/claude-code).

## What's included

### `/code-review`

Runs a senior-developer-level code review on the current branch. Reads every changed file and evaluates it across 12 dimensions: correctness, readability, error handling, scalability, consistency, tests, stale references, unnecessary complexity, boundaries, security, debuggability, and cross-module duplication.

If the project has a `.claude/CLAUDE.md` with coding standards, findings are checked against those. Otherwise it uses widely accepted practices for the language and framework.

Findings are grouped by severity (critical, high, medium, low) with file paths and line numbers.

## Installation

Claude Code loads custom commands from `~/.claude/commands/`. Copy the files there:

```bash
cp commands/* ~/.claude/commands/
```

Then in any Claude Code session, type `/code-review` to run it.

## Updating

Pull the latest and copy again:

```bash
git pull
cp commands/* ~/.claude/commands/
```
