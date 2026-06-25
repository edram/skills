# Plan: add a `tdd` skill

## Context

The user wants a test-driven-development skill for this personal skills repo, synthesized
from four public TDD skills:

- `mattpocock/skills` — behavior-through-public-interface, vertical tracer bullets, anti-patterns
- `obra/superpowers` — the Iron Law, "watch it fail", rationalizations table, red flags
- `NousResearch/hermes-agent` — adaptation of obra: RED-GREEN-REFACTOR enforcement, anti-patterns
- `affaan-m/ECC` — workflow-heavy: test types (unit/integration/E2E), coverage targets, RED gate, git checkpoints, evidence report

The repo already has a strong analog — the `coding` skill — a "discipline / constraints"
skill with the same shape (overview + principle bullets + Core/Features reference tables).
The new skill mirrors that shape. All authoring follows the repo's `skill-writer` skill;
content stays in English to match every existing skill.

The value-add over what an LLM already knows about TDD is the **discipline enforcement**
(test-first as non-negotiable, watch-it-fail, delete-code-written-first) and the distilled
distinctions (behavior vs implementation, vertical vs horizontal, the rationalization
rebuttals) — not a generic TDD tutorial.

## Skill name

`tdd` — short, single-word, matches the user's stated preference for concise skill names
and the `mattpocock` precedent. (Alternative `test-driven-development` is more discoverable
but verbose.)

## Structure

`skills/tdd/SKILL.md` — router: overview paragraph (synthesis + source attribution),
~5 principle bullets, Core table, Features table.

**Core** (must-know fundamentals):

| File | Covers |
|------|--------|
| `core-cycle` | RED→GREEN→REFACTOR mechanics: write one failing test → **watch it fail** (mandatory verify-RED) → minimal code to pass → verify GREEN → refactor only on green. Per-cycle checklist. Brief refactor-candidate note (extract duplication, deepen modules) — kept short since the model knows it. |
| `core-discipline` | The Iron Law (no production code without a failing test first); delete-and-restart for code written before its test ("delete means delete"); why order matters (tests-after prove nothing); the rationalization→rebuttal table; red-flag list = "stop and start over". The signature, highest-value content. |
| `core-tracer-bullets` | Vertical slices via tracer bullets vs horizontal slicing (one test → one impl → repeat); the planning step — confirm the public interface and which behaviors matter with the user; you can't test everything. |

**Features** (techniques that improve quality):

| File | Covers |
|------|--------|
| `features-good-tests` | Test observable behavior through public interfaces, not implementation details; the brittle-test smell (breaks on refactor though behavior is unchanged); anti-patterns (testing mocks, implementation details, happy-path only, brittle tests); mock only when unavoidable. |
| `features-coverage` | Test types (unit / integration / E2E) and where to spend effort; edge/error/boundary coverage and coverage targets; the RED gate (a test only counts once compiled, executed, and failed for the intended reason — runtime vs compile-time RED); optional git checkpoints + evidence report for a verifiable history. |

3 core + 2 features = 5 references, matching the `coding` skill exactly.

## Registration (per AGENTS.md checklist)

1. Create `skills/tdd/SKILL.md` + the 5 `references/*.md` files.
2. Symlink into both harness discovery dirs with **relative** targets (commit the links):
   - `.claude/skills/tdd -> ../../skills/tdd`
   - `.agents/skills/tdd -> ../../skills/tdd`
3. Add `tdd` to the Skills table — alphabetically after `skill-writer`? No: alphabetical order
   is `coding`, `git-commit-convention`, `skill-writer`, `tdd` → append last — in **both**
   `README.md` and `README.zh-CN.md` (canonical name only).

## Verification

- Frontmatter: every SKILL.md / reference has valid `name` + `description`; `name` matches filename.
- Router links resolve to the 5 reference files; table topics match filenames.
- Symlinks resolve: `ls -l .claude/skills/tdd .agents/skills/tdd` show `-> ../../skills/tdd`,
  and the target files are readable through the link.
- README tables (both languages) list `tdd`.
- Sanity-read SKILL.md alone — does it route a reader to the right reference for a given TDD question?
