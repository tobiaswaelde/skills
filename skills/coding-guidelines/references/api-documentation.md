# API documentation baseline

Use the project's established JSDoc/TSDoc dialect and tooling. In TypeScript, describe contracts with TSDoc-compatible comments rather than duplicating declared types in braces. In JavaScript, use JSDoc type annotations when they provide useful type information. Use native documentation comments in other languages.

## Coverage and content

- Document every hand-written function and method, including private/local helpers, accessors, arrow functions, callbacks, and composables. Visibility and simplicity do not exempt a function. For trivial functions, a brief accurate purpose summary is sufficient; avoid placeholder comments and unnecessary tag sets.
- Document every exported type declaration, including type aliases, interfaces, enums, and exported classes that define public types. Explain purpose and domain meaning even when the declared shape is simple. Document fields, component props/events/models, and other contract details where their semantics need explanation. Existing component-doc tooling may be the canonical source.
- Explain caller-relevant constraints where applicable: units, valid ranges, nullability not evident from the type, permissions, side effects, async completion/cancellation, and error/rejection conditions. Keep the required coverage broad while scaling comment detail to the symbol's complexity.
- Place documentation where the project's documentation tooling associates it with the declaration or function expression, including inline callbacks and Vue script functions. Reuse valid inherited/linked documentation where supported. Generated/vendor code remains excluded; changing generated files is not a way to satisfy this policy.
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

Reuse established documentation checks. If introducing ESLint documentation rules in a JS/TS project, use a compatible JSDoc or TSDoc integration and configure its language mode and selectors for the complete required function/composable/exported-type coverage above, rather than limiting checks to public exports. Presence rules and syntax rules are separate decisions: a TSDoc syntax checker does not prove documentation coverage or usefulness.

Validate syntax on representative `.ts`, `.js`, and Vue `<script>` blocks only where those file types exist. When presence checks are enabled, verify missing comments are detected on public and private functions, trivial helpers, callbacks, composables, and exported type declarations; verify documented equivalents pass and generated/vendor files stay excluded. Identify any constructs the selected tooling cannot enforce and review those manually. If no compatible enforcement is installed or agreed, document that coverage is reviewed manually; do not report complete machine enforcement when it only covers public APIs or syntax.

For existing projects with an intentional narrower coverage policy, report the conflict and agree on migration before replacing that policy. Existing documented functions retain useful information; extend coverage without discarding accurate comments or unrelated project conventions.

For dialect details consult the official [TSDoc parameter tag](https://tsdoc.org/pages/tags/param/) and [throws tag](https://tsdoc.org/pages/tags/throws/), or the project's installed JSDoc documentation.
