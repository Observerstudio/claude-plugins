---
description: Recap this conversation into Observer OS — tasks, milestones, and the sprint
---

Turn what was decided in this conversation into Observer OS records. This is
a **planning** command: it proposes first and writes only after the user
confirms. Nothing here is automatic.

## 1. Work out where you are

Resolve the project with `project_for_repo` (`git remote get-url origin`).
One result: use it. Several: ask which. None: `list_projects` and ask —
never match a project by how much its name resembles the folder.

Then load what already exists, so the recap updates rather than duplicates:
- `list_tasks` for the project (open statuses)
- `list_milestones` for the project
- `list_sprints` — the `ACTIVE` sprint and the next `PLANNED` one

## 2. Recap the conversation

Read the whole conversation back and pull out only the things that are
**decided work**, not ideas that were floated and dropped:

- **Tasks** — concrete, separable units of work. Title as the outcome
  ("Nav filter respects role"), not the activity ("fix nav"). Include the
  detail that would let someone else pick it up in `description`.
- **Updates to existing tasks** — a task discussed here that moved status,
  gained a due date, an estimate, or a subtask. Match by key (`OBS-123`)
  or by title against the list you loaded; when unsure whether something is
  new or an existing task, ask rather than create a duplicate.
- **Milestones** — a dated grouping the conversation committed to ("beta to
  client by the 20th"). Match against existing milestones first; a
  conversation rarely invents a genuinely new one.
- **Sprint** — which tasks belong in the active or next sprint, and the
  sprint goal if one was stated. Sprints are generated from the cadence, so
  you never create one — you name its goal and commit tasks to it.

Leave out: things the user explicitly deferred, speculation, and anything
that is really a note for the user rather than work for the team.

## 3. Propose, then wait

Show the plan as a short list, grouped: **create**, **update**, **milestones**,
**sprint**. One line per item, with the field values you intend to write.
Then stop and ask for a yes. The user may trim it — apply only what they
confirm.

Do not write before confirmation. A recap that files twenty tasks nobody
asked for is worse than no recap: it floods the list faster than anyone
reads it, and it drags conversation detail — sometimes client-confidential —
into a workspace with a client portal attached.

## 4. Apply

- New tasks: `create_task` with `projectId`, `title`, `description`,
  `priority`, `dueDate`, `milestoneId` where a milestone was chosen.
  Subtasks via `parentTaskId`.
- Existing tasks: `report_work` for status, comment, due date, estimate;
  `update_task` for title, priority, milestone.
- Milestones: `create_milestone` / `update_milestone`.
- Sprint: `update_sprint` for the goal, then `assign_tasks_to_sprint` with
  the task keys or ids in one call.

## 5. Report

List what landed with keys and ids, and quote anything a tool returned in
`notFound` or `skipped` verbatim. If the sprint assignment moved fewer
tasks than you passed, say which did not move rather than rounding it off.
