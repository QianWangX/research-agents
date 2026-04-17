# Research Agents for Claude Code

A set of specialized Claude Code agents for deep learning researchers who want to move faster without losing control of their work.

---

## What this is

You already have a research idea. You want to get some help with everything around the idea — figuring out which files to touch, writing boilerplate, remembering to capture logs, wondering whether your result actually supports your claim.

These agents handle that surrounding work. You stay in charge of the research.

This is **not** a pipeline where you describe an idea and get results automatically. You decide which agents to call, when to call them, and what to do with what they return. You can invoke them in any order, skip steps entirely, or take over and do things yourself.

This is also **not** production-grade software engineering tooling. The code quality bar is research-appropriate: readable, modular where it helps, well-commented on non-obvious logic — not full test coverage, not formal code review gates.

---

## Agents

| Agent | When to call |
|---|---|
| `@planner` | Before writing code — map out what needs to change |
| `@engineer` | When you're ready to implement — writes and modifies code |
| `@operator` | For experiments worth capturing — runs commands and logs results |
| `@auditor` | Before sharing results — checks whether your conclusion holds |
| `@tracer` | When something looks wrong — investigates model behaviour |
| `@cleaner` | Occasionally — cleans up after heavy iteration |

---

## Getting started

**Install:**

```bash
/plugin install research-agents@QianWangX/research-agents
```

**Initialize your project** (run once per project):

```
/init
```

This scans your project, asks a few questions about your model, dataset, and goal, and writes a `PROJECT_CONTEXT.md` to your project root. All agents read this file before doing any work. Keep it updated as your project evolves.

---

## How to use it

There is no required order. Call whatever agent is useful at the moment.

The typical flow for implementing something new:

```
@planner  →  @engineer  →  run it yourself  or  @operator
```

Repeat that loop as many times as needed. When results start to matter:

```
@auditor  (before writing up or sharing)
@tracer   (when something looks wrong)
@cleaner  (when the codebase feels messy)
```

**Some examples:**

- *New loss function idea* — `@planner` to map the changes, `@engineer` to implement, run training yourself because it's one command.
- *Loss suspiciously flat after epoch 2* — `@tracer` to investigate gradient flow and data pipeline, then decide whether to call `@engineer`.
- *Sending results to a collaborator* — `@auditor` in deep mode to check whether the conclusion actually holds before you share.
- *Fifteen abandoned experiment scripts* — `@cleaner` to produce a staging list, then delete what's safe.

Agents never call each other automatically. Each one does its job, produces a structured output, and stops. What happens next is always your decision.

---

## Agent details

### `@planner` — break down a task before writing anything

Reads your project, traces the relevant files with Glob and Grep, and produces a short numbered task list — specific files, what changes, and why. Before outputting the list, it runs a DL-specific risk check: does this invalidate checkpoints, touch the data pipeline, affect randomness, or require a config update? Only the risks that apply are flagged.

Distinguishes between **exploration** (quick, loose, expect to iterate) and **consolidation** (careful, meant to keep). Nothing is written until you decide to proceed.

Output: **Task List**

---

### `@engineer` — implement or modify code

Works from a planner task list or a direct instruction. Reads existing files first and matches your style. For exploration, writes quick code and marks shortcuts with `# TODO: clean up`. For non-obvious logic — attention operations, custom losses, data manipulation — adds shape comments: `# (B, T, D) → (B, H, T, d_k)`.

At the end, tells you whether the next command is simple enough to run yourself or worth routing through `@operator`. If the changes are meaningful, proposes a `git commit` command for you to run, modify, or skip.

Output: **Implementation Note**

---

### `@operator` — run experiments and capture results

Best for GPU jobs, multi-step pipelines, or anything where you want a clean record. Verifies the environment before running, captures stdout and stderr to a timestamped log, and checks sanity after: loss trend, output shapes, NaN/Inf, GPU utilisation, and whether the initial loss is in the expected range. For quick exploratory runs it produces a one-line note; for consolidation runs, a full report. Updating `PROJECT_CONTEXT.md` is optional — use your judgement.

Output: **Run Note** or **Execution Report**

---

### `@auditor` — check whether your result supports your claim

Not needed after every run. Call it when results matter. Works in two modes: **quick audit** (does this result actually prove what you think it does?) and **deep audit** (statistical validity, reproducibility, conclusion integrity). Always distinguishes between what blocks your conclusion and what is merely worth noting — most imperfections don't invalidate a result.

Output: **Quick Audit** or **Audit Report**

---

### `@tracer` — investigate unexpected model behaviour

For when something is wrong but you don't know why — not execution failures, but deeper questions: why is loss plateauing, are gradients dying, why does the model fail on one class, is the data loader producing the right distribution. Runs lightweight diagnostic scripts and produces findings with a most-likely-cause hypothesis and concrete next steps. Does not fix code — hands findings back to you.

Covers: gradient flow, loss dynamics, data pipeline, activation distributions, per-class behaviour, attention patterns.

Output: **Trace Report**

---

### `@cleaner` — audit the codebase after heavy iteration

Call sparingly. Scans for duplicate code, dead code, module boundary violations, missing docstrings on non-obvious core functions, and abandoned branches. Applies a lighter standard to experiment scripts and a stricter one to core modules. Produces a staging list — nothing is deleted or changed until you approve each item.

Output: **Cleanliness Report** + **Staging List**

---

## Requirements

- Claude Code v1.0.33 or later
- Claude Pro, Team, or API account
