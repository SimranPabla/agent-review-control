# PR Body

```markdown
## Summary

Updates the README type table for `receivedRedirectUri` to match the exported TypeScript type.

`src/types/index.ts` currently declares:

```ts
receivedRedirectUri?: string;
```

So the README should document this as `string | undefined`, not `string | null | undefined`.

Fixes https://github.com/plaid/react-plaid-link/issues/398

## Validation

- Confirmed `src/types/index.ts` declares `receivedRedirectUri?: string`
- Confirmed the diff only changes `README.md`
- Tests not run: README-only documentation change

## Review receipt

ARC Trust Brief: `Pass`

The change is limited to the README mismatch and does not change runtime behavior, exported types, package metadata, lockfiles, or config.

Focus questions:

1. Should `receivedRedirectUri` remain `string | undefined`, or does Plaid intend to support `null` as a public API type?
2. Is a README-only fix preferred over widening the TypeScript type for this mismatch?
```

