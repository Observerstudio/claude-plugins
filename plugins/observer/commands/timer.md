---
description: Start, stop or check your Observer timer
---

Use `timer_status`, `start_timer` and `stop_timer`.

- **status** — report the running timer, or say nothing is running.
- **start** — needs a project. Resolve the task too if the user names one,
  and check `timer_status` first: a second timer is refused, and telling the
  user what is already running is more useful than relaying the refusal.
- **stop** — report the hours written.

Say plainly that a stopped timer's hours count toward pay and project
figures before anyone approves them. That is surprising, and the person
tracking the time is the one who should know it.
