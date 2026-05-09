---
name: zoom-out
description: "Tell the agent to zoom out and give broader context or a higher-level perspective on an unfamiliar section of code. Use when user is lost in code details, needs to understand how code fits into the bigger picture, or wants to assess impact before refactoring."
---

# Zoom Out

## Core Directive

I don't know this area of code well. Go up a layer of abstraction. Give me a map of all the relevant modules and callers, using the project's domain glossary vocabulary.

## What To Produce

### 1. System Map
Show where the current module sits in the overall architecture. Use a visual representation (ASCII diagram, mermaid, or bullet tree) showing upstream and downstream relationships.

### 2. Call Relationships
- **Who calls it** (upstream callers)
- **What it calls** (downstream dependencies)
- **External state it touches** (stores, APIs, configs, databases)

### 3. Core Responsibility
Summarize in 1-2 sentences why this code exists. What capability disappears if you delete it?

### 4. Blast Radius
- If you modify this code, what else might break?
- Where are the coupling points with other modules?
- Which tests cover this path?

## Guidelines

- Use the project's domain vocabulary (check `CONTEXT.md` if it exists). Say "the Order intake module" — not "the FooBarHandler".
- Describe with **concepts**, not code dumps. The goal is understanding, not reading.
- If the module's responsibility is unclear or bloated, **call it out**. A module that does too many things is a finding, not just context.
- If you discover the module is a thin pass-through (shallow module), note that — it may be a candidate for `/improve-codebase-architecture`.

## When To Use

- Picking up unfamiliar code
- Before refactoring — assess impact radius
- During debugging — escape local tunnel vision, see the full chain
- Code review — quickly build mental model of the system
- Explaining a section of code to someone else
