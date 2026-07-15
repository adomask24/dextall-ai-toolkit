---
description: Generate or refresh documentation (XML comments, architecture diagram) for a Dextall project.
argument-hint: [project path or area]
---

Generate/refresh documentation for: $ARGUMENTS

Delegate to the `doc-writer` agent. Depending on what was requested:

- **Code docs** → add/update XML doc comments (`xml-doc-comments` skill) on public
  types and members of the target project. Read the implementation first; state
  units and coordinate frames explicitly.
- **Architecture** → update the dependency diagram / notes (`architecture-docs`
  skill), verified against `StandaloneMain2.sln` `ProjectDependencies`.

Prefer updating existing docs over creating duplicates. Keep it concise and
grounded in the actual code. If no area was given, ask which project to document.
