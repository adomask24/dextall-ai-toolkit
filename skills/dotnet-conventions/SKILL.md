---
name: dotnet-conventions
description: VB.NET vs C# conventions and the per-project language map for the Dextall solution. Use when writing or reviewing code to match the surrounding style.
---

# Dextall .NET conventions

All projects target **.NET Framework 4.8**. The solution mixes two languages —
match the language and idiom of the project you are editing.

## Language map

- **VB.NET**: `StandaloneMain2` (the WinForms app; entry point `Form1.vb` /
  `Form1.Designer.vb`) and `PanelBuilder`.
- **C#**: most other projects, including the central hub `PanelCalculator2`,
  `PanelDrawingBuilder2`, `PanelGeneratorInputDTO`, `PanelGeneratorVersioning`, etc.

## When reviewing / writing

- Write code that reads like the surrounding code: match its naming, comment
  density, and idioms. Do not import C# style into VB files or vice-versa.
- Keep new public surface minimal — expose only what callers need. Higher-tier
  projects depend on lower ones, so a new `public` member is a new contract.
- Respect the existing error-handling style of the project; don't introduce
  silent `catch`-and-continue where the code otherwise propagates errors.
- `Panel` and `InputData` live in the **global namespace** — don't wrap them or
  add namespace qualifiers.

## Related

- Traps that aren't about style: `dextall-gotchas`.
- Deciding what a change affects: `dependency-impact`.
