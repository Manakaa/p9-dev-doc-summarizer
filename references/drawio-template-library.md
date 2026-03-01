# Draw.io Template Library

Use these templates when diagram mode is `draw` (draw.io / diagrams.net).

## How to use

1. Create a new draw.io file.
2. Build nodes and edges using the `nodes` and `edges` sections below.
3. Keep IDs stable for minimal diff between revisions.
4. Export as `.drawio` or PNG/SVG.

## Common style tokens

- `style.service`: rounded rectangle, light blue fill
- `style.storage`: cylinder, light yellow fill
- `style.job`: rounded rectangle, light green fill
- `style.decision`: diamond

Recommended defaults:
- direction: `LR` for architecture/CRUD
- direction: `TB` for branching/decision flows

## D1: CRUD Main Flow (draw)

```yaml
diagram: drawio
layout: LR
nodes:
  - id: N1
    text: Client
    style: service
  - id: N2
    text: BizRecordController
    style: service
  - id: N3
    text: BizRecordService
    style: service
  - id: N4
    text: BizRecordMapper
    style: service
  - id: N5
    text: biz_record
    style: storage
edges:
  - from: N1
    to: N2
    text: HTTP CRUD
  - from: N2
    to: N3
    text: invoke
  - from: N3
    to: N4
    text: query/update
  - from: N4
    to: N5
    text: SQL
```

## D2: Scheduled Refresh Flow (draw)

```yaml
diagram: drawio
layout: LR
nodes:
  - id: N1
    text: Cron Trigger
    style: job
  - id: N2
    text: BizRecordStatusRefreshJob
    style: job
  - id: N3
    text: Acquire Lock
    style: decision
  - id: N4
    text: Query IDs Need Refresh
    style: service
  - id: N5
    text: Batch Update Status
    style: service
  - id: N6
    text: biz_record
    style: storage
  - id: N7
    text: Metrics/Logs
    style: service
edges:
  - from: N1
    to: N2
    text: schedule
  - from: N2
    to: N3
    text: start
  - from: N3
    to: N4
    text: lock ok
  - from: N4
    to: N5
    text: ids found
  - from: N5
    to: N6
    text: UPDATE
  - from: N5
    to: N7
    text: report
```

## D3: API Success/Failure Branch (draw)

```yaml
diagram: drawio
layout: TB
nodes:
  - id: N1
    text: PUT /api/biz-record/{id}
    style: service
  - id: N2
    text: Validate Params
    style: decision
  - id: N3
    text: Return 400
    style: service
  - id: N4
    text: Check Record Exists
    style: decision
  - id: N5
    text: Return 404
    style: service
  - id: N6
    text: Update Record
    style: service
  - id: N7
    text: Return 200
    style: service
edges:
  - from: N1
    to: N2
  - from: N2
    to: N3
    text: invalid
  - from: N2
    to: N4
    text: valid
  - from: N4
    to: N5
    text: no
  - from: N4
    to: N6
    text: yes
  - from: N6
    to: N7
```

## Minimal-change rules for draw mode

1. Keep `id` stable (`N1`, `N2`, ...).
2. Add new nodes with append-only numbering (`N10+`).
3. Do not move existing nodes unless readability is broken.
4. Reuse existing edge text and styles when logic is unchanged.
5. Prefer editing node text over deleting/re-creating nodes.
