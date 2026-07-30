# Raw sources

Your curated collection of source documents. **This directory is immutable to the LLM** —
it reads from here and never writes, edits, renames, or deletes. It is the source of
truth; everything in `wiki/` is derived from it and can be regenerated. Nothing here can.

## Adding a source

1. Drop the file in this directory. Markdown is easiest to work with — the
   [Obsidian Web Clipper](https://obsidian.md/clipper) converts web articles to markdown in
   one click. PDFs, transcripts, and data files are fine too.
2. Name it after its content, kebab-case: `as-we-may-think.md`, not `article-3.md`.
   The filename becomes the wiki slug.
3. Tell the agent to ingest it. It will read the source, discuss the takeaways with you,
   write a summary page, propagate the new information across the wiki, and log the ingest.

Keeping the original metadata in the file — title, author, URL, date — makes the source
page more accurate. The Web Clipper does this automatically.

## Images

`assets/` holds images referenced by sources. In Obsidian, set Settings → Files and links →
"Attachment folder path" to `raw/assets/`, then bind "Download attachments for current
file" to a hotkey — after clipping an article, one keypress pulls its images down to disk.
Local copies let the agent actually look at them, and they do not rot when the URL dies.

The agent reads a source's text first and views the images afterward; it cannot do both in
one pass.

## Anything goes here

Articles, papers, book chapters, meeting transcripts, podcast notes, your own journal
entries, CSVs. The only requirement is that you consider it worth keeping — curation is
your job, and it is the one thing the agent cannot do for you.
