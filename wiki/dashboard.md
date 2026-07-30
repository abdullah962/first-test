---
title: Dashboard
type: meta
tags: [meta]
created: 2026-07-30
updated: 2026-07-30
sources: []
status: developing
---

# Dashboard

Live views over the wiki, driven by page frontmatter. Requires the **Dataview** community
plugin — see `docs/obsidian-setup.md`. Without it the queries below render as plain code
blocks and nothing breaks; the wiki just loses these views.

For the catalog, see [[index]]. For structural problems Dataview cannot see — broken
links, orphans, type mismatches — run `tools/wiki-lint`.

## Active projects

```dataview
TABLE WITHOUT ID file.link AS Project, area AS Area, status, updated
FROM "wiki/projects"
WHERE state = "active"
SORT updated ASC
```

Sorted oldest-touched first: the top row is the project most likely to have quietly
stalled.

## Areas going untended

Areas with no update in 90 days.

```dataview
TABLE WITHOUT ID file.link AS Area, updated AS "Last touched", status
FROM "wiki/areas"
WHERE date(updated) < date(today) - dur(90 days)
SORT updated ASC
```

## Recently updated

```dataview
TABLE WITHOUT ID file.link AS Page, type, status, updated
FROM "wiki"
WHERE type != "meta"
SORT updated DESC
LIMIT 15
```

## Thin spots

Pages resting on a single source. Not wrong — just worth knowing which beliefs stand on
one entry (`CLAUDE.md` §4).

```dataview
TABLE WITHOUT ID file.link AS Page, type, sources AS Source
FROM "wiki"
WHERE type != "meta" AND type != "source" AND length(sources) = 1
SORT file.name ASC
```

## Stubs

```dataview
TABLE WITHOUT ID file.link AS Page, type, created
FROM "wiki"
WHERE status = "stub" AND type != "meta"
SORT created ASC
```

## Orphans

Pages nothing else links to. Usually means the propagation step was skipped.

```dataview
LIST
FROM "wiki"
WHERE length(file.inlinks) = 0 AND type != "meta"
SORT file.name ASC
```

## People to reconnect with

Anyone whose `last_contact` is over 60 days old.

```dataview
TABLE WITHOUT ID file.link AS Person, relationship, last_contact AS "Last contact"
FROM "wiki/people"
WHERE last_contact AND date(last_contact) < date(today) - dur(60 days)
SORT last_contact ASC
```

## Sources by month

```dataview
TABLE WITHOUT ID file.link AS Source, source_type AS Type, source_date AS Dated
FROM "wiki/sources"
SORT source_date DESC
LIMIT 20
```
