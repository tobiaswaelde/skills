<!-- TEMPLATE: Resolve every {{TOKEN}}, remove this note and the section markers, and omit technology-specific sections that do not apply. Preserve deliberate existing project conventions; agree on contradictory migrations first. Replace the commands table with commands verified in the target project. Do not publish unresolved tokens or references to this plugin. -->

# Coding guidelines — {{PROJECT_NAME}}

## Project scope and sources of truth

These guidelines apply to {{PROJECT_SCOPE}}.

- Runtime and package manager: {{RUNTIME_AND_PACKAGE_MANAGER}}.
- Canonical lint, formatting, type-check, and editor configuration: {{CONFIGURATION_PATHS}}.
- Project-specific adaptations and agreed deviations: {{PROJECT_ADAPTATIONS}}.
- Keep this document and effective tooling configuration aligned. Record intentional exceptions instead of silently changing conventions or weakening checks.
- Exclude dependencies, vendor sources, generated code/data, build output, coverage, and test reports from style cleanup. Let the owning package manager update lockfiles.
- Keep changes scoped. Style cleanup must preserve behavior, API/authentication contracts, permissions, pending/error states, drafts, confirmations, accessibility, and localization placeholders.

<!-- section:javascript-typescript:start -->
## JavaScript and TypeScript

### Formatting

| Convention | Standard |
| --- | --- |
| Indentation | Two spaces |
| JavaScript/TypeScript strings | Single quotes |
| Semicolons | Required |
| Trailing commas | All supported positions |
| Print width | 120 characters |
| Line endings | {{LINE_ENDING_POLICY}} |
| Imports | Adjacent imports together; one blank line before subsequent statements |

Use the established ESLint/framework integration and formatter configuration. Preserve useful recommended rules and TypeScript parsing. Do not edit generated framework configuration such as Nuxt's `.nuxt` output.

Prefer domain types or `unknown` with narrowing over `any`; boundary exceptions require a specific reason. Validate external inputs at established boundaries. Reuse existing modules, components, and composables instead of importing conventions or aliases from another repository.

Use existing import organization where configured, reviewing removals and reordering for side effects. Sort Tailwind classes only when Tailwind is used, through the compatible plugin and the actual project stylesheet/configuration. HTML attribute quotes remain controlled by the framework/formatter.

### Documentation

Document every hand-written function and method, including private/local helpers, accessors, arrow functions, callbacks, and composables. Also document every exported type declaration, including type aliases, interfaces, enums, and exported classes that define public types. Visibility and simplicity do not exempt a declaration; a brief accurate purpose summary is sufficient for a trivial helper.

- Use the project's JSDoc/TSDoc dialect. In TypeScript, describe contracts without repeating declared types in braces. In JavaScript, provide useful JSDoc type information where needed.
- Place comments where the documentation tooling associates them with the declaration or expression. Reuse valid inherited/linked documentation where supported.
- Explain relevant units, valid ranges, ownership, side effects, permissions, async completion/cancellation, and errors/rejections. Document component props/events/models and type fields when their semantics need explanation.
- Use a concise summary. Add useful `@param`, `@returns`, `@throws`, `@remarks`, or `@example` tags; omit irrelevant tags and placeholder descriptions. Update comments with contract changes and verify promises against implementation/tests.
- Generated/vendor declarations remain excluded. Document unsupported enforcement cases through manual review rather than claiming complete machine coverage.

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

This example illustrates a documented contract; it is not a utility every project must implement. A syntax-only documentation checker does not verify that required declarations are documented or that comments are correct. Verify presence and useful contract coverage separately.
<!-- section:javascript-typescript:end -->

<!-- section:vue:start -->
## Vue single-file components

### Blocks and components

- Order top-level blocks as `<template>`, `<script setup lang="ts">`, then `<style>` when present. Separate blocks with one blank line. Omit unused blocks; retain legitimate additional script/custom blocks according to project conventions.
- Keep script and style contents unindented relative to their block. Use two spaces per nested template level.
- Use multiline content for non-inline elements, with the `span` exception below. Preserve significant whitespace, preformatted content, and mixed inline text.
- Use typed `defineProps()` and `defineEmits()` in new TypeScript components. Use `defineModel()` for component-owned two-way binding contracts where appropriate; retain explicit validation/persistence adapters and existing event behavior.
- Reuse the project's UI library and local components. Preserve semantic elements, accessible names, keyboard behavior, focus handling, locale keys, supported locales, and interpolation placeholders.
- Do not convert Options API components or rewrite application behavior solely for style. Vue 2/JavaScript components require their compatible syntax; this Vue 3/TypeScript baseline does not justify a framework migration.

### Attribute order

The canonical order is structural/behavioral directives, identity bindings, two-way bindings and remaining directives, global attributes, static values, dynamic values, events, then content bindings. Preserve within-category order rather than sorting alphabetically. Keep `v-if` and `v-for` on appropriate separate wrappers when their scopes differ.

Apply this rule fragment within the project's compatible Vue ESLint configuration:

```js
const vueOrderingRules = {
  'vue/block-order': ['error', { order: ['template', 'script', 'style'] }],
  'vue/attributes-order': [
    'error',
    {
      order: [
        'DEFINITION',
        'LIST_RENDERING',
        'CONDITIONALS',
        'RENDER_MODIFIERS',
        'UNIQUE',
        'TWO_WAY_BINDING',
        'OTHER_DIRECTIVES',
        'GLOBAL',
        'ATTR_STATIC',
        'ATTR_DYNAMIC',
        'EVENTS',
        'CONTENT',
      ],
      alphabetical: false,
    },
  ],
};
```

The fragment does not register plugins or parsers. Preserve the framework integration and configure Vue script parsing for the actual language. Review ordering fixes for `v-bind` spread precedence, duplicate bindings/events, slots, comments, and component contracts; narrow the fix or document a justified exception when reordering changes behavior.

### Layout rules and formatting pipeline

```js
const vueSpacingRules = {
  'vue/html-indent': [
    'error',
    2,
    { attribute: 1, baseIndent: 1, closeBracket: 0, alignAttributesVertically: true, ignores: [] },
  ],
  'vue/multiline-html-element-content-newline': [
    'error',
    { ignoreWhenEmpty: true, ignores: ['span'], allowEmptyLines: false },
  ],
  'vue/padding-line-between-blocks': ['error', 'always'],
  'vue/singleline-html-element-content-newline': [
    'error',
    { ignoreWhenNoAttributes: false, ignoreWhenEmpty: true, ignores: ['span'] },
  ],
};
```

These fragments require compatible installed rules and a tested formatting pipeline. A plain Prettier pass may collapse text that these content-newline rules expand. Choose compatible configuration or a deterministic full formatting pipeline, and make the non-mutating format check accept that complete canonical output. Do not alternate incompatible formatter/linter writes or claim convergence from isolated checks.
<!-- section:vue:end -->

## Verification and contribution checks

Run the verified commands below from the stated working directory and with the project's configured runtime. Existing equivalent command names are acceptable; do not invent scripts or copy another repository's paths.

{{VERIFICATION_COMMANDS_TABLE}}

- Keep checks non-mutating and fixes explicit. Prefer zero-warning lint checks for new setups; record the established threshold for existing projects.
- Separate typechecking, unit tests, build checks, and relevant integration/component/browser tests. Run the checks warranted by the changed behavior; style-only checks do not prove functional correctness.
- Apply the agreed formatting/fix pipeline twice to representative files; the second pass must produce identical bytes and pass the final lint and formatting checks. Inspect remaining diagnostics rather than assuming one fix pass resolves everything.
- Verify documented and undocumented declarations against the intended documentation checks, including private helpers, callbacks, composables, and exported types. Review semantic accuracy and any tooling coverage gaps manually.
- Check fixtures with valid and invalid conventions. Re-running setup must not duplicate configuration, documents, scripts, or pipeline steps.

CI and optional hooks: {{CI_AND_HOOKS}}.

In monorepos, retain package-local overrides, the owning task runner, affected-project selection, and existing CI providers. Cache non-mutating checks where safe; do not cache mutating fix targets. Introduce hooks only when agreed, and ensure staged-file checks preserve unrelated changes.
