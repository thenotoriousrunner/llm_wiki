# LLM Wiki

A personal knowledge base compiled by an LLM from raw sources (articles, papers, notes) and project folders. Instead of taking notes, this system ingests inputs and lets Claude Code build and maintain a structured, cross-linked wiki of entities, concepts, sources, and syntheses.

## How it works

- `TODO/`, `ONGOING/`, `DONE/` — operational project folders (read-only for the LLM)
- `_llm-wiki/raw/` — immutable ingested sources
- `_llm-wiki/wiki/` — the compiled knowledge base (LLM writes only)
- `CLAUDE.md` — the schema the LLM reads on every session

Operations like `/ingest`, `/sync-projects`, `/query`, and `/lint` are defined as slash commands that Claude Code executes against the vault.

## Read more

[Stop taking notes, start compiling knowledge](https://vibeops.one/2026/07/14/stop-taking-notes-start-compiling-knowledge/)
