# 05 — Security

**Load when:** the system touches user data, authentication, payments, external input, secrets, or the public internet. **Mandatory** for anything handling money, PII, or health data.

Note: this bank is for *defensive* design review. It does not describe how to attack systems.

## Priority 1

1. What's the most sensitive data this system touches?
2. Where's the trust boundary — which inputs come from outside your control?
3. Who is allowed to do what, and where is that decision enforced?
4. Are there secrets involved, and how are they stored?
5. If this system were fully compromised, what's the blast radius?

## Authentication

6. How do users prove who they are — passwords, SSO, magic links, API keys?
7. Are passwords hashed with a modern algorithm (bcrypt/scrypt/argon2), never encrypted or plain?
8. Is there MFA, and is it available or enforced?
9. How long do sessions last, and can they be revoked server-side?
10. Are session tokens stored in httpOnly, Secure, SameSite cookies rather than localStorage?
11. What's the account recovery flow, and is it weaker than the login itself?
12. Is there rate limiting and lockout on login attempts?
13. Are login failures generic ("invalid credentials") rather than revealing which field was wrong?
14. Is session ID rotated on privilege change (login, role change)?
15. For service-to-service: are credentials scoped, rotatable, and distinct per caller?

## Authorization

16. Is authorization checked on every request, or assumed from the UI hiding a button?
17. Can a user access another user's resource by changing an ID in the URL? (Test this explicitly.)
18. Is the permission check at the data-access layer, or scattered across handlers?
19. Are there admin/superuser paths, and are they audited?
20. Do list endpoints filter by ownership, or return everything and filter client-side?
21. Can a user escalate their own role via a field in a profile-update payload?
22. Are there multi-tenant boundaries, and is tenant ID derived from the session rather than the request body?

## Input handling

23. Is every input validated against an allowlist rather than a denylist?
24. Are database queries parameterized everywhere, with no string concatenation?
25. Is user content escaped at render time, and is any HTML rendered raw?
26. Is there a Content Security Policy, and does it actually block inline scripts?
27. Are file uploads restricted by type, size, and stored outside the web root?
28. Are user-supplied URLs ever fetched server-side, and is that restricted to prevent internal network access?
29. Are user-supplied paths ever used in file operations?
30. Is deserialization of untrusted data avoided?
31. Are redirects validated against an allowlist of destinations?

## Secrets

32. Are any credentials committed to the repo, including in history?
33. Where do secrets come from at runtime — env vars, a secret manager, a mounted file?
34. Are secrets different per environment?
35. What's the rotation process, and has it ever been done?
36. Are secrets visible in logs, error messages, or crash dumps?
37. Do CI/CD pipelines expose secrets to forked PRs?

## Transport & storage

38. Is TLS enforced everywhere, including internal service calls?
39. Is sensitive data encrypted at rest, and who holds the keys?
40. Are there fields that should be encrypted at the column level, not just disk level?
41. Is data masked in non-production environments?

## Dependencies & supply chain

42. Are dependencies pinned, and is there a lockfile committed?
43. Is there automated vulnerability scanning on dependencies?
44. Were any dependencies added that are unmaintained, or have very few users?
45. Are build artifacts and container base images from trusted sources?
46. Does CI run untrusted code from pull requests with access to secrets?

## Logging & audit

47. What's logged on a security-relevant event — login, permission change, data export?
48. Do logs contain passwords, tokens, card numbers, or full PII?
49. Are logs tamper-evident and retained long enough to investigate an incident?
50. Would you be able to answer "what did this user access last month" from the logs?

## Abuse & availability

51. What's the most expensive operation an anonymous user can trigger?
52. Is there a limit on resource consumption per request — CPU time, memory, result size?
53. Can someone enumerate users, IDs, or emails through the API or error messages?
54. Are there costs an attacker could run up (SMS, email, third-party API calls)?
55. Is there protection against automated signup/spam?

## Incident readiness

56. How would you find out this system was breached?
57. Can you revoke all sessions / rotate all keys quickly if needed?
58. Is there a documented contact and process for someone reporting a vulnerability?
59. Do you have the logs needed to determine what data was accessed?

## Verification

- Log in as user A, take a resource ID belonging to user B, and try to read, update, and delete it.
- Remove the auth header from every endpoint and confirm 401, not 200.
- Submit `<script>` and SQL-ish strings into every text field and check the rendered output and query behavior.
- Scan the repo history for secrets (`gitleaks`, `trufflehog`).
- Run dependency audit and review anything high or critical.
- Grep logs for tokens, emails, and password-like fields.
- Confirm TLS and security headers with an external scanner.
