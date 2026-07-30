# Second Brain

A personal knowledge base that an LLM builds and maintains for you, read as an Obsidian
vault.

> [!warning] This repository is currently **public**.
> A second brain holds journal entries, health, money, and notes about named people who
> did not agree to be written about. Make it private before putting anything real in
> `raw/` — GitHub → Settings → General → Danger Zone → Change visibility. Anything already
> pushed to a public repo should be treated as public permanently, even after the switch.

Most LLM-plus-notes setups are RAG: you upload files, the model retrieves chunks at query
time, and rediscovers the same connections on every question. Nothing accumulates. This
repo does the opposite. When you write an entry, the agent reads it and **integrates** it —
updating the pages for the people, areas, and projects it touches, revising the current
picture, recording where your position moved. Knowledge is compiled once and kept current.

The cross-references are already there. The changes of mind have already been logged. The
synthesis already reflects everything you have written.

## Layout

```
CLAUDE.md     the schema — how the wiki is structured and how the agent maintains it
raw/          your material. Immutable: the agent reads, never writes
  journal/      daily notes, YYYY-MM-DD.md — Obsidian writes here directly
  assets/       images pulled down by the Web Clipper
wiki/         the agent's output. Markdown, interlinked. You read, it writes
docs/         Obsidian setup guide
tools/        wiki-search (BM25) and wiki-lint (structural health check)
```

`wiki/` holds seven meta pages that stay for the life of the vault —
[index](wiki/index.md) (catalog), [dashboard](wiki/dashboard.md) (live Dataview views),
[synthesis](wiki/synthesis.md), [open questions](wiki/open-questions.md),
[contradictions](wiki/contradictions.md), [overview](wiki/overview.md), and
[log](wiki/log.md) — plus category folders:

| Folder | Holds |
|---|---|
| `sources/` | one page per ingested item — entry, article, episode, conversation |
| `people/` | people in your life |
| `areas/` | standing domains with no finish line: health, career, money |
| `projects/` | efforts with a defined outcome and an end |
| `concepts/` | ideas, mental models, frameworks |
| `entities/` | other concrete things: organizations, places, tools, books |
| `analysis/` | filed answers: patterns, comparisons, deep dives |

## Using it

**Ingest.** Write your daily note or clip an article, then ask the agent to process it. It
reads, talks through the takeaways with you, writes a source page, and propagates the new
information everywhere it belongs. Four sentences about a rough week at work can touch
five pages.

**Query.** Ask questions. The agent reads the index, drills into the relevant pages, and
answers with citations. The questions worth asking here are the temporal ones — *how has
my thinking on this changed?*, *what shows up before a bad stretch?* — because those are
the ones you cannot answer from memory. Good answers get filed to `wiki/analysis/`.

**Lint.** Ask for a health check periodically. `tools/wiki-lint` catches broken links,
orphans, index drift, type/folder mismatches, and un-ingested material; the agent does the
judgment pass — stale pages, unflagged changes of position, patterns worth naming.

```bash
tools/wiki-search "sleep debt"   # ranked search over the wiki
tools/wiki-lint                  # structural health check
```

Both are dependency-free Python 3.

## Setup

Open the **repository root** as an Obsidian vault (not `wiki/` — `raw/` has to be inside
it). Settings come committed in `.obsidian/`: attachment folder, shortest-path wikilinks,
template folder, and daily notes writing straight into `raw/journal/`. Install the
Dataview plugin for the dashboard. Full guide: [docs/obsidian-setup.md](docs/obsidian-setup.md).

## Two design choices worth knowing

**Facts about a person expire**, so pages describe the present and are rewritten rather
than appended to. What changed moves to a dated *How this changed* section. Watching your
own positions move over years is the thing this gives you that a pile of notes cannot.

**One entry is a datapoint, not a pattern.** An entry written at 2am after a bad day is
evidence about that night. The agent will not promote it into a standing claim about you —
a pattern needs three or more dated instances, cited. It surfaces patterns; it does not
diagnose.

## Division of labor

You live the life, capture the material, and ask the questions. The agent does everything
else — the summarizing, cross-referencing, filing, and bookkeeping.

Humans abandon their second brains because maintenance grows faster than value. That is
the only reason this pattern is new: the bookkeeping was always the bottleneck, and it no
longer is.
