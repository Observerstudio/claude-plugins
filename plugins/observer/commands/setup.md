---
description: Check your Observer setup and say exactly what's missing
---

Report what is and isn't working, then stop. **Change nothing** — this
command diagnoses; the user decides what to fix.

## 1. Local facts

Run `"${CLAUDE_PLUGIN_ROOT}"/bin/observer-doctor`. It prints `key=value`
lines: whether this is a git repo, its remote and branch, whether the
branch carries a task key, whether the commit hook is installed, and
whether `OBSERVER_TOKEN` is set.

## 2. Is the connection live?

Call `list_projects`. That is the cheapest authenticated read, so it
answers three things at once:

- **It errors with an auth problem** → not signed in. Tell them to run
  `/mcp`, pick `observer`, and authenticate — a browser opens and they sign
  in normally. There is no token to paste.
- **It errors about a scope** → signed in, but the grant is missing that
  scope. Name the scope and say they can re-authenticate to add it.
- **It returns projects** → connected. Say so and move on.

## 3. Is this folder a project?

Only if `git_remote` is not `none`. Call `project_for_repo` with that
remote.

- **One project** → say which. This is the good case.
- **Several** → say so; the commands will ask each time. Not a problem.
- **None** → the repo isn't linked. Tell them: **Project → Code → Link
  repository** in the web app. Be clear this is the difference between the
  plugin knowing where it is and asking every time.

## 4. Do tasks have short keys?

Call `list_tasks` (narrowed to the project from step 3 if there is one) and
look at whether the returned tasks carry a `key`.

- **They do** → branch naming works. Show one real example using an actual
  key from the result, e.g. `git checkout -b feat/OBS-123-nav-filter`.
- **They don't** → say the backfill hasn't run, and that until it does a
  branch name can't identify a task. This is an admin action, not something
  they can fix.

## 5. The commit hook

From step 1's `commit_hook` and `observer_token`:

- **Installed and a token is present** → working.
- **Installed but no token** → it is silently doing nothing on every
  commit. Say that plainly; it is the one failure here that looks like
  success.
- **Not installed** → say it is optional and what it adds: reporting
  commits made outside an agent session. Give the command
  (`plugins/observer/bin/observer-report install`) and mention it needs
  `OBSERVER_TOKEN` because a shell script cannot do the browser sign-in the
  rest of the plugin uses.

## Then report

One short block. Lead with what works, then what doesn't, then the single
next action if there is one. Concretely:

- Don't print the raw `key=value` output — read it and say what it means.
- Don't list a step as a problem when it isn't. A repo with no linked
  project on a machine that is otherwise fine is one line, not a section.
- If everything works, say so in a sentence. Don't manufacture advice.
- Never echo the token, present or not.
