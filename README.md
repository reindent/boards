# BOARDS.md

**The project board that writes itself while you build.**

Boards is a *convention* for tracking work with a coding agent (Claude Code,
Cursor, Codex, …). A board is a plain markdown file — `PROJECT.md`,
`MARKETING.md` — that is the single source of truth for a stream of work. Your
agent already reads and edits files while it builds, so it keeps the board
current in the **same commit** as the code. A ~70-line, dependency-free parser
renders the file inside your own app's admin — or as a standalone page, in
whatever environment you choose.

There is no CLI to run, no MCP server, no database, no SaaS. The file is the
database, git is the history, the admin is a renderer.

## Install

```
npx skills add reindent/boards
```

This installs the `boards` skill into your agent (`.claude/skills/`,
`.agents/skills/`, …). Then just talk to your agent:

> "Set up boards in this project."
> "Add a bug: the export button 500s on empty state."
> "What's left before launch?"
> "Move T-12 to done."

The board file changes as a byproduct — in the same commits as your code.

## Why a convention instead of a tool

Great tools already track work for AI agents — [Backlog.md](https://github.com/MrLesk/Backlog.md)
(a git-native CLI), [claude-todos](https://github.com/eranhirs/claude-todos) (a
self-managing todo backend beside your code), CodeAgentSwarm (a board over MCP). Each works, and
each is a separate moving part to invoke or operate. Backlog.md keeps its board
in committed markdown too — the difference is you drive it through its CLI ("no
automatic synchronization"), rather than the agent updating the file in the same
commit as a byproduct.

Boards takes the opposite bet: the agent already edits files, so the cheapest
correct home for the board is a markdown file it edits in the same commit. Zero
dependency, same-commit truth, rendered inside the admin you already have. If you
want a hosted product with a write-UI, use one of the tools above. If you want
the board to *be* your repo, use this.

## What you get

- `skills/boards/SKILL.md` — the skill (the convention + one-time setup + the
  daily loop).
- `skills/boards/reference/` — worked examples you own outright: the parser
  (`parse.ts` + tests), a view (`Board.tsx`), API routes (Express + Next.js), a
  self-contained `standalone.html` renderer, a starter board template, and the
  `CLAUDE.md` snippet that makes the board self-update. The examples are
  TypeScript/React because that's where they were extracted from — the
  convention is **stack-agnostic**, and each piece ports to any framework or
  language in minutes. Nothing runs in the background.
- `skills/boards/commands/boards.md` — an optional `/boards` command to show the
  board on demand.
- `skills/boards/examples/PROJECT.md` — a filled-in example board.

## Read the story

https://boards.reindent.com

---

From the team at [Reindent](https://reindent.com) — we harden AI-built apps for
production. Boards is how we run our own roadmap. MIT licensed.
