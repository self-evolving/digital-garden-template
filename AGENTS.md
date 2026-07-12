# Agent Instructions

This repository is a digital-garden template with shipped example content.

When implementing a user-requested change that adapts the template for a real
garden, remove the existing example notes and replace them with content that
follows the user's requirements. Do not keep the shipped placeholder notes
(the `content/ideas/` examples, `content/topics/gardening-practice.md`, and
the `content/diary/` example entry) unless the user explicitly asks to
preserve them.

Keep the related garden files in sync with the replacement, including the
`_meta.json` navigation manifests and topic maps when those files are
affected.

## Working Locally

- The local site (`npm run dev`) renders **this repository's** `content/` —
  idea notes under `content/ideas/`, topic maps under `content/topics/`, and
  dated entries under `content/diary/`. A checkout of another project
  outside this repo will never appear on the site.
- Links resolve Obsidian-style by shortest path, so vault-style wikilinks
  and relative links both work.
- Folder listing order is controlled by `_meta.json` manifests; list new
  pages there when adding content (diary newest first).

## Pull Requests

For user-submitted pull requests, add the `agent/review` label or comment
`@sepo-agent /review` on the PR to launch a Sepo review. Add the `sepo-preview`
label to request a preview deployment for a non-agent PR.

## Garden Work

For planting, tending, and diary work, use the repository `garden` skill
under `.skills/garden` — the routing lives here, not in the request:

- **Capture an idea**: read `.skills/garden/SKILL.md` and follow its capture
  workflow — one idea per note, `status: seedling`, planted date, linked
  into the garden with no orphans.
- **Record a diary entry**: same skill, diary conventions — one
  `YYYY-MM-DD.md` note per day, append within a day, replant durable
  thoughts as seedling ideas.
- **Tend the garden**: same skill, tending checklist — link orphans, review
  maturity statuses (`seedling` → `budding` → `evergreen`), refresh topic
  maps, note pruning in the diary.
- **Ground an idea in sources** (papers, repos, posts): use
  `.skills/deep-research/SKILL.md`.

Single captures can also run through the `Agent / Add Idea` and
`Agent / Record Diary` workflows (manual dispatch with natural-language
input).

Mutating work — planting notes, editing content, changing manifests — should
go through the normal Sepo `/implement` workflow so changes are verified and
proposed by PR. To discuss or scope work first, use `/answer` (or a plain
`@sepo-agent` mention) and switch to `/implement` when ready to dispatch.
