# Contributing to the Dextall AI Toolkit

## Adding an agent

Create `agents/<name>.md` with YAML frontmatter:

```markdown
---
name: my-agent
description: One or two sentences on WHAT it does and WHEN to use it. This is what
  Claude reads to decide whether to delegate — be specific about triggers.
tools: Read, Grep, Glob, Bash, Edit    # omit to inherit all tools
# model: sonnet                        # optional override
---

System prompt: the agent's role, workflow, and rules.
```

Keep the body focused: role, a numbered workflow, and hard rules. Point the agent
at the skills it should load rather than duplicating that content here.

## Adding a skill

Create `skills/<name>/SKILL.md`:

```markdown
---
name: my-skill
description: What knowledge/procedure this provides and when to load it.
---

# Title

The procedure, commands, conventions, or reference material.
```

Optional supporting files go alongside: `scripts/`, `references/`.

## Principles

- **Agents do; skills know.** An agent orchestrates; a skill is reusable knowledge
  an agent (or the main session) loads on demand. One agent can use many skills.
- **Ground everything in the real codebase.** Commands and paths must match
  `C:\Projects\Dextall`. If a path changes, update the affected `SKILL.md`.
- **No secrets.** Nothing under version control should contain credentials; see
  `.gitignore`. Personal Claude settings belong in `settings.local.json` (ignored).
- Prefer editing an existing agent/skill over adding a near-duplicate.

## After changing plugin contents

Consumers refresh with:

```
/plugin marketplace update dextall-ai-toolkit
```
