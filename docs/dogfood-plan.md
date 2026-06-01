# Dogfood Plan

This repo exists to create one useful public dogfood loop for ARC.

The goal is not to polish the brand. The goal is to create evidence.

## Objective

Run ARC on a real AI-generated pull request and produce a Trust Brief that a real developer can judge.

## What counts as useful evidence

Good evidence:

- A real issue from a real open source repo
- A real AI-assisted implementation
- A clear pre-work contract
- A pull request with changed files and test evidence
- An ARC Trust Brief attached to the PR
- Feedback from maintainers or experienced reviewers

Weak evidence:

- A toy repo
- A fake issue
- A PR nobody reviews
- A pretty summary without a frozen contract
- A demo that does not touch a real review workflow

## Dogfood steps

1. Pick a small open source issue with clear scope.
2. Write the ARC contract before coding.
3. Use an AI coding agent to implement the change.
4. Capture changed files and command receipts.
5. Generate an ARC Trust Brief.
6. Open a pull request.
7. Ask the reviewer whether the Trust Brief made review easier.

## What to avoid

- Do not pick a huge issue.
- Do not pick an issue that requires deep maintainer context.
- Do not submit drive-by low-quality PRs.
- Do not claim ARC proves the code is correct.
- Do not frame ARC as replacing maintainers.

## Ideal first dogfood issue

The first issue should be:

- Small
- Real
- Testable
- In an active repo
- Limited to a few files
- Easy for a maintainer to review

Good categories:

- Bug with reproduction
- Missing edge case test
- Documentation/code mismatch
- Small CLI behavior fix
- Typed validation bug

Bad categories:

- Architecture rewrite
- Performance tuning without benchmarks
- Security-sensitive change
- Maintainer preference debate
- Large dependency migration

## Success criteria

The dogfood is successful if we can answer:

- Did ARC catch any scope/evidence gap?
- Did the Trust Brief reduce review uncertainty?
- Would a real team want this on their AI-generated PRs?
- What part of the workflow felt annoying or unnecessary?
- What would someone pay for: CLI, GitHub app, CI check, or concierge review?

