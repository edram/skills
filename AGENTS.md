# AGENTS.md

## Repository Structure

A personal collection of agent skills. Each skill is a self-contained, pure-markdown
document set — no manifest, config, or tooling files are required.

```
.
├── AGENTS.md                     # This file — house rules for agents
├── CLAUDE.md                     # → symlink to AGENTS.md
├── README.md / README.zh-CN.md   # Human-facing index
│
├── skills/                       # Canonical source — ALL skills live here
│   └── {skill-name}/
│       ├── SKILL.md              # Index: metadata + overview + reference table
│       └── references/
│           └── *.md              # One concept per file
│
├── .claude/skills/               # Harness discovery dir — symlinks into ../../skills/
│   └── {skill-name} → ../../skills/{skill-name}
├── .agents/skills/               # Same, for the .agents harness
│   └── {skill-name} → ../../skills/{skill-name}
│
└── plans/                        # Scratch planning docs (not shipped as skills)
```

## Creating / Updating Skills

- **ALWAYS use the `skill-writer` skill.** It is the source of truth for this repo's
  format, frontmatter, structure, and writing guidelines — don't hand-roll a skill or
  duplicate those rules elsewhere.
- **`skills/` is canonical.** `.claude/skills/` and `.agents/skills/` are convenience
  mirrors of it via symlink, not separate skills. Do **not** author per-skill alias or
  symlink "skills."
- **List only canonical skills** in public inventories (e.g. the `README.md` Skills
  table) — exclude the mirror symlinks.

### Registration checklist

When adding a new skill `<skill-name>`:

1. Create `skills/<skill-name>/SKILL.md` (+ any `references/*.md`).
2. Mirror it into both harness discovery dirs so each can find it (run from repo root):
   ```bash
   ln -s ../../skills/<skill-name> .claude/skills/<skill-name>
   ln -s ../../skills/<skill-name> .agents/skills/<skill-name>
   ```
   The target is **relative** (`../../skills/...`) so the links resolve on any clone;
   commit the symlinks themselves, not copies of the files.
3. Add the skill to the `README.md` Skills section — canonical name only, alphabetical.
