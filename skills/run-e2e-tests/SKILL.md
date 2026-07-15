---
name: run-e2e-tests
description: Run the TestSuite end-to-end tests (xUnit, net48) that drive the Form1 UI and produce an Excel report. Use for full pipeline / integration verification.
---

# Run end-to-end tests — `TestSuite`

`TestSuite` is an **xUnit** project (net48) that drives the `StandaloneMain2`
`Form1` UI end-to-end and produces an **Excel report** of the results. It **is**
part of `StandaloneMain2.sln`.

## Run

```powershell
cd C:\Projects\Dextall\TestSuite
dotnet test
dotnet test --logger "console;verbosity=detailed"   # per-test output
```

## Notes

- These tests exercise the whole generation pipeline through the UI, so they are
  slower and heavier than `PanelCalculatorTests`. Prefer unit tests for fast
  feedback on `PanelCalculator2` logic; use `TestSuite` to confirm the full path.
- After the run, locate and report the generated Excel report (check the test
  output / working directory for its path).
- Because it drives WinForms (`Form1`), it needs a desktop session — headless CI
  may behave differently.
- Build the solution first (`build-solution` skill); a broken build makes e2e
  failures misleading.
