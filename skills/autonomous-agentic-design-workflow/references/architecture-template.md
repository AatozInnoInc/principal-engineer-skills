# Architecture template

Adapt this skeleton. Keep it high-level: decisions and their rationale, not implementation detail. Apply the principles — no fabricated numbers, accurate control flow including short-circuits, "domain" as DDD.

---

# Architecture

## Overview

One or two paragraphs: what this system is, the shape of the solution, and the core idea that holds it together.

## Bounded contexts (DDD)

List the domains as bounded contexts and what each owns. Note any **Shared Kernel** between contexts where structure genuinely overlaps — prefer sharing over duplicating identical structure.

| Bounded context | Owns | Depends on |
| --- | --- | --- |
| ... | ... | ... |

Embed the context map here (see `docs/charts/`).

## Key decisions

For each significant decision: the choice, the alternatives considered, and why. Keep rationale honest — record real trade-offs, not post-hoc justification.

- **Decision:** ...
  - **Alternatives:** ...
  - **Why:** ...

## Constraints and budgets

State constraints as **targets to be set**, not measured values, until they are actually decided and measured.

- Performance budget: `TODO — define`
- Resource ceiling: `TODO — define`
- (Replace each TODO with a real, decided value when it exists. Never invent a measured figure.)

## Control flow

Describe how the main flow actually behaves, **including early exits**. If a stage can short-circuit the rest when a condition holds, say so explicitly — do not write "all stages always run" when they do not. Embed the pipeline chart from `docs/charts/`.

## Non-goals (what we will NOT do)

An explicit list of things deliberately out of scope. This is as important as the goals — it prevents scope creep and tells agents where to stop.

- ...

## Edge cases considered

- ...

## Handoff protocol

> **Handoff protocol — every agent, every cycle:**
> 1. Read `docs/HANDOFF.md`, `docs/ARCHITECTURE.md`, and `CONTRIBUTING.md` (if present) before touching code.
> 2. Do the work described in the current **Prompt for Next Agent**.
> 3. Update every document and chart the work affected, and adjust `ROADMAP.md` status.
> 4. Move the completed prompt into the **Handoff Log** with an ISO 8601 **sign-off timestamp** and the model/agent that did it.
> 5. Write the next **Prompt for Next Agent**, leaving it as the only active prompt.

This protocol is the single source of coordination. It is stated here and in `docs/HANDOFF.md`; agents follow it without it being repeated inside each prompt.
