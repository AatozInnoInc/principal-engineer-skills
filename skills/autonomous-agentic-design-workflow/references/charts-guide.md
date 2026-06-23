# Charts guide

One chart per domain (bounded context), system, or feature, stored in `docs/charts/` as `.md` files with fenced ` ```mermaid ` blocks (these render directly on GitHub). Embed the most relevant chart inline in the document it supports; keep the standalone file as the source of truth and keep both in sync with the prose.

Choose the type by what you are conveying. Worked examples follow.

## Context map — bounded contexts and their relationships

Use `flowchart`/`graph`. Show contexts as nodes and relationships as edges; label a Shared Kernel where contexts genuinely overlap.

```mermaid
flowchart LR
    A[Ordering context] -->|places| B[Fulfillment context]
    A -->|reads pricing from| C[Catalog context]
    SK([Shared Kernel: Money, Identifiers]) --- A
    SK --- B
    SK --- C
```

## Domain model within a context

Use `classDiagram` for aggregates, entities, and value objects.

```mermaid
classDiagram
    class Order {
        +OrderId id
        +place()
        +cancel()
    }
    class LineItem
    class Money
    Order "1" *-- "many" LineItem
    LineItem --> Money
```

## Cross-component flow / use case

Use `sequenceDiagram`.

```mermaid
sequenceDiagram
    participant U as User
    participant API
    participant Svc as Service
    U->>API: request
    API->>Svc: handle
    Svc-->>API: result
    API-->>U: response
```

## Lifecycle / state machine

Use `stateDiagram-v2`.

```mermaid
stateDiagram-v2
    [*] --> Draft
    Draft --> Submitted
    Submitted --> Approved
    Submitted --> Rejected
    Approved --> [*]
```

## Persistence / data model

Use `erDiagram`.

```mermaid
erDiagram
    ORDER ||--o{ LINE_ITEM : contains
    ORDER {
        uuid id
        timestamp created_at
    }
    LINE_ITEM {
        uuid id
        int quantity
    }
```

## Pipeline / decision logic — show real branches

Use `flowchart`, and **draw the short-circuits**. If a stage can satisfy the goal and exit early, the chart must show that exit. Do not draw a straight line through stages that can actually branch (principle 3).

```mermaid
flowchart TD
    Start([Input]) --> A[Stage A]
    A --> Q{Goal already met?}
    Q -->|yes| Done([Propose result])
    Q -->|no| B[Stage B]
    B --> C[Stage C]
    C --> Done
```
