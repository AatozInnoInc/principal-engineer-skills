# Handoff template

Adapt this skeleton. This file is the execution baton. Fill the model-roles table with **current** model names at the time of writing. Leave the Handoff Log empty at first and place a single, concrete first **Prompt for Next Agent** at the bottom.

---

# Handoff

## How to use this file

> **Handoff protocol — every agent, every cycle:**
> 1. Read `docs/HANDOFF.md`, `docs/ARCHITECTURE.md`, and `CONTRIBUTING.md` (if present) before touching code.
> 2. Do the work described in the current **Prompt for Next Agent**.
> 3. Update every document and chart the work affected, and adjust `ROADMAP.md` status.
> 4. Move the completed prompt into the **Handoff Log** with an ISO 8601 **sign-off timestamp** and the model/agent that did it.
> 5. Write the next **Prompt for Next Agent**, leaving it as the only active prompt.

You do not need to restate this protocol inside a prompt — every agent reads it here.

## Model roles

Match each task to the cheapest tier that does it well. **Do not pin versions in spirit — update the Model column as newer models ship.** Names below are current as of the date noted and are expected to change.

_Models current as of: `<YYYY-MM-DD>`_

| Task | Capability tier | Current model | Why |
| --- | --- | --- | --- |
| Design, architecture, design review, planning | Top-reasoning | `<fill in>` | Conceptual integrity is the bottleneck. |
| Core implementation, non-trivial logic, tests | Strong implementation | `<fill in>` | The bulk of precise code generation. |
| Glue, wiring, small features, fast iteration | Fast mid | `<fill in>` | Turnaround over deep reasoning. |
| Mechanical refactor, rename, format, cleanup | Cheap/fast | `<fill in>` | Reasoning is not the bottleneck; do not overpay. |

**Rule:** design changes require sign-off from the top-reasoning tier.

## Handoff log

Most recent first. Each entry is a completed cycle.

<!-- Template for a completed entry:

### <ISO 8601 timestamp> — <model/agent> — <short task title>
- **Did:** what was completed.
- **Decisions / deviations:** anything chosen or changed mid-flight, and why.
- **Docs & charts updated:** which files, and what changed.
- **Roadmap:** items moved to `done` / `in progress`.
- **Sign-off:** <ISO 8601 timestamp>

-->

_(empty — first cycle has not run yet)_

---

## Prompt for next agent

<!-- Exactly one active prompt lives here. When you finish it, move it up into the
     Handoff Log with a sign-off timestamp, then replace this with the next one. -->

**Goal:** <the next concrete slice of work>

**Context:** <what the agent needs to know that is not already in ARCHITECTURE.md / ROADMAP.md>

**Definition of done:** <observable, checkable outcomes>

**Suggested tier:** <which capability tier this task calls for>
