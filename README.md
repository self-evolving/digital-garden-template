# Digital Garden Template

A live digital garden: an Obsidian-style notes vault published as a Quartz
site and tended through Sepo — ideas arrive as pull requests, previews come
per branch, and an agent helps you plant, link, and maintain.

## The garden model

- **Ideas** (`content/ideas/`) — atomic notes, one idea per note, each with
  a maturity `status`: `seedling` (fresh, may be wrong) → `budding` (tended,
  taking shape) → `evergreen` (distilled, stable). Frontmatter records
  `planted` and `tended` dates.
- **Topics** (`content/topics/`) — maps of content that gather related
  ideas into curated trails and name what's missing.
- **Diary** (`content/diary/`) — one dated note per day (`YYYY-MM-DD.md`),
  append-mostly; durable thoughts get replanted as idea seedlings.

Notes link Obsidian-style (shortest-path resolution), folders are ordered by
`_meta.json` manifests, and the graph, backlinks, and popovers come from
Quartz.

## Sepo routes

Open a prefilled issue (or mention `@sepo-agent` anywhere):

| Route                    | What happens                                                |
| ------------------------ | ----------------------------------------------------------- |
| **Add an idea**          | Plants a seedling note, linked into the garden, as a PR     |
| **Record a diary entry** | Creates or appends today's `content/diary/` note            |
| **Tend the garden**      | Links orphans, promotes matured notes, refreshes topic maps |
| **Ask Sepo**             | Discuss anything without changing content                   |

`Agent / Add Idea` and `Agent / Record Diary` are also available as manual
workflow dispatches with natural-language inputs. Conventions live in
`.skills/garden/SKILL.md`; agent behavior in [AGENTS.md](AGENTS.md).

## Local development

Requires Node 22.x.

```bash
npm ci
npm run install-plugins
npm run dev            # local preview
npm run check          # typecheck + formatting
npm run check:site     # check + plugins + build
```

## Deployment, previews, and comments

The template ships the shared Sepo site infrastructure: canonical
main-branch deploys (`.github/workflows/agent-deploy-site-main.yml`),
per-branch PR previews (`agent-site-preview.yml`, controlled by
`AGENT_PREVIEW_POLICY` and the `sepo-preview` label), and the Sepo comments
drawer (configured through `SEPO_COMMENTS_*` build variables). Set
`AGENT_ENABLED=false` to pause all agent workflows.
