# Wiki Schema

This repository is a **persistent wiki**: a knowledge base that you (the LLM) build and
maintain from a curated collection of raw sources. Knowledge is compiled once, on ingest,
and then kept current — not re-derived from scratch on every question.

You own the `wiki/` directory entirely. The human owns `raw/`. Read this file at the start
of every session before touching anything.

---

## 1. Layers

| Layer | Path | Who writes it | Rules |
|---|---|---|---|
| Raw sources | `raw/` | Human | **Immutable.** Read only. Never edit, rename, move, or delete. |
| Wiki | `wiki/` | You | You create, update, cross-reference, and reorganize freely. |
| Schema | `CLAUDE.md` | Both | Co-evolved. Propose changes; the human approves. |

The human curates sources, directs analysis, and asks questions. You do the summarizing,
cross-referencing, filing, and bookkeeping.

---

## 2. Wiki layout

```
wiki/
  index.md            catalog of every page (content-oriented)
  log.md              append-only chronological record
  overview.md         entry point: what this wiki is about, current state
  synthesis.md        the evolving thesis — what the sources add up to
  open-questions.md   unresolved questions, gaps, things to look for
  contradictions.md   register of conflicts between sources
  sources/            one page per ingested raw source
  entities/           people, orgs, products, places, characters — concrete things
  concepts/           ideas, mechanisms, themes, methods — abstract things
  analysis/           filed answers to queries: comparisons, deep dives, arguments
  templates/          page templates to copy when creating new pages
```

`sources/`, `entities/`, `concepts/`, `analysis/` are the defaults. Add or rename
categories when the domain calls for it (`characters/`, `companies/`, `chapters/`,
`experiments/`) — then update this file and `index.md` to match.

---

## 3. Page conventions

**Filenames** are kebab-case slugs: `wiki/entities/vannevar-bush.md`. One topic per page.
Slugs are stable — if a page must be renamed, update every inbound link in the same pass.

**Frontmatter** is required on every page:

```yaml
---
title: Vannevar Bush
type: entity          # source | entity | concept | analysis | meta
tags: [memex, information-science]
created: 2026-07-30
updated: 2026-07-30
sources: [as-we-may-think]   # slugs of wiki/sources/ pages backing this page
status: stub          # stub | developing | mature
---
```

Source pages add: `source_path`, `source_type`, `author`, `source_date`, `ingested`.

`status` means: **stub** = placeholder, one or two facts; **developing** = real content,
known gaps; **mature** = well-covered, would only change if a source contradicts it.

**Links** are Obsidian wikilinks by slug, no path and no extension:
`[[vannevar-bush]]` or `[[vannevar-bush|Bush]]`. Slugs are unique across the whole wiki,
so links resolve regardless of folder. Link generously — the cross-reference graph is the
point. Every claim traceable to a source cites it: `([[as-we-may-think]])`.

**Never link to a page you have not created.** If a concept deserves a page you are not
writing yet, create a stub with frontmatter and a one-line definition rather than leaving
a dangling link.

**Contradictions** are flagged inline where they occur:

```markdown
> [!warning] Contradiction
> [[source-a]] says X (p. 12); [[source-b]] says not-X (2024 data, more recent).
> Unresolved — see [[contradictions]].
```

and registered as a row in `wiki/contradictions.md`.

**Uncertainty is marked, not smoothed over.** Distinguish what a source claims from what
is established. Attribute contested claims to their source. Do not average conflicting
numbers into a fake consensus.

---

## 4. Operation: Ingest

Trigger: the human drops a file in `raw/` and asks you to process it.

1. **Read the source fully.** If it references local images (`raw/assets/`), read the text
   first, then view the images that matter — you cannot do both in one pass.
2. **Discuss key takeaways** with the human before writing, unless they asked for an
   unsupervised batch ingest. Surface anything surprising or contradictory early.
3. **Write `wiki/sources/<slug>.md`** from `wiki/templates/source.md`: citation metadata,
   a summary proportional to the source's density, key claims with locators (page,
   section, timestamp), notable quotes, and how it relates to what is already in the wiki.
4. **Propagate.** This is the part that makes the wiki worth having. For each entity and
   concept the source touches:
   - update the existing page with the new information, or create it if missing;
   - add the source slug to that page's `sources:` frontmatter and bump `updated:`;
   - add cross-links in both directions — the new page links out, and the pages it
     relates to gain a link back;
   - if the source contradicts or supersedes an existing claim, flag it (§3) and add a
     row to `contradictions.md`. Do not silently overwrite the old claim.
   A substantive source normally touches 5–15 pages. If you touched only the source page,
   you did not ingest it — you filed it.
5. **Revise `synthesis.md`** if the source shifts the overall picture. Say what changed.
6. **Update `open-questions.md`**: resolve questions the source answered, add new ones.
7. **Update `index.md`** with new pages and changed one-line summaries.
8. **Append to `log.md`** (§6).
9. **Run `tools/wiki-lint`** and fix what it reports.
10. **Report** to the human: pages created, pages updated, contradictions found, questions
    opened or closed.

---

## 5. Operation: Query

Trigger: the human asks a question.

1. **Read `index.md` first** to find candidate pages, then drill in. Use
   `tools/wiki-search "terms"` when the index is not specific enough.
2. **Answer from the wiki.** Go back to `raw/` only when a claim needs verification at the
   source or the wiki is thin on the subject — and when you do, note the gap.
3. **Cite pages** with wikilinks so the human can follow the trail.
4. **Say when the wiki cannot answer.** "No source in the collection covers this" is a
   real, useful answer. Never fill a gap with general knowledge presented as if it came
   from the sources — if you add outside context, label it as such.
5. **File good answers back.** If the answer is a genuine piece of synthesis — a
   comparison, an argument, a discovered connection — write it to `wiki/analysis/`, link
   it from the relevant pages, add it to `index.md`, and log it. Explorations should
   compound in the wiki, not disappear into chat history. Skip this for lookups and
   trivial questions; ask if unsure.

Output format follows the question: prose, a comparison table, a chart, a Marp deck. The
filed page is markdown regardless.

---

## 6. Operation: Lint

Trigger: the human asks for a health check, or you finish a batch of ingests.

Run `tools/wiki-lint` for the mechanical checks (broken links, orphans, index drift,
frontmatter, un-ingested sources). Then do the judgment pass the tool cannot:

- **Contradictions** between pages that no one has flagged.
- **Stale claims** a newer source has superseded.
- **Missing pages** — concepts referenced repeatedly in prose with no page of their own.
- **Missing cross-references** — pages that clearly relate but do not link.
- **Thin spots** — `stub` pages that matter, claims resting on a single source.
- **Data gaps** worth a web search or a new source, plus specific suggestions for what
  the human should read next.

Report findings and proposed fixes. Apply the mechanical ones; check before large
restructures.

---

## 7. index.md and log.md

**`index.md` is content-oriented** — a catalog, organized by category. Every page appears
exactly once with a wikilink, a one-line summary, and its status. It is how you (and the
human) navigate the wiki. Update it on every ingest and every filed analysis. A page that
is not in the index is invisible.

**`log.md` is chronological** — append-only, newest at the bottom. Never rewrite history.
Every entry starts with a machine-greppable header:

```markdown
## [2026-07-30] ingest | As We May Think

**Source:** `raw/as-we-may-think.md` → [[as-we-may-think]]
**Created:** [[vannevar-bush]], [[memex]], [[associative-trails]]
**Updated:** [[index]], [[synthesis]], [[open-questions]]
**Notes:** First source on the pre-digital lineage. Opened the question of who maintains
associative trails at scale.
```

Types: `ingest`, `query`, `lint`, `schema`, `refactor`. `grep "^## \[" wiki/log.md | tail -5`
gives recent activity — check it at session start to see where things left off.

---

## 8. Tools

```bash
tools/wiki-search "associative trails"      # ranked search over wiki/ (BM25)
tools/wiki-search "memex" --raw             # search raw sources instead
tools/wiki-search "memex" -n 20 --context   # more hits, with matching lines
tools/wiki-lint                             # structural health check
tools/wiki-lint --json                      # machine-readable output
```

Both are dependency-free Python 3. Extend them as the wiki grows; if search stops being
good enough at scale, swap in a real engine (e.g. `qmd`) and update this section.

---

## 9. Working rules

- **Never modify `raw/`.** It is the source of truth.
- **Never invent facts, citations, or page references.** Every substantive claim in the
  wiki traces to a source page or is explicitly marked as your own inference.
- **Prefer editing over appending.** Integrate new information into the existing prose;
  a page that grows by accretion becomes unreadable. Rewrite sections when they get
  tangled.
- **Bump `updated:` whenever you change a page.** Today's date, from the environment.
- **Keep the log honest.** Log what you actually did, including partial work.
- **Propose schema changes** when you notice a convention that is not working. This file
  should improve as the wiki grows.
