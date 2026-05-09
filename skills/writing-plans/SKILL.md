---
name: writing-plans
description: Use when you have a spec or requirements for a multi-step task, before touching code
---

# Writing Plans

## Overview

Write comprehensive implementation plans assuming the engineer has zero context for our codebase and questionable taste. Document everything they need to know: which files to touch for each task, code, testing, docs they might need to check, how to test it. Give them the whole plan as bite-sized tasks. DRY. YAGNI. TDD. Frequent commits.

Assume they are a skilled developer, but know almost nothing about our toolset or problem domain. Assume they don't know good test design very well.

**Announce at start:** "I'm using the writing-plans skill to create the implementation plan."

**Context:** This should be run in a dedicated worktree (created by brainstorming skill).

**Save plans to:** `docs/plans/YYYY-MM-DD-<feature-name>.md`

## Bite-Sized Task Granularity

**Each step is one action (2-5 minutes):**
- "Write the failing test" - step
- "Run it to make sure it fails" - step
- "Implement the minimal code to make the test pass" - step
- "Run the tests and make sure they pass" - step
- "Commit" - step

## Plan Document Header

**Every plan MUST start with this header:**

```markdown
# [Feature Name] Implementation Plan

> **For Claude:** Execute this plan task-by-task in auto mode. Finish a task, commit, move to the next without asking. See "Auto-Execute Mode" at the end of the writing-plans skill for stop conditions.

**Goal:** [One sentence describing what this builds]

**Architecture:** [2-3 sentences about approach]

**Tech Stack:** [Key technologies/libraries]

---
```

## Task Structure

````markdown
### Task N: [Component Name]

**Files:**
- Create: `exact/path/to/file.py`
- Modify: `exact/path/to/existing.py:123-145`
- Test: `tests/exact/path/to/test.py`

**Step 1: Write the failing test**

```python
def test_specific_behavior():
    result = function(input)
    assert result == expected
```

**Step 2: Run test to verify it fails**

Run: `pytest tests/path/test.py::test_name -v`
Expected: FAIL with "function not defined"

**Step 3: Write minimal implementation**

```python
def function(input):
    return expected
```

**Step 4: Run test to verify it passes**

Run: `pytest tests/path/test.py::test_name -v`
Expected: PASS

**Step 5: Commit**

```bash
git add tests/path/test.py src/path/file.py
git commit -m "feat: add specific feature"
```
````

## Remember
- Exact file paths always
- Complete code in plan (not "add validation")
- Exact commands with expected output
- Reference relevant skills with @ syntax
- DRY, YAGNI, TDD, frequent commits

## Auto-Execute Mode

After saving the plan, **immediately start executing it. Do NOT ask the user which mode to run, do NOT pause between tasks for confirmation.**

For each task in order:
1. Carry out every step in the task (write failing test → run → minimal impl → run → commit).
2. Run the `git commit` defined in the task's final step. One commit per task.
3. Move to the next task without asking.

**Only stop and ask the user when:**
- A test fails in a way the plan didn't anticipate, or the fix is non-obvious.
- A file/path in the plan doesn't exist or conflicts with current code unexpectedly.
- The plan itself turns out to be internally inconsistent or wrong.
- The user has interrupted with new instructions.

**When all tasks are done**, report a brief summary:
- What shipped (1–2 sentences)
- List of commit hashes (`git log --oneline <base>..HEAD`)
- Any deviations from the plan

Do not append a "next steps" or "want me to continue?" question — the plan is the contract.
