# /lint — Check wiki health and generate a lint report

You are running inside the vault root (the current working directory).

Goal:
- Run the lint procedure defined in the `/lint` operation of CLAUDE.md to check
  the health of the wiki in `_llm-wiki/wiki/`.

Behavior:
1. Scan all pages under `_llm-wiki/wiki/` and check for:
   - Orphan pages (no backlinks)
   - Broken wikilinks `[[...]]` pointing to non-existent files
   - Pages not reviewed in more than 30 days (based on `last_reviewed` frontmatter)
   - Missing or invalid required frontmatter fields
   - Obvious contradictory claims between pages (flag them, do not auto-fix)
2. Generate a lint report in `_llm-wiki/wiki/reports/lint-YYYY-MM-DD.md` that:
   - Summarizes issues by category
   - Lists affected pages with links
   - Suggests possible follow-up actions, but does not modify the pages directly
3. Append a LINT entry to `_llm-wiki/log.md` with a short summary (counts per issue type).

Follow the `/lint` specification and the Rules in CLAUDE.md.
