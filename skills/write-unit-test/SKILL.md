---
name: write-unit-test
description: Conventions and templates for writing new unit tests in PanelCalculatorTests (MSTest). Use when adding tests for PanelCalculator2 logic.
---

# Write a unit test — `PanelCalculatorTests`

Target **pure decision logic**, not heavy geometry. Add tests under
`C:\Projects\Dextall\PanelCalculator2\PanelCalculatorTests\`.

## Conventions

- **MSTest**: `[TestClass]` on the class, `[TestMethod]` on each test.
- Assertions: `Assert.AreEqual`, `Assert.IsNull`, `Assert.AreSame`, etc.
- **Namespace mirrors the folder**, e.g. a test in `PanelFrame/` uses
  `namespace PanelCalculatorTests.PanelFrame`.
- `Panel` and `InputData` are in the **global namespace** — do not qualify them.

## What is cheap vs expensive to test

- ✅ `new Panel(inputData)` is **cheap** — its constructor only creates a
  `PanelSystemVersionSwitch` (flag-setting only, no I/O, no geometry). `PanelFrame`
  stays `null` until populated.
- ❌ Heavy geometry (`PinBracket`, `PinCatcher`, …) is **not** unit-test friendly.
  Test the *decision logic* that drives geometry, not the geometry itself.
- Private methods can be exercised via **reflection** — see
  `AdditionalCatcherCreatorTests` for the established pattern.

## Template

```csharp
using Microsoft.VisualStudio.TestTools.UnitTesting;

namespace PanelCalculatorTests.PanelFrame
{
    [TestClass]
    public class MyFeatureTests
    {
        [TestMethod]
        public void Does_expected_thing_for_given_input()
        {
            var inputData = new InputData(/* minimal fixture */);
            var panel = new Panel(inputData);   // cheap: no geometry built

            // Act on the pure decision logic under test.
            // For private members, reflect (see AdditionalCatcherCreatorTests).

            Assert.AreEqual(expected, actual);
        }
    }
}
```

Run what you wrote with the `run-unit-tests` skill.
