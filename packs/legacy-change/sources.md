# Changing existing code: sources

**Primary sources:**

- Michael C. Feathers, *Working Effectively with Legacy Code* — seams,
  dependency-breaking techniques, characterization tests, and the definition
  of legacy code as code without tests.
- Martin Fowler, *Refactoring* — behavior-preserving transformation in small
  verified steps, and the discipline of never mixing refactoring with a
  behavior change.

**Supporting material:**

- Martin Fowler, *StranglerFigApplication* — incremental replacement of a
  system by routing slices of behavior to a new implementation, rather than
  a long-lived rewrite branch.
- Michael T. Nygard, *Release It!* — already the source for the `operations`
  pack. Incremental replacement is also a deployment problem, and the two
  packs pair well when the change must ship without downtime.

A future `full.md` could add decision clusters for specific dependency-breaking
techniques by language, characterizing behavior through logs and traffic
capture when the code resists testing, deciding what to delete versus preserve
during extraction, and sequencing a multi-quarter migration. Add a card only
when it changes a real design choice or verification step.
