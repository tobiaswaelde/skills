---
name: coding-guidelines
description: Analyze and improve coding guidelines in existing projects or establish consistent standards in new projects. Use for project setup, standards audits, or gaps in linting, JSDoc/TSDoc, formatting, and Vue SFC block/attribute ordering. Apply framework-specific rules only to matching projects; ordinary feature work follows established project standards without triggering a repository-wide audit.
---

# Coding guidelines

Establish clear, executable coding standards, adapting to the project's language, framework, and existing conventions. The bundled web baseline distills CRM tenant-web and Crateview registry-ui; it is self-contained and does not require either repository, its paths, or its dependencies.

## 1. Inspect before changing standards

- Read applicable project instructions, contribution guides, coding guidelines, manifests and lockfiles, formatter/linter/type-check configuration, editor settings, CI, and any hooks. In monorepos inspect inherited and package-local configuration. Sample representative source, including Vue components and public APIs where applicable.
- Distinguish documented rules, effective executable rules, and observed code habits. Check effective configuration for representative files, including parser and file globs; an installed plugin or documented rule alone does not prove enforcement.
- Identify whether this is new-project setup, an existing-project extension, or an audit-only request. For existing projects, summarize current coverage, gaps, and conflicts in linting, documentation, and relevant framework conventions. Propose concrete changes rather than silently treating this baseline as superior to deliberate project choices.
- Preserve existing intentional conventions. Add missing compatible rules within the requested setup/update scope; obtain agreement before contradictory standards, toolchain migrations, or repository-wide reformatting. An audit-only request remains read-only. Follow the active delivery workflow and its approvals; reuse its issue, branch, and approvals instead of starting a second delivery process.

## 2. Choose the applicable baseline

- For JavaScript/TypeScript projects, read [web standards](references/web-standards.md) for formatting, linting, script contracts, and the conditional Vue rules. Use the installed framework's compatible integration and package manager. For other languages, use their native equivalents; do not install ESLint, Prettier, Vue tooling, or Node solely to apply this skill.
- For JavaScript/TypeScript documentation, read [API documentation](references/api-documentation.md). For other languages, apply its contract-focused approach through native doc comments and tooling.
- In new projects without established conventions, use the bundled defaults for applicable technologies and document the chosen standards. In existing projects, preserve intentional deviations and explain which gaps were filled. If prose and effective configuration contradict each other and intent is unclear, resolve the intended rule before changing behavior.

## 3. Document and enforce the agreed standards

- Extend the existing canonical guideline document. If absent, create `docs/coding-guidelines.md` or the project's established documentation equivalent and link it from the existing contributor entry point. Add only a concise pointer to an existing agent-instruction file when needed; keep detailed rules in one place.
- Cover applicable formatting and naming conventions, lint/type checks, public API documentation, framework/component structure, exact Vue block and attribute ordering where relevant, generated/vendor exclusions, and the actual check/fix commands. Use en-US for new guideline documentation unless the project's established documentation language or the user specifies otherwise.
- Implement agreed lint/format enforcement in existing configuration and scripts. Preserve framework presets, parsers, monorepo boundaries, package-specific rules, and CI behavior. Generated framework configuration is an input to the integration, never a hand-edited output. A formatter does not substitute for Vue ordering or semantic lint rules.
- Keep non-mutating checks separate from explicit fix commands. Add missing CI checks to the existing pipeline when CI is in scope; otherwise document the remaining gap. Reuse existing hooks if present; installing hooks or adding a new CI provider is not mandatory. Update dependency manifests and the owning lockfile only when compatible tooling changes are actually needed.
- Document meaningful contracts rather than adding comments that repeat names and types. Syntax checks can validate doc-comment structure; review actual API coverage and semantic accuracy separately. If introducing a new documentation lint plugin, verify its supported language mode, installed versions, and rule selectors before enabling it.
- Limit code changes to agreed scope. Preserve behavior, binding/spread precedence, side-effect imports, API contracts, localization, and accessibility. Separate broad cleanup from standards setup unless expressly requested. Never weaken rules or blanket-ignore source merely to get a passing check.

## 4. Verify the rules work

- Run the actual project checks and show any pre-existing failures separately. Validate a representative source file for each affected language/package and a Vue SFC when present. Confirm agreed rules are active in effective configuration and CI commands really execute the intended checks.
- In isolated fixtures, verify a valid example passes and representative violations fail: Vue block order and attribute grouping, relevant lint rules, and documentation syntax/coverage rules if configured. Keep fixtures and temporary dependencies out of project source and do not alter the reference repositories.
- When formatter or auto-fix behavior changes, apply the chosen formatting/fix pipeline twice to representative fixtures. The second pass must produce identical bytes, and the resulting files must pass both non-mutating checks. Resolve conflicts in configuration or a documented deterministic pipeline; report a remaining conflict instead of claiming convergence.
- For existing projects, check that the audit and fixes left unrelated files intact and deliberate conventions preserved. Re-running the setup should not create duplicate guideline files, config entries, scripts, or CI steps.
- Report established/retained standards, checks actually run, unresolved decisions, and remaining enforcement gaps. Follow the user's concise `Changed`, `Tests`, and `Notes` format.
