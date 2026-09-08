# Observer plugin

A **thin adapter**. Everything it can do, the Observer MCP server can do —
this only packages the commands and the automatic triggers for Claude Code.

If you use Cursor, Codex, Copilot, Windsurf, Zed or n8n you don't need it.
Install the MCP server directly and you get the same tools with the same
permissions:

```bash
curl -fsSL https://os.observer.studio/install/mcp.sh | sh -s -- obs_live_YOUR_TOKEN
```

That split is deliberate: the capability lives in the server so it is
harness-agnostic, and only the ergonomics live here.

---

## Install

**No token to paste.** The MCP server is its own OAuth 2.1 authorization
server with dynamic client registration, so Claude Code registers itself,
opens your browser, you sign in with your normal account, pick a workspace
and approve a scope list. The grant is per-person and revocable from
**Settings → API tokens**.

> The marketplace is named **`observer-os`**, not `observer` — this
> account already has an `observer` marketplace pointing at
> `Observerstudio/claude-plugins`, and two marketplaces cannot share a
> name. Hence `observer@observer-os`.

**1. Add the marketplace** — from GitHub:

```bash
claude plugin marketplace add Observerstudio/observer-os
```

…or from a local clone (either form works; in-session use `/plugin
marketplace add` instead):

```bash
claude plugin marketplace add /path/to/observer-os
```

**2. Install:**

```bash
claude plugin install observer@observer-os
```

**3. Sign in.** Start Claude Code and run `/mcp`. `observer` will be listed
as needing authentication — authenticate it and a browser opens. Approve
the scopes you want the agent to hold:

| Scope | Lets the agent |
| --- | --- |
| `tasks:read` | list your tasks, read your standup |
| `tasks:write` | file tasks, update status, comment, add subtasks |
| `projects:read` | work out which project a folder is |
| `time:read` | see hours on your standup |
| `time:write` | report time (still reviewed before it lands) |

`tasks:*` + `projects:read` is the useful minimum. `time:write` is separate
on purpose — time feeds the timesheet, payroll and project margin.

**4. Check it.** In a repo linked to an Observer project, run
`/observer:tasks`. If it lists your real work, you're done.

### Just trying it out?

Skip installing and load the plugin directly:

```bash
claude --plugin-dir /path/to/observer-os/plugins/observer
```

### Static token instead

The git hook (below) is a shell script, not an MCP client, so it cannot do
OAuth — it needs a token. Mint one at **Settings → API tokens** and export
it:

```bash
export OBSERVER_TOKEN=obs_live_…
export OBSERVER_URL=https://os.observer.studio   # optional; the default
```

The same token also works for any MCP client you would rather configure by
hand:

```bash
curl -fsSL https://os.observer.studio/install/mcp.sh | sh -s -- obs_live_YOUR_TOKEN
```

---

## Commands

| Command | What it does |
| --- | --- |
| `/observer:tasks` | what's assigned to you, narrowed to this folder's project |
| `/observer:task` | file a task (or a subtask) |
| `/observer:report` | report a unit of work — status, comment, subtasks, time |
| `/observer:standup` | your standup, from real tasks and tracked time |
| `/observer:timer` | start · stop · status |
| `/observer:recap` | recap the conversation into tasks, milestones and the sprint — proposes, then writes on a yes |
| `/observer:link` | link this repo to a project, so the plugin knows where it is |

---

## It knows where it is

At session start the plugin reads your git remote and tells the agent which
Observer project this folder is, and which task the branch refers to.

That resolution is **exact**: `ProjectRepository.fullName` stores
`owner/name` for every repo linked in a project's Code tab, so the folder
either maps to a project or it doesn't. It deliberately does *not* match a
directory name against project names — that guess succeeds confidently, and
its failure mode is filing work into the wrong client's project.

Link a repo from the terminal with `/observer:link`, or in the web app under
**Project → Code → Link repository**. Until then, commands fall back to
asking.

---

## The automatic part

`bin/observer-report` is a **git hook**, and that is the agnostic choice: a
commit happens whichever agent — or person — made it, so one hook covers
every tool anyone on the team uses. Install it per repo:

```bash
plugins/observer/bin/observer-report install     # → .git/hooks/post-commit
plugins/observer/bin/observer-report uninstall
```

It appends to an existing `post-commit` rather than replacing it, and
**never fails a commit** — a reporting tool that can block `git commit`
gets uninstalled by the first person it inconveniences, so every failure
path warns and exits 0.

It reads the branch for a task key (`feat/OBS-123-nav-filter`) and posts the
commit as a comment. It **comments only**: a commit is evidence work
happened, not evidence a task is done.

---

## What lands, and what waits

| | |
| --- | --- |
| Status, comments, subtasks, due date, estimate | **applied immediately** |
| Time | **filed for review** |

Time waits because hours count toward pay and project figures before
anyone approves them. So reported time lands in Observer's review queue
first, and approving it creates a normal timesheet entry attributed to
whoever did the work.

---

## Limitations, stated plainly

- **A branch with no task key reports nothing.** Not a guess — wrong data
  that looks right is worse than no data. `feat/OBS-123-nav` works;
  `feat/nav` doesn't. Tasks created before keys existed need the backfill
  (`pnpm --filter @observer/web backfill:task-keys --apply`).
- **Per-prompt logging is not offered.** It would flood the task list
  faster than anyone reads it, and prompt text routinely contains
  client-confidential detail — this workspace has a client portal attached.
  The unit is a commit, a status move, or a finished task.
- **No team data.** An agent reads *your* tasks, *your* standup, *your*
  hours. Reading the team's day — including their hours — is not exposed.
