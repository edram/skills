# CLAUDE.md

This file provides guidance to AI agents working with code in this repository.

## Repository Purpose

Personal collection of agent skills for AI coding assistants, distributed via `npx skills@latest add edram/skills`. Skills are standalone Markdown files that help AI agents understand tools, frameworks, and coding conventions — focused on practical patterns rather than user-facing documentation.

## Repository Structure

```
skills/
  {skill-name}/
    SKILL.md              # Skill index: metadata + overview + reference list
    references/
      {category}-{topic}.md   # One concept per file
```

A skill only needs to conform to the skill file spec below — no additional configuration, manifest, or tooling files are required.

## Skill File Format

### `SKILL.md` (index)

```markdown
---
name: {skill-name}
description: {one-line description used for discovery — when to invoke this skill}
metadata:
  author: edram
  version: YYYY.MM.DD
  source: {optional URL if derived from external docs}
---

Brief overview paragraph.

Key concepts or features as a bullet list.

> Version note if applicable.

## Core

| Topic | Description | Reference |
|-------|-------------|-----------|
| Topic Name | Short description | [core-topic](references/core-topic.md) |

## Features

| Topic | Description | Reference |
|-------|-------------|-----------|
| Topic Name | Short description | [features-topic](references/features-topic.md) |
```

### `references/*.md` (individual concepts)

```markdown
---
name: {concept-name}
description: {one-line summary}
---

## Usage

{Practical code examples — the primary content}

## Key Points

- Concise bullets on non-obvious behavior
- Edge cases, gotchas, defaults

<!--
Source references:
- {source-url}
-->
```

## Writing Guidelines

- **Focus on agent capabilities and practical usage patterns.** Ignore user-facing guides, introductions, get-started, and install guides.
- **Ignore what LLMs already know.** Skip content an agent is already confident about from training data.
- **Rewrite for agents, not copy.** Synthesize docs for LLM consumption — restructure around what an agent needs to *do*.
- **Be concise.** Remove fluff; keep essential information. Avoid creating too many reference files.
- **One concept per file.** Name files `{category}-{topic}.md` (e.g., `core-config.md`, `features-mocking.md`).
- **Always include code.** Provide working examples; explain *when and why*, not just *how*.

