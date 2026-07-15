---
name: dependency-impact
description: The Dextall project dependency graph and how to reason about the blast radius of a change. Use when reviewing or deciding what to re-test after editing a project.
---

# Dependency impact / blast radius

Arrow = "depends on". Higher tiers depend on lower ones. A change in a low-tier
project can affect everything above it.

```
StandaloneMain2 (app, VB.NET)     SplitPanelGenerator     TestSuite (independent)
        │                                │
        └───────────────┬────────────────┘
                        ▼
                  PanelBuilder (VB.NET)
                        ▼
               PanelDrawingBuilder2
                        ▼
   PanelDrawingCalculator2  MaterialDB  TemplatesLibrary  PanelInputDataReader
                        └─────────┬──────────┴────────────────────┘
                                  ▼
                        ★ PanelCalculator2  (central hub, C#)
                                  ▼
     PanelGeneratorInputDTOChecker   PanelGeneratorInputDTOCorrector
                                  ▼
     PanelGenerator_Variables   PanelGeneratorCladdingCalculator
                                  ▼
               PanelGeneratorDesignLimitsVariables
                                  ▼
                     PanelGeneratorInputDTO
                                  ▼
                ▼ PanelGeneratorVersioning  (foundation — depends on nothing)
```

## How to use it in review / testing

1. **Locate the changed project** in the graph.
2. **Everything above it** is potentially affected — call it out.
3. Pick the test scope accordingly:
   - Change in `PanelCalculator2` or below → run `PanelCalculatorTests` (unit) and
     strongly consider `TestSuite` (e2e), since most of the app flows through the hub.
   - Change in `PanelGeneratorVersioning` (foundation) → build the **whole** `.sln`
     and run tests broadly; almost everything depends on it.
   - Change only in `StandaloneMain2` / `PanelBuilder` (top, VB.NET) → e2e
     (`TestSuite`) is the most relevant check.
4. `TestSuite` is independent of the app's internal tiers but drives the app
   end-to-end, so it catches integration regressions the unit tests miss.

Exact per-project dependency lists live in the `.sln` `ProjectDependencies` sections.
