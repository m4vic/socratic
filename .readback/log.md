# Readback log — socratic

Append only. Newest at the bottom. See `index.md` for the format and index.

---

## 2026-08-03 20:55 — Fix broken instructions before expanding packs

**You:** "we need to redesign the stuff... currently, Pax has different
specific skills like AI engineering and data design and etcetera. And we need
to improve it. And one thing I am wondering, how top people like gStack and
Anthropic is skills designed... it has too much of text token, how it works.
it is the same. Like, it's very small compared to those big skills."

**I suggested:** four possible starting points — fix the broken instructions,
add a scripts layer, write new packs, or restructure the pack tiers entirely.

**Decided:** fix what is broken first. Expanding a system whose escalation
path is a dead link would build on top of a defect.

Measured Anthropic's 31 official skills and OpenAI's 6 bundled ones before
deciding anything. Anthropic's SKILL.md median is 2,255 tokens; socratic's was
2,268. The assumption that socratic was small compared to real skills was
wrong — it is almost exactly median. Large skills are large in their bundles,
not their entry point.

**I did:** three edits to SKILL.md. Made the `full.md` reference conditional
to match what every other file already said; rewrote the description into
Anthropic's third-person trigger-phrase format; changed one second-person line
to imperative.

**Not taken:** writing `full.md` for all four packs. Every other file in the
repo already describes it as optional and "when justified", and the packs'
own software-design guidance warns against adding generality before there is
evidence it is needed. Only SKILL.md claimed both tiers exist.

Files: SKILL.md

## 2026-08-03 21:20 — Two copies of socratic now exist

**You:** "currently, just I moved Socratic folder. Right? So a single folder
has multiple files. How can we manage it? Like, sometimes we move folders like
files... if we move the folder to any other location. In future, if it reuses,
it first checks the older location of that file and checks if it has so it can
copy it from there and move it to the current location."

**Decided:** track identity rather than location. The record now carries a
`Project:` name that does not depend on any path, plus an append-only
`Origins:` list of where it has lived. Two records with the same `Project:`
are the same lineage no matter where either sits.

This copy at F:\change-atlas\socratic is ahead of the installed one at
C:\Users\ZENITH\.claude\skills\socratic — only SKILL.md differs, from the
three fixes made at 20:55, and the installed copy has no record at all.
Whichever becomes authoritative, the other must be updated deliberately rather
than assumed stale.

**I did:** added `Project:` and `Origins:` to the readback format, plus rules
for inheriting files from another project, splitting a project, and finding
two records of the same lineage.

**Not taken:** automatic discovery of a moved project's old history. Scanning
a filesystem for orphaned records is slow and unreliable. Inside a repository
`git log --follow` handles it; across drives only the user knows, so the move
gets recorded when they mention it.

Files: .readback/index.md, .readback/log.md

## 2026-08-03 22:10 — Everything since the first commit is unpushed

**You:** "Now which one is ready to push? Like, I can update the GitHub of
soccer... Socratic. And also, how can we better the token usage and tell about
the... and other things in detail in read me, like, kind of we need to tell and
market it like that. because many people have commented in read me even though
it has got seventy five stars on GitHub."

**Decided:** socratic ships, readback does not. Checking git before answering
turned up the real finding: only the original 697-question bank was ever
committed. The packs system, the core/full tiering, references/, agents/, and
PROMPT_LITE.md are all untracked, so the 75 stars are on a version that has
none of the work being discussed. The README problem is downstream of that —
the published README is the old one.

**I did:** rewrote the README opening around "the agent interviews itself, not
you" with a sample output contract, added a "why not just tell the model to
think harder" section, and replaced the vague token-loading section with
measured tiktoken numbers — 2,296 for SKILL.md, ~2,800 typical, 28,908 total,
against Anthropic's 2,255 median. Fixed a truncated sentence that was visible
on the repo page.

**Not taken:** rewriting the README wholesale. Its structure and voice are
already good; the gaps were a weak opening, no numbers, and one broken
sentence. A rewrite would have cost the author's voice for no gain.

Files: README.md, .readback/index.md, .readback/log.md
