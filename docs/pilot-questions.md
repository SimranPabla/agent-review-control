# Pilot Questions

Use these questions when talking to teams that use AI coding agents.

The point is to find payment/workflow evidence, not compliments.

## Qualifying questions

- Are you using AI coding agents in real repositories today?
- Which tools are you using: Claude Code, Codex, Cursor, Devin, internal agents, or something else?
- How many AI-generated PRs or commits do you review per week?
- Who reviews them?
- What kinds of changes are you willing to let agents make?
- What kinds of changes are off-limits?

## Pain questions

- What makes an AI-generated PR hard to trust?
- Have you seen agents touch files they should not have touched?
- Have agents claimed they ran tests that were missing, failed, or irrelevant?
- Do reviewers spend time reconstructing what the agent was asked to do?
- What is the most expensive agent mistake you have seen?
- What do you currently do to control scope before the agent starts?

## Workflow questions

- Where would a control layer need to live for you to use it?
- Before the agent starts?
- In CI?
- As a GitHub PR check?
- As a GitHub comment?
- Inside your agent harness?
- What would be too much ceremony?

## Payment questions

- If this reduced review uncertainty for AI-generated PRs, who would approve buying it?
- What would it need to replace or improve to justify budget?
- Would you pay for a CLI, GitHub app, or concierge review service?
- Would you try it on your next 5 AI-generated PRs?
- Would you pay for a pilot if we manually ran it on your PRs first?

## Strong buying signals

- They already restrict agent scope manually.
- They have had agent-caused review incidents.
- They use AI agents often enough that review overhead is recurring.
- A lead/founder owns the pain directly.
- They agree to test ARC on a real upcoming PR.
- They ask about installation, CI, GitHub integration, or pricing.

## Weak signals

- They think the idea is interesting but have no AI PR volume.
- They only use AI for personal experiments.
- They want a generic AI reviewer.
- They want dashboards before workflow control.
- They will not test on a real PR.
- They will not identify a buyer or budget owner.

