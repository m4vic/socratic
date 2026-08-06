# MVP grade

Supersedes: — (base grade)
Superseded by: `production`, `enterprise` — both include every item below by reference, never restated. Resolve this gate once; a higher grade never asks it again in different words.

The bar: it does the one thing it claims, for a real input, and fails
honestly when it can't. Nothing here is about scale, polish, or surviving
production load — that's the `production` grade. This is the floor below
which it isn't actually built yet, just started.

Mandatory domains: Requirements, Testing. No pack is mandatory at this grade.

## Gate

| # | Item | Resolved when | Escalate to user only if |
|---|---|---|---|
| 1 | Does the one core action work end to end, for a real input, not a hardcoded one | run it against something real and observe the output | it doesn't, and the fix changes the scope |
| 2 | What happens on the first error a real user will actually hit | the failure is visible — an error message, a non-zero exit, a logged line — not a silent wrong answer or a hang | never — this is always resolvable by the agent |
| 3 | Is the boundary of what it does *not* do stated somewhere a reader will see it | one sentence in the README or the entry point | never |
| 4 | Can someone who isn't the builder run it from the README alone | a fresh read of the README contains every step actually required | never |
| 5 | Is there any signal at all that it's broken — a log line, an exit code, anything | confirmed present, even if crude | never |
| 6 | If it touches real data or a real external system, is there one way to tell something went wrong before it's discovered downstream | confirmed, even if manual | if the honest answer is "no reasonable way yet" — that's a scope call |

Six items. Most resolve by reading the code and running it once. The two
escalation paths exist because "the core action doesn't work and fixing it
changes scope" and "there's no way to catch a bad write before it propagates"
are genuinely the user's calls, not engineering defaults.

Passing this gate does not mean production ready. It means it's real enough
to hand to someone else and have them not immediately hit a wall.
