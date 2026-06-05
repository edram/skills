# Plan: git-commit-convention Skill

## Context

Creating the first skill in this repo: a reference skill for writing structured git commit messages following Conventional Commits 1.0.0. Sources: Angular commit guidelines (generic parts only), conventionalcommits.org spec, and two existing skill examples for structure reference.

## File Structure

```
skills/
  git-commit-convention/
    SKILL.md
    references/
      core-format.md        ← grammar, header/body/footer rules
      core-types.md         ← all types with SemVer correlation + examples
      features-breaking.md  ← ! syntax and BREAKING CHANGE footer
      features-footers.md   ← footer tokens, issue refs, common tokens
```

## Files to Create

### `skills/git-commit-convention/SKILL.md`

Frontmatter:
- `name: git-commit-convention`
- `description:` trigger on "write a commit", "commit message", "conventional commits", or any commit message generation request
- `metadata.author: edram`, `version: 2026.06.05`, `source: https://www.conventionalcommits.org/en/v1.0.0/`

Content:
- One-paragraph overview: Conventional Commits 1.0.0 — structured, semantic commit messages that map to SemVer
- Quick-format block: `<type>[optional scope]<!>: <description>`
- Types summary table (type → SemVer impact → purpose) — 11 types
- Core and Features reference tables pointing to the 4 reference files

---

### `references/core-format.md`

Full message grammar:
```
<type>[optional scope]<!>: <description>
<BLANK LINE>
[optional body]
<BLANK LINE>
[optional footer(s)]
```

Header rules:
- Imperative present tense ("add", not "added" or "adds")
- Lowercase first letter after the colon
- No trailing period
- ≤72 characters (aim for ≤50 in description)

Body rules:
- Explain the **why**, not the what
- Separated from header by one blank line
- Wrap at 72 characters

Footer rules:
- One blank line after body (or header if no body)
- Token format: `Token: value` or `Token #value`
- Token names use hyphens instead of spaces (e.g., `Co-Authored-By`)
- `BREAKING CHANGE` is the one case-sensitive exception

Examples (minimal + complex):
```
fix: prevent racing condition in request queue

feat(parser): add support for array destructuring

fix: correct timeout handling

Reviewed-by: Alice
Refs: #42
```

---

### `references/core-types.md`

Full table: type | SemVer | When to use

| Type | SemVer | Use for |
|------|--------|---------|
| `feat` | MINOR | New feature visible to users/API consumers |
| `fix` | PATCH | Bug fix |
| `perf` | PATCH | Performance improvement with no API change |
| `refactor` | — | Code restructuring, no feature or fix |
| `style` | — | Formatting, whitespace, missing semicolons — no logic change |
| `test` | — | Adding or correcting tests |
| `docs` | — | Documentation only |
| `build` | — | Build system or external dependency changes |
| `ci` | — | CI configuration files and scripts |
| `chore` | — | Miscellaneous maintenance (no source/test change) |
| `revert` | — | Reverts a previous commit |

Key point: only `feat` and `fix` have defined SemVer semantics in the spec; others are community convention. `revert` header: `revert: <original header>`, body must include `This reverts commit <SHA>`.

---

### `references/features-breaking.md`

Two syntaxes — both indicate MAJOR SemVer bump:

**Exclamation mark (inline):**
```
feat!: remove POST /users/legacy
feat(api)!: rename config key `timeout` to `timeoutMs`
```

**BREAKING CHANGE footer:**
```
feat: allow config object to extend other configs

BREAKING CHANGE: `extends` key now merges rather than replaces
```

Both can be combined:
```
feat(auth)!: drop support for JWT v1 tokens

BREAKING CHANGE: JWT v1 tokens are rejected. Migrate to v2 using
the /auth/migrate endpoint before upgrading.
```

Key points:
- `BREAKING CHANGE` MUST be uppercase and at the start of a footer token
- A commit with `BREAKING CHANGE` footer and no `!` is still a breaking change
- Body is strongly recommended when breaking — document migration path

---

### `references/features-footers.md`

Footer format rules:
- Each token on its own line, no blank lines between footers
- `Token: value` (colon + space) for prose values
- `Token #value` (space + hash) for reference numbers
- Tokens use hyphens not spaces (except `BREAKING CHANGE`)
- All tokens case-insensitive except `BREAKING CHANGE`

Common tokens:
```
Closes #123          ← closes GitHub/GitLab issue on merge
Fixes #123           ← same intent as Closes
Refs #456            ← reference without closing
Co-Authored-By: Name <email@example.com>
Reviewed-by: Name
BREAKING CHANGE: description of what broke and how to migrate
```

Example with multiple footers:
```
fix: handle null pointer in user serializer

Closes #88
Co-Authored-By: Alice <alice@example.com>
Reviewed-by: Bob
```

---

## Verification

1. Check all files exist under `skills/git-commit-convention/`
2. Validate SKILL.md frontmatter is valid YAML
3. Confirm reference links in SKILL.md tables match actual filenames
4. Verify the skill can be discovered by running: `npx skills@latest add edram/skills` (after pushing)
