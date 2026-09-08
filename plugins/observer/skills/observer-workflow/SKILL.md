---
name: observer-workflow
description: How this team files and tracks work in Observer OS. Use when creating or updating tasks, reporting finished work, or logging time.
---

# Reporting work to Observer OS

Observer is where this studio's work is tracked. The MCP tools write to it
directly, with the same permissions the person's own account has.

## Report at boundaries, not continuously

Call `report_work` when a **unit of work** ends: a task moves status, a
commit lands, a piece of work is finished. Not on every prompt.

This is a real constraint, not shyness. Per-prompt logging floods the task
list faster than anyone reads it, and prompt text routinely carries
client-confidential detail into a workspace that has a client portal
attached.

## One call, not six

`report_work` takes everything at once — status, comment, subtasks, due
date, estimate, minutes. Prefer it over calling `update_task_status`, then
`comment_on_task`, then `create_task`: a single call at a boundary actually
happens, whereas a six-step ritual gets skipped.

## The comment is the payload

A status change with no explanation tells a teammate nothing. "Moved to In
review" is noise; "Moved to In review — auth flow done, rate limiting
deferred to #204" is the thing someone reads and acts on.

Write it for the person picking this up next, not as a changelog line.

## Time waits for review; task updates do not

Task updates land immediately. **Time does not** — it is filed as a
`LOG_TIME` action in Observer's review queue, and a person approves it
before it reaches the timesheet.

The reason is worth knowing, because it is not obvious: hours count
toward pay and project figures **before** anyone approves them. The review
queue is the gate that protects the person whose timesheet it is.

So when you report time, say that it is awaiting review. Do not tell
someone their hours are logged.

## Never guess a task

Resolve the task from what the user said, or from a GitHub issue number in
the branch name via `list_github_links`. If neither resolves, **ask**.

A commit against the wrong task is worse than no report: it is wrong data
that looks like real data, and nobody goes looking for it.

Note the limitation honestly if it comes up — branch matching only works
for tasks pushed to GitHub, because `githubIssue` is the only human-typable
handle a task has.

## Don't infer status from a commit

A commit is evidence that work happened. It is not evidence that a task is
done. Set `status` only when the work genuinely moved, and prefer leaving it
alone over guessing.

## Planning from a conversation proposes first

`/observer:recap` turns a conversation into tasks, milestones and sprint
commitments. It reads what exists first (`list_tasks`, `list_milestones`,
`list_sprints`) so it updates rather than duplicates, shows the plan, and
writes nothing until the user says yes.

The order matters: a recap that files twenty tasks nobody asked for floods
the list, and it drags conversation detail — sometimes client-confidential
— into a workspace with a client portal attached. Sprints are never created;
they come from the cadence. Planning one means `update_sprint` for the goal
and `assign_tasks_to_sprint` for the tasks, in that order.
