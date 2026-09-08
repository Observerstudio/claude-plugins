---
description: Your standup, assembled from real work rather than memory
---

Call `my_standup` and read it back as a standup, not as a data dump.

Structure it the way a person would say it out loud:
1. **Closed** — what actually landed. Use the titles, not the ids.
2. **In progress** — including anything in review. Review-blocked work is
   not done, and saying so is the point of mentioning it.
3. **Blocked** — and if the task carries a reason, give it. A blocker
   nobody names does not get unblocked.
4. **Next** — what's planned.

Then the hours. If `timerRunning` is true, say so: the running timer's time
is **not** in the total, so the number understates the day until it stops.

Two things not to do:
- **Don't pad.** If a bucket is empty, leave it out. A standup listing four
  empty headings is worse than three lines.
- **Don't editorialise progress.** Report what the data says. If someone
  closed nothing today, that is the standup — invent no narrative for it.

If the caller asks for another day, pass `date` as `YYYY-MM-DD`.

Nothing here writes. To record work, use `/observer:report`.
