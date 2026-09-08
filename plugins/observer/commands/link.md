---
description: Link this repo to an Observer OS project
---

Link the current folder's git repository to a project so `project_for_repo`
— and every command that relies on it — knows where it is.

1. Read the remote: `git remote get-url origin`.
2. Check first with `project_for_repo`. If it already resolves, say which
   project(s) and stop — there is nothing to do unless the user wants a
   second project linked.
3. If the user named a project, find it with `list_projects`. Otherwise show
   the active projects and ask which. Do not pick by name similarity to the
   folder: that guess succeeds confidently and points the repo at the wrong
   client's project.
4. Call `link_repo` with the `projectId` and the remote URL as `repo`. The
   server resolves it against the workspace's GitHub installation and
   refuses repos the app cannot see — if that happens, say so and point at
   `list_github_repos`.
5. Confirm with `project_for_repo` and report what it now returns.

`unlink_repo` takes the same two arguments to undo it.
