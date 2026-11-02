# Documentation: integration and verification

Read the documentation section of the [canonical guideline template](../assets/templates/coding-guidelines.md) before configuring checks. That section owns the required coverage and complete example; do not maintain a competing public-only or trivial-helper exemption here. For non-JS/TS projects, retain the coverage policy but express it through native documentation comments and tooling.

## Dialect and coverage

Reuse the project's established JSDoc/TSDoc dialect and valid inherited/linked documentation where supported. Align comments with implementation and tests, including async cancellation/rejection contracts. Place comments where tools actually associate them with declarations and expressions, particularly inline callbacks and Vue script functions.

For an existing intentional narrower documentation policy, report the conflict and agree on migration before replacing it. Audit-only tasks remain read-only. Keep code cleanup and behavior changes outside a documentation setup unless agreed.

## Enforcement

- Reuse existing documentation checks; add a compatible integration only within the agreed scope. Verify installed versions, language mode, parser coverage, and selectors before enabling rules.
- Configure presence checks for the template's entire required function/composable/exported-type coverage, including private helpers, accessors, and callbacks. Syntax validation alone does not prove presence, usefulness, or semantic accuracy.
- Test documented and undocumented examples in the file types that actually exist, including Vue scripts where applicable. Check declaration forms rather than assuming an export-only selector covers everything. Generated/vendor files remain excluded.
- Identify constructs that the chosen tooling cannot enforce and review those manually. Without installed/agreed enforcement, disclose manual coverage review rather than claiming machine checks that do not exist.

For syntax details consult the official [TSDoc parameter tag](https://tsdoc.org/pages/tags/param/) and [throws tag](https://tsdoc.org/pages/tags/throws/), or the project's installed JSDoc documentation.
