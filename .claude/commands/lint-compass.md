### /lint-compass
1. Scan `_llm-wiki/wiki/entities/projects/` for all project entity pages
2. Identify:
   - all projects with `role: compass`
   - all projects with `role: pillar`
   - all projects with `role: output`
3. For each `role: compass` project, validate:
   - frontmatter contains:
     - `role: compass`
     - `priority: 0`
   - recommended fields exist:
     - `north_star`
     - `exclude_from_kanban`
     - `exclude_from_execution_views`
   - body contains these sections:
     - `## One-line compass`
     - `## Purpose`
     - `## Non-goals`
     - `## Decision rules`
     - `## Signals of drift`
     - `## Dependencies`
     - `## Downstream projects`
4. For each `role: output` or `role: pillar` project, check whether:
   - it links at least one relevant `compass` or `pillar` project when such a relationship is apparent from name, tags, dependencies, or content
   - it appears to contradict the decision rules or non-goals of any linked `compass`
5. Check derived views:
   - flag any `role: compass` project that appears inside operational kanban / execution views
6. Produce `_llm-wiki/reports/compass-lint.md` with:
   - healthy compass projects
   - missing metadata
   - missing sections
   - orphan execution projects (no linked compass/pillar)
   - possible drift or contradiction warnings
   - suggested promotions (projects that seem strategic but are still `role: output`)
7. Append log entry to `_llm-wiki/log.md`:
   `YYYY-MM-DD HH:MM | LINT | compass lint completed`
8. Do not automatically rewrite project roles or content during lint; only report findings unless explicitly asked to fix them