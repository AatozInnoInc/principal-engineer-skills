---
name: autonomous-agentic-design-workflow
description: "Helps unify and organize an agentic, self-maintaining workflow that takes a concept to a plan, to a clear execution, and finally to an autonomously maintained codebase. It scaffolds a living document set (Architecture, Roadmap, Handoff) plus mermaid charts that both humans and coding agents consult, and a cyclical handoff protocol in which each agent finishes its work, signs off with a timestamp, and writes the prompt for the next agent. Use it as soon as an idea begins to take shape and no later than the moment before coding begins. Use it as you start to understand what it is you want to code, or use right after your ideas are finalized, and you're ready to start coding. It works in two contexts: First, at the start of a session to signal an intentional design that the rest of the chat fills in, and second near the end of a design or Plan Mode session to crystallize the discussion into durable docs before handing off to Claude Code, Cursor, or similar. Trigger it on design docs, architecture or roadmap planning, repo scaffolding, agent handoff, domain-driven design, or summarizing a system with diagrams, even when the user does not say the word skill."
---

# Agentic Design Workflow

This skill sets up a design session **intentionally** so that the work can flow, without friction, into autonomous execution by coding agents. It produces a small set of living documents and mermaid charts that serve as a shared source of truth for both the human and the agents, plus a self-perpetuating handoff protocol that keeps those documents current as work proceeds.

The goal is a workflow that carries a concept to a plan, to a clear execution, and finally to a codebase that agents can maintain on their own — each cycle leaving the next one set up to begin without re-litigating context.

## When this runs

This skill fits two moments. Behavior is the same in both; only the amount of pre-existing material differs.

- **At the start of a session** — to declare intent up front and build the scaffolding that the rest of the chat fills in.
- **Near the end of a design or Plan Mode session** — to crystallize the accumulated discussion into durable documents before handing off to coding agents.

If invoked early, with little actually decided, generate skeletons with **explicit open questions** rather than inventing decisions. The documents track whatever design is real *in this chat*; this skill provides the structure, not the design itself.

## Non-negotiable principles

These exist because they are the most common ways these documents go wrong. Hold them firmly.

1. **"Domain" means Domain-Driven Design.** Unless the user is unmistakably talking about DNS or websites, read and write "domain" as a DDD bounded context, not a website domain. Frame boundaries and charts around bounded contexts, aggregates, entities, and value objects. Where two contexts genuinely share structure, name a **Shared Kernel** rather than duplicating — do not over-isolate things that legitimately overlap.

2. **Never fabricate numbers.** No invented latencies, throughputs, percentages, frame rates, or benchmark figures. "The pass is typically under 5ms" is a fabrication if nothing measured it. If a constraint has not been decided, write it as a target to be set (for example, `Performance budget: TODO — define`), never as a fake measured value. Constraints get set deliberately, by the user, later.

3. **Never assert false invariants.** Describe control flow as it actually is, including early exits and short-circuits. If a pipeline can stop early when a condition is met, both the prose and the chart must show that branch. Do not flatten a branching flow into "all stages always run and contribute" when they do not.

4. **Charts are dual-audience.** Every chart is at once a visual summary for the human and a machine-consultable source of truth for agents. Keep charts and prose in sync; when one changes, update the other.

5. **The documents are alive.** They are maintained by the agents doing the work, through the handoff protocol below — not written once and abandoned.

## The document set and repo layout

**Always generate (the agentic working set):**

| File | Purpose |
| --- | --- |
| `docs/ARCHITECTURE.md` | High-level decisions, the specific choices made, explicit non-goals (what we will NOT do), edge cases considered, and the handoff protocol stated verbatim. |
| `docs/ROADMAP.md` | All planned work as versions / phases / slices (e.g., v1, v1.5, v2, future), each with a status. Maintained by agents working from `HANDOFF.md`. |
| `docs/HANDOFF.md` | The execution baton: the handoff protocol, the model-roles table, the handoff log, and the single active "Prompt for Next Agent." |
| `docs/charts/` | One mermaid chart per domain (bounded context), system, or feature. |

**Optional (repo-facing docs — ask; default to off unless requested):**

| File | Purpose |
| --- | --- |
| `README.md` (repo root) | Standard GitHub readme. |
| `CONTRIBUTING.md` (repo root) | Standard GitHub contributing guide. Agents read it before writing code when present. |

Follow GitHub convention: `README.md` and `CONTRIBUTING.md` live at the **repo root**; all additional design docs live under `docs/`; charts live under `docs/charts/`. Do not invent source directories, a build skeleton, or a license unless the user asks — this skill produces design and coordination docs, not application code.

## The cyclical handoff protocol (the heart)

This is the mechanism that makes the workflow self-maintaining. The trick is to state the rule **once, structurally**, so it never has to be repeated inside each "Prompt for Next Agent." Agents learn the rule from the document itself.

Document this rule **verbatim in both `docs/ARCHITECTURE.md` and `docs/HANDOFF.md`:**

> **Handoff protocol — every agent, every cycle:**
> 1. Read `docs/HANDOFF.md`, `docs/ARCHITECTURE.md`, and `CONTRIBUTING.md` (if present) before touching code.
> 2. Do the work described in the current **Prompt for Next Agent**.
> 3. Update every document and chart the work affected, and adjust `ROADMAP.md` status.
> 4. Move the completed prompt into the **Handoff Log** with an ISO 8601 **sign-off timestamp** and the model/agent that did it.
> 5. Write the next **Prompt for Next Agent**, leaving it as the only active prompt.

`HANDOFF.md` is laid out so this flow is obvious: the protocol and model-roles table sit at the top for reference, the Handoff Log records completed cycles (most recent first, each with its sign-off timestamp), and a single **Prompt for Next Agent** sits at the bottom as the one open directive. A prompt earns its sign-off timestamp only when its work is done and it moves into the log; the active prompt at the bottom carries none yet. See `references/handoff-template.md`.

## Model roles — by capability tier, not by version

**Never pin specific model versions in the documents.** Named versions go stale fast. Describe the *tier* a task needs; the session fills the actual model column in `HANDOFF.md` with whatever is current at execution time. The principle is to match each task to the cheapest tier that does it well: do not burn a top-reasoning model on mechanical cleanup, and do not hand architecture to a cleanup-tier model.

| Task | Capability tier | Why |
| --- | --- | --- |
| Design, architecture, design review, planning | Top-reasoning tier | Conceptual integrity and big-picture coherence are the bottleneck. |
| Core feature implementation, non-trivial logic, tests | Strong implementation tier | Precise code generation and good tool use carry the bulk of the build. |
| Glue, wiring, small features, fast iteration | Fast mid tier | Turnaround matters more than deep reasoning. |
| Mechanical refactor, rename, formatting, cleanup | Cheap/fast tier | Reasoning is not the bottleneck; do not overpay. |

Rule to record alongside the table: **design changes require sign-off from the top-reasoning tier.** When writing `HANDOFF.md`, fill the model column with current model names — and note in the doc that those names are expected to be updated over time.

## Charts — what to draw and how

Place one chart per domain (bounded context), system, or feature in `docs/charts/`, as `.md` files containing fenced ` ```mermaid ` blocks so they render on GitHub. Embed the most relevant chart inline in the document it supports, and keep the standalone file as the source of truth.

Choose the mermaid type by concern:

| Concern | Mermaid type |
| --- | --- |
| Bounded contexts and their relationships (context map) | `flowchart` / `graph` |
| Domain model within a context (aggregates, entities, value objects) | `classDiagram` |
| Cross-component flows / use cases | `sequenceDiagram` |
| Lifecycles / state machines | `stateDiagram-v2` |
| Persistence / data model | `erDiagram` |
| Pipelines and decision logic | `flowchart` — show real branches, including short-circuits (principle 3) |

See `references/charts-guide.md` for worked examples of each.

## Running the skill, step by step

1. **Establish what is actually decided.** Wrapping up a design session: harvest the real decisions from the conversation. Starting fresh: capture what is known and mark everything else as an open question. Do not invent.
2. **Confirm scope.** Ask which optional repo-facing docs (`README.md`, `CONTRIBUTING.md`) the user wants; default to none unless asked.
3. **Create the layout:** `docs/` and `docs/charts/`.
4. **Write `ARCHITECTURE.md`** — decisions, explicit non-goals, edge cases, and the handoff protocol verbatim. No fabricated metrics; unset constraints as targets-to-set. Accurate control flow, including short-circuits.
5. **Write `ROADMAP.md`** — versions/phases/slices with status.
6. **Draft charts** per domain/system/feature in `docs/charts/`.
7. **Write `HANDOFF.md`** — protocol, model-roles table (current models), an empty Handoff Log, and the first **Prompt for Next Agent** describing the first slice of work.
8. **If requested, write `README.md` and `CONTRIBUTING.md`** at the repo root.
9. **Hand off.** The coding agents take it from `HANDOFF.md`.

## Templates

Fillable skeletons for every document live in `references/`. Read the one you are about to write and adapt it — they already encode the structure and the principles above.

- `references/architecture-template.md`
- `references/roadmap-template.md`
- `references/handoff-template.md`
- `references/readme-template.md`
- `references/contributing-template.md`
- `references/charts-guide.md`
