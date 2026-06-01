# ARC Trust Brief: Pass

The change stayed inside the frozen contract scope and updates only the README type table so it matches the existing exported TypeScript type.

## Focus Questions

1. Should `receivedRedirectUri` remain `string | undefined`, or does Plaid intend to support `null` as a public API type?
2. Is a README-only fix preferred over widening the TypeScript type for this mismatch?

<details>
<summary>Receipts</summary>

- Target repository: `plaid/react-plaid-link`
- Target issue: https://github.com/plaid/react-plaid-link/issues/398
- Contract: `dogfood/001-plaid-react-plaid-link-398/contract.md`
- Branch: `fix-received-redirect-uri-readme-type`
- Commit: `3c9b3d2aa7f324f81e88ca34515c8b4de5764db0`
- Changed files: `README.md`
- Allowed scope satisfied: yes
- Excluded files touched: no
- Required evidence collected:
  - `git diff -- README.md`
  - `git diff --name-only`
- Tests not run: README-only documentation change.

</details>

## Verdict Notes

ARC cannot prove the semantic API choice is correct. It only verifies that the implementation followed the frozen assignment:

- Do not change runtime behavior.
- Do not widen TypeScript types.
- Update only the README mismatch.

