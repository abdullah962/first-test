# Installing into an existing vault

This package is designed to drop into an Obsidian vault you already use. It does not
replace your vault, reorganize your notes, or ask you to change how you capture. It adds
one folder the agent owns and two scripts, and leaves everything else alone.

## What to copy

Copy these three into your vault root:

```
CLAUDE.md     the schema — the agent reads this first, every session
wiki/         the agent's output. Starts nearly empty and fills in as you ingest
tools/        wiki-search, wiki-lint, wiki.config.json
```

Nothing else. In particular this package deliberately ships **no `.obsidian/` folder** —
yours already exists, and overwriting it would blow away your settings, hotkeys, and
plugin config.

If `CLAUDE.md` at the vault root would collide with something, or you would rather keep
the vault root clean, put all three inside a subfolder and tell the agent where they are.

## Point the tools at your folders

`tools/wiki.config.json` is the only place paths are written down:

```json
{
  "wiki_dir": "wiki",
  "raw_dirs": ["Clippings", "Daily Notes"]
}
```

`raw_dirs` is the list of folders you capture into — where the Web Clipper saves, where
daily notes land, wherever you keep book notes. The agent reads from these and ingests
what it finds; `tools/wiki-lint` uses the same list to tell you what has been captured but
not yet ingested. Add a folder here whenever you start capturing into a new one.

Check your actual folder names first — Obsidian's clipper and daily-note settings vary:
Settings → Files and links → "Default location for new attachments", and Settings → Daily
notes → "New file location".

## Settings worth checking

These are settings you apply in your own vault; the agent cannot change them.

| Setting | Where | Why it matters |
|---|---|---|
| New link format: **Shortest path** | Files and links | The `[[slug]]` convention depends on this. With "Absolute path" the agent's links still resolve but read badly |
| Use `[[Wikilinks]]`: **on** | Files and links | Markdown links break the convention |
| Automatically update links: **on** | Files and links | Renaming a page fixes inbound links instead of breaking them |
| **Dataview** plugin | Community plugins | `wiki/dashboard.md` is entirely Dataview queries. Without it the dashboard renders as inert code blocks; nothing else is affected |
| Templates folder → `wiki/templates` | Core plugins → Templates | Optional. Lets you insert page skeletons by hand |

## The name collision to watch for

Obsidian resolves `[[shortest-path]]` links by filename, so a wiki page sharing a name with
one of your existing notes makes links to it ambiguous — Obsidian picks one silently.
`tools/wiki-lint` reports these. The rule in `CLAUDE.md` is that the agent renames **its**
page, never yours.

Run `tools/wiki-lint` right after copying the folder in. On a vault with a few hundred
notes, expect one or two collisions on common words.

## Working rhythm

1. **Capture** during the day, the way you already do — clipper, daily note, book notes.
   Nothing changes here.
2. **Ingest.** Point the agent at what you captured. It reads, discusses the takeaways,
   then writes the source page and propagates it across every page it touches.
3. **Review in Obsidian.** Open the updated pages, follow the new links, check the graph.
   Reviewing by browsing is much easier than reviewing a diff, and this is the step where
   you catch the agent over-claiming.
4. **Ask questions.** Good answers are filed to `wiki/analysis/` and become part of the
   vault.

## Views worth setting up

- **Graph view**, filtered to `path:wiki` — the shape of what has been built. Hubs and
  orphans are obvious at a glance.
- **Local graph**, depth 2, on a person or area page — everything connected to one thing.
- **Backlinks pane**, pinned. On a person's page it becomes the list of every entry that
  mentioned them, in date order.

## Version control

The wiki is markdown, so `git init` in your vault gives you history — and `git log` on a
page is a real record of how a view of yourself changed over time, which is the point of
the whole exercise. If you already sync the vault with Obsidian Sync or a cloud folder,
let one system own the files; two syncers over one directory produce conflict files.
