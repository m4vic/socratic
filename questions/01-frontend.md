# 01 — Frontend & UI

**Load when:** building any user interface — web app, dashboard, form, component library, landing page, admin panel.

## Priority 1

1. Is this a new app, or a component inside an existing one? (Determines whether you pick a stack or inherit one.)
2. Which framework — React, Vue, Svelte, plain HTML, or whatever's already there?
3. Server-rendered, client-rendered, or static? What's driving that choice — SEO, interactivity, or simplicity?
4. Desktop-first, mobile-first, or both equally?
5. Is there a design system, component library, or Figma file to follow?

## Architecture

6. Where does state live — component-local, context, a store, the URL, or the server?
7. Is server state (fetched data) handled differently from client state (UI toggles)?
8. Is there a routing library, and does the app need deep-linkable URLs?
9. Are components split by feature or by type? Which convention does the repo use?
10. Is any of this reused across other apps, and does it need to be a package?
11. Is there a build step, and what's the bundler?
12. Should this work without JavaScript at all (progressive enhancement)?

## Data fetching

13. Where is data fetched — on the server, on mount, on interaction?
14. What's shown while data loads — spinner, skeleton, stale content, nothing?
15. What's shown when the fetch fails, and can the user retry?
16. Is data cached, and when is the cache invalidated?
17. Are there race conditions if the user triggers several fetches quickly?
18. Is pagination, infinite scroll, or a "load more" button appropriate for the list sizes expected?
19. Are optimistic updates worth it here, and what's the rollback if the server rejects?

## Forms & input

20. What's validated client-side, and is the same validation enforced server-side?
21. When does validation fire — on change, on blur, on submit?
22. Is form state preserved if the user navigates away and comes back?
23. What prevents double-submission?
24. Are destructive actions confirmed, undoable, or both?
25. Are file uploads involved — size limits, type restrictions, progress indication?

## Rendering & performance

26. What's the largest list or table this will render, and does it need virtualization?
27. Are there expensive re-renders — is memoization needed, or is that premature?
28. What's the bundle size budget, and is code-splitting needed?
29. Are images optimized, lazy-loaded, and correctly sized?
30. Is there layout shift as content loads?
31. What's the target for time-to-interactive, and on what device class?
32. Are third-party scripts (analytics, chat widgets) blocking render?

## Accessibility

33. Is the whole flow operable by keyboard alone?
34. Do interactive elements have accessible names, and is semantic HTML used before ARIA?
35. Is focus managed on route change, modal open, and modal close?
36. Does color contrast meet WCAG AA, and is color the only signal for anything?
37. Are errors announced to screen readers, not just shown visually?
38. Does the layout survive 200% browser zoom?
39. Is `prefers-reduced-motion` respected for animations?

## Cross-cutting UI

40. Which browsers and versions must be supported?
41. Is dark mode required, and is it a system preference or a user toggle?
42. Is internationalization needed — and if so, RTL layouts too?
43. How are dates, numbers, and currency formatted for different locales?
44. What's the empty state for every list, table, and dashboard?
45. What's the behavior on a slow or flaky connection?
46. Is there anything that breaks when the browser tab is backgrounded?

## Verification

- Tab through the entire primary flow with no mouse — does it work?
- Throttle the network to slow 3G — is every loading and error state sane?
- Render the biggest realistic dataset — does it stay responsive?
- Check every list for its empty state and its error state.
- Run an automated a11y check (axe, Lighthouse) and fix anything above "minor."
- Resize to 320px wide — is anything cut off or overlapping?
