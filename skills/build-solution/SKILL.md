---
name: build-solution
description: Build the Dextall solution or a single project with dotnet build. Use before running tests or to check that a change compiles.
---

# Build the Dextall solution

The primary solution is `StandaloneMain2.sln`. All projects target **.NET Framework 4.8**.

## Whole solution

```powershell
dotnet build "C:\Projects\Dextall\StandaloneMain2\StandaloneMain2\StandaloneMain2.sln" -c Debug
```

## A single project

```powershell
dotnet build "C:\Projects\Dextall\PanelCalculator2\PanelCalculator2\PanelCalculator2.csproj" -c Debug
```

Swap the path for the project you need. Most projects are **siblings** under
`C:\Projects\Dextall\` (the `.sln` references them with `..\..\`).

## Notes

- Building a leaf/low-tier project (e.g. `PanelGeneratorVersioning`) is fast and a
  good first check. Building the whole `.sln` validates the full dependency chain.
- ⚠️ Edit the **top-level sibling** projects (e.g. `PanelCalculator2\...`), not the
  duplicate copies under `Dextall-Generator-Engine\...`. The `.sln` references the
  siblings. See the `dextall-gotchas` skill.
- A red build → stop and report the compiler errors verbatim before running tests.
- **Before running `PanelCalculatorTests`**, make sure `PanelGeneratorInputDTO` and
  `PanelGenerator_Variables` are built — that test project consumes them as prebuilt
  DLLs via `HintPath` (not project references), so stale/missing DLLs cause test
  build failures or tests running against old code. Building the whole `.sln`
  covers this. See the `run-unit-tests` skill.
