# 11 — Product & User Experience

**Load when:** a human will use this directly. Also useful for CLIs and developer tools — developers are users too.

## Priority 1

1. What is the user trying to accomplish, and what's the shortest path to it?
2. What's the state of the world when the user first arrives — is there an empty state, and is it helpful?
3. What's the most likely mistake a user will make, and what happens when they make it?
4. Is anything here irreversible?
5. How does the user know the thing they did worked?

## Flow design

6. How many steps to complete the primary task, and can any be removed?
7. What's required upfront versus deferrable — can you ask less to start?
8. Are there sensible defaults for every option, so nothing is mandatory?
9. Is progress saved if the user leaves partway?
10. Is there a way back from every screen?
11. Does the flow work for the returning expert user as well as the first-timer?

## Feedback & state

12. What's shown during a slow operation — and is there an indication at all after 300ms?
13. If an operation takes more than a few seconds, is progress or an estimate shown?
14. Is success confirmed explicitly, or does the screen just change?
15. Are optimistic updates used, and is rollback visible if the server disagrees?
16. What does the user see when they have no data yet, one item, and ten thousand items?

## Errors

17. Does every error message say what happened, why, and what to do next?
18. Are errors shown near the thing that caused them?
19. Do errors preserve the user's work, or wipe the form?
20. Is there a distinction between "you did something wrong" and "we broke"?
21. Is there a support path from an error — a code, a link, a retry?
22. Are technical errors translated, or does the user see a stack trace?

## Safety & reversibility

23. Are destructive actions confirmed, undoable, or delayed (trash rather than delete)?
24. Does confirmation actually make the user think, or is it a reflexive OK?
25. Is there a way to recover from the worst accidental action?
26. Are bulk operations previewed before execution?

## Trust & clarity

27. Does the interface promise anything it can't guarantee?
28. Is it clear what data is collected and why?
29. Is the pricing/cost implication of an action visible before the action?
30. Is anything hidden that the user would be upset to discover later?

## Onboarding & learning

31. Can a new user succeed without documentation?
32. Is there example or seed data so the empty product isn't a blank wall?
33. Are advanced features discoverable without cluttering the default view?
34. Is help contextual, or does it live in a separate manual nobody reads?

## Accessibility & inclusion

35. Does this work for someone using a keyboard only, a screen reader, or magnification?
36. Does it work for colorblind users — is color ever the sole signal?
37. Is the reading level appropriate for the audience?
38. Are names, addresses, phone numbers, and dates handled in non-US formats?
39. Are assumptions made about gender, family structure, or naming conventions?

## CLI / developer-tool specifics

40. Does `--help` explain everything needed to use the tool?
41. Are there sane defaults so the common case is one command?
42. Does it fail with a useful message and a nonzero exit code?
43. Is output machine-parseable when piped, and human-readable when not?
44. Is there a dry-run mode for anything destructive?
45. Does it respect `NO_COLOR`, quiet, and verbose flags?

## Verification

- Watch someone unfamiliar attempt the primary task without help. Don't intervene.
- Trigger every error state deliberately and read the message as a stranger would.
- Complete the flow with the slowest realistic network.
- Use the product with an empty account and with a very full one.
- Do the destructive action and try to undo it.
