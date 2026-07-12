## Task Description

Plant one idea note in this Quartz digital garden.

The idea is provided in `${REQUEST_TEXT}`, in natural language, optionally
followed by additional context such as where the idea came from, related
notes or topics, or links worth keeping.

Read `.skills/garden/SKILL.md` first and follow its content model and
capture workflow. In short:

- Idea notes live in `content/ideas/`, one idea per note, with frontmatter
  `title`, `type: idea`, `status: seedling`, `planted: <today>`, and 1–3
  topical `tags`.
- Diary entries live in `content/diary/`; topic maps live in
  `content/topics/`.
- Folder navigation is controlled by `_meta.json` manifests.

Instructions:

1. Distill the request into a single idea. If it genuinely contains several
   independent ideas, plant separate notes.
2. Choose a short, kebab-case, claim-shaped slug for
   `content/ideas/<slug>.md`.
3. New captures are always `status: seedling` — keep the author's voice and
   hedges, preserve open questions, and do not polish the idea into a
   conclusion it hasn't earned.
4. Link the note into the garden: at least one link from the new note to a
   related idea or topic, and one link to it from the most related topic map
   in `content/topics/` (or from the diary entry that spawned it). Do not
   leave the note orphaned. Do not invent a new topic map unless the request
   asks for one.
5. Add the slug to `content/ideas/_meta.json`.
6. Follow any additional context from the workflow input, such as linking a
   named note or recording a source link.
7. Run focused validation when practical, such as `npm run check` and
   `npm run build`, or at least check the modified Markdown and JSON for
   obvious syntax errors.
8. Do not commit. Leave changes in the working tree.

Return exactly one JSON object and nothing else:

```json
{
  "summary": "One short paragraph describing the planted note and its links.",
  "commit_message": "Concise commit message under 72 characters.",
  "pr_title": "Concise pull request title under 72 characters.",
  "pr_body": "GitHub-flavored markdown pull request body."
}
```

Rules:

- `summary` should mention the note slug and which notes now link to it.
- `commit_message` should describe the planted idea.
- `pr_title` should identify the idea.
- `pr_body` should quote the original request briefly and summarize any
  validation run.
- If you cannot determine better PR metadata from the work performed, return
  empty strings for `commit_message`, `pr_title`, or `pr_body` so the
  workflow can fall back to defaults.
