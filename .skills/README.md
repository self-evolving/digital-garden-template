# Garden Skills

This repository includes skills for Sepo skill runs and digital-garden
maintenance:

- `garden`: the core content skill — capture ideas as atomic linked notes,
  record dated diary entries, and run tending passes (orphan linking,
  maturity promotion, topic-map upkeep).
- `deep-research`: broad investigations that combine academic search, web
  browsing, repositories, docs/releases, datasets, benchmarks, and optional
  social discovery signals — useful when an idea needs grounding before it
  can grow.

The research skill installs the pinned research tooling version:

```bash
.skills/deep-research/setup.sh
```

The setup script installs `agent-papers-cli==0.2.1` and smoke-checks `paper`,
`paper-search`, and `paper-search env`. Installation does not require API
keys. `garden` is documentation-only and does not require a setup script.

## Garden Workflows

`Agent / Add Idea` (`.github/workflows/agent-add-idea.yml`) accepts an idea
in natural language plus optional context. It opens a PR that plants a
seedling note under `content/ideas/`, links it into the garden, and updates
navigation metadata.

`Agent / Record Diary` (`.github/workflows/agent-record-diary.yml`) accepts
an entry (and an optional date) and opens a PR that creates or appends the
matching `content/diary/YYYY-MM-DD.md` note.

Both are manual-dispatch workflows; the same actions are available as
prefilled issue forms (**Add an idea**, **Record a diary entry**, **Tend the
garden**) that dispatch through the normal `/implement` route.
