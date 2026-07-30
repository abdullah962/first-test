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
