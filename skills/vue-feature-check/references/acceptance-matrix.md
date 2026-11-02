# Vue feature acceptance matrix

Select cases that apply to the changed feature and its acceptance criteria; this is not an instruction to rebuild every screen or invent all states.

| Area | Evidence to obtain |
| --- | --- |
| Main journey | Expected visible result and the intended persisted/event/navigation effect |
| Pending and repeated actions | Loading indication, disabled/deduplicated submits, preserved drafts, completion state |
| Empty and populated data | Empty-state action, list/detail content, relevant pagination/filter/sort behavior |
| Validation and failures | Field errors, rejected/failed requests, retry, stale data, and retained user input |
| Permissions | Visibility and allowed/denied interactions using approved synthetic roles; server authorization is a separate check |
| Keyboard and focus | Logical tab sequence, named controls, modal focus entry/return, escape behavior, visible focus |
| Assistive output | Labels, associations, error/status announcements, semantic structure; manual checks supplement scans |
| Responsive content | Supported viewport sizes, overflow, long labels/content, dialog/table usability |
| Localization | All project-supported locales, no hidden missing keys, placeholders/plurals, locale formatting, switching |
| Theme | Existing light/dark modes if the changed surface is theme-sensitive |
| Navigation and rendering | Direct load, client navigation, SSR/hydration, history or refresh only where changed behavior depends on them |

Record the revision, route, scenario, fixture identity (redacted), locale, viewport, and observed result for each relevant case. A missing environment is an explicit gap; an irrelevant case is not a failed requirement. Keep approved screenshots and failing-test references close to their findings rather than producing an unfiltered trace dump.
