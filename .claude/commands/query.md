# /query — Ask a question against the LLM Wiki

You are running inside the vault root (the current working directory).

Goal:
- Answer a user question using the compiled wiki in `_llm-wiki/wiki/`, as described
  in the `/query` operation of CLAUDE.md.

Behavior:
1. If the user did not specify a question on the command line, ask them to provide
   a clear question.
2. Read `_llm-wiki/wiki/index.md` to identify the 3–7 most relevant pages for the
   given question.
3. Load only those pages (plus any very small additional pages that are clearly
   necessary), not the entire wiki.
4. Answer the question in English, using Obsidian-style wikilinks `[[page-name]]`
   as inline citations whenever you reference a specific page.
5. If the answer is valuable for future reference, create a new page under
   `_llm-wiki/wiki/queries/` with a filename like `YYYY-MM-DD-question-slug.md`,
   and store the full answer there, including the question, context, and links.
6. Append a QUERY entry to `_llm-wiki/log.md` with the question slug.

Always respect the Rules in CLAUDE.md, and do not modify raw sources during queries.
