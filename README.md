# Spec Before Lean Implementation Skill

`spec-before-lean-implementation` is a reusable agent skill for implementing code carefully, minimally, and safely.

It combines two ideas:

1. **Spec-first implementation**
   Do not start coding from vague intent. First summarize the task, identify ambiguities, ask only necessary questions, freeze a small implementation spec, then code.

2. **Minimal implementation ladder**
   Before writing new code, check whether the task can be solved with less implementation: doing nothing, reusing existing code, using the standard library, using framework/platform features, using installed dependencies, or writing one small function.

The goal is simple:

> Understand deeply. Implement minimally.

---

## What This Skill Does

This skill helps an AI coding agent avoid two common mistakes:

* building the wrong thing because the requirement was ambiguous
* building too much because a simpler solution was not considered

Before implementation, the skill guides the agent to:

1. inspect the existing codebase
2. search for reusable existing code
3. apply a Ponytail-style minimal implementation ladder
4. summarize the implementation target
5. identify blocking and non-blocking ambiguities
6. ask at most five blocking questions
7. freeze a mini-spec
8. implement the smallest sufficient change
9. verify the result
10. report what changed and why

---

## Minimal Implementation Ladder

Before writing new code, the agent should walk down this ladder and stop at the first rung that solves the task:

1. Can the task be solved by doing nothing?
2. Can it be solved by reusing existing code?
3. Can it be solved with the standard library?
4. Can it be solved with framework or platform features?
5. Can it be solved with an already installed dependency?
6. Can it be solved with one line or one small function?
7. Only then create new implementation.

This prevents unnecessary abstractions, new dependencies, duplicate utilities, large refactors, and speculative “future-proofing.”

---

## When to Use

Use this skill when asking an agent to:

* implement a new feature
* modify a non-trivial module
* add a script or CLI
* refactor code
* integrate a library, service, API, dataset, model, or system component
* reproduce or adapt an existing method
* debug and patch a codebase
* add tests, tooling, automation, or workflow support

Do not use it for:

* typo fixes
* simple one-line edits
* pure explanations
* translation
* writing tasks
* tasks where no implementation decision is involved

---

## Installation

Clone this repository under your home directory, then create a symbolic link into `~/.agents/skills`.

```bash
cd ~

git clone <YOUR_REPOSITORY_URL> lean-spec-implementation

mkdir -p ~/.agents/skills

ln -s ~/lean-spec-implementation ~/.agents/skills/lean-spec-implementation
```

Verify that the symbolic link was created correctly:

```bash
ls -l ~/.agents/skills
```

You should see something like:

```bash
lean-spec-implementation -> /Users/<your-name>/lean-spec-implementation
```

On Linux, the path may look like:

```bash
lean-spec-implementation -> /home/<your-name>/lean-spec-implementation
```

---

## Expected Directory Structure

The repository should contain a `SKILL.md` file at its root:

```text
~/lean-spec-implementation/
├── SKILL.md
└── README.md
```

After linking, the agent skill path should look like this:

```text
~/.agents/skills/
└── lean-spec-implementation -> ~/lean-spec-implementation
```

---

## Usage

In your coding agent, ask it to use the skill explicitly:

```text
Use the lean-spec-implementation skill.

I want to add [feature/module/script] to this repository.
First inspect the existing code, check whether the task can be solved with less new implementation, apply the minimal implementation ladder, ask only blocking questions, freeze a mini-spec, then implement the smallest sufficient change.
```

Example:

```text
Use the lean-spec-implementation skill.

I want to add a CLI script that converts input JSON files into CSV files.
Before writing new code, check whether the repository already has a similar converter or utility.
Then apply the minimal implementation ladder, freeze a mini-spec, and implement the smallest sufficient solution.
```

---

## Skill Workflow

When this skill is used, the agent should follow this flow:

1. Inspect relevant context if available.
2. Perform existing code reuse check.
3. Apply the minimal implementation ladder.
4. Provide a minimality decision.
5. Summarize implementation understanding.
6. Scan for ambiguities.
7. Ask at most five blocking questions if needed.
8. Freeze a mini-spec.
9. Provide an implementation plan.
10. Implement the smallest sufficient change.
11. Verify the result.
12. Provide a final report.

---

## Frozen Mini-SPEC

A Frozen Mini-SPEC is a short implementation contract created before coding.

It defines:

* goal
* inputs
* outputs
* user-facing interface
* constraints
* assumptions
* acceptance criteria
* out-of-scope items

The purpose is to prevent the agent from silently expanding the task or implementing features that were not requested.

Example:

```markdown
## Frozen SPEC

### Goal
Add a small CLI that converts JSON files into CSV files.

### Inputs
- Input JSON file path
- Output CSV file path

### Outputs
- CSV file written to the requested path

### User-Facing Interface
python scripts/json_to_csv.py --input input.json --output output.csv

### Constraints
- Do not add new dependencies.
- Reuse existing utilities if available.
- Do not modify unrelated files.

### Assumptions
- Input JSON is an array of objects.
- Keys become CSV columns.

### Acceptance Criteria
- Command runs successfully on a small sample JSON file.
- CSV output is created.
- Existing tests still pass.

### Out of Scope
- Nested JSON flattening
- GUI
- batch processing
- new config system
```

---

## Design Philosophy

This skill prefers:

* reuse over new code
* small functions over new classes
* standard library over new dependencies
* existing project conventions over new patterns
* minimal diffs over broad refactors
* concrete acceptance criteria over vague completion
* explicit assumptions over silent guesses

It avoids:

* building a framework for one use case
* adding dependencies for tiny utilities
* duplicating existing code
* changing public behavior silently
* mixing large refactors with feature work
* modifying unrelated files
* implementing hypothetical future requirements

---

## Uninstall

Remove the symbolic link:

```bash
rm ~/.agents/skills/lean-spec-implementation
```

Optionally remove the cloned repository:

```bash
rm -rf ~/lean-spec-implementation
```

---

## Recommended Prompt

```text
Use the lean-spec-implementation skill.

Before implementing, check whether this task can be solved by:
1. doing nothing
2. reusing existing code
3. using the standard library
4. using framework or platform features
5. using installed dependencies
6. writing one small function
7. only then creating new implementation

Then summarize the requirement, ask only blocking questions, freeze a mini-spec, and implement the smallest sufficient change.
```
