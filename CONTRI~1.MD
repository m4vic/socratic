# Contributing

## What makes a good question

A question earns its place if a **wrong answer changes the design.** "What color should the button be?" doesn't. "Is this data regulated?" does.

Good questions tend to:

- Come from a real incident. If you shipped a bug because nobody asked something, that's the best possible source.
- Be answerable yes/no or with one short phrase.
- Have an obvious sensible default that the agent can propose.
- Be concrete. "Is the system robust?" is useless; "Is there a timeout on every outbound call?" is actionable.

Avoid:

- Questions that only apply to one company's stack.
- Anything that encodes a trend rather than a durable principle.
- Duplicates of a question already in another domain file — cross-reference instead.

## File structure

Every domain file follows the same shape:

```markdown
# NN — Domain Name

**Load when:** <one line describing the trigger>

## Priority 1
1. ...   ← the 3-6 questions to ask if you only ask a few

## <Theme>
7. ...

## Verification
- <checks to run after the build, not before>
```

Numbering is continuous through the file. Verification items are bullets, not numbers, so the question count stays accurate.

## Adding a domain

1. Create `questions/NN-name.md` following the shape above.
2. Add a row to the routing table in `SKILL.md`.
3. Add a row to the table in `README.md` with the question count.
4. If it belongs in a common combination, add it to the preset stacks in both files.

Count questions with:

```bash
grep -cE '^[0-9]+\. ' questions/NN-name.md
```

## Editing existing questions

Corrections are welcome, particularly where a question assumes a practice that's since been superseded. Say in the PR what changed and why — "this recommends X but Y is now standard because Z."

## Scope

This repo is questions and routing logic. It isn't a place for code samples, framework tutorials, or opinionated architecture guides. If a question needs three paragraphs of explanation to make sense, it's probably the wrong question.
