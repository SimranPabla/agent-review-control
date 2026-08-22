# What ARC Is

ARC stands for **Agent Review Control**.

It is an upstream integrity and review-control layer for AI coding-agent work.

The core primitive is:

```text
Contract + Evidence + Verdict
```

ARC's job is not to decide whether code is universally correct. Its job is to make the delegated assignment, the evidence produced during execution, and the final reviewer-facing verdict independently checkable.

## The problem

AI coding agents create two related reliability problems.

First, the agent can operate from an incorrect reconstruction of project state. Long conversations are compressed, new sessions reconstruct prior decisions, rejected directions can reappear, and important constraints can disappear.

Second, even when the assignment is correct, the agent can drift outside scope or report evidence that a reviewer cannot independently tie to the final repository state.

That means a reviewer needs answers to two questions:

1. Was the agent working from the correct approved task and project state?
2. Did the final work and evidence remain consistent with that task?

ARC addresses both questions.

## The ARC approach

ARC separates the assurance chain into five parts.

### 1. Canonical project state

Chat is useful context but is not treated as authoritative project state.

ARC reconstructs the current situation from canonical artifacts such as:

- repository files
- Git history
- tests and CI results
- approved decisions and constraints
- project trackers
- continuity records

This is the epistemic-integrity layer: establish what the agent should believe about the project before relying on its reasoning.

### 2. Contract

Before execution, ARC freezes the approved work boundary:

- task and intended outcome
- acceptance criteria
- allowed scope
- excluded scope
- non-goals
- required commands/tests
- risk-sensitive areas
- required evidence
- reviewer focus

The executor cannot silently redefine the contract or self-approve a scope expansion.

### 3. Evidence

During and after execution, ARC expects evidence that can be tied to the exact work being reviewed:

- changed files and diff scope
- commands/checks run
- exit status and relevant outputs
- repository/base/head identity
- contract identity
- assumptions, deviations, skipped checks, and known gaps

The agent's statement that a command ran or that a requirement was satisfied is context, not proof by itself.

### 4. Verification

ARC independently compares the final state and evidence against the frozen contract.

Examples include:

- changed files against allowed/excluded scope
- required command receipts
- missing evidence
- contract or repository-state drift
- approval binding
- stale or replayed evidence
- risk-sensitive changes

### 5. Verdict / Trust Brief

ARC produces one reviewer-facing result:

- `Pass`
- `Needs Review`
- `Blocked`

The Trust Brief should explain the deviation first: what matched, what did not, what evidence exists, and what a reviewer should inspect next.

`Pass` does **not** mean the code is correct. It means the checked ARC contract and evidence requirements were satisfied.

## ARC and execution governance

ARC is intentionally upstream from runtime consequence/admissibility systems.

For example, Google CAGE focuses on runtime governance: intercepting agent actions, applying policy, and determining whether an action can proceed to the environment.

ARC asks a different question before that boundary becomes meaningful:

> Is the agent operating from the correct project state and approved contract, and are the resulting claims/evidence tied to that contract?

The two layers can compose:

```text
Canonical project state
        |
        v
ARC contract / integrity checks
        |
        v
Agent proposes action
        |
        v
Runtime governance boundary
        |
        v
Environment consequence
```

ARC is not a replacement for execution governance, and execution governance does not solve ARC's upstream project-state reconstruction problem.

## Product boundary

ARC should remain narrow.

It is not:

- a generic AI PR reviewer
- a full compliance suite
- a dashboard-first governance product
- an auto-merge bot
- a generic PR summary generator
- a replacement for tests or CI
- a proof of semantic truth

The value is in preserving **assignment integrity, evidence integrity, and reviewer-visible deviations** across AI coding work.

## Public versus private implementation

The public repository contains architecture, dogfood artifacts, and reviewer-facing examples.

Active deterministic implementation work is currently private. The public repository should therefore be read as the public specification/evidence surface, not as a complete installable release of ARC.
