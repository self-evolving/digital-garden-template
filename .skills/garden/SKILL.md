---
name: garden
description: Tend this digital garden — capture ideas as atomic linked notes, record dated diary entries, and run tending passes that link orphans, promote note maturity, and refresh topic maps. Use for any request that plants, updates, links, promotes, prunes, or reorganizes garden content.
argument-hint: "[capture|diary|tend] [text, or empty for a full tending pass]"
allowed-tools: Bash, Read, Glob, Grep, Write, Edit
---

# Garden

Work on this repository's published digital garden: an Obsidian-style vault
under `content/`, rendered by Quartz and tended through Sepo PRs.

## Content model

| Kind  | Lives in          | Frontmatter                                                        |
| ----- | ----------------- | ------------------------------------------------------------------ |
| idea  | `content/ideas/`  | `title`, `type: idea`, `status`, `planted`, `tended` (opt), `tags` |
| diary | `content/diary/`  | `title` (= date), `type: diary`, `date`, `tags`                    |
| topic | `content/topics/` | `title`, `type: topic`, `tags`                                     |

- `status` is one of `seedling` (fresh, unvetted), `budding` (tended, taking
  shape), `evergreen` (distilled, stable). New captures are always
  `seedling` — polish is a tending action, not a capture action.
- Diary notes are named `YYYY-MM-DD.md`, one per day; append new sections to
  an existing day rather than creating duplicates.
- Every folder has a `_meta.json` manifest (`label`, `pages`); list new
  pages there so they appear in navigation, newest first for the diary.
- Links are Obsidian-style and resolve by shortest path
  (`markdownLinkResolution: shortest`); relative paths also work.

## Capture (add an idea)

1. One idea per note. If the request contains several ideas, split them.
2. Slug: short, kebab-case, claim-shaped when possible
   (`tend-small-and-often`, not `note-2026-07-12-a`).
3. Frontmatter: `type: idea`, `status: seedling`, `planted: <today>`, and
   1–3 topical tags. Do not invent an `evergreen` status at capture.
4. Keep the author's voice and hedges; a seedling may be wrong. Record open
   questions instead of resolving them.
5. Link it in — at least one link **from** the new note to a related idea or
   topic, and one link **to** it from the most related topic map (or the
   diary entry that spawned it). No orphans.
6. Add the slug to `content/ideas/_meta.json`.

## Diary (record an entry)

1. Target `content/diary/<YYYY-MM-DD>.md` for the given or current date;
   create it with `type: diary` frontmatter or append a new `##` section to
   the existing day.
2. Keep entries chronological and concrete: what happened, what was decided,
   what was noticed. Do not editorialize into conclusions — replant durable
   thoughts as idea seedlings and link them from the entry.
3. Newest first in `content/diary/_meta.json`.

## Tend (maintenance pass)

Work through, smallest diff that leaves the garden healthier:

1. **Orphans** — find idea notes with no inbound links (`grep` for the slug
   across `content/`); link each from the best-fitting topic map.
2. **Promotions** — a `seedling` that has been revisited, linked, and still
   holds becomes `budding`; a `budding` note that is distilled and stable
   becomes `evergreen`. Update `tended:` when touching a note. Demote or
   prune notes that stopped being true (note the pruning in today's diary).
3. **Topic maps** — every topic lists its ideas with one-line context and
   names what is missing; add new maps only when several unmapped ideas
   cluster.
4. Do not rewrite note content during a tending pass beyond what the
   status change requires — tending is gardening, not ghostwriting.

## Validate

Run `npm run check` and `npm run build` before proposing changes; fix broken
links and manifest mismatches they surface. Leave changes uncommitted for
the Sepo workflow to verify and propose as a PR.
