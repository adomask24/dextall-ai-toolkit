---
name: run-e2e-tests
description: Run the TestSuite end-to-end test (xUnit, net48) that drives the Form1 UI on an STA thread and validates an Excel report. Use for full pipeline / integration verification.
---

# Run end-to-end tests — `TestSuite`

`TestSuite` (xUnit `2.9.2`, net48) drives the real `StandaloneMain2` `Form1` UI
end-to-end and validates the result via an **Excel report** (`ClosedXML`). It has
`coverlet.collector` wired in for coverage. It **is** part of `StandaloneMain2.sln`
and references `PanelBuilder`, `PanelDrawingBuilder2`, and `StandaloneMain2`.

## Run

```powershell
cd C:\Projects\Dextall\TestSuite
dotnet test --nologo
dotnet test --logger "console;verbosity=detailed"    # see ITestOutputHelper lines
```

## What the test actually does

`RunThroughForm_E2ELogs_Tests.RunThroughForm_ClickButton_CollectsE2ELogs`:

1. Starts an **STA thread**, sets up an `E2ELog` sink (AsyncLocal — must be on the
   same thread as the form), and `Application.Run(new Form1())`.
2. On `Form1.Shown`, finds the button **`btnLouvreGrillDrawings`** by control name
   and calls `PerformClick()`, then closes the form.
3. Waits up to **2 minutes** for the UI work to finish (`"UI test timeout"` if not).
4. Reads input JSON from **`C:\Projects\Dextall\TestJson\*.json`**, writes a
   timestamped report to **`C:\Projects\Dextall\TestJson\TestRun_<yyyyMMdd_HHmmss>\Report_<...>.xlsx`**,
   and asserts `ExcellBuilder.GetTestResult()` (fails on "Excel report validation
   failed. Data mismatches").

## Prerequisites & gotchas

- **Desktop session required.** It creates a real WinForms window on an STA thread;
  a headless/CI agent without a desktop will hang until the 2-minute timeout.
- **`C:\Projects\Dextall\TestJson\` must exist** and contain the input `*.json`
  files — the report validation compares against them. Missing inputs → validation
  failure, not a code bug.
- **Depends on the Designer control name `btnLouvreGrillDrawings`.** If that button
  is renamed in `Form1`, the test throws "Button 'btnLouvreGrillDrawings' not found".
- Build the solution first (`build-solution` skill); a broken build makes e2e
  failures misleading.

## Reading the result

- Report pass/fail plus the **path of the generated `Report_*.xlsx`** (under a fresh
  `TestJson\TestRun_*` folder) so the human can inspect the data mismatches.
- The `ITestOutputHelper` lines ("Form shown", "Clicking…", "Validating Results")
  are only visible with `--logger "console;verbosity=detailed"` — use it when
  diagnosing where the run stalled.
- Prefer `run-unit-tests` for fast feedback on `PanelCalculator2` logic; use this
  only to confirm the full generation path.
