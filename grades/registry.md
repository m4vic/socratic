# Grade registry

A grade is different from a pack. A pack adds depth on one topic, loaded zero
to two at a time. A grade sets the **target** for the whole run — which
domains go Full, which packs are mandatory rather than optional, and what
"done" means for this specific build.

Load a grade when the user names one explicitly — "make this production
ready", "MVP is fine for now", "get this to production grade", `$socratic
mvp`, `$socratic production` — or when the request itself states the target
("this needs to survive real traffic", "just a prototype for now").

## What changes when a grade is active

1. The grade file's gate becomes the stopping condition, replacing the
   generic sufficiency check in `SKILL.md`. The run does not stop when the
   next question stops changing the design — it stops when every item in the
   gate is resolved, mitigated with a stated reason, or marked not applicable
   by the grade file itself.
2. The grade names which domains go Full and which packs are mandatory. Load
   those regardless of what the dynamic domain scan alone would select.
3. Gate items are self-answered the same way everything else in Socratic is:
   read the repo, apply the default, escalate only the ones that are
   genuinely a business or authority call. Most gate items resolve silently.
   Only the ones marked for escalation in the gate file should reach the
   user as open questions.
4. A gate item never re-explains a pack's decision card. It links to it —
   `packs/operations/core.md#retry`, not a restatement. Only items nothing
   else covers get written out in the gate file itself.

## Grades are cumulative, on purpose

`mvp` → `production` → `enterprise` is a chain, not three independent lists.
Each grade's file states `Supersedes:` the one below it and resolves that
gate first — it does not repeat those items in its own words. This is the
mechanism, not a style choice: **an item is defined in exactly one place**,
so loading `enterprise` can never produce a different answer than loading
`production` alone would have for the same question. Two descriptions of the
same check would eventually drift apart as either file gets edited; a single
description referenced from three places cannot.

When resolving a grade, resolve every item in its `Supersedes:` chain down to
`mvp`, not just the items listed in that file — a "production" run that skips
MVP's six items has not actually checked whether the thing works.

## Grades

| Grade | Bar | Supersedes | File |
|---|---|---|---|
| `mvp` | Does it actually work, end to end, and fail honestly when it doesn't | — | [`grades/mvp.md`](mvp.md) |
| `production` | Would an on-call engineer accept ownership of this at 3am | `mvp` | [`grades/production.md`](production.md) |
| `enterprise` | Would this pass procurement, an audit, and a real regional outage | `production` | [`grades/enterprise.md`](enterprise.md) |

No grade active is the default: the generic sufficiency check runs as
before, nothing in this directory loads, and cost is unaffected.

## Source

The production gate is adapted from the **Production Readiness Review**
practice described in Google's *Site Reliability Engineering* and *The Site
Reliability Workbook* — a checklist gate a system passes before it is
allowed to take real traffic, distinct from the resilience mechanisms
`packs/operations` covers. A PRR asks "are we allowed to call this done";
`operations` answers "how do we build it to survive being live." Both matter
and neither substitutes for the other.
