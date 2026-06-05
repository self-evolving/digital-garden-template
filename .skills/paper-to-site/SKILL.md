---
name: paper-to-site
description: Build a hosted Quartz/Sepo literature-template site from an arXiv, LaTeX, PDF/HTML, DOI, or review-style paper source. Use when making the paper the site entry point, splitting it into section pages, preserving citations, cleaning LaTeX cross-reference artifacts, and preparing a standalone paper-hosting repository or PR.
argument-hint: [paper URL, arXiv ID, DOI, title, PDF URL, or source archive]
allowed-tools: Bash, Read, Glob, Grep, Write, Edit
---

# Paper to Site

Use this workflow when turning an arXiv, LaTeX, or review-style paper into a hosted `literature-template` site.

The intended outcome is a readable paper site whose root page is the paper itself, with section pages, citations, figures, graph/backlinks, and reference-paper notes all following the template's content model.

## Target site shape

For a single hosted paper, prefer this structure:

```text
content/
├── index.md                         # main paper note and site entry point (/)
├── _meta.json                       # root label and sidebar order
├── papers/                          # cited-paper/reference notes, not hosted full text
│   ├── index.md
│   └── _meta.json
└── <paper-slug>/                    # hosted-paper section tree
    ├── index.md                     # section index, labeled "Sections"
    ├── _meta.json
    ├── 01-introduction/
    │   ├── index.md
    │   └── _meta.json
    ├── 02-.../
    │   ├── index.md
    │   ├── _meta.json
    │   └── 01-subsection.md
    └── figures/
```

The sidebar should read like:

```text
<Short paper/site title>
├── Sections
│   ├── Introduction
│   ├── ...
└── Papers
```

Do **not** put the hosted full text under `content/papers/`. Keep `content/papers/` for reference-paper notes that citations can link to later.

Do **not** create a separate `Full text` navigation node. The paper note at `/` is the entry point; its section tree lives directly below it.

## Import workflow

1. Start from the latest useful `literature-template` baseline with citation-link, paper-note, graph, and navigation fixes.
2. Fetch the source paper:
   - arXiv source archive when available
   - PDF/HTML only if source is unavailable
   - bibliography files, figures, and supplemental assets
3. Choose one stable citekey, usually `surnameyearshorttitle`.
4. Replace starter content:
   - remove demo notes unless the target site needs them
   - remove sample papers and sample bibliography entries
   - keep only content needed for the hosted paper and references
5. Put the paper metadata, source links, abstract, and section outline in `content/index.md`.
6. Split major sections into folders under `content/<paper-slug>/`.
7. Split subsections into child pages when this improves sidebar navigation, popovers, or backlinks.
8. Put figures under `content/<paper-slug>/figures/` with lowercase deployment-safe filenames.
9. Update every relevant `_meta.json` for deterministic sidebar order.
10. Import the paper bibliography into `bibliography.bib` so Markdown citations render.
11. Update site branding, `SITE_URL` guidance, and deployment config when creating a standalone paper repository.

## Frontmatter conventions

For the root paper page:

```yaml
---
title: "Short paper title"
description: "Hosted Markdown edition of <source paper>."
type: paper
citekey: surnameyearshorttitle
authors:
  - First Author
year: 2026
venue: arXiv
url: https://arxiv.org/abs/...
doi:
status: imported
tags:
  - paper
---
```

List the complete author set in `authors` for the root paper note; the single entry above is only for brevity. Keep `year`, `venue`, `url`, and `doi` populated from the source so the page header and citation metadata stay complete.

For section pages:

```yaml
---
title: "Section title"
type: paper-section
paper: surnameyearshorttitle
tags:
  - paper
  - paper-section
---
```

Use lightweight graph type tags deliberately:

- hosted paper pages and paper-section pages include `paper`
- paper-section pages may also include `paper-section`
- synthesis notes include `note`
- do not invent `note` tags if no synthesis-note pages exist
- when the site mixes papers and synthesis notes, enabling graph tag nodes (`showTags: true` in the graph plugin options) lets the graph group by `#paper`/`#note`; a single hosted-paper site (where every page is `#paper`) should keep tag nodes hidden and rely on the `paper` styling hint to avoid one giant hub node

## Reference and link cleanup

Clean converted LaTeX artifacts aggressively:

- Replace visible labels like `sec:data`, `subsec:...`, `subsub:...`, `fig:...`, and `[[fig:...]]` with descriptive links.
- Prefer page-level links such as `[Data for HCLLMs](../03-data/)` over anchor links created only for LaTeX labels.
- Remove leftover `<span id="sec..."></span>` / `<span id="fig..."></span>` anchors when they only supported old cross-references.
- Preserve real bibliography citations like `[@key]`; the citation plugin should render those.
- Fix relative image links after moving section files.
- Figure placement does not need to match LaTeX float placement exactly; source-order placement is usually good enough for a Markdown site.

## Figures and captions

Render each figure with its caption visible: LaTeX captions carry real content, often including cross-references.

- A semantic `<figure>` with `<figcaption>` renders cleanly and keeps the caption visually distinct from body text:

  ```html
  <figure>
    <img src="../figures/02_hci.png" alt="HCI roadmap" />
    <figcaption>
      We can draw on the field of HCI ... <a href="01-design">Understanding the Who</a>.
    </figcaption>
  </figure>
  ```

- A Markdown image plus a bold-led caption paragraph also works and stays pure Markdown:

  ```markdown
  ![](../figures/02_hci.png)

  **Figure.** We can draw on the field of HCI ...
  ```

- Do not wrap an entire caption in `*...*` or `_..._`: captions frequently contain `\emph{...}` that becomes nested emphasis and breaks rendering. Keep caption emphasis inline instead.
- Rewrite `\ref{fig:...}` and `\ref{subsec:...}` inside captions into the same descriptive links used in body text.
- Use lowercase, deployment-safe filenames under `content/<paper-slug>/figures/`, with `../figures/<name>` relative links from section and subsection pages.

## Backlink hygiene

Quartz backlinks should reflect meaningful textual relationships, not navigation scaffolding.

- Do not add boilerplate parent/previous/next blockquote navigation bars to Markdown pages.
- Keep genuine in-text links between related sections, concepts, and reference notes.
- Use the sidebar and page hierarchy for structural navigation.
- If previous/next navigation is desired, implement it in layout/component UI rather than Markdown body links.

## Title policy

Use frontmatter `title:` as the single source of truth.

- Do not duplicate the page title as a leading Markdown `# ...` heading.
- Let the page header render the title from frontmatter.
- Demote non-page-title top-level headings to `##` or lower.

## Section index pages

Each `content/<paper-slug>/<NN-section>/index.md` is a chapter landing. Give it:

- the chapter's intro prose and overview figure, converted from the section's lead `.tex`, and
- a short `## Subsections` list linking the chapter's child pages in reading order.

The `## Subsections` list is page content — a chapter table of contents — not a per-page parent/next/previous navigation bar, so it is consistent with the backlink-hygiene rules above. It gives the landing real text and backlinks even when the sidebar is collapsed.

## Root page and layout

The root page `/` should look like a normal content/library page, not a special landing page.

Check that `/` has the same:

- left navigation/sidebar
- content card width
- top spacing/padding behavior
- page header/title metadata
- citekey and source-link display for papers
- graph/backlinks behavior when enabled

If the Quartz config or styles exclude `index` from these components, adjust the gating so the root paper note renders with the same chrome as section pages.

In this template the gating lives in two conditions, both of which drop the root via `slug === "index"`:

- `library-page` (a custom condition in `quartz.ts`) gates the explorer sidebar, the paper page header, and comments.
- the builtin `not-index` gates the graph, backlinks, table of contents, search, page title, and dark-mode toolbar.

Relax both so the root counts as a content page when it hosts a paper. Gate on `type: paper` so a non-paper root keeps its bare-landing behavior:

```ts
const isPaperNote = (page) => page.fileData.frontmatter?.type === "paper"

const isLibraryPage = (page) => {
  const slug = page.fileData.slug
  if (!slug || slug.startsWith("tags/")) return false
  if (slug === "index") return isPaperNote(page) // root is a content page when it hosts a paper
  return true
}
registerCondition("library-page", (page) => isLibraryPage(page))
registerCondition("not-index", (page) => page.fileData.slug !== "index" || isPaperNote(page))
```

Then rebuild and confirm in the emitted HTML that `/index.html` contains the explorer sidebar, paper page header, graph, and dark-mode toggle, matching a section page.

Conditions are necessary but not sufficient — two more layers can keep pages looking like bare landings even after the conditions pass. Check all three:

- **Component conditions** (above): make the root and paper pages eligible for each component.
- **Layout positions.** `layout.byPageType` in `quartz.config.yaml` can clear a whole region for a page type. A `folder.positions.right: []` override drops the entire right sidebar — graph, backlinks, table of contents — from folder/index pages (including every section index), regardless of conditions. Remove it so section index pages show the right sidebar.
- **Index-specific CSS.** The stylesheet may special-case the landing, e.g. a `body[data-slug="index"] { ... }` block that hides sidebars on tablet/mobile and centers the content card at a different width. Delete that block and apply the normal content-page alignment to every page (switch `body:not([data-slug="index"])` selectors to `body[data-slug]`) so the root paper note uses the same content card as section pages.

After fixing all three, confirm in the built HTML that both `/index.html` and a section `index.html` carry the sidebar, header, and graph, with a content card width matching a leaf page.

## Example: arXiv 2605.06901 HCLLM site

The first paper-hosting example imported arXiv `2605.06901`, "Reflections and New Directions for Human-Centered Large Language Models," with citekey `ziems2026reflections`.

The resulting site used these decisions:

- `/` is `content/index.md`, the main paper note.
- `content/ziems2026reflections/` contains the section tree.
- `content/ziems2026reflections/index.md` is titled `Sections`.
- `content/papers/` remains a reference-paper area, not the hosted full text.
- figures live under `content/ziems2026reflections/figures/` with lowercase filenames.
- starter template/demo content was removed.
- visible `sec:`, `subsec:`, `subsub:`, and `fig:` references were rewritten into descriptive internal links.
- boilerplate top-of-page nav blockquotes were removed to avoid backlink pollution.
- graph tag nodes were enabled and hosted paper pages were tagged `paper`.
- the root page was styled with the same content card and page chrome as regular pages.

Before finalizing another import, inspect the built site or local dev server and confirm that the root page, section index, section pages, graph, sidebar, citations, and figures all match these expectations.

## Standalone repository workflow

When the paper should become a standalone hosted example:

1. Create or initialize the destination repository from the selected `literature-template` baseline.
2. Apply the paper-to-site changes on a feature branch.
3. Keep the branch history clear enough that the template baseline and paper-import changes are understandable.
4. Validate locally.
5. Push the branch and open a PR against the destination repository's `main`.
6. Link the PR back to the tracking issue in `self-evolving/literature-template` when relevant.

Use SSH remotes if HTTPS tokens cannot update workflow files.

## Validation checklist

Before committing:

```bash
rg -n 'sec:|subsec:|subsub:|fig:|Full text' content || true
rg -n '^> ' content/<paper-slug> || true
rg -n '^# ' content || true
npm run check
npm run build
```

Expected results:

- no user-visible LaTeX label references
- no boilerplate nav blockquotes
- no duplicate page-title H1s
- root `/` renders like a regular content page
- citation links and bibliography render
- graph/backlinks show meaningful content relationships
- build/check pass
- working tree contains only intended content, config, bibliography, skill, and documentation changes
