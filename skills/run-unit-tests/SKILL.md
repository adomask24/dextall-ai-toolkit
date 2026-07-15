---
name: run-unit-tests
description: Run the PanelCalculator2 unit tests (PanelCalculatorTests, MSTest, net48). Use to verify PanelCalculator2 logic and geometry after a change.
---

# Run unit tests — `PanelCalculatorTests`

`PanelCalculatorTests` is an **MSTest** project (`MSTest 3.6.4`, net48) that tests
`PanelCalculator2` — including **geometry** (bounding boxes, contour lines, faces,
work points). It lives in the `PanelCalculator2` repo and is **NOT** part of
`StandaloneMain2.sln`, so Visual Studio's Test Explorer won't show it when that
solution is open.

## Run

```powershell
cd C:\Projects\Dextall\PanelCalculator2\PanelCalculatorTests

dotnet test --nologo                                                   # all tests
dotnet test --filter "FullyQualifiedName~PL1BoundingBoxTest"           # one class
dotnet test --filter "FullyQualifiedName~PL1"                          # a namespace/area
dotnet test --logger "console;verbosity=detailed"                      # per-test output
```

A full run (build + test) takes on the order of **~15 s**; the tests themselves run
in well under a second.

## ⚠️ Build-order dependency (common failure cause)

`PanelCalculatorTests.csproj` pulls two dependencies as **prebuilt DLLs via
`HintPath`**, not project references:

```
..\..\PanelGeneratorInputDTO\PanelGeneratorInputDTO\bin\Debug\net48\PanelGeneratorInputDTO.dll
..\..\PanelGenerator_Variables\PanelGenerator_Variables2\bin\Debug\net48\PanelGenerator_Variables.dll
```

If those DLLs are **missing or stale**, the test build fails or tests run against
old code. Before a clean run, build those projects (or the whole `.sln`) first —
see the `build-solution` skill. `PanelCalculator2` itself is a normal
`ProjectReference` and builds automatically.

## Reading the result

Output ends with a summary line, e.g.:

```
Failed!  - Failed: 3, Passed: 6, Skipped: 0, Total: 9, Duration: 674 ms
```

- Report the counts and, for each failure, the **assertion message + the failing
  frame** (test file:line, and whether the throw is in test code or production
  code — the stack trace distinguishes them).
- Float assertions use a tolerance (`TOL = 1e-6`); an "Expected a difference no
  greater than <1E-06>…" message is a geometry mismatch, not a flaky test.

## Baseline caveat

The suite is **not guaranteed green** — some tests may be WIP or failing against
current code. Establish the current pass/fail baseline before blaming your change:
run the suite on the unmodified code first, or check whether the failing test
touches code you changed. Do **not** edit production code just to make a test
pass; report and let the human decide.

## Notes

- `ProfileTUTests.cs` is a **commented-out template** (its `[TestClass]` /
  `[TestMethod]` are disabled) kept as a scaffold for new tests — it contributes
  no live tests.
- To add tests, see the `write-unit-test` skill.
