# The board that writes itself

You already spend all day talking to a coding agent. You describe a bug; it edits three files and writes a test. You ask for a feature; it touches the router, the component, the migration. Reading and writing files in your repo is the whole job. That is the insight everything here rests on.

Every project tracker asks you to do a second, redundant thing: tell it what you just did. You finish the code, then you go somewhere else — a tab, an app, a CLI — and re-describe the work you already described to your agent. The tracker is a second surface, and a second surface has to be kept in sync with the first one, the code. Sync is manual, so sync rots. Give it two weeks and the board is fiction: half-closed tickets, a "doing" column no one is doing, a status you reconstruct by asking people. The board stops being the truth and becomes a thing you reconcile against the truth.

So here is a question worth sitting with. If you want a board that never drifts from the code, where is the cheapest *correct* place to keep it? Not a SaaS with its own login. Not a database you migrate. Not a CLI you have to remember to run, or an MCP server that has to be up. The cheapest correct home is a file the agent is already editing.

Boards deletes the second surface.

## The board is a file, and the agent already edits files

A board is a plain markdown file in your repo — `PROJECT.md` for the product, `MARKETING.md` for the funnel, whatever stream of work you want. It is the single source of truth, and it lives next to the code it describes. Items are lines: `## Section` headings, then `- [ ] **T-01 · Title** — body`. Status is the checkbox glyph — `[ ]` to-do, `[~]` doing, `[x]` done. Ids are stable and never renumbered.

You never open the file to groom it. You talk to your agent the way you already do — "add a bug for the double-charge on retries," "what's left before launch?," "move T-12 to done" — and because it is editing the repo anyway, it edits the board as a byproduct, the same way it edits a component. The board update lands in the **same commit** as the code that caused it. Not a follow-up commit, not a nightly sync job — the same diff.

That single property is the whole argument. If the board changes atomically with the code, it cannot drift, because there is no window in which the two disagree. `git log -- PROJECT.md` is a real, blame-able history: who moved what, when, and alongside which change. The file is the database. Git is the history. There is no third thing to trust.

To *see* it, a ~70-line, zero-dependency parser renders the file read-only inside your own app's admin panel — no markdown library, everything printed as text (no `dangerouslySetInnerHTML`), so there is no XSS surface and no supply chain to inherit. It runs in local dev only: the API route 404s in production and the board files are never bundled into a deploy. The admin is a renderer, nothing more. The board does not need a server to be true; it is already true in the file.

## What is actually novel here (and what isn't)

Let me be honest, because overclaiming is the fastest way to lose a developer's trust: "your tasks update as you code" is not, by itself, a new idea. Good people are working the same seam, and some of their tools are genuinely better than a convention for what they are built to do.

- **Backlog.md** is a git-native task CLI for agents. Real, popular, well made. But it is a tool you *invoke* — its own docs tell you to "use the backlog CLI" and note there is "no automatic synchronization." A different bet.
- **claude-todos** is a self-managing todo app, but a FastAPI/JSON backend that spawns `claude -p` sessions through hooks. Capable, and heavier — a service that lives beside your code.
- **CodeAgentSwarm** exposes a real kanban board to agents over MCP. A separate app.
- **claude-task-viewer** gives you a live read-only board too — but of ephemeral, in-session task state under `~/.claude/`, not a durable thing you commit.

Take them seriously. If you want a hosted product with a proper write-UI and a swarm of agents pushing cards around, one of those is the right call. The only thing Boards can honestly claim as unclaimed is the *conjunction*: a convention instead of a tool, with **zero runtime dependency**, where the board diff is **atomic with the code**, rendered read-only **inside the admin of the app you already own**. Each piece exists somewhere. Putting all four together — and refusing to add a fifth moving part — is the whole design.

And it is a trade-off, not a free win. You give up the hosted write-UI, the app you open on your phone to drag a card, the non-technical stakeholder editing status directly. In exchange, the board doesn't drift, owes nothing to a third party, and disappears when you `rm` a file. If that exchange sounds bad, one of the tools above is your answer. If you want the board to *be* your repo, this is.

## The irony I owe you

Yes: we are shipping a "stop adding tools" philosophy *as an installable skill*, via `npx skills add`. I know how that reads.

Here is the distinction that makes it honest. Installing Boards does not start a process. Nothing runs in the background, nothing phones home, nothing sits between you and your repo. The skill drops *knowledge and a few files you now own* into `.claude/skills/` — and the equivalent paths for 70-plus other agents — the format contract, the conventions, and the tiny renderer you can read in one sitting. It teaches your agent a habit and gets out of the way. Uninstall it afterward and your boards keep working, because they are just markdown in your git history. That is the test of a convention over a dependency: take the tool away, and nothing breaks.

We run our own roadmap this way. It is how Boards itself got built.

If your board is a lie two weeks after you write it, the problem is the second surface. Move the board to where your attention already goes — the files your agent edits all day — and the cost of keeping it honest drops to zero.

```
npx skills add reindent/boards
```

Read the renderer. Steal the format. Then just keep prompting.

— Reindent
