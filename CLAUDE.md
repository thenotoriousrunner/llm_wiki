# LLM Wiki Schema

## Vault Structure

- `TODO/`, `ONGOING/`, `DONE/` — operational project folders. NEVER modify, move, or delete files here.
- `_llm-wiki/raw/` — immutable external sources (articles, papers, web clips, meeting notes). Never modify once ingested.
- `_llm-wiki/wiki/` — compiled knowledge base. Only the LLM writes here.
- `_llm-wiki/templates/` — Markdown templates for page types.
- `_llm-wiki/index.md` — global catalog of all wiki pages. Keep it under 3000 tokens.
- `_llm-wiki/log.md` — append-only log of all ingest / query / sync / lint operations.
- `CLAUDE.md` — this file. The schema the LLM reads on every session start.

## Wiki Page Types

| Folder | Page type | Description |
|--------|-----------|-------------|
| `wiki/entities/projects/` | Project entity | One page per project, derived from TODO/ONGOING/DONE |
| `wiki/entities/tools/` | Tool entity | Tools, platforms, frameworks used |
| `wiki/entities/people/` | Person entity | Stakeholders, clients, collaborators |
| `wiki/concepts/` | Concept | Patterns, methodologies, principles |
| `wiki/sources/` | Source | One page per ingested raw source |
| `wiki/synthesis/` | Synthesis | Cross-source analysis, comparisons |
| `wiki/queries/` | Query archive | Archived answers to important queries |
| `wiki/boards/` | Board | Aggregate views: kanban, roadmap |
| `wiki/reports/` | Lint report | Output of /lint operations |

## Page Frontmatter (mandatory for all wiki pages)

Every page in `wiki/` must start with this YAML frontmatter:

```yaml
---
title: ""
type: ""            # project | tool | person | concept | source | synthesis | query | board | report
status: ""          # for projects: TODO | ONGOING | DONE. For others: active | archived
priority: ""        # for projects: 1, 2, 3... For others: leave blank
tags: []
created: YYYY-MM-DD
last_reviewed: YYYY-MM-DD
---
```

## Operations

### /ingest [filepath or description]
1. Read the source file in `_llm-wiki/raw/` or from the specified path
2. Extract: key concepts, entities (tools, people, projects), facts, decisions
3. Create or update relevant pages in `wiki/` (typically 3–10 pages per ingest)
4. Add a new entry in `wiki/sources/` for this source
5. Update `_llm-wiki/wiki/index.md` and `_llm-wiki/index.md`
6. Add cross-links using wikilinks `[[page-name]]` where relevant
7. Append a log entry to `_llm-wiki/log.md`: `YYYY-MM-DD HH:MM | INGEST | filename | pages updated`

### /sync-projects
1. Recursively scan `TODO/`, `ONGOING/`, `DONE/` and list all subfolders (each subfolder = one project)
2. Compare against `_llm-wiki/wiki/entities/projects/` to detect:
   - New projects (create new entity page)
   - Status changes (project moved between TODO/ONGOING/DONE folders)
   - Priority changes (folder prefix number changed)
3. For each change: update the frontmatter fields `status`, `priority`, `folder` in the entity page
4. For each **new or empty entity page (and for empty consider also pages that don't have any description)**, read the source folder contents and compile the page body:
   - Description: 1-2 sentence summary (what it is, goal), generally found in base-idea.md
   - Current status: one-line note on where the project stands
   - Stack / key components: bullet list of main technologies or tools involved
   - Related projects: scan _llm-wiki/wiki/entities/projects/ and link any pages sharing technologies, goals, or dependencies — add as [[wiki-link]] references
   - Source: populate the frontmatter resource: field with the relative path to the source folder
   - Always output in English
5. Rebuild `_llm-wiki/wiki/boards/kanban-projects.md` from all project entity pages
6. Update `_llm-wiki/wiki/index.md` projects section
7. Append log entry to `_llm-wiki/log.md`: `YYYY-MM-DD HH:MM | SYNC | N projects updated`

### /query [question]
1. Read `_llm-wiki/wiki/index.md` to identify 3–7 most relevant pages
2. Read only those pages
3. Answer the question with inline wikilinks `[[page]]` as citations
4. If the answer is worth archiving, save it to `wiki/queries/YYYY-MM-DD-slug.md`
5. Append log entry: `YYYY-MM-DD HH:MM | QUERY | question slug`

### /lint
1. Scan all pages in `wiki/` and check for:
   - Pages with no backlinks (orphan pages)
   - Broken wikilinks `[[page]]` pointing to non-existent files
   - Pages not reviewed in more than 30 days
   - Frontmatter missing required fields
   - Contradictory claims across pages (flag, do not auto-fix)
2. Generate a report in `wiki/reports/lint-YYYY-MM-DD.md`
3. Append log entry: `YYYY-MM-DD HH:MM | LINT | N issues found`

## Rules

1. Never write to `TODO/`, `ONGOING/`, `DONE/` — read-only for the LLM.
2. Never modify files in `_llm-wiki/raw/` after initial creation.
3. Every wiki page must have valid YAML frontmatter.
4. Keep `_llm-wiki/index.md` compact: one table row per page, one-line description.
5. Always add cross-links between related pages using `[[wikilink]]` syntax.
6. Use ISO dates (YYYY-MM-DD) everywhere.
7. Write all wiki content in English.

## Strategic project roles

Project entity pages under `_llm-wiki/wiki/entities/projects/` may include a `role` field in YAML frontmatter.

Allowed values:
- `compass` → strategic backbone / north-star project
- `pillar` → enabling project, infrastructure, or reusable system
- `output` → concrete deliverable or execution project

Rules:
- Preserve `role` when editing existing project entities.
- If a project entity has no `role`, default to `output` unless the user explicitly says otherwise.
- Treat `compass` projects as strategic references for related work.
- Do not include `compass` projects in operational kanban or execution views unless the user explicitly asks for them.
- When updating a `compass` project, maintain these sections if present:
  - `One-line compass`
  - `Purpose`
  - `Non-goals`
  - `Decision rules`
  - `Signals of drift`
  - `Dependencies`
  - `Downstream projects`
- When creating or updating `output` or `pillar` projects, link them to relevant `compass` projects when such relationships are clear.
- If a project appears strategic but still has `role: output`, do not change the role automatically; suggest the promotion instead.

Compass-specific metadata may include:
- `north_star`
- `exclude_from_kanban`
- `exclude_from_execution_views`
- `publish`

Use these fields as instructions for filtering and visibility in derived views.

Read `@_llm-wiki/docs/compass-projects.md` when working on compass projects or evaluating project alignment.
---
