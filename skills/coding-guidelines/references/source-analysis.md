# Reference-project analysis

Inspected on 2026-11-02 from clean local checkouts. This is a recorded source snapshot, not a dependency on those repositories or a claim that their complete CI suites were run.

| Source | Commit | Inspected evidence |
| --- | --- | --- |
| CRM / tenant-web | `e5066fbf1655e45a0ed2c4919b3bae6fee6f78eb` | Root `AGENTS.md`; `apps/tenant-web/{eslint.config.mjs,.prettierrc,package.json,project.json}`; `config/eslint/eslint.config.{base,web,web-spacing}.mjs`; `docs/lint-format-hooks-plan.md`; `.forgejo/workflows/nx-lint.yml` |
| Crateview / registry-ui | `9b79ae77be440193a92c9059b676e284aa27b80a` | `docs/coding-guidelines.md`; `eslint.config.mjs`; `.prettierrc.json`; `package.json`; `README.md`; `.github/workflows/ci.yml` |

Effective ESLint configuration was queried with `--print-config app/app.vue` from each application root, using the existing installed dependencies. Prose, shared configurations, and app-effective rules are distinguished below.

## Common baseline and differences

| Topic | CRM tenant-web | Registry-ui | Template decision |
| --- | --- | --- | --- |
| Basic format | Two spaces, semicolons, single quotes, trailing commas, width 120 | Same | Shared new-project baseline; retain intentional existing deviations |
| Line endings | Prettier `endOfLine: auto` | Same | Resolve project policy explicitly; prefer LF for a new project, not falsely attribute LF to the source configs |
| Vue script/style indentation | Unindented by Vue override | Same | Shared baseline |
| Vue attribute categories | Identical 12 categories; no alphabetical sorting; effective severity warning | Same categories, effective severity error | Shared exact order; default to error for new projects |
| Warning threshold | `lint:check` rejects warnings via `--max-warnings=0` | Established `lint` runs `eslint .` | Preserve actual project command semantics; do not claim both source scripts reject all warnings |
| Vue block order | No active `vue/block-order` on sampled SFC | Active template/script/style error rule | Explicit default derived from registry-ui, not claimed as shared enforcement |
| Vue spacing | Import separation, block padding, HTML indentation and content-newline rules | Same layout rules | Shared baseline; verify formatter convergence |
| TypeScript `any` | Not configured in sampled app-effective SFC config; shared base config turns it off but tenant-web does not import that base | Effective error | Prefer typed boundaries for new standards; do not misreport shared config as active tenant-web enforcement |
| Nuxt/ESLint integration | Direct Vue flat config with delegated TypeScript script parser | Generated Nuxt integration through `withNuxt` | Adapt the project integration; never copy generated files or assume one config shape |
| Import/Tailwind formatting | Organize-imports and Tailwind Prettier plugins | Same, with project stylesheet path | Conditional integrations; discover real target stylesheet and preserve side-effect imports |
| Documentation prose | `AGENTS.md` asks for JSDoc only for valuable public APIs | No comprehensive JSDoc/TSDoc coverage policy in inspected guidelines | User's explicit all-functions/composables/exported-types preference is an added template policy; flag conflicts in existing projects |
| Documentation lint | No active `jsdoc/*` or `tsdoc/*` rules on sampled SFC | Same | Do not claim comprehensive documentation enforcement exists in either source |
| Commands and CI | pnpm/Nx, package-local scripts, affected lint/format checks in Forgejo | npm, Nuxt checks and GitHub CI | Render verified target commands; no provider, package manager, Nx, or CRM alias dependency |
| Hooks | Root Husky/lint-staged documented in lint/format plan | Not a shared requirement in inspected guidelines | Optional, not a universal installation requirement |
| Behavior protection | Existing project contracts and boundaries apply | Explicit accessibility, localization, model adapters, auth/protocol preservation | Preserve project behavior and existing contracts during style cleanup |

## Reusable template

The concrete [guideline template](../assets/templates/coding-guidelines.md) is the canonical reusable policy. It includes formatting, full documentation coverage, exact Vue rule fragments, behavior safeguards, and contribution checks. Supporting web/API references explain integration and verification rather than maintaining separate copies of that policy.

Resolve its project fields from the target repository and remove irrelevant technology sections. The verification table must state actual working directories, existing or implemented commands, and check/fix behavior. New-project defaults, observed source rules, and the user's later documentation preference must remain distinguishable when discussing provenance.

## Validation limits

This analysis inspected source/configuration and representative effective rules; it did not run either repository's full tests or production build, audit every source declaration, or modify either reference project. The existing isolated Vue fixture tests demonstrate ordering enforcement and a canonical compound formatting pipeline; they do not prove that each source repository's existing standalone Prettier/ESLint commands already converge for all files.
