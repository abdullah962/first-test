---
title: Log
type: meta
tags: [meta]
created: 2026-07-30
updated: 2026-07-30
sources: []
status: developing
---

# Log

Append-only, newest at the bottom. Never rewrite past entries.

Every entry starts with `## [YYYY-MM-DD] <type> | <subject>` so the file stays greppable:

```bash
grep "^## \[" wiki/log.md | tail -5      # recent activity
grep "^## \[" wiki/log.md | grep ingest  # every ingest
```

Types: `ingest`, `query`, `lint`, `schema`, `refactor`.

---

## [2026-07-30] schema | Wiki initialized

**Created:** [[index]], [[log]], [[overview]], [[synthesis]], [[open-questions]],
[[contradictions]], page templates, `tools/wiki-search`, `tools/wiki-lint`
**Notes:** Empty scaffold, no sources ingested. Conventions defined in `CLAUDE.md`;
category folders (`entities/`, `concepts/`) are defaults to be adjusted once the domain
is known.

## [2026-07-30] schema | Specialized as a second brain, wired to Obsidian

**Created:** [[dashboard]], `people/`, `areas/`, `projects/`, `raw/journal/`,
person/area/project templates, `.obsidian/` vault settings, `docs/obsidian-setup.md`
**Updated:** `CLAUDE.md`, [[index]], [[overview]], [[synthesis]], [[open-questions]],
[[contradictions]], source/concept/entity templates, `tools/wiki-lint`, `README.md`
**Notes:** Domain fixed as personal knowledge management. Added §4 *Time and change* —
present-tense pages, dated *How this changed* sections, and the rule that a pattern needs
three dated instances before it is stated as one. [[contradictions]] now registers changes
of position alongside source conflicts. Linter gained folder/type matching and project
`state` validation. Repository is public and must be made private before real material
lands in `raw/`.
