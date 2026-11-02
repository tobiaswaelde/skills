# Dependency compatibility matrix

Use the rows relevant to the requested update; unsupported or untested combinations remain explicit. Do not infer compatibility solely from a version number's apparent SemVer category.

| Dimension | Inspect and verify |
| --- | --- |
| Source of truth | Owning package manifest, workspace catalog or included config, override/patch, and lockfile |
| Scope | Requested direct packages and necessary coupled/transitive/peer changes; unrelated churn excluded |
| Runtime/toolchain | Node or native runtime, package manager, framework, TypeScript/compiler, CI/container engine constraints |
| Peers and consumers | Library/plugin/parser peer ranges and all affected workspace/application consumers |
| Runtime APIs | Changed defaults, exports/module formats, serialization, generated clients, SSR/browser/native behavior |
| Project pins | Exact protected versions and permitted range semantics; an update report is not override permission |
| Reproducibility | Correct manager and supported runtime, unchanged lockfile after immutable install, install/build scripts understood |
| Validation | Focused group checks and affected-project evidence, with pre-existing failures and unavailable environments separated |
| Release impact | Required changeset/packages, consumer migration notes, and any explicitly deferred major upgrade |

For security-driven updates, verify the advisory, affected resolved version/path, and proposed fixed version from authoritative sources. Keep the remediation scoped; do not label every outdated package vulnerable or treat a dependency override as proof that all affected consumers now work.
