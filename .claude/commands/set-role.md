### /set-role <project> <role>
1. Resolve `<project>` to the matching project entity under `_llm-wiki/wiki/entities/projects/`
2. Validate `<role>` against the allowed values:
   - `compass`
   - `pillar`
   - `output`
3. Update the project entity frontmatter:
   - set `role: <role>`
4. If `<role>` is `compass`:
   - set `priority: 0` if missing or higher than 0
   - add these frontmatter fields if missing:
     - `north_star: <project-slug>`
     - `exclude_from_kanban: true`
     - `exclude_from_execution_views: true`
     - `publish: false`
   - ensure the body contains these sections, creating them if missing:
     - `## One-line compass`
     - `## Purpose`
     - `## Non-goals`
     - `## Decision rules`
     - `## Signals of drift`
     - `## Concrete examples`
     - `## Dependencies`
     - `## Downstream projects`
     - `## Review cadence`
5. If `<role>` is `pillar` or `output`:
   - preserve existing content
   - do not remove compass-specific fields unless explicitly asked
6. Update `_llm-wiki/wiki/index.md` project metadata if needed
7. Append log entry to `_llm-wiki/log.md`:
   `YYYY-MM-DD HH:MM | ROLE | <project> -> <role>`
8. Report what was changed