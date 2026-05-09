# ADR (Architecture Decision Record) Format

ADRs live in `docs/adr/` within each project and use sequential numbering: `0001-slug.md`, `0002-slug.md`, etc.

Create the `docs/adr/` directory lazily — only when the first ADR is needed.

## Template

```md
# {Short title of the decision}

{1-3 sentences: what's the context, what did we decide, and why.}
```

That's it. An ADR can be a single paragraph. The value is in recording *that* a decision was made and *why* — not in filling out sections.

## Optional sections

Only include these when they add genuine value. Most ADRs won't need them.

- **Status** (`proposed | accepted | deprecated | superseded by ADR-NNNN`) — useful when decisions are revisited
- **Considered Options** — only when the rejected alternatives are worth remembering
- **Consequences** — only when non-obvious downstream effects need to be called out

## When to write an ADR

All three must be true:

1. **Hard to reverse** — the cost of changing your mind later is meaningful
2. **Surprising without context** — a future reader will wonder "why did they do it this way?"
3. **The result of a real trade-off** — there were genuine alternatives and you picked one for specific reasons

### What qualifies

- Architectural shape ("We're using a monorepo")
- Technology choices that carry lock-in (database, auth provider, deployment target)
- Deliberate deviations from the obvious path ("Manual SQL instead of ORM because X")
- Constraints not visible in the code ("Can't use AWS because of compliance")
- Rejected alternatives when the rejection is non-obvious

### What does NOT qualify

- Easy to reverse decisions (just reverse them)
- Obvious choices with no real alternatives
- Implementation details that are self-explanatory from the code

## Example

```md
# Use IndexedDB instead of localStorage for log storage

The fishing log app stores Base64 signature images inline, making single records
hundreds of KB. localStorage's 5MB cap overflows after ~20 logs. We chose IndexedDB
with an in-memory cache layer for read performance. Rejected localStorage sharding
(still limited) and direct upload (breaks offline mode).
```
