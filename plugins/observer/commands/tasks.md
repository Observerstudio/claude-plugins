---
description: What's assigned to me in Observer OS
---

List the caller's open work with `list_tasks`.

If the caller is in a repo, narrow to this folder's project first with
`project_for_repo` (`git remote get-url origin`) and say which project you
narrowed to. "Everything assigned to me everywhere" is rarely the question
someone is asking from inside a project directory — but say what you did,
so they can ask for the wider list.

Resolve the caller with `list_members` and filter to them. Show status,
priority and due date, most urgent first, and keep it to one line per task —
this is a terminal, not a dashboard.

If nothing is assigned, say so plainly rather than listing the whole
project.
