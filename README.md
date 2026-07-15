# Dextall AI Toolkit

In-house [Claude Code](https://code.claude.com/docs) **agents** and **skills** for the
Dextall panel generator (`C:\Projects\Dextall`). Packaged as a Claude Code
**plugin + marketplace** so it can be installed from any project on your machine.

## What's inside

### Agents (`agents/`)
| Agent | Purpose |
|---|---|
| `test-runner` | Builds `PanelCalculator2` and runs its unit tests, interprets failures |
| `test-author` | Writes new unit tests following the project's real conventions, then runs them |
| `code-reviewer` | Reviews a diff against Dextall conventions, gotchas and the dependency graph |
| `doc-writer` | Writes XML doc comments, architecture docs and READMEs |

### Skills (`skills/`)
| Skill | Category | Purpose |
|---|---|---|
| `build-solution` | testing | `dotnet build` the whole `.sln` or a single project |
| `run-unit-tests` | testing | Run `PanelCalculatorTests` (MSTest, net48) |
| `write-unit-test` | testing | Conventions & template for new `PanelCalculatorTests` tests |
| `dotnet-conventions` | review | VB.NET vs C# idioms, per-project language map |
| `dextall-gotchas` | review | Duplicate-folder, dual-`AdditionalPinDTO`, foundation-project traps |
| `dependency-impact` | review | Project dependency graph & blast-radius of a change |
| `xml-doc-comments` | docs | XML doc-comment style for public APIs |
| `architecture-docs` | docs | Dependency diagrams (Mermaid) & architecture notes |

## Install

From **any** project directory (one-time):

```
/plugin marketplace add adomask24/dextall-ai-toolkit
/plugin install dextall-toolkit@dextall-ai-toolkit
```

> Once installed, the agents are available in every project via `@`-mention
> (e.g. `@dextall-toolkit:test-runner`). Their file access is **not** limited to
> this repo — they operate on whatever project directory Claude Code was launched
> in (e.g. `C:\Projects\Dextall`).

### Alternative: global install without the plugin system

Clone this repo and copy (or symlink) `agents/` and `skills/` into `~/.claude/`.

## Layout

```
dextall-ai-toolkit/
├── .claude-plugin/
│   ├── plugin.json          # plugin manifest
│   └── marketplace.json     # marketplace catalog (this repo = both)
├── agents/                  # subagent definitions (.md)
├── skills/                  # <skill>/SKILL.md (+ optional scripts/, references/)
├── commands/                # slash commands (/review-diff, /gen-docs)
└── docs/                    # contributing notes
```

## Notes

Skills reference the Dextall paths under `C:\Projects\Dextall`. If your checkout
lives elsewhere, adjust the paths in the affected `SKILL.md` files.
