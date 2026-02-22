---
name: summarize-git-changes
description: Summarizes git changes and generates commit messages as markdown bullet lists. Use when the user asks for a commit message, to summarize staged or unstaged changes, or to describe what changed in a diff.
---

# Summarize Git Changes

## When to Use

- User asks for a commit message or to summarize git changes
- User wants a bullet-list summary of staged/unstaged changes or a given diff

## Output Format

Produce a commit message or change summary as a **markdown bullet list**:

- One bullet per **distinct change** (one logical change per item)
- Use a single hyphen `-` for each bullet
- Separate bullets with newlines
- Simple changes can stay on one line, e.g. `- Added try/catch, better logging`
- Prefer slightly more detail over too little; when in doubt, add more detail

## How to Summarize

1. **Gather the diff**: Run `git diff` (or `git diff --staged` for staged only) or use the diff the user provided.
2. **Group by change**: Treat each logical change (e.g. one fix, one refactor, one new function) as one bullet.
3. **Write bullets**: Start each with `- ` then a short, clear phrase. Optionally add a second clause for context (e.g. `- Added try/catch, better logging`).
4. **Detail level**: Include enough that someone reading the list knows what changed without opening the diff. Omit low-level noise (exact variable names unless they matter). When unsure, err toward more detail.

## Examples

**Input**: Staged changes add error handling in `middleware.ts` and add two tests in `middleware.test.ts`.

**Output**:
```
- Wrap middleware handler in try/catch, log errors before rethrow
- Add tests for middleware error path and success path
```

**Input**: Diff touches 4 files; one adds a config type, one reads it, one passes it through, one test updates.

**Output**:
```
- Add Config type and default in config module
- Read config in gateway and pass to session parser
- Update gateway test to supply mock config
```

**Input**: Single-file change that only fixes a typo in a log message.

**Output**:
```
- Fix typo in gateway connection log message
```

## Checklist

Before returning the summary:

- [ ] Every distinct change has its own bullet
- [ ] Bullets use `- ` and are newline-separated
- [ ] Detail level is useful without being noisy; when in doubt, more detail
- [ ] Simple changes are one line; multi-part changes can be one or two clauses on one line
