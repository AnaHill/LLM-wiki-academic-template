# LLM Academic Wiki

A wiki template for academic researchers, maintained by an AI agent, using VS Code as the editor. The pattern works with any LLM that supports an instruction file; the reference implementation uses [Claude Code](https://claude.ai/code) and its `CLAUDE.md` convention.

Drop a paper (PDF/other) into `raw/`. The LLM reads it, writes a summary page, updates the concept and gap pages, and appends a citation to the reference list. When you're ready to write, tell it to draft or update a section — it draws from everything it has read.

Based on [Andrej Karpathy's LLM wiki idea](https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f).

---

## Quick start

### Starting a new paper project (recommended)

Use the GitHub template to get a clean repo with no connection to this template:

```bash
gh repo create my-paper-on-topic-X --public --template AnaHill/llm-wiki-academic-template --clone
cd my-paper-on-topic-X
```

Or click **"Use this template"** on [github.com/AnaHill/llm-wiki-academic-template](https://github.com/AnaHill/llm-wiki-academic-template) and clone the result.

Each paper gets its own isolated repo. Edit the agent instructions file (`CLAUDE.md` for Claude Code) to tailor the wiki schema for that paper's topic.

### Then

1. Open the project folder in VS Code and start an AI agent session in this directory.
2. Drop a paper (PDF/other) into `raw/` and say: **"ingest `raw/<filename>`"**
3. When ready to write: open `draft.md`, select a section, and say e.g. **"write the introduction"**.

---

## How it works

| Layer | Path | Who owns it |
|-------|------|-------------|
| Source papers | `raw/` | You — PDFs, notes. Immutable. |
| Wiki pages | `wiki/` | LLM — paper summaries, concepts, gaps |
| Paper draft | `draft.md` | LLM writes, you direct |

```
ingest PDF                                    write draft
    │                                              │
    ▼                                              ▼
raw/ ──► wiki/papers/ ──► wiki/index.md ──► draft.md
              │                   │
              └── wiki/references.md (citations auto-appended)
```

**Four operations:**
- **Ingest** — LLM reads a PDF, creates a paper page, updates concepts/gaps, appends citation
- **Query** — ask anything; LLM reads the wiki and synthesizes an answer with citations
- **Draft** — ask to write or rewrite any section of `draft.md`
- **Lint** — health check: stale claims, orphans, unsupported draft assertions

See [CLAUDE.md](CLAUDE.md) for the full wiki schema (the agent instructions file) and [llm-wiki.md](llm-wiki.md) for the concept.

---

## Why not just RAG?

|                   | Classic RAG         | This wiki                      |
|-------------------|---------------------|--------------------------------|
| What's retrieved  | raw chunks          | curated wiki pages             |
| Quality over time | flat                | compounds with each ingest     |
| Storage           | vector DB           | markdown in git                |
| Contradictions    | silently coexist    | surfaced by `lint`             |
| Ownership         | vendor-specific DB  | a git repo you own             |
| Portability       | migrate DB, reindex | `git clone` / local copy       |

---

## Repository layout

```
raw/               source PDFs (immutable)
wiki/
  papers/          one page per ingested paper (firstauthor-year-keyword.md)
  index.md         content catalog — LLM reads this first on every query
  log.md           append-only operation log
  references.md    auto-maintained citation list (APA by default)
skills/
  karpathy-guidelines/  behavioral guidelines for the LLM
draft.md           evolving paper draft — IMRaD sections, single file
CLAUDE.md          wiki schema and agent instructions (Claude Code convention)
llm-wiki.md        the pattern explained
```

---

## Tips

- **Select a section** in `draft.md` and say *"rewrite this"* — the agent edits only that section.
- **`[CITATION NEEDED]`** markers in the draft mean the agent couldn't find a supporting source — add the paper and re-ingest.
- **Graph view** in Foam shows how paper pages link to concept and gap pages — useful for spotting holes in coverage.
- **`wiki/references.md`** is copy-paste ready for submission; specify your citation style if not APA.

---

## Optional: VS Code Foam

[Foam](https://foambubble.github.io/foam/) adds a graph view and `[[wikilink]]` navigation on top of this repo. It's not required — the wiki works fine without it.

To try it:
1. Install the [Foam extension](https://marketplace.visualstudio.com/items?itemName=foam.foam-vscode) from the VS Code marketplace.
2. Reload the window — Foam will index the `wiki/` files automatically.
3. Open **Foam: Show Graph** from the command palette to explore link relationships between pages.
