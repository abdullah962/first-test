# Persistent Wiki

A knowledge base that an LLM builds and maintains for you.

Most LLM-plus-documents setups are RAG: you upload files, the model retrieves chunks at
query time, and rediscovers the same connections on every question. Nothing accumulates.
This repo does the opposite. When a source arrives, the agent reads it and **integrates**
it — updating entity pages, revising the synthesis, flagging where new data contradicts
old claims, maintaining cross-references. Knowledge is compiled once and kept current.

The cross-references are already there. The contradictions have already been flagged. The
synthesis already reflects everything read so far.

## Layout

```
CLAUDE.md     the schema — how the wiki is structured and how the agent maintains it
raw/          your source documents. Immutable: the agent reads, never writes
wiki/         the agent's output. Markdown pages, interlinked. You read, it writes
tools/        wiki-search (BM25 over the wiki) and wiki-lint (structural health check)
```

`wiki/` holds six meta pages that stay for the life of the wiki — [index](wiki/index.md)
(catalog), [log](wiki/log.md) (chronology), [overview](wiki/overview.md),
[synthesis](wiki/synthesis.md), [open questions](wiki/open-questions.md), and
[contradictions](wiki/contradictions.md) — plus category folders for sources, entities,
concepts, and filed analyses.

## Using it

**Ingest.** Drop a file in `raw/` and ask the agent to process it. It reads the source,
talks through the takeaways with you, writes a summary page, propagates the new
information across every page it touches, and appends to the log. A substantive source
normally touches 5–15 pages.

**Query.** Ask questions. The agent reads the index, drills into the relevant pages, and
answers with citations. Good answers get filed back into `wiki/analysis/` so your
explorations compound the same way ingested sources do.

**Lint.** Ask for a health check periodically. `tools/wiki-lint` catches broken links,
orphan pages, index drift, and un-ingested sources; the agent does the judgment pass —
contradictions, stale claims, missing pages, gaps worth a new source.

```bash
tools/wiki-search "associative trails"   # ranked search over the wiki
tools/wiki-lint                          # structural health check
```

Both are dependency-free Python 3.

## Reading the wiki

It is plain markdown with Obsidian-style `[[wikilinks]]`, so it works in any editor. Point
Obsidian at this directory and you get backlinks, graph view, and live preview — the
intended way to browse: the agent edits on one side, you follow links and watch the graph
fill in on the other. It is also just a git repo, so you get version history and diffs of
how the synthesis changed over time.

## Making it yours

`CLAUDE.md` is the configuration that turns a generic chatbot into a disciplined wiki
maintainer. It ships domain-neutral. Once you know what you are building — a research
wiki, a companion wiki for a book, a personal knowledge base, a competitive-analysis
file — adjust it: rename the category folders (`characters/`, `companies/`, `chapters/`),
add fields to the frontmatter, change what an ingest should touch. It is meant to be
co-evolved with the agent as you learn what works for your domain.

## Division of labor

You curate sources, direct the analysis, and ask good questions. The agent does everything
else — the summarizing, cross-referencing, filing, and bookkeeping.

Humans abandon wikis because maintenance grows faster than value. That is the only reason
this pattern is new: the bookkeeping was always the bottleneck, and it no longer is.
