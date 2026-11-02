# API documentation baseline

Use the project's established JSDoc/TSDoc dialect and tooling. In TypeScript, describe contracts with TSDoc-compatible comments rather than duplicating declared types in braces. In JavaScript, use JSDoc type annotations when they provide useful type information. Use native documentation comments in other languages.

## Coverage and content

- Document meaningful public APIs, exported reusable functions/composables, and non-obvious contracts. Explain purpose and caller-relevant constraints: units, valid ranges, nullability not evident from the type, permissions, side effects, async completion/cancellation, and error/rejection conditions where applicable.
- Document exported domain types or fields when their semantics exceed their names and declared shapes. Component props/events/models need contract documentation when meaning, ownership, units, side effects, or constraints are non-obvious. Existing component-doc tooling may be the canonical source.
- Keep trivial local helpers, generated declarations, and self-explanatory accessors free of ceremonial comments. Add a focused explanation to a private helper when it carries a non-obvious invariant. Do not mechanically require a full tag set on every symbol or create placeholders to satisfy a metric.
- Use a concise summary. Add `@param` for meaningful parameter semantics, `@returns` for useful result semantics, `@throws` for caller-relevant failures, and `@remarks`/`@example` when they materially clarify use. Explain promise rejection in async contracts. Omit irrelevant tags, including a meaningless return description for `void`.
- Keep comments aligned with runtime behavior and tests. Do not promise validation, atomicity, thrown types, or access control that implementation does not provide. Update affected API comments when a contract changes.

## TypeScript example

```ts
/**
 * Converts an integer storage quota from MiB to bytes.
 *
 * @param quotaMiB - Non-negative integer quota in mebibytes.
 * @returns The quota in bytes; zero means no storage allocation.
 * @throws {@link RangeError} When the input or result is outside the safe integer range.
 */
export function quotaToBytes(quotaMiB: number): number {
  const bytes = quotaMiB * 1024 * 1024;

  if (!Number.isSafeInteger(quotaMiB) || quotaMiB < 0 || !Number.isSafeInteger(bytes)) {
    throw new RangeError('Quota must fit within the non-negative safe integer range.');
  }

  return bytes;
}
```

The comment supplies units, the zero convention, and error conditions that `number` alone cannot express. This is an illustrative contract, not a utility to copy into every project.

## Enforcement

Reuse established documentation checks. If introducing ESLint documentation rules in a JS/TS project, use a compatible JSDoc or TSDoc integration and configure its language mode and selectors for the intended API surface. Presence rules and syntax rules are separate decisions: a TSDoc syntax checker does not prove that public APIs have useful documentation.

Validate syntax on representative `.ts`, `.js`, and Vue `<script>` blocks only where those file types exist. Verify targeted missing-documentation cases fail when presence rules are enabled, and that trivial private helpers and ignored generated files are not accidentally included. Manually review useful contract coverage even when syntax checks pass. If no compatible enforcement is installed or agreed, document that coverage is reviewed manually; do not report machine enforcement that does not exist.

For dialect details consult the official [TSDoc parameter tag](https://tsdoc.org/pages/tags/param/) and [throws tag](https://tsdoc.org/pages/tags/throws/), or the project's installed JSDoc documentation.
