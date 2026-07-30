# 13 — Compliance, Legal & Ethics

**Load when:** the system touches personal data, regulated industries, users in the EU/UK/California, payments, health data, or minors.

*These questions surface issues to escalate. They are not legal advice — when an answer is unclear, the right move is to route it to counsel, not to guess.*

## Priority 1

1. Does this system collect, store, or process personal data?
2. Where are the users, and where is the data stored?
3. Is the industry regulated — health, finance, education, children's services?
4. Is payment card data touched at any point, even in transit?
5. Is there an existing privacy policy, DPA, or compliance program to conform to?

## Personal data

6. What personal fields are collected, and is each one actually needed?
7. What's the lawful basis or user-facing justification for collecting each?
8. Is consent required, and is it specific, informed, and revocable?
9. Can a user see everything you hold about them?
10. Can a user export their data in a portable format?
11. Can a user's data be genuinely deleted, including from backups, logs, analytics, and third parties?
12. Is there a retention schedule, or does data live forever by default?
13. Is data minimized — do you avoid collecting "just in case" fields?
14. Are there special categories (health, biometric, religion, sexuality, union membership) that carry stricter rules?

## Data residency & transfer

15. Does any data cross a border, and is there a mechanism covering that transfer?
16. Do any subprocessors (cloud, analytics, model providers, email) receive personal data?
17. Is there a list of subprocessors, and is it accurate?
18. Do contracts with those vendors cover the data they receive?

## Sector-specific

19. **Health:** is this covered by HIPAA or equivalent, and is there a BAA with every vendor touching PHI?
20. **Payments:** does the design keep you out of PCI scope by using a hosted/tokenized processor?
21. **Finance:** are there KYC/AML, record-keeping, or reporting obligations?
22. **Education:** are student records covered by FERPA or local equivalents?
23. **Children:** could users be under 13/16, and does that trigger COPPA/GDPR-K requirements?
24. **Accessibility:** is there a legal accessibility standard that applies (ADA, EN 301 549, Section 508)?

## Security obligations

25. Is there a breach notification obligation, and do you know the deadline?
26. Would you be able to determine the scope of a breach from your logs?
27. Are there required security controls (encryption, access review, audit logging) for this data class?
28. Is there an access review process — does anyone still have access who shouldn't?

## Licensing & IP

29. Are all dependencies' licenses compatible with how you're distributing this?
30. Is there any copyleft (GPL/AGPL) dependency in a product you ship or host?
31. Is third-party content — images, fonts, datasets, code snippets — properly licensed?
32. Who owns the output of this system, and is that stated anywhere?
33. If AI-generated content is involved, are there attribution or disclosure expectations?

## Ethics & impact

34. Could this system produce systematically different outcomes for different groups?
35. Is there a decision here that materially affects someone (credit, hiring, access), and is there recourse?
36. Is automation replacing a judgment that should stay human?
37. Could this be misused, and does the design make misuse easier than it needs to be?
38. Are dark patterns present — is anything harder to cancel than to start?
39. Does the system tell users what it's doing with their data in language they'd understand?

## Records

40. Are the decisions above documented somewhere an auditor could read?
41. Is there a data flow diagram showing where personal data goes?
42. Is there an owner responsible for compliance questions on this system?

## Verification

- Trace one user's data through every system, including logs, analytics, backups, and vendors.
- Exercise the deletion path end to end and then search everywhere for remnants.
- Run a license scan on the dependency tree.
- Review the privacy policy against what the system actually does.
- Confirm every vendor receiving personal data has an appropriate agreement in place.
