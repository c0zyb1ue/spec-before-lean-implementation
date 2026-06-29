---
name: spec-before-lean-implementation
description: Use this skill when implementing a non-trivial feature, module, script, refactor, integration, or code modification where ambiguous requirements or over-engineering could lead to incorrect or unnecessary implementation. This skill first checks whether new code is needed at all, searches for existing reusable code, applies a minimal implementation ladder, summarizes requirements, asks only necessary questions, freezes a mini-spec, then implements and verifies the smallest sufficient change.
---

# Lean Spec Implementation Skill

## Goal

Prevent two common implementation failures:

1. Building the wrong thing because requirements were ambiguous.
2. Building too much because the simpler solution was not considered.

Before writing code, first check whether new code is needed at all.  
If implementation is necessary, clarify the target, identify ambiguity, freeze a small specification, then implement the smallest sufficient change and verify it.

The objective is not to think less.  
The objective is to understand deeply, then implement minimally.

---

## When to Use

Use this skill when the user asks to:

- implement a new feature
- modify a non-trivial module
- add a script or CLI
- refactor code
- integrate a library, service, API, dataset, model, or system component
- reproduce or adapt an existing method
- debug and patch a codebase where the root cause is not obvious
- add tests, tooling, automation, or workflow support

Do not use this skill for:

- very small typo fixes
- simple one-line edits
- purely explanatory questions
- translation or writing tasks
- tasks where no implementation decision is involved

---

# Phase 0: Inspect Before Implementing

If a repository or codebase is available, inspect relevant context before proposing new code.

Check for:

- existing files and modules
- existing utilities or helper functions
- existing scripts or commands
- existing tests
- existing configs
- existing documentation
- package manager files
- installed dependencies
- framework or platform conventions already used in the project

Do not create a new module, class, abstraction, dependency, or framework until the existing codebase has been checked.

---

# Phase 1: Existing Code Reuse Check

Before implementing anything new, search for existing code that already solves or partially solves the task.

Look for:

- functions with similar behavior
- scripts with similar input/output
- utilities that can be reused
- existing config patterns
- existing command-line interfaces
- existing tests that define expected behavior
- existing abstractions that should be extended rather than duplicated

Use this format:

## Reuse Check

- Existing relevant code found:
- Can it be reused directly?
- Can it be lightly adapted?
- Is new code actually necessary?
- Reason:

Prefer reuse over new implementation.

If similar code exists, choose one of:

1. Reuse directly.
2. Add a small adapter.
3. Extend the existing function/module minimally.
4. Only create new code if reuse would be more complex, unsafe, or misleading.

Do not duplicate existing behavior without explaining why.

---

# Phase 2: Minimal Implementation Ladder

Before writing new code, walk down this ladder and stop at the first rung that solves the task.

1. **Can the task be solved by doing nothing?**
   - If the requested behavior is unnecessary, already satisfied, or premature, do not implement it.
   - Explain why no change is needed.

2. **Can it be solved by reusing existing code?**
   - Reuse existing functions, modules, scripts, configs, or tests.
   - Prefer a small adapter over a new implementation.

3. **Can it be solved with the standard library?**
   - Use the language standard library when sufficient.
   - Do not add dependencies for standard behavior.

4. **Can it be solved with framework or platform features?**
   - Use built-in capabilities from the framework, runtime, operating system, or platform.
   - Do not recreate framework behavior.

5. **Can it be solved with an already installed dependency?**
   - Use dependencies already present in the project.
   - Do not add a new dependency unless clearly necessary.

6. **Can it be solved with one line or one small function?**
   - Keep the solution local and small.
   - Avoid new classes, registries, factories, decorators, plugin systems, or abstractions unless required.

7. **Only then create new implementation.**
   - Implement only the behavior required by the frozen spec.
   - Do not build for hypothetical future requirements.

After applying the ladder, summarize:

## Minimality Decision

- Highest solved rung:
- Why this rung is sufficient:
- New code needed:
- New dependency needed:
- Larger implementation rejected because:

---

# Phase 3: Understand the Request

Summarize the request before coding.

Use this format:

## Implementation Understanding

- Goal:
- Inputs:
- Outputs:
- Existing code or module likely involved:
- Main behavior:
- Expected user-facing command/API:
- Constraints:
- Success criteria:
- Current assumptions:

If the repository is available, update this summary after inspection.

---

# Phase 4: Ambiguity Scan

Identify ambiguity using three classes.

## Blocking Ambiguities

Questions that must be answered before implementation because the wrong choice would change:

- architecture
- data format
- public API
- user-facing behavior
- evaluation criteria
- compatibility
- destructive or irreversible behavior
- user intent

## Non-Blocking Ambiguities

Details that can be reasonably assumed for the first implementation.

## Risks

Issues that do not block implementation but could cause bugs, mismatch, or future rework.

Use this format:

| Type | Issue | Why it matters | Proposed default |
|---|---|---|---|

---

# Phase 5: Question Budget

Ask at most 5 blocking questions before implementation.

Prioritize questions affecting:

1. input/output format
2. architecture
3. compatibility with existing code
4. user-facing behavior
5. evaluation or acceptance criteria
6. destructive or irreversible changes

Do not ask questions about details that can be safely assumed.  
List safe assumptions instead.

If the user explicitly asks to proceed without questions, make reasonable assumptions and continue.

If ambiguity exists but does not block implementation, do not pause. State the assumption and proceed.

---

# Phase 6: Frozen Mini-SPEC

After receiving answers, or after choosing safe assumptions, write a Frozen SPEC.

Use this format:

## Frozen SPEC

### Goal

...

### Inputs

...

### Outputs

...

### User-Facing Interface

...

### Constraints

...

### Assumptions

...

### Acceptance Criteria

...

### Out of Scope

...

Do not implement anything outside the Frozen SPEC unless the user changes the spec.

If the task can be solved without new code, the Frozen SPEC should explicitly say that no new implementation is required.

Acceptance criteria should be concrete and verifiable. Prefer criteria such as:

- command runs successfully
- expected output is produced
- existing behavior remains unchanged
- tests pass
- error case is handled
- public API remains compatible
- no new dependency is added unless required

---

# Phase 7: Implementation Plan

Before editing, write a concise implementation plan.

Use this format:

## Implementation Plan

1. Files to inspect:
2. Existing code to reuse:
3. Files to modify:
4. New files to create:
5. Algorithm or behavior steps:
6. Verification commands:
7. Rollback or safety notes:

Apply the Minimal Diff Rule:

- Make the smallest change that satisfies the Frozen SPEC.
- Prefer reusing or lightly extending existing code.
- Add a new module/script only when it is smaller and safer than modifying stable code.
- Prefer configuration over code changes when appropriate.
- Prefer small functions over new classes.
- Prefer local changes over global refactors.
- Do not rewrite unrelated modules.
- Do not change defaults silently.
- Do not delete files unless explicitly approved.
- Do not add dependencies unless the ladder proves they are necessary.

---

# Phase 8: Implementation Rules

## General Rules

- Inspect before editing.
- Preserve existing style and conventions.
- Avoid speculative abstractions.
- Avoid premature generalization.
- Avoid new framework layers unless required.
- Avoid future-proofing without a current requirement.
- Keep behavior changes explicit.
- Add comments only for non-obvious logic.
- Prefer explicit configuration over hard-coded values.
- Separate pure refactors from behavior changes.

## Dependency Rules

Before adding a dependency, verify that:

- the standard library cannot solve it
- the framework or platform cannot solve it
- existing dependencies cannot solve it
- the new dependency is worth its maintenance cost
- the project accepts dependency changes

If adding a dependency is not essential, do not add it.

## Refactor Rules

Do not refactor unless:

- the Frozen SPEC requires it
- the current structure blocks implementation
- the refactor is smaller and safer than duplicating fragile logic
- the user explicitly requested it

Do not mix large refactors with feature work unless necessary.

## Behavior Change Rules

Do not silently change:

- public APIs
- default behavior
- input/output formats
- configuration semantics
- file locations
- error-handling behavior
- dependency requirements
- performance characteristics in a meaningful way

If any of these must change, report it explicitly.

---

# Phase 9: Verification

Always verify with the smallest meaningful test first.

Use this format:

## Verification

- Command run:
- Result:
- Output files or observable behavior:
- Sanity checks:
- Remaining issues:

Verification should check the acceptance criteria from the Frozen SPEC.

Prefer at least one of:

- unit test
- smoke test
- import check
- syntax check
- CLI help check
- existing test suite
- minimal example run
- build command
- lint/type check if already configured

If verification cannot be run, explain why and provide exact commands for the user.

Do not run expensive full workflows unless explicitly requested.

---

# Phase 10: Final Report

After implementation, summarize:

## Final Report

- What changed:
- Why this was the minimal sufficient solution:
- Files modified:
- Existing code reused:
- New files created:
- New dependencies:
- How to run:
- How to verify:
- Important assumptions:
- Known limitations:
- Recommended next step:

If no code was needed, say so clearly and explain the existing solution or simpler alternative.

---

# Anti-Patterns to Avoid

Avoid:

- building a framework for one use case
- adding a dependency for a tiny utility
- creating a class when a function is enough
- creating a config system before there are multiple configs
- adding caching before there is a measured bottleneck
- adding async or concurrency before there is a proven need
- adding plugin architecture before there are plugins
- adding CLI flags nobody asked for
- implementing a custom parser when a standard parser works
- duplicating existing utilities
- changing public behavior silently
- large refactors mixed with feature work
- modifying unrelated files
- deleting code without confirmation
- running expensive workflows before minimal verification

---

# Preferred Response Flow

When this skill is used, follow this response flow:

1. Inspect relevant context if available.
2. Perform Reuse Check.
3. Apply Minimal Implementation Ladder.
4. Provide Minimality Decision.
5. Provide Implementation Understanding.
6. Provide Ambiguity Scan.
7. Ask at most 5 blocking questions if needed.
8. Freeze Mini-SPEC.
9. Provide Implementation Plan.
10. Implement the smallest sufficient change.
11. Verify.
12. Provide Final Report.

If the task is simple and no blocking ambiguity exists, combine the early phases briefly and proceed.
