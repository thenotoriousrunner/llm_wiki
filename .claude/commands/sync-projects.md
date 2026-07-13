# /sync-projects — Sync project folders into wiki entities and boards

You are running inside the vault root (the current working directory).

Goal:
- Synchronize the operational project folders (`TODO/`, `ONGOING/`, `DONE/`)
  with the project entity pages and boards in `_llm-wiki/wiki/`, following the
  `/sync-projects` operation in CLAUDE.md.

Behavior:
1. Recursively scan `TODO/`, `ONGOING/`, and `DONE/`.
   - Each direct subfolder is treated as one project.
2. For each project:
   - Derive `status` from the parent folder (TODO | ONGOING | DONE).
   - Derive `priority` from the numeric prefix in the folder name (e.g. `01-...` -> 1).
   - Derive `folder` as the relative path (e.g. `TODO/01-llm-wiki-poc`).
3. For projects that do not yet have a corresponding entity page in
   `_llm-wiki/wiki/entities/projects/`, create one using the project template described
   in CLAUDE.md (or `_llm-wiki/templates/project.md` if present).
4. For existing projects, update frontmatter fields `status`, `priority`, and `folder`
   to match the current folder state.
5. Rebuild `_llm-wiki/wiki/boards/kanban-projects.md` so that it lists all projects
   grouped by status (TODO / ONGOING / DONE), ordered by priority within each group.
6. Update the projects section of `_llm-wiki/wiki/index.md` and `_llm-wiki/index.md`.
7. Append a SYNC entry to `_llm-wiki/log.md` indicating how many projects were created
   or updated.

Always follow the `/sync-projects` specification and the Rules in CLAUDE.md.
