# Templates

Reusable templates that can be copied into any project.

## How to use

Copy the template you need into your project root or designated directory, then fill in the placeholders.

| Template | Copy to | Purpose |
|----------|---------|---------|
| `CONTEXT.md.template` | Project root as `CONTEXT.md` | Define shared domain language between you and AI |
| `ADR-FORMAT.md` | Keep as reference, create ADRs in `docs/adr/` | Record important architectural decisions |

## Quick start

```bash
# Copy CONTEXT.md template into your project
cp templates/CONTEXT.md.template /path/to/your-project/CONTEXT.md

# Create your first ADR
mkdir -p /path/to/your-project/docs/adr
# Then write docs/adr/0001-your-decision.md following ADR-FORMAT.md
```
