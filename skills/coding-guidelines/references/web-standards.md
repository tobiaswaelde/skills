# Web standards: integration guidance

Read the [canonical guideline template](../assets/templates/coding-guidelines.md), especially JavaScript/TypeScript and Vue sections, before implementing rules. Formatting defaults, documentation policy, and executable Vue rule fragments live there rather than in a second baseline. Read [source analysis](source-analysis.md) when an observed source rule or a difference between the references needs verification.

## Adapt to the target project

- Preserve existing intentional choices and agree on contradictory migrations. For new projects use the applicable template defaults; prefer LF when no line-ending policy exists. Do not describe those defaults as universal framework requirements.
- Reuse the installed ESLint/framework integration and package manager. Nuxt integrations may combine generated inputs with project rules; edit project-owned config, not `.nuxt` output. Confirm file globs and Vue/TypeScript parser delegation in effective configuration.
- The template's JavaScript fragments are rule objects, not complete configurations or dependency manifests. Merge them into compatible Vue-file configuration with registered plugins/parsers; verify supported options against the installed version.
- Preserve working formatter/linter alternatives instead of adding overlapping tools. Use zero-warning checks in new setups; retain established script names and actual thresholds for existing projects. In monorepos retain package ownership, affected checks, and check-only versus mutating cache behavior.
- Import organization and Tailwind sorting are conditional integrations. Discover the real stylesheet/configuration and review import edits for side effects. Do not transfer CRM aliases, Nx targets, registry protocols, locale namespaces, or dependency versions into unrelated projects.
- For Vue 2, JavaScript SFCs, Options API, or legitimate extra script/custom blocks, preserve compatible syntax and existing component contracts. A style setup does not authorize a Vue migration. Significant whitespace and `v-bind`/event precedence require actual diff review, not a claim that every autofix is behavior-neutral.

## Formatter and lint agreement

A Prettier pass may collapse a short text element while the template's content-newline rules expand it. Use compatible configuration or a deterministic full pipeline and make the non-mutating format check compare against its complete canonical output. A separate vanilla Prettier check must not reject the chosen final layout.

Verify the pipeline on representative fixtures twice, requiring identical second-pass bytes and passing final lint/format checks. Inspect remaining diagnostics after autofixes; one invocation may not settle every ordering violation. Report conflicts instead of silently dropping agreed rules or alternating writes indefinitely. A custom Vue formatting pass is an integration option, not a requirement to copy a reference repository's script.

## Version-specific sources

Use the official [attribute-order rule](https://eslint.vuejs.org/rules/attributes-order), [block-order rule](https://eslint.vuejs.org/rules/block-order), and the installed framework/formatter documentation to resolve compatibility questions. The skill and template are self-contained; reference-project checkouts are not runtime dependencies.
