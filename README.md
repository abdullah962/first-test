# Second Brain

An LLM-maintained knowledge base that drops into an Obsidian vault you already use.

Most LLM-plus-notes setups are RAG: you upload files, the model retrieves chunks at query
time, and rediscovers the same connections on every question. Nothing accumulates. This
does the opposite. When you clip an article or write an entry, the agent reads it and
**integrates** it — updating the pages for the people, areas, and projects it touches,
revising the current picture, recording where your position moved. Knowledge is compiled
once and kept current.

The cross-references are already there. The changes of mind have already been logged. The
synthesis already reflects everything you have read.

## Installing

Copy three things into your vault root:

```
CLAUDE.md     the schema — how the wiki is structured and how the agent maintains it
wiki/         the agent's output. Markdown, interlinked. You read, it writes
tools/        wiki-search (BM25), wiki-lint (structural health check), wiki.config.json
```

Then edit `tools/wiki.config.json` so `raw_dirs` names the folders you actually capture
into:

```json
{ "wiki_dir": "wiki", "raw_dirs": ["Clippings", "Daily Notes"] }
```

That is the whole install. Full guide, including the Obsidian settings worth checking:
[docs/obsidian-setup.md](docs/obsidian-setup.md).

**No `.obsidian/` folder ships with this package** — yours already exists, and overwriting
it would destroy your settings and plugin config.

## The ownership rule

The agent writes inside `wiki/` and nowhere else. Your notes, your clippings, your daily
entries, your folder conventions — all read-only to it. It reads widely and writes
narrowly. A wiki page that needs a rename gets renamed; one of your notes never does.

## Layout inside `wiki/`

Seven meta pages that stay for the life of the vault — [index](wiki/index.md) (catalog),
[dashboard](wiki/dashboard.md) (live Dataview views), [synthesis](wiki/synthesis.md),
[open questions](wiki/open-questions.md), [contradictions](wiki/contradictions.md),
[overview](wiki/overview.md), [log](wiki/log.md) — plus category folders:

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

**Ingest.** Capture the way you already do, then ask the agent to process what you
captured. It reads, talks through the takeaways, writes a source page, and propagates the
new information everywhere it belongs. Four sentences about a rough week at work can touch
five pages. The capture itself never moves.

**Query.** Ask questions. The agent reads the index, drills into the relevant pages, and
answers with citations. The questions worth asking are the temporal ones — *how has my
thinking on this changed?*, *what shows up before a bad stretch?* — because those are the
ones you cannot answer from memory. Good answers get filed to `wiki/analysis/`.

**Lint.** Ask for a health check periodically.

```bash
tools/wiki-search "sleep debt"   # ranked search over the wiki
tools/wiki-search "sleep" --raw  # search your capture folders instead
tools/wiki-lint                  # structural health check
```

`wiki-lint` catches broken links, orphans, index drift, type/folder mismatches, captures
you have not ingested, and name collisions between wiki pages and your existing notes. The
agent does the judgment pass — stale pages, unflagged changes of position, patterns worth
naming. Both scripts are dependency-free Python 3.

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
