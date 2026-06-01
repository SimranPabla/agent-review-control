# ARC Dogfood Contract 001

## Target

- Repository: `plaid/react-plaid-link`
- Issue: https://github.com/plaid/react-plaid-link/issues/398
- Title: `Type/docs mismatch: receivedRedirectUri docs say string|null|undefined but type only permits string|undefined`
- Local worktree: `/home/simranjit/.openclaw/dogfood-work/react-plaid-link`

## Assignment

Resolve the documented mismatch for `receivedRedirectUri`.

The README currently says `receivedRedirectUri` accepts `string | null | undefined`, while the TypeScript type declares:

```ts
receivedRedirectUri?: string;
```

The low-risk fix is to update the README so the documented type matches the existing exported TypeScript type.

## Intended Outcome

The public README should no longer tell users that `receivedRedirectUri` accepts `null` unless the code actually supports that type.

## Allowed Scope

The agent may change only:

- `README.md`

## Excluded Scope

The agent must not change:

- `src/**`
- `examples/**`
- `stories/**`
- `test/**`
- `.github/**`
- `package.json`
- lockfiles
- build/config files

## Non-Goals

- Do not widen the TypeScript type.
- Do not change runtime behavior.
- Do not add or modify tests unless README-only validation unexpectedly requires it.
- Do not reformat unrelated README sections.
- Do not update dependencies.

## Required Evidence

Required commands:

```bash
git diff -- README.md
git diff --name-only
```

Optional command if dependencies are already installed or install is cheap:

```bash
yarn test
```

If tests are not run, the Trust Brief must clearly say why.

## Reviewer Focus

The reviewer should inspect:

1. Whether the README now matches `src/types/index.ts`.
2. Whether the PR avoids changing runtime/type behavior.
3. Whether the diff is limited to the documented mismatch.

## Failure Conditions

The dogfood should be marked `Blocked` if:

- Any excluded file changes.
- The agent changes runtime behavior without maintainer confirmation.
- The agent changes package metadata, lockfiles, or config files.

The dogfood should be marked `Needs Review` if:

- README is changed but evidence is incomplete.
- Tests are skipped without a clear reason.
- The final PR wording overclaims correctness.

