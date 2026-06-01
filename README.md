# Agent Review Control

Agent Review Control (ARC) is a control layer for AI-generated pull requests.

CI checks whether the code passes.
ARC checks whether the agent stayed inside the assignment.

The core idea is simple:

```text
Contract + Evidence + Verdict
```

Before an AI coding agent starts, ARC captures the work contract: what the agent is allowed to change, what it must not touch, what evidence it must produce, and what would count as failure.

Before a pull request is reviewed, ARC compares the actual work against that contract and produces a short reviewer-facing Trust Brief.

## Why this exists

AI coding agents make it easier to produce code, but harder to trust the process behind the code.

Reviewers often have to answer questions like:

- Did the agent only change the files it was supposed to change?
- Did it silently refactor nearby systems?
- Did it run the required checks, or just say it did?
- Did it touch auth, payments, config, dependencies, or infrastructure?
- What should I inspect first?

ARC is meant to reduce that uncertainty.

## What ARC does

ARC turns an AI coding task into a bounded review packet:

1. **Contract**
   - Original task
   - Allowed scope
   - Excluded scope
   - Non-goals
   - Required commands/tests
   - Reviewer focus areas

2. **Evidence**
   - Changed files
   - Diff scope
   - Command receipts
   - Test/check status
   - Risk-sensitive file touches

3. **Verdict**
   - `Pass`
   - `Needs Review`
   - `Blocked`

The verdict is not a claim that the code is correct. It is a claim about whether the agent's work matched the approved assignment and produced the expected evidence.

## What ARC is not

ARC is not:

- An AI code reviewer
- A generic PR summarizer
- A replacement for tests or CI
- A replacement for human review
- An auto-merge system
- A governance dashboard

ARC is deliberately narrower: it checks whether the agent stayed within the contract.

## Example Trust Brief

```markdown
## ARC Trust Brief: Needs Review

The changed files stayed inside the allowed area, but one required command receipt is missing.

### Focus Questions
1. Was the missing integration test skipped because the environment was unavailable, or because the agent forgot?
2. Do the auth-adjacent changes preserve the existing session behavior?

<details>
<summary>Receipts</summary>

- Changed files: `src/auth/session.ts`, `src/auth/session.test.ts`
- Required command missing: `npm run test:auth`
- No excluded files touched

</details>
```

## Current stage

The goal is to run ARC on real AI-generated pull requests, show the Trust Brief to real reviewers, and learn whether teams want this control layer in their workflow.

## Who this is for

ARC is for teams that:

- Use Claude Code, Codex, Cursor, or other AI coding agents in real repos
- Delegate implementation work to agents and then struggle to review what happened
- Care about scope drift, missing evidence, risky file touches, and review confidence
- Want deterministic review receipts instead of another AI-generated opinion

## What we want to learn

If you use AI coding agents in production repos and this pain is familiar, open an issue or reach out with a real PR you would want checked.

## Docs

- [What ARC is](/docs/what-is-arc.md)
- [Dogfood plan](/docs/dogfood-plan.md)
- [Pilot questions](/docs/pilot-questions.md)

