# /ingest — Ingest a raw source into the LLM Wiki

You are running inside the vault root (the current working directory).

Goal:
- Ingest a raw source file into the LLM Wiki using the `/ingest` operation defined
  in CLAUDE.md.

Behavior:
1. If the user specifies a file path or folder path (relative to the project root),
   use that as the source to ingest.
2. If the user does NOT specify a file, ask a single clarifying question to select
   a file or folder (for example: a file under `_llm-wiki/raw/` or a project file
   under TODO/ONGOING/DONE).
3. Then perform the ingest exactly as described in the `/ingest` operation in
   CLAUDE.md:
   - Read the raw file(s)
   - Extract entities, concepts, key facts
   - Create or update the appropriate pages in `_llm-wiki/wiki/`
   - Update `_llm-wiki/wiki/index.md` and `_llm-wiki/index.md`
   - Append an INGEST entry to `_llm-wiki/log.md`.
4. Always respect the Rules section in CLAUDE.md (never write to TODO/ONGOING/DONE,
   never modify files in `_llm-wiki/raw/` after creation, etc.).

Ask for confirmation if you are about to touch more than ~20 pages in a single ingest.
