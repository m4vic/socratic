# 00 — Requirements & Scope

**Load: always.** These run before every other domain. If you only ask three questions total, ask them from here.

## Priority 1 — ask these first

1. Restate the task in one sentence and confirm it. *"You want X that does Y for Z. Right?"*
2. Is this a throwaway, a prototype, or something meant to run in production? (This sets the budget for every other question.)
3. Who uses this — you alone, an internal team, or external customers?
4. Roughly how many users/requests/records? Order of magnitude only.
5. Is there an existing system this replaces, extends, or must coexist with?
6. What's the deadline or urgency? "Today" and "next quarter" produce different architectures.

## Problem definition

7. What problem is actually being solved, versus the solution that was requested?
8. What happens if this doesn't get built — what's the cost of doing nothing?
9. Is this solving a real observed pain, or an anticipated one?
10. What's the manual process today, and could improving it beat building software?
11. Who asked for this, and are they the person who'll use it?
12. What does success look like in a measurable way?

## Scope boundaries

13. What's explicitly **out** of scope for v1?
14. Is there a smaller version that delivers 80% of the value?
15. Which parts are must-have versus nice-to-have?
16. Should this be built to be extended later, or built to be thrown away?
17. Are there features you're deliberately *not* building, and should the design leave room for them?
18. Is this a feature inside an existing product, or a standalone thing?

## Constraints

19. What language/framework/stack is required or already in use?
20. Are there libraries or vendors that are mandated or forbidden?
21. What's the budget — compute, licensing, third-party API spend?
22. Are there existing conventions in the codebase to follow (naming, structure, error handling)?
23. Who will maintain this after it ships?
24. Are there platform constraints — must run on-prem, air-gapped, in a specific cloud, in a browser?
25. Is there an existing design doc, ticket, or spec I should read first?

## Users & workflow

26. What's the primary user journey, start to finish?
27. Are there multiple user types with different permissions or needs?
28. How technical are the users?
29. What's the frequency of use — constant, daily, once a quarter?
30. What tool are users switching *from*, and what will they expect to work the same way?
31. Is anyone using this in a high-pressure or time-critical moment?

## Integration surface

32. What does this system read from, and what does it write to?
33. Which of those dependencies do you control, and which are third-party?
34. Are there existing APIs, schemas, or event formats to conform to?
35. Does anything downstream depend on this system's output format?
36. Is there an authentication system already in place to plug into?

## Assumptions & risk

37. What's the riskiest assumption in this plan, and can it be validated cheaply first?
38. What's the most likely reason this project fails?
39. Is there a part of this that no one on the team has built before?
40. What would make you kill this project three months from now?

## Verification (run before declaring done)

- Does the built thing solve the problem stated in Q7, or just the request in Q1?
- Is every must-have from Q15 present?
- Is anything from the out-of-scope list (Q13) accidentally in there?
- Can a new person understand what this does from the README alone?
- Were any Q37 assumptions proven wrong during the build, and does the design still hold?
