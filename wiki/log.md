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

## [2026-07-30] schema | Reworked to install into an existing vault

**Removed:** `raw/`, `.obsidian/` — the vault already exists and already has both
**Created:** `tools/wiki.config.json`
**Updated:** `CLAUDE.md`, [[overview]], `tools/wiki-lint`, `tools/wiki-search`,
`docs/obsidian-setup.md`, `README.md`, source template
**Notes:** The wiki is now a guest in a vault in active use rather than a vault of its own.
Hard rule added: write inside `wiki/`, read everywhere, modify nothing else. Capture
folders (`Clippings/`, `Daily Notes/`) replace `raw/` and are named in the config rather
than hardcoded. Slugs must now be unique across the whole vault, not just `wiki/`, because
Obsidian resolves links by filename — the linter checks this and the rule is that our page
gets renamed, never theirs. `source_path` is vault-relative and its absence is only a note,
since the wiki may be checked out away from the material.
