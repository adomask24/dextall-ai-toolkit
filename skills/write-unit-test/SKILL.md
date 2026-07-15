---
name: write-unit-test
description: Conventions and a template for writing new unit tests in PanelCalculatorTests (MSTest, net48), based on the project's actual test style. Use when adding tests for PanelCalculator2.
---

# Write a unit test — `PanelCalculatorTests`

Add tests under `C:\Projects\Dextall\PanelCalculator2\PanelCalculatorTests\`,
in a folder that mirrors the area under test (e.g. `FrameProfiles\PL1\`). These
conventions are taken from the existing tests (`PL1BoundingBoxTest`,
`PL1FacesTest`, …) — follow them, not older docs.

## Conventions (as actually used)

- **MSTest** with `[TestClass]` / `[TestMethod]`. Classes are `public sealed`.
- **Do NOT** add `using Microsoft.VisualStudio.TestTools.UnitTesting;` — it is a
  **global using** (declared in the `.csproj`). Same for the test project's other
  implicit usings; `ImplicitUsings` is enabled.
- **Namespace mirrors the folder**: a test in `FrameProfiles\PL1\` uses
  `namespace PanelCalculatorTests.FrameProfiles.PL1`.
- **Method names describe the expectation**: `Thing_Should_Be_Correct_For_X`,
  `Must_...`. (See the note in `ProfileTUTests.cs`.)
- **Float comparisons use a tolerance**: declare `private const double TOL = 1e-6;`
  and use the overload `Assert.AreEqual(expected, actual, TOL, "message")`. Never
  compare doubles for exact equality.
- The project targets **net48** with **C# latest** — collection expressions (`[]`),
  target-typed `new()`, and `sealed` are all in use.

## Fixtures

Real input is loaded from JSON copied to the output directory
(`InputDTOs\input.json`, `InputDTOs\cornerinput.json`). The typical constructor:

```csharp
panelInputDTO = JsonConvert.DeserializeObject<PanelInputDTO>(
        System.IO.File.ReadAllText("InputDTOs\\input.json"))
    ?? throw new InvalidOperationException("Failed to deserialize panelInputDTO");
CustomConfigDTO[] customConfigDTO = [];
panelVars = new PanelGeneratorVariables(panelInputDTO, customConfigDTO);
```

`PanelGeneratorVariables(inputDTO, CustomConfigDTO[])` is the real entry object —
it exposes computed values (e.g. `panelVars.L5_Thickness`) that tests feed into
geometry classes. For geometry with fixed dimensions, many tests construct the
geometry type directly with literal numbers instead of going through fixtures.

## Exposing protected/internal members

Geometry **is** tested here. To reach `protected` members, the established pattern
is a small **test subclass**, not reflection:

```csharp
private sealed class PL1BoundingBoxTestVertices : PL1BoundingBox
{
    public PL1BoundingBoxTestVertices(PL1WorkPoints wp) : base(wp) { }
    public Vector3d[] VerticesPublic => this.Vertices.ToArray();
}
```

(Reflection is used in some older tests, but prefer the subclass approach.)

## Template

```csharp
using Newtonsoft.Json;
using PanelCalculator.FrameProfiles.PL1;
using PanelGenerator_Variables;
using PanelGeneratorInputDTO;
using PanelGeneratorInputDTO.CustomConfigurator;

namespace PanelCalculatorTests.FrameProfiles.PL1
{
    [TestClass]
    public sealed class MyFeatureTests
    {
        private readonly PanelInputDTO panelInputDTO;
        private readonly PanelGeneratorVariables panelVars;
        private const double TOL = 1e-6;

        public MyFeatureTests()
        {
            panelInputDTO = JsonConvert.DeserializeObject<PanelInputDTO>(
                    System.IO.File.ReadAllText("InputDTOs\\input.json"))
                ?? throw new InvalidOperationException("Failed to deserialize panelInputDTO");
            CustomConfigDTO[] customConfigDTO = [];
            panelVars = new PanelGeneratorVariables(panelInputDTO, customConfigDTO);
        }

        [TestMethod]
        public void Result_Should_Be_Correct_For_90_Degrees()
        {
            double thickness = panelVars.L5_Thickness;
            var subject = new PL1WorkPoints(20.0, 5.15, thickness, 90.0);

            // Act, then assert with tolerance:
            Assert.AreEqual(expectedX, subject.SomeValue, TOL, "SomeValue x mismatch");
        }
    }
}
```

## Watch out for

- Two `AdditionalPinDTO` classes exist (see the `dextall-gotchas` skill) — pick the
  right one for the DTO you're deserializing.
- If a new test throws `NullReferenceException` in the constructor, the JSON fixture
  probably didn't copy to output, or a required field is missing — check
  `CopyToOutputDirectory` in the `.csproj`.

Run what you wrote with the `run-unit-tests` skill.
