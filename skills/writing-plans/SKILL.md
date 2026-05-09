---
name: writing-plans
description: Use when you have a spec or requirements for a multi-step frontend or full-stack task, before touching code.
---

# Writing Plans

## Overview

Write implementation plans for a teammate who knows the language and modern tooling, but does NOT know this specific codebase, its conventions, or its domain.

Be specific about file paths, module boundaries, and project-specific patterns. Do NOT over-explain general programming concepts.

**Announce at start:** "I'm using the writing-plans skill to create the implementation plan."

**Save plans to:** `docs/plans/YYYY-MM-DD-<feature-name>.md`

---

## STEP 0: Load project context (REQUIRED)

Before drafting the plan, look for and read project context files. Common locations:

- `docs/ai-context/` — preferred location if it exists
- `.cursorrules`, `CLAUDE.md`, `AGENTS.md` — root-level context files
- `README.md` — fallback for basic project info

You are looking for information about:
- Project architecture (monorepo / single repo, layering, module boundaries)
- Tech stack and key libraries
- Naming and file placement conventions
- Runtime environment constraints (browser, WebView, Electron, SSR, mobile, etc)
- Domain-specific patterns and anti-patterns
- Build, test, and deployment commands

**If no context files exist, or they look stale (>1 month old), STOP and ask the user:**
- "I don't see project context docs. Should I infer conventions from the existing code, or do you want to provide context first?"

Do NOT silently guess project conventions from code alone — code is often inconsistent. Ask first.

---

## STEP 1: Classify the task

Classify the task into one of these types. Verification strategy depends on it.

| Type | Examples | Verification |
|---|---|---|
| **pure-logic** | utils, hooks, services, state machines, parsers, algorithms | TDD: failing test → implement → pass |
| **ui-component** | new component, new page, layout change, styling | Build → visual check (Storybook / dev server / screenshot) → interaction tests for critical paths only |
| **api-integration** | consume new endpoint, modify data flow, add request layer | Type the contract → mock data → wire up → swap to real → handle error/empty/loading |
| **runtime-bridge** | JSBridge, native ↔ webview, postMessage, worker comm, IPC | Define message contract → mock the other side → wire up → integration test on real runtime |
| **scaffold** | new package, build config, CI, dev tooling | No tests. Plan = changes + verification commands + expected output |
| **refactor** | move/rename, extract module, change abstraction | Lock behavior with tests FIRST → refactor → re-run tests |
| **bugfix** | fix a known bug | Reproduce with failing test → fix → test passes → check regressions |
| **investigation** | "figure out why X happens" before knowing the fix | No plan yet. Plan a debugging session, not an implementation |

**State the classification at the top of the plan.** If a task spans multiple types, split it into multiple tasks.

If you're unsure which type fits, ask the user before proceeding.

---

## STEP 2: Reverse-clarify before writing the plan

After loading context and classifying, but BEFORE writing the plan, output:

1. **My understanding of the goal** — in your own words, 2-3 sentences
2. **Key assumptions I'm making** — list all, including ones that feel obvious
3. **Things I'm uncertain about** — list all, even small ones
4. **Files I expect to modify outside the obvious scope** — any "while I'm there" changes you're considering
5. **Risks** — what could go wrong, what existing code this might affect
6. **Runtime/environment concerns** — anything specific to where this code runs (browser version, WebView quirks, mobile constraints, network conditions) that affects the approach

Then **STOP and wait for user confirmation**. Do not proceed to write the plan until the user says "looks good" or gives feedback.

This step is non-negotiable. The cost of waiting 30 seconds is much lower than the cost of executing a wrong plan.

---

## STEP 3: Plan document header

Every plan MUST start with this header:

````markdown
# [Feature Name] Implementation Plan

> **For Claude:** Execute this plan task-by-task in auto mode after user confirmation. See "Auto-Execute Mode" below for stop conditions.

**Goal:** [One sentence]

**Task type:** [pure-logic | ui-component | api-integration | runtime-bridge | scaffold | refactor | bugfix | mixed]

**Approach:** [2-3 sentences on the approach, referencing project conventions]

**Affected areas:** [paths or modules]

**Tech stack:** [Key libraries used in this task]

**Out of scope:** [Explicitly list what this plan will NOT touch]

---
````

---

## STEP 4: Task structure

Each task is a coherent unit (15-30 min of work). Within a task, steps are 2-5 minute actions.

````markdown
### Task N: [Short name]

**Type:** [one of the 8 types]

**Files:**
- Create: `exact/path/to/file`
- Modify: `exact/path/to/existing:line-range`
- Test: `tests/exact/path/to/file` (if applicable)

**Steps:** [tailor to task type — see templates below]

**Commit point:** [Yes — feature unit complete | No — continue to Task N+1]

**Verification:** [How user can verify this task works, if commit point]
````

### Step templates by task type

**pure-logic:**
1. Write failing test
2. Run, verify fails with expected error
3. Implement minimal code
4. Run, verify passes
5. (Commit if commit point)

**ui-component:**
1. Build component skeleton with props/types
2. Implement layout and styles
3. Wire up state/events
4. Visual check — STOP, ask user to confirm rendering (provide URL or screenshot location)
5. Add interaction test ONLY for critical user paths (form submit, key validation, conditional rendering)
6. (Commit if commit point)

**api-integration:**
1. Define/update types for the API contract
2. Add mock data matching the contract
3. Wire UI/service to mock, verify works
4. Switch to real endpoint
5. Handle error / empty / loading states explicitly
6. (Commit if commit point)

**runtime-bridge:**
1. Define the message/event contract (what's sent both ways, types, error shapes)
2. Mock the other side (e.g. fake native handler in pure web, fake web in native test harness)
3. Implement the bridge layer
4. Test against the mock
5. Test on the real runtime — STOP, ask user to confirm (this often requires a real device/build)
6. Document any runtime quirks discovered for `docs/ai-context/`
7. (Commit if commit point)

**scaffold:**
1. Make the changes (config, new package, etc)
2. Run verification command (e.g. `pnpm build`, `pnpm lint`)
3. Verify expected output
4. (Commit if commit point)

**refactor:**
1. Run existing tests, confirm they pass (lock current behavior)
2. If coverage is missing on the area being refactored, ADD tests first — STOP, ask user before continuing
3. Make the refactor
4. Re-run all tests
5. (Commit if commit point)

**bugfix:**
1. Write a test that reproduces the bug (should fail)
2. Apply the fix
3. Re-run, test passes
4. Run related tests to check for regressions
5. (Commit if commit point)

**investigation:**
This is not an implementation task. Don't write a normal plan. Instead:
1. State the question to answer
2. List hypotheses
3. List what to check (logs, code paths, repro steps, network calls, etc)
4. Run the investigation, report findings
5. THEN, if a fix is identified, START A NEW PLAN as bugfix or refactor

---

## STEP 5: Commit strategy

**Do NOT commit after every task.** Commit at logical units of completeness:
- A user-facing feature is end-to-end working
- A refactor is finished and tests pass
- A scaffold change is verified

Mark commit points explicitly in each task (`**Commit point:** Yes/No`).

Commit messages follow Conventional Commits: `feat:`, `fix:`, `refactor:`, `chore:`, `docs:`, `test:`.

---

## Auto-Execute Mode

After the plan is saved AND the user confirms both the reverse-clarify output AND the plan itself, immediately start executing.

For each task in order:
1. Carry out every step
2. If task is a commit point, run the commit
3. Move to the next task without asking

**Stop and ask the user whenever reality diverges from the plan, however slightly.** Specifically:

- A test fails in a way the plan didn't anticipate
- A file/path in the plan doesn't exist or differs from what was assumed
- You need to modify a file NOT listed in the task's `Files:`
- Existing code behaves differently from what the plan assumed
- A previous task's changes affect the current task in ways the plan didn't account for
- You're tempted to "improve" something not in the task scope
- The plan itself turns out to be internally inconsistent
- A `ui-component` task reaches the visual check step
- A `runtime-bridge` task reaches the real-runtime test step
- A `refactor` task finds missing test coverage
- The user has interrupted with new instructions

**When in doubt, stop.** Cost of stopping = 30s confirmation. Cost of silent drift = hours of cleanup.

---

## Final Report

When all tasks are done, report:

1. **What shipped** — 1-2 sentences
2. **Commit hashes** — `git log --oneline <base>..HEAD`
3. **Deviations from the plan** — anything done differently and why
4. **Patterns worth remembering** — reusable patterns, gotchas, or decisions made during execution that might belong in `docs/ai-context/`. List as bullet points; the user decides what to actually add.
5. **Open questions / follow-ups** — anything left undone or worth revisiting

Do NOT append "want me to continue?" — the plan is the contract, it's done.

---

## Principles

- DRY, YAGNI — but not dogmatically. Sometimes a little duplication is clearer than a bad abstraction.
- TDD where it makes sense (logic, bugfix, refactor). Not where it doesn't (UI visuals, scaffold, bridge integration on real runtime).
- Specific over general — exact paths and line numbers beat vague descriptions.
- Reference project context, not generic advice.
- Frequent verification, less frequent commits.
- When uncertain, stop and ask.