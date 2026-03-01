# Diagram Templates

Use stable IDs and preserve layout direction to keep visual diff small.

## Template A: Mermaid Flowchart (Preferred)

```mermaid
flowchart LR
  N1[Request Received] --> N2[Read Target Files]
  N2 --> N3[Extract Facts]
  N3 --> N4[Infer Change Logic]
  N4 --> N5[Generate Summary]
  N5 --> N6{Need Diagram?}
  N6 -- Yes --> N7[Render Mermaid]
  N6 -- No --> N8[Skip Diagram]
  N7 --> N9[Publish Local or Yuque]
  N8 --> N9[Publish Local or Yuque]
```

Rules:
- Keep node IDs (`N1...N9`) unchanged across versions.
- Add new nodes as `N10+`; do not renumber existing nodes.
- Keep arrows and labels stable unless logic changes.

## Template B: Structured Diagram Code

Use when the user asks for "code mode" rather than Mermaid render.

```yaml
diagram_type: flow
layout: LR
nodes:
  - id: N1
    label: Request Received
  - id: N2
    label: Read Target Files
  - id: N3
    label: Extract Facts
  - id: N4
    label: Infer Change Logic
  - id: N5
    label: Generate Summary
  - id: N6
    label: Publish Result
edges:
  - from: N1
    to: N2
    label: input
  - from: N2
    to: N3
    label: parse
  - from: N3
    to: N4
    label: synthesize
  - from: N4
    to: N5
    label: compose
  - from: N5
    to: N6
    label: output
```

Rules:
- Reuse IDs and labels whenever possible.
- Append-only for new nodes/edges.
- Keep `layout` fixed unless readability clearly degrades.
