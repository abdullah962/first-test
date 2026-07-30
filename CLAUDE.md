# Wiki Schema — Second Brain

This repository is a **persistent personal wiki**: a knowledge base about one person's
life, work, and thinking, built and maintained by you (the LLM) from a curated collection
of raw material. Knowledge is compiled once, on ingest, and then kept current — not
re-derived from scratch on every question.

You own the `wiki/` directory entirely. The human owns `raw/`. It is read as an Obsidian
vault. Read this file at the start of every session before touching anything.

---

## 1. Layers

| Layer | Path | Who writes it | Rules |
|---|---|---|---|
| Raw material | `raw/` | Human | **Immutable.** Read only. Never edit, rename, move, or delete. |
| Wiki | `wiki/` | You | You create, update, cross-reference, and reorganize freely. |
| Schema | `CLAUDE.md` | Both | Co-evolved. Propose changes; the human approves. |

The human lives the life, captures the material, and asks the questions. You do the
summarizing, cross-referencing, filing, and bookkeeping.

**This is personal material.** Journal entries, health, finances, relationships, notes on
named people who did not consent to being written about. Treat every page as private by
default. Never copy `raw/` or `wiki/` content into an external service, a public artifact,
or a commit message. If you are ever asked to publish, share, or summarize this wiki
outward, confirm first — every time.

---

## 2. Wiki layout

```
wiki/
  index.md            catalog of every page (content-oriented)
  log.md              append-only chronological record
  overview.md         entry point: what this wiki holds, current state
  dashboard.md        live Dataview queries — what needs attention
  synthesis.md        the current picture: what it all adds up to right now
  open-questions.md   unresolved questions about yourself, gaps, things to watch
  contradictions.md   register of conflicts between sources AND changes of mind
  sources/            one page per ingested item — article, entry, episode, conversation
  people/             people in your life
  areas/              ongoing life domains with no end date: health, career, money
  projects/           efforts with a defined outcome and an end
  concepts/           ideas, mental models, methods, frameworks
  entities/           other concrete things: organizations, places, tools, products
  analysis/           filed answers to queries: patterns, comparisons, deep dives
  templates/          page templates to copy when creating new pages
```

**Areas vs. projects** is the distinction that keeps this from turning to mush. An area is
a standing responsibility you never finish — `areas/health`, `areas/career`. A project has
a finish line — `projects/learn-arabic-calligraphy`. Projects belong to an area; say so in
frontmatter (`area: health`). When a project ends, set `state: done` and fold what it
taught you back into its area page. Do not delete it.

Add or rename categories when your life calls for it — then update this file and
`index.md` to match.

---

## 3. Page conventions

**Filenames** are kebab-case slugs: `wiki/people/samir-haddad.md`. One topic per page.
Slugs are stable — if a page must be renamed, update every inbound link in the same pass.

**Frontmatter** is required on every page:

```yaml
---
title: Sleep
type: area              # source | person | area | project | concept | entity | analysis | meta
tags: [health, energy]
created: 2026-07-30
updated: 2026-07-30
sources: [2026-07-14, huberman-sleep-toolkit]   # slugs of wiki/sources/ pages backing this
status: developing      # stub | developing | mature
---
```

Per-type additions:

| Type | Adds |
|---|---|
| `source` | `source_path`, `source_type`, `author`, `source_date`, `ingested` |
| `project` | `state` (`active` / `paused` / `done` / `abandoned`), `area` |
| `person` | `relationship` (how you know them), optional `last_contact` |

`status` means: **stub** = placeholder, one or two facts; **developing** = real content,
known gaps; **mature** = well-covered, would only change on new information.

**The frontmatter is load-bearing.** `wiki/dashboard.md` runs Dataview queries over these
fields — stale areas, stub pages that matter, single-source claims, orphans. Sloppy
frontmatter silently empties the dashboard, so fill every field on every page.

**Links** are Obsidian wikilinks by slug, no path and no extension: `[[samir-haddad]]` or
`[[samir-haddad|Samir]]`. Slugs are unique across the whole wiki, so links resolve
regardless of folder. Link generously — the cross-reference graph is the point, and it is
what the graph view shows. Every claim traceable to a source cites it: `([[2026-07-14]])`.

**Never link to a page you have not created.** If something deserves a page you are not
writing yet, create a stub with frontmatter and a one-line definition rather than leaving
a dangling link.

**Callouts** render natively in Obsidian — use them:

```markdown
> [!warning] Changed position
> Until 2026-03 the working theory was X ([[2026-01-08]]). [[2026-06-20]] contradicts it.
> See [[contradictions]].
```

---

## 4. Time and change

This is the part a personal wiki gets wrong most often, and the part worth getting right.

**Facts about a person expire.** Jobs, goals, opinions, weight, who you are close to. A
wiki that only accumulates becomes a pile of stale claims stated in the present tense.

- Every page's main body describes **the present**. Rewrite it; do not append to it.
- When something changes, the old version does not vanish — it moves to a **How this
  changed** section, dated, one line: `2026-03 → 2026-07: stopped wanting the management
  track; see [[2026-06-20]].` Being able to watch your own positions move over years is
  the single most valuable thing this wiki can give you.
- Register genuine reversals in [[contradictions]] as `changed` — same register as source
  conflicts, different kind.

**One entry is a datapoint, not a pattern.** A journal entry written at 2am after a bad
day is evidence about that night, not about the person. Do not promote a single bad mood
into a standing claim on a person page. Say "wrote on [[2026-07-14]] that…" until the
thing recurs; call it a pattern only when you can cite three or more instances across
time, and cite them.

**Record and connect; do not diagnose.** Surface the pattern — "sleep under 6h shows up in
four of the five entries before a stalled week" — and let the human draw the conclusion.
No clinical labels, no psychoanalysis, no advice unless asked.

---

## 5. Operation: Ingest

Trigger: the human drops a file in `raw/` and asks you to process it. Daily notes land in
`raw/journal/` automatically.

1. **Read the source fully.** If it references local images (`raw/assets/`), read the text
   first, then view the images that matter — you cannot do both in one pass.
2. **Discuss key takeaways** with the human before writing, unless they asked for an
   unsupervised batch ingest. Surface anything surprising early.
3. **Write `wiki/sources/<slug>.md`** from `wiki/templates/source.md`. Journal entries use
   the date as the slug: `wiki/sources/2026-07-14.md`.
4. **Propagate.** This is the part that makes the wiki worth having. For each person,
   area, project, and concept the source touches:
   - update the existing page's present-tense body, or create the page if missing;
   - add the source slug to that page's `sources:` frontmatter and bump `updated:`;
   - add cross-links in both directions;
   - if it changes a standing claim, follow §4 — move the old version to *How this
     changed*, do not silently overwrite it.

   A journal entry about a rough week at work might touch `areas/career`,
   `projects/q3-migration`, `people/your-manager`, `areas/sleep`, and `concepts/burnout`
   — five pages from four sentences. If you touched only the source page, you did not
   ingest it, you filed it.
5. **Revise `synthesis.md`** if the source shifts the overall picture. Say what changed.
6. **Update `open-questions.md`**: resolve what the source answered, add what it opened.
7. **Update `index.md`** with new pages and changed one-line summaries.
8. **Append to `log.md`** (§8).
9. **Run `tools/wiki-lint`** and fix what it reports.
10. **Report**: pages created, pages updated, positions changed, questions opened or closed.

---

## 6. Operation: Query

Trigger: the human asks a question.

1. **Read `index.md` first** to find candidate pages, then drill in. Use
   `tools/wiki-search "terms"` when the index is not specific enough.
2. **Answer from the wiki.** Go back to `raw/` only when a claim needs verification at the
   source or the wiki is thin on the subject — and when you do, note the gap.
3. **Cite pages** with wikilinks so the human can follow the trail.
4. **Say when the wiki cannot answer.** "Nothing in your material covers this" is a real,
   useful answer. Never fill a gap with general knowledge presented as if it came from
   their own material — if you add outside context, label it plainly as outside context.
5. **Temporal questions are the specialty.** "How has my thinking on X changed?", "what
   shows up before a bad stretch?", "what did I say I wanted a year ago?" — answer these
   from the *How this changed* sections, [[contradictions]], and dated source pages. Cite
   dates, always.
6. **File good answers back.** If the answer is genuine synthesis — a pattern, a
   comparison, a discovered connection — write it to `wiki/analysis/`, link it from the
   relevant pages, add it to `index.md`, and log it. Explorations should compound. Skip
   this for lookups; ask if unsure.

---

## 7. Operation: Lint

Trigger: the human asks for a health check, or you finish a batch of ingests.

Run `tools/wiki-lint` for the mechanical checks (broken links, orphans, index drift,
frontmatter, type/folder mismatches, un-ingested material). Open `wiki/dashboard.md` in
Obsidian for the live views. Then do the judgment pass neither can:

- **Stale pages** — an area untouched for months, a project still `active` that clearly is
  not, a person page whose `last_contact` is a year old.
- **Unflagged changes of position** — pages that still assert something a later entry
  contradicts.
- **Missing pages** — someone or something referenced repeatedly in prose with no page.
- **Missing cross-references** — pages that clearly relate but do not link.
- **Thin spots** — claims resting on a single entry, `stub` pages that matter.
- **Patterns worth naming** — recurring themes across entries that deserve a
  `concepts/` page of their own. This is where the real value surfaces.

Report findings and proposed fixes. Apply the mechanical ones; check before large
restructures.

---

## 8. index.md and log.md

**`index.md` is content-oriented** — a catalog, organized by category. Every page appears
exactly once with a wikilink, a one-line summary, and its status. Update it on every
ingest and every filed analysis. A page that is not in the index is invisible.

**`log.md` is chronological** — append-only, newest at the bottom. Never rewrite history.
Every entry starts with a machine-greppable header:

```markdown
## [2026-07-30] ingest | Journal — 2026-07-14

**Source:** `raw/journal/2026-07-14.md` → [[2026-07-14]]
**Created:** [[q3-migration]], [[burnout]]
**Updated:** [[career]], [[sleep]], [[index]], [[synthesis]]
**Changed:** [[career]] — management track no longer a stated goal (was, as of 2026-01)
**Notes:** Third entry in six weeks mentioning short sleep before a stalled work week.
Not yet a named pattern; watch for a fourth.
```

Types: `ingest`, `query`, `lint`, `schema`, `refactor`. `grep "^## \[" wiki/log.md | tail -5`
gives recent activity — check it at session start to see where things left off.

---

## 9. Tools and Obsidian

```bash
tools/wiki-search "sleep debt"              # ranked search over wiki/ (BM25)
tools/wiki-search "sleep" --raw             # search raw material instead
tools/wiki-search "sleep" -n 20 --context   # more hits, with matching lines
tools/wiki-search "burnout" --type area     # restrict to a page type
tools/wiki-lint                             # structural health check
tools/wiki-lint --json                      # machine-readable output
```

Both are dependency-free Python 3. Vault settings live in `.obsidian/` and the setup guide
is `docs/obsidian-setup.md`; `wiki/dashboard.md` needs the Dataview plugin. If you change
a convention here that the dashboard queries depend on, update the queries in the same
pass.

---

## 10. Working rules

- **Never modify `raw/`.** It is the record of what actually happened.
- **Treat everything as private.** §1. Nothing leaves this repo without confirmation.
- **Never invent facts, citations, or page references.** Every substantive claim traces to
  a source page or is explicitly marked as your own inference.
- **Prefer editing over appending.** Integrate new information into the existing prose.
  A page that grows by accretion becomes unreadable.
- **Bump `updated:` whenever you change a page.** Today's date, from the environment.
- **Keep the log honest.** Log what you actually did, including partial work.
- **Be direct.** This wiki is only useful if it says true things plainly. Do not soften a
  pattern because it is unflattering, and do not sharpen one for effect.
- **Propose schema changes** when a convention is not working. This file should improve as
  the wiki grows.
