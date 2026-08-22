# Agent Review Control (ARC)

**Upstream integrity and reliability layer for AI coding agents.**

ARC is built around one primitive:

```text
Contract + Evidence + Verdict
```

Before an agent is trusted to complete software work, ARC establishes what the assignment actually is, what project state it is grounded in, what the agent is allowed to change, and what evidence must exist. After execution, ARC independently checks the resulting work and evidence against that frozen contract and produces a reviewer-facing verdict.

> **Public-repository scope:** This repository contains ARC's public architecture, product boundary, dogfood evidence, and reviewer-facing artifacts. Active deterministic implementation work is currently private. This repository does not claim to be a turnkey public release of the ARC engine.

## The problem

AI coding agents can write code quickly, but long-running agent work creates a different reliability problem: the agent can start from the wrong premise.

Conversation history is lossy. Summaries compress decisions. A new session can reconstruct previous work incorrectly. Constraints can disappear, rejected approaches can reappear, and an agent can confidently report that it followed an assignment or ran a check without providing independently verifiable evidence.

That creates two distinct questions:

1. **Was the agent operating from the correct, approved task and project state?**
2. **Did the final work actually stay inside that task and produce the required evidence?**

ARC is designed to make those questions explicit and deterministic where possible.

## Assurance model

```text
Canonical project facts
(Git, files, tests, decisions, approved constraints)
              |
              v
   Project-state reconstruction
              |
              v
        Frozen Contract
              |
              v
       Agent execution
              |
              v
   Evidence / receipts / diff
              |
              v
 Deterministic verification
              |
              v
          Verdict
  Pass / Needs Review / Blocked
              |
              v
     Human review / release
```

ARC treats the agent's own summary as context, not proof.

## Architecture

### 1. Contract integrity

The contract defines the work before execution:

- task and intended outcome
- acceptance criteria
- allowed scope
- excluded scope
- non-goals
- risk-sensitive boundaries
- required commands/checks
- required evidence
- reviewer focus areas

The executor cannot silently redefine the assignment or self-approve a scope expansion.

### 2. Project-state / epistemic integrity

A contract is only useful if it is built from the right project state.

ARC's integrity model therefore treats chat history as non-authoritative context and reconstructs current state from canonical project artifacts such as repository files, Git history, tests, approved decisions, trackers, and continuity records.

The goal is not to prove that every underlying artifact is objectively true. The goal is to prevent an agent from silently replacing canonical project facts with a lossy reconstruction of previous conversation.

### 3. Control

During execution, policy-relevant actions can be evaluated against the frozen contract:

- in-scope writes can proceed
- excluded or destructive operations can be denied
- scope expansion, deletes, dependency changes, or ambiguous actions can require human approval
- missing trusted state can defer authorization rather than guessing

Active private implementation work includes deterministic contract/action/state binding and human-approval paths. The implementation is not published in this repository.

### 4. Evidence integrity

ARC is designed around evidence that is bound to the work being reviewed rather than free-form agent claims.

Relevant evidence can include:

- changed files and diff scope
- commands/checks run
- exit status and selected output digests
- repository/base/head identity
- contract identity
- producer identity
- assumptions, deviations, skipped checks, and known gaps

Private implementation work also includes content-bound receipt verification, tamper/replay checks, and ordered event validation. Those internals are intentionally not exposed here.

### 5. Independent verification

Before review or release, ARC compares the final state and available evidence to the approved contract.

Examples of deterministic checks include:

- excluded paths were not changed
- required files or evidence exist
- required command receipts are present
- contract identity did not change
- evidence is tied to the expected repository state
- approval applies to the exact action/state it was issued for
- stale or reused authorization/evidence is rejected

A verdict does **not** mean ARC proved the code correct. Tests, CI, static analysis, security tooling, and human review remain separate assurance layers.

### 6. Trust Brief

ARC reduces the verification result to a concise reviewer-facing artifact:

- `Pass`
- `Needs Review`
- `Blocked`

The Trust Brief is deviation-first: it should tell the reviewer what happened, what evidence exists, what is missing, and what deserves attention first.

## ARC and CAGE: complementary layers

ARC and execution-governance systems such as [Google CAGE](https://github.com/google/cybernetic-agent-governance-engine) address different parts of the assurance chain.

| | ARC | CAGE-style execution governance |
|---|---|---|
| Primary question | Is the agent operating from a verified project state and approved contract, and does the resulting evidence match that contract? | Is this runtime action admissible and allowed to bind to the environment? |
| Layer | Upstream epistemic / contract / evidence integrity | Downstream consequence / execution governance |
| Typical timing | Before, during, and after delegated work | At or immediately before runtime consequence |
| Examples | contract freeze, canonical-state reconstruction, evidence binding, scope verification, Trust Brief | tool-call interception, policy gates, approval/defer decisions, runtime enforcement |
| Replaces the other? | No | No |

A useful composition is:

```text
ARC: verify premise + contract + evidence
                    |
                    v
Agent proposes an action
                    |
                    v
Execution-governance boundary (for example CAGE)
                    |
                    v
Environment consequence
```

ARC does not need to inspect or trust an agent's hidden reasoning. It focuses on whether externally checkable claims, state, scope, and evidence are consistent with the approved work.

## Current public evidence

This repository includes a dogfood run against a real public repository issue:

- target: `plaid/react-plaid-link`
- issue: documentation/type mismatch for `receivedRedirectUri`
- frozen allowed scope: `README.md` only
- required evidence: `git diff -- README.md` and `git diff --name-only`
- runtime/type changes explicitly excluded
- resulting ARC verdict: `Pass`

See:

- [Contract](dogfood/001-plaid-react-plaid-link-398/contract.md)
- [Execution log](dogfood/001-plaid-react-plaid-link-398/execution-log.md)
- [Trust Brief](dogfood/001-plaid-react-plaid-link-398/trust-brief.md)

The dogfood demonstrates the review model; it does not by itself prove production readiness.

## Related open-source work

The ARC architecture is being developed alongside hands-on work in external agent systems.

A related public contribution to CAGE identified that `MANUAL_REVIEW` and `DEFER` were being collapsed at the gateway boundary. The issue proposed preserving human-approval and insufficient-context states as distinct governance decisions; the issue was closed after the project confirmed the identified semantics had been resolved.

- [Google CAGE issue #53](https://github.com/google/cybernetic-agent-governance-engine/issues/53)

This contribution is related context, not evidence that ARC is part of or endorsed by CAGE.

## Reliability and security principles

ARC is designed around several fail-closed principles:

- no contract, no trusted authorization
- scope expansion requires explicit handling rather than silent drift
- agent self-report is not sufficient evidence
- evidence should be bound to exact work/state identities
- human approval should apply to an exact action and state, not a vague future permission
- stale, mismatched, replayed, or tampered evidence/authorization should not silently pass
- verification output must not overclaim semantic correctness

## What ARC is not

ARC is not:

- an AI code reviewer
- a generic PR summarizer
- a replacement for tests or CI
- a proof of semantic truth or code correctness
- a replacement for execution-governance systems
- an auto-merge system
- a generic compliance dashboard
- a commercially validated product

The product wedge is deliberately narrower: **preserve assignment integrity, evidence integrity, and reviewer-visible deviations in AI coding work.**

## Current stage

ARC is in active development and dogfooding.

The public repository is intentionally documentation/evidence-first. Related implementation work is private while the control model, integrity boundaries, and release surface continue to change.

Current work includes:

- contract-bound deterministic controls
- human approval for scope expansion
- evidence/receipt integrity
- continuity and canonical-state reconstruction
- deterministic claim/decision validation
- Git/repository-state binding
- release-control experiments
- real-world dogfood against external repositories

No claim is made that ARC is production-ready, commercially validated, or able to prove software correctness.

## Setup / demo

There is currently no public installable ARC package in this repository.

To inspect the public model:

```bash
git clone https://github.com/SimranPabla/agent-review-control.git
cd agent-review-control
```

Then review the `docs/` and `dogfood/` directories.

The first complete public example is:

```text
dogfood/001-plaid-react-plaid-link-398/
```

## Limitations

- The active implementation is not publicly inspectable from this repository.
- ARC can verify consistency with supplied canonical artifacts; it cannot prove those artifacts are objectively correct.
- Deterministic checks cannot infer every omitted semantic dependency or architectural consequence.
- Tests and CI remain authoritative for the properties they actually check.
- Human review remains necessary for design judgment, security context, and ambiguous scope decisions.
- The public dogfood set is still small.
- There is no public hosted service or stable public API.

## Future work

- expand dogfood across larger, higher-risk AI-generated changes
- publish additional non-sensitive contract/evidence examples
- define stable public schemas for reviewer-facing artifacts
- strengthen repository/CI evidence binding
- test continuity reconstruction across long-running agent sessions
- evaluate integrations with multiple coding-agent providers
- measure whether Trust Briefs reduce reviewer reconstruction time and improve detection of scope/evidence gaps

## Docs

- [What ARC is](docs/what-is-arc.md)
- [Dogfood plan](docs/dogfood-plan.md)
- [Pilot questions](docs/pilot-questions.md)
