---
name: xml-doc-comments
description: Style for XML documentation comments on public APIs in the Dextall solution (C# /// and VB.NET ''' ). Use when documenting types and members.
---

# XML doc comments

Document the **intent, contract, and units** of public types and members — not the
obvious mechanics. Read the implementation before describing behavior.

## C# (`///`)

```csharp
/// <summary>
/// Calculates the catcher positions for a panel frame.
/// </summary>
/// <param name="frame">The populated panel frame. Must not be null.</param>
/// <param name="spacingMm">Target spacing between catchers, in millimetres.</param>
/// <returns>Catcher positions ordered bottom-to-top.</returns>
public IReadOnlyList<CoordDTO> CalculateCatchers(PanelFrame frame, double spacingMm)
```

## VB.NET (`'''`)

```vb
''' <summary>
''' Builds the panel from the supplied input data.
''' </summary>
''' <param name="inputData">Validated generator input.</param>
Public Sub BuildPanel(inputData As InputData)
```

## Rules

- **Units matter.** This code is geometry-heavy — always state mm / degrees /
  coordinate frame where a number could be ambiguous.
- Document **non-null / range expectations** in `<param>` when the code assumes them.
- Don't restate the method name in prose ("Gets the name" on `GetName`). Say what
  it means or when to use it.
- Prefer documenting **public surface** of shared/low-tier projects (e.g.
  `PanelCalculator2`, `PanelGeneratorInputDTO`) — those are real contracts other
  tiers depend on.
- Match terminology already used in the codebase: panel, frame, catcher, bracket,
  pin, cladding.
