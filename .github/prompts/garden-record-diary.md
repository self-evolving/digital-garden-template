## Task Description

Record one diary entry in this Quartz digital garden.

The entry is provided in `${REQUEST_TEXT}`, in natural language, optionally
followed by an explicit entry date (`YYYY-MM-DD`); when no date is given,
use today's date.

Read `.skills/garden/SKILL.md` first and follow its content model and diary
conventions. In short:

- Diary entries live in `content/diary/`, one note per day, named
  `YYYY-MM-DD.md`, with frontmatter `title` (the date), `type: diary`,
  `date`, and `tags: [diary]`.
- Idea notes live in `content/ideas/`; topic maps live in `content/topics/`.
- Folder navigation is controlled by `_meta.json` manifests.

Instructions:

1. Resolve the entry date. If `content/diary/<date>.md` already exists,
   append a new `##` section to it; otherwise create the note with the
   standard frontmatter.
2. Keep the entry chronological and concrete — what happened, what was
   decided, what was noticed. Preserve the author's voice; do not
   editorialize the entry into conclusions.
3. If the entry contains a durable thought worth keeping, plant it as a
   separate `status: seedling` idea note under `content/ideas/` (following
   the capture workflow in the skill) and link it from the diary entry. Only
   do this when the thought clearly stands alone; when in doubt, leave it in
   the entry.
4. Link mentioned garden notes where natural.
5. Keep `content/diary/_meta.json` listing entries newest first, including
   the new entry.
6. Run focused validation when practical, such as `npm run check` and
   `npm run build`, or at least check the modified Markdown and JSON for
   obvious syntax errors.
7. Do not commit. Leave changes in the working tree.

Return exactly one JSON object and nothing else:

```json
{
  "summary": "One short paragraph describing the entry and any planted ideas.",
  "commit_message": "Concise commit message under 72 characters.",
  "pr_title": "Concise pull request title under 72 characters.",
  "pr_body": "GitHub-flavored markdown pull request body."
}
```

Rules:

- `summary` should mention the entry date and any idea notes planted.
- `commit_message` should describe the diary update.
- `pr_title` should identify the entry date.
- `pr_body` should summarize the entry briefly and note any validation run.
- If you cannot determine better PR metadata from the work performed, return
  empty strings for `commit_message`, `pr_title`, or `pr_body` so the
  workflow can fall back to defaults.
