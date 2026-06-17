# Plan: `skill-writer` skill

## Context

This repo is a personal **skills library** (`skills/{name}/SKILL.md` + `references/*.md`).
Right now it has one skill (`git-commit-convention`) and a house spec in `AGENTS.md`,
but no skill that captures *how to author skills* themselves. The user wants a
general-purpose `skill-writer` skill so that any future "create/modify a skill"
request loads concrete, repo-correct guidance instead of relying on memory.

The skill synthesizes three sources into this repo's conventions:
- Anthropic best-practices doc (concise, descriptions, progressive disclosure, degrees of freedom, anti-patterns, eval-driven dev)
- Anthropic `skill-creator` (capture-intent → research → author → evaluate → iterate loop)
- Sentinel/Sentry `skill-writer` (mode selection, precision passes, deterministic reference-routed authoring)

**Important constraint:** the skill must teach **this repo's format** (from `AGENTS.md`),
not Anthropic's generic `# heading` / `FORMS.md` layout. Repo format = SKILL.md with a
short overview + `## Core` / `## Features` tables linking to `references/{category}-{topic}.md`,
each reference using `## Usage` / `## Key Points` + a source-reference HTML comment.
Pure markdown, no scripts/tooling (per `AGENTS.md` and the user's "lightweight" choice).

## Deliverable

New directory `skills/skill-writer/` conforming to the repo spec:

```
skills/skill-writer/
  SKILL.md
  references/
    core-structure.md
    core-frontmatter.md
    core-workflow.md
    features-progressive-disclosure.md
    features-writing-style.md
    features-anti-patterns.md
    features-evaluation.md
```

Model the structure and tone on the existing `skills/git-commit-convention/` skill
(read `SKILL.md` and `references/core-format.md` there for exact house style).

## File-by-file content

### `SKILL.md`
- Frontmatter: `name: skill-writer`; third-person `description` with what + when triggers
  (e.g. "Author and revise agent skills in this repository's format. Use when asked to
  create a new skill, add/edit references, fix a SKILL.md, or improve skill discovery.");
  `metadata.author: edram`, `version: 2026.06.17`, `source:` the best-practices URL.
- Overview paragraph: skills = progressive-disclosure docs; metadata always loaded,
  body on trigger, references on demand. State the prime directive: **SKILL.md is a
  router, not an encyclopedia; only add what the model doesn't already know.**
- Short bullets of key principles + the repo path reminder (`skills/{name}/`, NOT repo root —
  cross-links the existing memory note).
- `## Core` table → core-structure, core-frontmatter, core-workflow.
- `## Features` table → features-progressive-disclosure, features-writing-style,
  features-anti-patterns, features-evaluation.

### `references/core-structure.md`
Repo directory layout + both file templates (SKILL.md index, references/*.md), the
`{category}-{topic}.md` naming rule, `core-`/`features-` category prefixes, and naming
conventions for the skill name (gerund/noun-phrase, lowercase-hyphen). This is the
"produce a conforming skeleton" reference.

### `references/core-frontmatter.md`
The discovery-critical rules: `name` (≤64 chars, lowercase/numbers/hyphens, no XML,
no reserved words `anthropic`/`claude`); `description` (≤1024, non-empty, **third person**,
state *what it does* AND *when to use it*, include concrete trigger terms). Good vs bad
description examples. Note repo's `metadata` block (author/version/source).

### `references/core-workflow.md`
End-to-end process for both **create** and **modify**:
1. Resolve target — new skill vs edit existing; inspect prior art (read sibling skills).
2. Capture intent — what it does, trigger conditions, expected output.
3. Research/synthesize sources — pull only agent-actionable content; ignore what LLMs know.
4. Author — write SKILL.md router first, then references one concept per file.
5. Review — run the anti-patterns checklist; bump `version` on edits.
Include a copyable checklist (per best-practices "workflow" pattern).

### `references/features-progressive-disclosure.md`
SKILL.md-as-router, references **one level deep** (no nested refs), split when body
approaches ~500 lines, table-of-contents for reference files >100 lines, domain-based
organization to avoid loading irrelevant context.

### `references/features-writing-style.md`
Conciseness ("does this justify its token cost?"), **degrees of freedom** (high/medium/low
with the narrow-bridge vs open-field analogy), consistent terminology, and the
examples/templates/checklist patterns. Emphasize trusting model capability over heavy
MUST/NEVER language (theory-of-mind point from skill-creator).

### `references/features-anti-patterns.md`
Quick do/don't + final review checklist: forward-slash paths only, avoid offering many
options (give one default + escape hatch), no time-sensitive info (use "old patterns"
collapsible), no deeply nested references, fully-qualified MCP tool names
(`Server:tool`), don't assume packages installed. Ends with the pre-ship checklist
condensed for this repo.

### `references/features-evaluation.md`
**Lightweight, no scripts.** Eval-driven mindset: identify gaps by running the task
without the skill first; write 2-3 concrete trigger scenarios (should-trigger and
should-NOT-trigger) to sanity-check the `description`; verify the skill actually changes
behavior; iterate description wording to cut false positives/negatives. Frame as a manual
review loop, not a tooling harness.

## House-style rules to follow (from `AGENTS.md` + existing skill)
- Each `references/*.md`: frontmatter (`name`, `description`) → `## Usage` (code-first) →
  `## Key Points` → trailing `<!-- Source references: ... -->` comment.
- Be concise; avoid creating extra reference files beyond the seven above.
- Always include concrete examples/code blocks; explain *when/why*.
- Use forward slashes in all in-content paths (the skill teaches this — must practice it).

## Verification
1. Structural: confirm tree matches `skills/skill-writer/SKILL.md` + 7 references; every
   `## Core`/`## Features` table link resolves to an existing file (one level deep).
2. Frontmatter lint: `name: skill-writer` valid (lowercase-hyphen, no reserved word);
   `description` is third person and names triggers; every reference has `name` + `description`.
3. Self-consistency: the skill obeys its own rules (no nested refs, forward slashes,
   no time-sensitive text, no scripts).
4. Dogfood trigger test: pose 2-3 prompts ("create a skill for X", "fix this SKILL.md",
   "improve a skill's description") and confirm the `description` would plausibly trigger;
   pose 1 unrelated prompt that should NOT trigger.
5. Cross-check against the best-practices checklist (description specific, body <500 lines,
   one-level refs, consistent terminology, concrete examples).
