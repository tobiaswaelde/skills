# JavaScript, TypeScript, and Vue standards

Use these defaults when starting a matching project or filling an agreed gap. Existing deliberate conventions take precedence until a migration is agreed. These are personal project conventions, not claims that a framework requires this style.

## Formatting and linting

- Two-space indentation, semicolons, single-quoted JavaScript/TypeScript strings, trailing commas, and a 120-character print width. Preserve the existing line-ending policy; use LF in a new project. HTML attribute quotes remain controlled by the framework/formatter.
- Separate the final import from following statements with a blank line; keep adjacent imports together. Use existing import organization where configured. Review side-effect imports before applying import removal or sorting.
- In new JS/TS projects, prefer ESLint plus Prettier with compatible framework/TypeScript presets and a single clear formatting pipeline. Preserve an existing working alternative rather than installing overlapping tools. Retain Nuxt's integration when present; do not replace it with a generic Vue config or edit generated `.nuxt` files.
- Use strict static checks appropriate to the existing type system. Prefer known domain types or `unknown` with narrowing over `any`; exceptions need a concrete boundary reason. Add compatible rules for previously uncovered cases rather than disabling recommended presets.
- Exclude dependencies, generated code, framework/build output, coverage/test reports, and vendor sources. Let the package manager update lockfiles; do not run style fixes over lockfiles or generated data. Tailwind class sorting is conditional on Tailwind being used, with its real stylesheet/configuration supplied to the matching plugin.
- Standard command intent: `lint:check` is non-mutating and fails on warnings; `lint:fix` explicitly applies ESLint fixes; `format:check` checks formatting; `format:fix` explicitly formats. Reuse equivalent established commands instead of adding aliases just to rename them. Keep typechecking separate and discover its actual command. In monorepos use the owning package/task runner and preserve check-only versus mutating cache behavior.

## Vue single-file components

Default top-level order is `<template>`, `<script setup lang="ts">`, then `<style>` when present. Separate top-level blocks with one blank line. Do not add empty blocks or convert existing Options API/custom blocks solely for style. Account for legitimate additional script/custom blocks through the project's existing conventions.

Keep script and style contents unindented relative to their block. Templates use two spaces per nested level. Use multiline element content for non-inline elements, retaining the reference projects' `span` exception; preserve meaningful whitespace in text, preformatted content, and mixed inline markup. Apply structure rules only when they preserve rendering and behavior.

For matching versions of `eslint-plugin-vue`, the canonical ordering rules are:

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

Merge these rules into the project's Vue-file configuration and ensure `vue-eslint-parser` delegates TypeScript script parsing appropriately. The snippet is a rule fragment, not a complete ESLint config or dependency manifest. Verify actual categories and supported options against the installed plugin version.

Attribute groups mean structural/behavioral directives first; identity bindings such as `key`/`ref`, two-way binding and remaining directives next; then global attributes such as `id`, static values, dynamic values, event listeners, and content bindings. Preserve order within categories rather than imposing alphabetical sorting. Use a separate wrapper if a loop and a condition need different scopes; an ordering rule is not a reason to put `v-if` and `v-for` on the same element.

Ordering fixes must preserve `v-bind` spread precedence, duplicate binding/event semantics, comments, slots, and component contracts. Review the actual diff and narrow a fix or record a justified exception when simple reordering changes behavior.

The reference spacing rules, where compatible with the project's formatter, are:

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

Do not assume these ESLint layout rules and Prettier are compatible in every project. Preserve the chosen final template layout through a tested deterministic pipeline if needed, and make check commands verify that final output. Never claim compatibility based only on both tools being installed. The reference projects sometimes use a dedicated Vue formatting pass; reproduce the required behavior locally only when necessary, not their repository-specific paths.

For example, Prettier can collapse a short text element while the content-newline rule expands it. If the agreed pipeline is Prettier followed by Vue lint fixes, its non-mutating format check must compare the source against that complete canonical output. A separate vanilla Prettier check rejecting that final output would contradict the pipeline; align the check instead of alternating writes or silently dropping the intended layout rule. Inspect remaining diagnostics after fixes; one autofix invocation may not resolve every ordering violation.

For new Vue components use typed props/emits and `<script setup lang="ts">`. Follow existing model/event contracts; do not rewrite components or remove explicit validation/persistence adapters just for style. Preserve accessibility, localization, and pending/error states.

## Reference provenance

Defaults were derived from CRM tenant-web's Prettier, Vue attribute/spacing config, and lint/format contract, together with Crateview registry-ui's `docs/coding-guidelines.md` and ESLint block ordering. Both use the attribute categories above. CRM can express ordering as warnings while its check script rejects warnings; new-project defaults above use errors directly. Project names are provenance only, never required runtime checkouts or package aliases.

For version-specific semantics consult the official [Vue attribute-order rule](https://eslint.vuejs.org/rules/attributes-order), [block-order rule](https://eslint.vuejs.org/rules/block-order), and the installed formatter/framework documentation.
