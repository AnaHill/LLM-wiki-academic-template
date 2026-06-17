# LLM Academic Wiki — VS Code Foam

A wiki template for academic researchers, maintained by an LLM, using VS Code + [Foam](https://foambubble.github.io/foam/) as the IDE.

Drop a paper PDF into `raw/`. The LLM reads it, writes a summary page, updates the concept and gap pages, and appends a citation to the reference list. When you're ready to write, tell it to draft or update a section — it draws from everything it has read.

Based on [Andrej Karpathy's LLM wiki idea](https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f).

---

## Quick start

### Starting a new paper project (recommended)

Use the GitHub template to get a clean repo with no connection to this template:

```bash
gh repo create my-paper-on-topic-X --public --template AnaHill/LLM-wiki-academic-template --clone
cd my-paper-on-topic-X
```

Or click **"Use this template"** on [github.com/AnaHill/LLM-wiki-academic-template](https://github.com/AnaHill/LLM-wiki-academic-template) and clone the result.

Each paper gets its own isolated repo. Edit `CLAUDE.md` in it to tailor the wiki schema for that paper's topic.

### Then

1. Open the project folder in VS Code — accept the Foam extension recommendation.
2. Open a Claude Code session in this directory.
3. Drop a PDF into `raw/` and say: **"ingest `raw/<filename.pdf>`"**
4. When ready to write: open `draft/index.md`, select a section, say **"write the introduction"**.

---

## How it works

| Layer | Path | Who owns it |
|-------|------|-------------|
| Source papers | `raw/` | You — PDFs, notes. Immutable. |
| Wiki pages | `wiki/` | LLM — paper summaries, concepts, gaps |
| Paper draft | `draft/index.md` | LLM writes, you direct |

```
ingest PDF                                    write draft
    │                                              │
    ▼                                              ▼
raw/ ──► wiki/papers/ ──► wiki/index.md ──► draft/index.md
              │                   │
              └── wiki/references.md (citations auto-appended)
```

**Four operations:**
- **Ingest** — LLM reads a PDF, creates a paper page, updates concepts/gaps, appends citation
- **Query** — ask anything; LLM reads the wiki and synthesizes an answer with citations
- **Draft** — ask to write or rewrite any section of `draft/index.md`
- **Lint** — health check: stale claims, orphans, unsupported draft assertions

See [CLAUDE.md](CLAUDE.md) for the full schema and [llm-wiki.md](llm-wiki.md) for the concept.

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
draft/
  index.md         evolving paper draft — IMRaD sections, single file
skills/
  karpathy-guidelines/  behavioral guidelines for the LLM
CLAUDE.md          wiki schema and LLM instructions
llm-wiki.md        the pattern explained
```

---

## Tips

- **Select a section** in `draft/index.md` and say *"rewrite this"* — Claude edits only that section.
- **`[CITATION NEEDED]`** markers in the draft mean Claude couldn't find a supporting source — add the paper and re-ingest.
- **Graph view** in Foam shows how paper pages link to concept and gap pages — useful for spotting holes in coverage.
- **`wiki/references.md`** is copy-paste ready for submission; tell Claude your citation style if not APA.
