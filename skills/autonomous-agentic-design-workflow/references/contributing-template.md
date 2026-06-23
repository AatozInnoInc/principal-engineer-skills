# CONTRIBUTING template (optional)

Only generate this when the user wants repo-facing docs. Place the result at the **repo root** as `CONTRIBUTING.md`. Agents read this before writing code when it is present, so make the workflow expectations explicit.

---

# Contributing

## Before you write code

Read, in order: `docs/HANDOFF.md`, `docs/ARCHITECTURE.md`, and this file. The active work is always the single **Prompt for Next Agent** at the bottom of `HANDOFF.md`.

## The handoff protocol

Every contributor — human or agent — follows the same cycle:

1. Read the docs above.
2. Do the work in the current **Prompt for Next Agent**.
3. Update every document and chart the work affected, and adjust `docs/ROADMAP.md` status.
4. Move the completed prompt into the Handoff Log with an ISO 8601 sign-off timestamp.
5. Write the next **Prompt for Next Agent**.

## Design changes

Design changes require sign-off from the top-reasoning model tier (see the model-roles table in `HANDOFF.md`). Do not quietly change architecture inside an implementation task — raise it, get sign-off, then record it in `ARCHITECTURE.md`.

## Documentation discipline

- No fabricated numbers. Unset constraints are written as `TODO — define`, never as fake measured values.
- Describe control flow accurately, including short-circuits.
- Keep charts and prose in sync.
- "Domain" means a DDD bounded context unless stated otherwise.

## Style and tooling

<Fill in language, formatting, lint, and test conventions once they exist.>
