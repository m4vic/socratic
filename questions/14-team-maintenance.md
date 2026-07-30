# 14 — Team, Maintenance & Longevity

**Load when:** this code will outlive the current session, or anyone other than the author will touch it.

## Priority 1

1. Who maintains this in six months?
2. Could a new engineer understand and change this without asking the author?
3. What's the one thing about this system that's non-obvious and will bite someone?
4. Is this a technology the team already knows, or one person's preference?
5. What happens to this system if the person who built it leaves?

## Code comprehensibility

6. Is the structure conventional for this language/framework, or novel?
7. Do names describe intent rather than implementation?
8. Is there a comment explaining *why* for every non-obvious decision?
9. Are there abstractions with only one implementation, added speculatively?
10. Is there clever code that should be boring code?
11. How deep is the call stack for a typical operation — can you follow it?
12. Are there hidden side effects — functions that do more than their name suggests?

## Documentation

13. Does the README explain what this is, why it exists, and how to run it?
14. Can someone go from clone to running locally following only the docs?
15. Are architectural decisions recorded, with the alternatives considered?
16. Is there a diagram for anything with more than three moving parts?
17. Are runbooks written for the operations someone will need at 3am?
18. Is documentation stored next to the code, so it gets updated with it?
19. Is anything documented that's already out of date and actively misleading?

## Change safety

20. If someone changes X, what breaks, and would they find out before production?
21. Are there tests that fail loudly when a contract is broken?
22. Are there types or schemas that make invalid states unrepresentable?
23. How coupled is this to other systems — what's the blast radius of a change?
24. Is there a deprecation path for the parts you already know are wrong?

## Dependencies over time

25. Are the dependencies actively maintained, and when were they last released?
26. How many dependencies are there, and is each one earning its place?
27. What's the upgrade story — can you bump a major version without a rewrite?
28. Is there a process for keeping dependencies current, or will this rot?
29. Is any critical function dependent on a single unmaintained package?
30. Is there a vendor lock-in that would be expensive to reverse?

## Technical debt

31. What shortcuts are being taken deliberately, and are they written down?
32. Is there a TODO in this code that will never be done — should it be honest about that?
33. What would you do differently with two more weeks, and is that recorded?
34. Which part of this will need to be rewritten first, and when?

## Ownership & process

35. Is there a code owner, and is it in CODEOWNERS?
36. What's the review process, and does anyone actually read the diffs?
37. Where do bugs get reported, and who triages?
38. Is there a changelog, and does it mean anything to users?
39. How are breaking changes communicated to dependents?

## Handoff readiness

40. Could you hand this to another team tomorrow with a one-page document?
41. Are there credentials or accounts tied to one person?
42. Is there anything only running because it happens to be on someone's machine?
43. Are there manual steps that exist only in someone's head?

## Verification

- Have someone else clone the repo and get it running using only the README. Time it.
- Pick a plausible future change and estimate how many files it touches.
- Check the dependency tree for anything unmaintained for over two years.
- Review every TODO and either fix, ticket, or delete it.
- Confirm no credential or account is bound to a single individual.
