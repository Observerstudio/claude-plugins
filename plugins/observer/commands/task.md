---
description: File a task in Observer OS
---

Create a task with `create_task`.

Resolve the project first, in this order:

1. **`project_for_repo`** with `git remote get-url origin` — this folder's
   project, resolved exactly from the repo link rather than guessed from a
   name. If it returns one project, use it.
2. If it returns **several** (a monorepo backing more than one project),
   ask which.
3. If it returns **none**, fall back to `list_projects` and ask.

Never invent a project id, and never match a project by how much its name
looks like the folder's. That guess succeeds confidently and files work
into the wrong client's project.

Write the title as the outcome, not the activity: "Nav filter respects role"
rather than "Fix nav". Put context in `description`.

For a subtask, pass `parentTaskId`. A subtask is a real task with the full
surface — assignee, status, its own time — so use one when the work is
genuinely separable, not to make a checklist.

Report the created task's id and title back.
