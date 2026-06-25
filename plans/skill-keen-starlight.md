# Plan: `coding` skill

## Context

The user wants a skill that **constrains an AI agent while it writes or edits code** — biasing
toward less code, tighter scope, and verified results. It synthesizes two external sources:

- **Karpathy guidelines** (4 principles): Think Before Coding · Simplicity First · Surgical
  Changes · Goal-Driven Execution.
- **Sentry code-simplifier** (5 principles): Preserve Functionality · Apply Project Standards ·
  Enhance Clarity · Maintain Balance (don't over-simplify) · Focus Scope (only touched code).

Plus an explicit **comment discipline** requirement: code should be self-explanatory; comments
explain *why*, not *how*; avoid unnecessary comments.

**Decisions made with the user:**
- Build the skill now, in **English**, named **`coding`** (invoked as `/coding`). Single word kept
  intentionally short; discovery is driven by the `description`, not the name.

## Authoring rules (from `skill-writer`)

- `skills/` is canonical; SKILL.md is a **router**, depth lives in `references/`; references are **one
  level deep**; forward slashes only; no time-sensitive text.
- Add only what the model doesn't already know — keep it lean (this skill is *about* minimalism, so
  it must practice it).
- `description` = third person, what + when, with concrete trigger terms (the discovery driver — and
  doubly important here because the name `coding` is generic).
- File names `{category}-{topic}.md`; reference `name:` == filename; `metadata.version` = `2026.06.25`.

## Files to create

```
skills/coding/
  SKILL.md
  references/
    core-simplicity.md
    core-scope.md
    core-comments.md
    features-think-first.md
    features-verify.md
```

### `SKILL.md`
- **Frontmatter** — `name: coding`; `description` (third person, what + when, triggers:
  *write code, implement, edit, refactor, simplify, clean up, add a function/feature, review for
  over-engineering*); `metadata.author: edram`, `version: 2026.06.25`. Source URLs go in a closing
  `<!-- Source references -->` comment (two sources, so no single `source:` field).
- **Overview** — one paragraph + a short key-principles bullet list.
- **`## Core`** table (constraints on the code itself) and **`## Features`** table (workflow around
  writing), per repo house format.

### Core references (must-know constraints)
- **`core-simplicity.md`** — Simplicity first / YAGNI: minimum code for the *stated* problem; no
  speculative features, abstractions for single use, or unrequested config/flexibility; don't handle
  impossible errors. The "would a senior engineer call this overcomplicated?" check. **Balance guard**
  (from Sentry): don't over-simplify into clever-but-unclear code or optimize for line-count.
  *(Karpathy #2 + Sentry #3/#4)*
- **`core-scope.md`** — Surgical changes + focus scope: every changed line traces to the request;
  match existing style/conventions; don't refactor unbroken or adjacent code; clean up only the mess
  *your* edits create; mention pre-existing dead code, don't delete it unasked; preserve behavior when
  refactoring. *(Karpathy #3 + Sentry #1/#2/#5)*
- **`core-comments.md`** — Comment discipline: code self-explanatory; explain **why** (intent,
  non-obvious tradeoff, workaround + link, warning), not **how**; delete obvious/redundant comments;
  don't narrate the code line-by-line. Include a tight good/bad code example. *(user requirement +
  Sentry "remove obvious comments")*

### Features references (workflow around the code)
- **`features-think-first.md`** — Before coding: clarify the goal; state assumptions and ask when
  uncertain; present multiple interpretations rather than choosing silently; propose the simpler
  approach and push back on overcomplication; name confusion before proceeding. *(Karpathy #1)*
- **`features-verify.md`** — Goal-driven execution: turn the task into verifiable success criteria;
  state a brief plan with verification steps; loop until verified (run it / run tests).
  *(Karpathy #4)*

Each reference follows the repo template: `## Usage` (code-first examples) + `## Key Points` +
closing `<!-- Source references -->` comment.

## Registration (AGENTS.md checklist)

1. Create the files above.
2. Mirror into both harness discovery dirs (relative targets, from repo root):
   ```bash
   ln -s ../../skills/coding .claude/skills/coding
   ln -s ../../skills/coding .agents/skills/coding
   ```
   (Windows: repo already commits symlinks for existing skills, so `core.symlinks` is enabled; if a
   link materializes as a copy, recreate it as a symlink before committing.)
3. Add a row to the Skills table in **`README.md`** and **`README.zh-CN.md`** (alphabetical →
   `coding` goes first, before `git-commit-convention`; canonical name only).

## Verification

- **Structure**: `ls -R skills/coding` shows SKILL.md + 5 references; the two mirror symlinks
  resolve (`readlink .claude/skills/coding` → `../../skills/coding`).
- **Self-conformance** (run `coding` against `skill-writer`'s own pre-ship checklist):
  description is third person with triggers and what+when; SKILL.md body lean, depth in references;
  references one level deep; consistent terminology; concrete code examples; forward slashes only;
  filenames `{category}-{topic}.md` matching `name:`; `version` = today.
- **Triggering smoke test**: the `description` should plausibly fire on prompts like "implement X",
  "refactor this", "simplify this function", "add comments here" — confirm the trigger terms are
  present (extra care since the name `coding` is generic).
