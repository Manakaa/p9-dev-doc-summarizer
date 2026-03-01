# Mermaid Template Library

Use these templates as presets. Keep node IDs stable and do append-only updates for minimal visual diff.

## T1: CRUD Service Flow (flowchart)

```mermaid
flowchart LR
  N1[Client] --> N2[Controller]
  N2 --> N3[Service]
  N3 --> N4[Mapper/Repository]
  N4 --> N5[(DB Table)]
  N5 --> N6[Result]
  N6 --> N2
```

Best for:
- Standard CRUD chains
- Controller-Service-DAO layered architecture

## T2: CRUD + Validation + Error Path (flowchart)

```mermaid
flowchart TB
  N1[Request] --> N2[Controller]
  N2 --> N3{Param Valid?}
  N3 -- No --> N4[Return 4xx Error]
  N3 -- Yes --> N5[Service]
  N5 --> N6{Record Exists?}
  N6 -- No --> N7[Return Not Found]
  N6 -- Yes --> N8[Mapper]
  N8 --> N9[(biz_record)]
  N9 --> N10[Return Success]
```

Best for:
- Update/Delete with existence check
- API behavior documentation

## T3: Scheduled Batch Update (flowchart)

```mermaid
flowchart LR
  N1[Cron Trigger] --> N2[Job Start]
  N2 --> N3[Acquire Lock]
  N3 --> N4{Lock Acquired?}
  N4 -- No --> N5[Skip Run]
  N4 -- Yes --> N6[Query IDs Need Refresh]
  N6 --> N7{Any IDs?}
  N7 -- No --> N8[Release Lock]
  N7 -- Yes --> N9[Batch Update Status]
  N9 --> N10[Metrics/Logs]
  N10 --> N8[Release Lock]
```

Best for:
- Scheduled tasks
- Retry/lock/idempotency discussion

## T4: Service + Async Event (flowchart)

```mermaid
flowchart LR
  N1[HTTP Request] --> N2[Service]
  N2 --> N3[(Main DB)]
  N2 --> N4[Publish Domain Event]
  N4 --> N5[(MQ Topic)]
  N5 --> N6[Consumer]
  N6 --> N7[(Read Model / Cache)]
```

Best for:
- Event-driven architecture
- Write path and async side effects

## T5: Status Transition (stateDiagram-v2)

```mermaid
stateDiagram-v2
  [*] --> Created
  Created --> Active: validate ok
  Active --> Refreshing: cron trigger
  Refreshing --> Active: update success
  Refreshing --> Failed: update exception
  Failed --> Refreshing: retry
  Active --> Deleted: delete request
  Deleted --> [*]
```

Best for:
- Lifecycle/status fields
- Workflow transitions and retry paths

## T6: API Sequence (sequenceDiagram)

```mermaid
sequenceDiagram
  autonumber
  participant C as Client
  participant CT as Controller
  participant S as Service
  participant M as Mapper
  participant DB as biz_record

  C->>CT: PUT /api/biz-record/{id}
  CT->>S: update(id, req)
  S->>M: selectById(id)
  M->>DB: SELECT ... WHERE id=?
  DB-->>M: row/null
  M-->>S: exists?
  alt not exists
    S-->>CT: NotFound
    CT-->>C: 404
  else exists
    S->>M: updateById(...)
    M->>DB: UPDATE ...
    DB-->>M: affected=1
    M-->>S: success
    S-->>CT: ok
    CT-->>C: 200
  end
```

Best for:
- API interaction details
- Success/failure branching at runtime

## T7: Batch Loop Detail (flowchart)

```mermaid
flowchart TB
  N1[Load Candidate IDs] --> N2{Has More Batches?}
  N2 -- No --> N3[Finish]
  N2 -- Yes --> N4[Pick Next 500 IDs]
  N4 --> N5[Begin Tx]
  N5 --> N6[Batch Update]
  N6 --> N7{Success?}
  N7 -- Yes --> N8[Commit]
  N7 -- No --> N9[Rollback + Record Failed IDs]
  N8 --> N2
  N9 --> N2
```

Best for:
- Batch update behavior
- Tx boundary and error handling

## T8: Architecture Context (flowchart)

```mermaid
flowchart LR
  N1[Web/App Client] --> N2[API Gateway]
  N2 --> N3[Biz Service]
  N3 --> N4[(MySQL: biz_record)]
  N3 --> N5[(Redis Cache)]
  N3 --> N6[Scheduler]
  N6 --> N4
  N3 --> N7[Observability]
  N7 --> N8[Logs/Metrics/Alerts]
```

Best for:
- High-level architecture section
- Context + dependencies in one view

## Minimal-Change Rules

1. Keep ID namespace stable: `N1`, `N2`, `N3`...
2. Add new nodes as `N10+`; never renumber existing IDs.
3. Keep layout direction fixed (`LR` or `TB`) unless unreadable.
4. Prefer text edits on existing nodes over topology rewrites.
5. When deleting behavior, mark as deprecated first, then remove in later revision if needed.
