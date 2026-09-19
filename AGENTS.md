# Repository guidance

## Book reference search

The book has been converted to [docs/book/book.md](docs/book/book.md) and is
also indexed in an OpenAI managed vector store. For a request about the book's
content, chapter locations, terminology, examples, or relationship between
book material and repository code, search the local Markdown first with `rg`.
It is faster, incurs no API cost, and is the primary source in this checkout.

- Vector store ID: `vs_6aae036ef384819194b46dee6cc2bc24`
- Indexed source: `docs/book/book.md`
- OpenAI file ID: `file-NJgbrFTfK33CL3ekRV5ZY5`

Use the Responses API `file_search` tool with the vector-store ID above only
when local search is insufficient, semantic retrieval would materially improve
the answer, or the user explicitly requests the hosted search. Read the API
key from `~/.openai/api_key`; never print, commit, or copy its value into logs,
source code, prompts, or documentation. Ask the search query in terms of the
user's request and use the retrieved passages as evidence for book-specific
claims.

Do not create a duplicate vector store or re-upload the book unless explicitly
asked.
