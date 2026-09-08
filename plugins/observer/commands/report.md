---
description: Report what you just did on a task — status, a comment, subtasks, time
---

Report the current unit of work to Observer OS using the `report_work` MCP tool.

Work out the task first:
1. If the user named a task, use it — `report_work` takes a short key
   (`OBS-123`) or a UUID, so pass whichever you have.
2. Otherwise read the branch name for a key. `feat/OBS-123-nav-filter`
   resolves to `OBS-123` and needs no lookup.
3. Failing that, read it for a GitHub issue number and resolve with
   `list_github_links` — the fallback for branches cut before keys existed.
4. If none of those gives a task, **ask**. Do not guess: reporting against
   the wrong task is worse than reporting nothing, because it is wrong data
   that looks right and nobody goes looking for it.

Then call `report_work` once with everything you know:
- `comment` — what changed and why. This is the part a teammate reads, so
  write it for them, not as a changelog line.
- `status` — only if the work genuinely moved. A commit is not evidence a
  task is done.
- `subtasks` — only for work you actually discovered and did not do.
- `minutes` — only if you can account for the time honestly.

Report what came back verbatim, including anything in `skipped`. If time was
filed for review, say so and say where: it becomes a `LOG_TIME` action in
Observer's review queue, and it does not reach the timesheet until someone
approves it.
