# 10 — Mobile & Offline

**Load when:** iOS, Android, React Native, Flutter, PWA, or anything expected to work on a phone or without a network.

## Priority 1

1. Native, cross-platform, or web-based — and what drives that?
2. Which OS versions must be supported, and what percentage of users are on old ones?
3. Does this need to work offline, or just tolerate brief connectivity loss?
4. Is there an existing app this ships inside, with its own conventions?
5. Does this require app store review, and does that affect your release cadence?

## Connectivity

6. What's the behavior on no connection — blocked, cached, or queued?
7. What's the behavior on a slow or intermittent connection, which is worse than none?
8. Are requests retried when connectivity returns, and are they idempotent?
9. Is there a sync engine, and what's the conflict resolution rule?
10. Can the user tell what's synced and what's pending?
11. What happens if the app is killed with pending writes?

## Local storage

12. What's stored on device, and is any of it sensitive?
13. Is sensitive data in the keychain/keystore rather than plain preferences?
14. What's the storage size ceiling, and what gets evicted first?
15. Is local data migrated when the schema changes across app versions?
16. Is local data cleared on logout?

## Platform behavior

17. What happens when the app is backgrounded mid-operation?
18. Are there background execution limits that affect long-running work?
19. Are push notifications needed, and what's the permission-request moment?
20. Which permissions are requested, when, and what's the degraded experience if denied?
21. Is deep linking supported, and does it handle the not-logged-in case?
22. Does the app handle interruptions — calls, low battery, low storage?

## Device diversity

23. What's the smallest screen supported, and does the layout hold?
24. Does it work on tablets and in split-screen?
25. Is the app usable with the OS font size set large?
26. Does it handle notches, safe areas, and gesture navigation bars?
27. What's the oldest/slowest device you'll test on?
28. Does it work in both orientations, or is one locked deliberately?

## Performance & resources

29. What's the app launch time on a mid-range device?
30. What's the battery impact — location, background sync, polling?
31. What's the data usage per session, and does it matter on metered connections?
32. What's the install size, and is it near a store download threshold?
33. Are images and assets sized per screen density?

## Release & lifecycle

34. How are users on old app versions handled when the API changes?
35. Is there a forced-update mechanism, and is it ever justified?
36. Is there staged rollout and a way to halt a bad release?
37. How are crashes reported, and are they symbolicated?
38. Can features be toggled remotely without a store release?

## Accessibility

39. Does VoiceOver/TalkBack work through the primary flow?
40. Are touch targets at least the platform minimum size?
41. Does dynamic type resize text without clipping?

## Verification

- Enable airplane mode mid-flow and watch what happens.
- Throttle to poor connectivity and complete the primary task.
- Kill the app during a write and reopen it.
- Test on the oldest supported OS and the smallest supported screen.
- Set system font to maximum and check every screen.
- Deny every permission and confirm the app still explains itself.
