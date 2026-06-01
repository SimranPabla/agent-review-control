# What ARC Is

ARC stands for Agent Review Control.

It is a deterministic review-control layer for AI coding agent work.

The product wedge is:

```text
Contract + Evidence + Verdict
```

## The problem

AI coding agents can produce useful pull requests quickly, but the reviewer often inherits an unclear process:

- The task was vague.
- The allowed scope was not frozen.
- The agent touched extra files.
- Tests may or may not have run.
- The summary sounds confident, but the evidence is weak.
- The reviewer has to reconstruct what happened from the diff.

This creates review drag. The reviewer is not only reviewing code. They are also auditing the agent's behavior.

## The ARC approach

ARC separates three things that are often mixed together:

1. **The assignment**
   - What was the agent asked to do?
   - What was in scope?
   - What was out of scope?
   - What evidence was required?

2. **The work**
   - What files changed?
   - What commands were run?
   - What evidence exists?
   - What risky areas were touched?

3. **The review receipt**
   - Did the work match the assignment?
   - What is missing?
   - What should the reviewer inspect first?

## Gate 1: Before coding

Before the agent starts, ARC should capture a contract:

- Task summary
- Acceptance criteria
- Allowed files or directories
- Excluded files or directories
- Non-goals
- Required commands/tests
- Risky areas
- Reviewer focus

This makes the delegation clear enough for an agent, a human reviewer, and a verification tool.

## Gate 2: Before PR review

Before the PR is reviewed, ARC checks:

- Changed files against allowed/excluded scope
- Required command receipts
- Missing evidence
- Risk-sensitive files
- Dependency/config/security-sensitive changes
- Scope drift

Then it renders a short Trust Brief for the reviewer.

## The Trust Brief

The Trust Brief is not a long report.

It should answer:

- What is the verdict?
- Why?
- What should the reviewer inspect first?
- What evidence supports that verdict?

## Verdicts

ARC uses three plain verdicts:

- `Pass`: the checked contract requirements were satisfied.
- `Needs Review`: something is missing, risky, or outside the normal path.
- `Blocked`: the contract was invalid, unsafe, or clearly violated.

`Pass` does not mean the code is correct. It means the checked ARC contract was satisfied.

## Product boundary

ARC should stay narrow.

It should not become:

- A generic AI PR reviewer
- A full compliance suite
- A dashboard-first product
- An auto-merge bot
- A generic PR summary generator

The value is in deterministic control: the agent either stayed inside the approved assignment with evidence, or it did not.

