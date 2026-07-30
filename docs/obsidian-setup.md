# Obsidian setup

This repo is an Obsidian vault. **Open the repository root as the vault** — not `wiki/`.
The vault has to include `raw/` so that daily notes, clipped articles, and attachments
land inside it, and so links from wiki pages back to raw material resolve.

Obsidian will find `.obsidian/` and pick up the settings below on first open.

## What is already configured

`.obsidian/` is committed, so these come with the repo:

| Setting | Value | Why |
|---|---|---|
| Attachment folder | `raw/assets` | Clipped images land in the raw layer, not loose in the vault |
| New link format | Shortest path | Makes `[[slug]]` work regardless of folder — the whole link convention depends on this |
| Use `[[Wikilinks]]` | on | Markdown links would break the convention |
| Automatically update links | on | Renaming a page fixes inbound links |
| Template folder | `wiki/templates` | Core Templates plugin inserts the right skeleton |
| Daily note folder | `raw/journal`, `YYYY-MM-DD` | Today's note is written straight into the raw layer, ready to ingest |

Per-machine state (`workspace.json`, caches, plugin data) is gitignored, so opening the
vault on a second device does not fight with the first.

## What you need to do once

**1. Install Dataview.** Settings → Community plugins → Browse → "Dataview" → Install →
Enable. [[dashboard]] is nothing but Dataview queries; without the plugin it renders as
inert code blocks. Everything else in the wiki works fine without it.

**2. Enable the core plugins** if they are off: Templates, Backlinks, Outgoing links,
Graph view, Daily notes. Settings → Core plugins.

**3. Bind the attachment hotkey.** Settings → Hotkeys → search "Download" → bind
"Download attachments for current file" to something like `Ctrl+Shift+D`. After clipping
an article, one keypress pulls its images to `raw/assets/` so they survive the source URL
dying — and so the agent can actually look at them.

**4. Install the Web Clipper** browser extension (obsidian.md/clipper) and point it at
`raw/`. It is the fastest path from "read something worth keeping" to "in the vault".

## Working rhythm

The setup is meant to be used with the agent on one side and Obsidian on the other:

1. Capture during the day — daily note, clipped articles, book notes. All into `raw/`.
2. Tell the agent to ingest. It reads, discusses the takeaways with you, then writes.
3. Watch it land in Obsidian. Open the updated pages, follow the new links, check the
   graph view. This is the review step — you are checking the agent's work, and it is much
   easier to do by browsing than by reading a diff.
4. Ask questions. Good answers get filed to `wiki/analysis/` and become part of the vault.

## Views worth setting up

- **Graph view**, filtered to `path:wiki` — the shape of what you have built. Hub pages and
  orphans are obvious at a glance.
- **Local graph** on a person or area page, depth 2 — everything connected to one thing.
- **Backlinks pane**, pinned. On a person's page it is the list of every entry that
  mentioned them, in order.

## A note on sync

The vault is a git repo, so version history and multi-device sync are free — and `git log`
on a page is a real record of how a view of yourself changed over time, which is the
point. If you also use Obsidian Sync or a cloud folder, let one of them own the files, not
both; two syncers over the same directory produce conflict files.
