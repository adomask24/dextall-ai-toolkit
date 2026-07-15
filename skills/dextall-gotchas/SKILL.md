---
name: dextall-gotchas
description: Known traps in the Dextall codebase (duplicate project folders, dual AdditionalPinDTO classes, the foundation project). Use when editing or reviewing to avoid the common mistakes.
---

# Dextall gotchas

Read this before editing or reviewing — these are the mistakes that pass a build
but break things or waste time.

## 1. Duplicate project copies under `Dextall-Generator-Engine`

The `Dextall-Generator-Engine\` folder contains **duplicate copies** of several
projects (e.g. `Dextall-Generator-Engine\PanelCalculator2\...`).

- ✅ Edit the **top-level sibling** paths (e.g. `PanelCalculator2\...`) — that is
  what `StandaloneMain2.sln` actually references.
- ❌ Editing the copy under `Dextall-Generator-Engine\...` compiles nothing in the
  main solution and the change appears to "do nothing".

When reviewing a diff, check the file path prefix.

## 2. Two `AdditionalPinDTO` classes

- `PanelGeneratorInputDTO.AdditionalPinDTO` — `Position` is a **`CoordDTO`**.
- `PanelGeneratorInputDTO.CustomConfigurator.CustomObjects.AdditionalPinDTO` —
  `Position` is a **`double`**.
- `CustomConfigDTO.AdditionalPins` uses the **`CustomObjects`** one.

Confirm which type is in scope before touching `Position`; the wrong one will
either not compile or silently mean something different.

## 3. `PanelGeneratorVersioning` is the foundation

Nearly everything depends on `PanelGeneratorVersioning`. A change there has the
widest blast radius in the solution — build the whole `.sln` and run tests broadly
(see `dependency-impact`).

## 4. `PanelCalculatorTests` is not in the main solution

`PanelCalculatorTests` is **not** part of `StandaloneMain2.sln` — VS Test Explorer
won't show it. Run it from its own folder (see `run-unit-tests`).

## 5. Git layout

Each top-level folder is its **own git repo**; there is no repo at
`C:\Projects\Dextall` root and `StandaloneMain2\` itself is not versioned. Projects
are normally on `master`. Commit in the correct sub-repo.
