---
name: run-unit-tests
description: Run the PanelCalculator2 unit tests (PanelCalculatorTests, MSTest, net48). Use to verify pure decision logic in PanelCalculator2.
---

# Run unit tests — `PanelCalculatorTests`

`PanelCalculatorTests` is an **MSTest** project (net48) that unit-tests
`PanelCalculator2` classes. It lives in the `PanelCalculator2` repo and is **NOT**
part of `StandaloneMain2.sln`, so Visual Studio's Test Explorer will not show it
when that solution is open.

## Run

```powershell
cd C:\Projects\Dextall\PanelCalculator2\PanelCalculatorTests

dotnet test                                                            # all tests
dotnet test --filter "FullyQualifiedName~AdditionalCatcherCreatorTests" # one class
dotnet test --logger "console;verbosity=detailed"                      # per-test output
```

## Notes

- Use `--filter "FullyQualifiedName~<ClassOrMethod>"` to target the area you changed.
- Conventions: `[TestClass]` / `[TestMethod]`, `Assert.AreEqual`, `Assert.IsNull`,
  `Assert.AreSame`. Namespaces mirror folders (e.g. `PanelCalculatorTests.PanelFrame`).
- `Panel` and `InputData` are in the **global namespace** (no namespace declaration).
- To add tests, see the `write-unit-test` skill.
- If tests fail, report the real assertion message + failing frame; do not edit
  production code just to make them pass.
