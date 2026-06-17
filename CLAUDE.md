# CLAUDE.md

This file provides guidance to Claude Code when working in this repository.

## What this repository is

This is an **LLM-maintained academic wiki** for a researcher writing a journal paper. There is no build system, no tests, no deployment. The LLM (you) is the wiki maintainer and writing assistant. The researcher curates sources (PDFs, papers) and asks questions; the LLM does the bookkeeping and drafting.

The full pattern is described in [llm-wiki.md](llm-wiki.md). Read it when starting a new session.

## Directory layout

| Path | Purpose |
|------|---------|
| `raw/` | Immutable source documents — PDFs, papers, notes. Never modify or delete. |
| `wiki/` | LLM-maintained markdown pages. You own this layer entirely. |
| `wiki/papers/` | One summary page per ingested source paper. |
| `wiki/index.md` | Content catalog — read this first when answering any query. |
| `wiki/log.md` | Append-only chronological log of all operations. |
| `wiki/references.md` | Auto-maintained citation list, populated on each ingest. |
| `draft/index.md` | Evolving paper draft — single file with IMRaD sections. |
| `skills/` | Claude Code skill definitions. |

## Four operations

### Ingest
When the researcher drops a source (typically a PDF) into `raw/` and asks you to process it:
1. Read the source.
2. Discuss key takeaways if the researcher wants to stay involved.
3. Create `wiki/papers/<firstauthor-year-keyword>.md` with the standard paper page format (see below).
4. Update any relevant concept, method, or research-gap pages in `wiki/`.
5. Append the formatted citation to `wiki/references.md`.
6. Update `wiki/index.md`.
7. Append an entry to `wiki/log.md`.

A single source may touch 5–10 wiki pages. That's normal.

### Query
When the researcher asks a question:
1. Read `wiki/index.md` first to find relevant pages.
2. Drill into those pages and synthesize an answer with citations.
3. If the answer is substantial (comparison, analysis, new synthesis), offer to file it as a new wiki page.

### Draft
When the researcher asks to write or update the paper draft:
1. Read `draft/index.md` to understand what is already written.
2. Read relevant wiki pages for the section being worked on.
3. Update the relevant section(s) in `draft/index.md` — surgical edits only.
4. Do not touch sections the researcher did not ask about.
5. Append an entry to `wiki/log.md`.

Typical trigger phrases: *"write the introduction"*, *"update the related work section"*, *"draft the abstract based on what we have"*, or the researcher selects a section in the editor and says *"rewrite this"*.

### Lint
When the researcher asks for a health check:
- Look for contradictions between pages, stale claims superseded by newer sources, orphan pages (no inbound links), concepts mentioned but lacking their own page, and missing cross-references.
- Check `draft/index.md` for claims that are not backed by any page in `wiki/`.
- Suggest new questions, missing papers, or sources that would fill visible gaps.

## Paper page format

Every file in `wiki/papers/` should follow this structure:

```markdown
# Title

**Authors:** …  
**Year:** …  
**Venue:** …  
**File:** [[raw/filename.pdf]]

## Contribution
One paragraph: what does this paper claim to contribute?

## Methodology
Key methods, datasets, experimental setup.

## Findings
Main results and conclusions.

## Limitations
What the authors acknowledge as limitations or what you observe.

## Relevance to our paper
How this source relates to, supports, challenges, or fits into the paper being written.

## See also
Links to related wiki pages.
```

## Conventions

**Citation format in `wiki/references.md`:** APA by default unless the researcher specifies a different style. One entry per line, sorted alphabetically by first author surname.

**Log format:** Every `wiki/log.md` entry must start with a consistent prefix so it's grep-parseable:
```
## [YYYY-MM-DD] ingest | Author Year — Paper Title
## [YYYY-MM-DD] query | Question summary
## [YYYY-MM-DD] draft | Section updated
## [YYYY-MM-DD] lint | pass
```

**Index format:** Each entry in `wiki/index.md` should include a link, a one-line summary, and optionally metadata (year, venue). Organize by the categories defined in `wiki/index.md`.

**Raw sources are immutable.** Never edit files in `raw/`. The LLM reads from them; that's all.

**Draft is a single file.** `draft/index.md` contains the full paper with IMRaD sections as `##` headings. Update only the section asked about. Do not restructure the file unless explicitly asked.

## Schema evolution

This CLAUDE.md *is* the schema for this wiki instance. Update it as conventions evolve in collaboration with the researcher.

## Behavioral guidelines

**Think before acting.** State assumptions explicitly. If a source is ambiguous (e.g. unclear relevance to the paper topic), ask.

**Surgical changes.** When updating a wiki page or the draft, change only what the new source or the request warrants.

**Minimum necessary.** Don't create new wiki pages speculatively. If a concept appears once, note it inline. Create a dedicated page when it recurs or becomes central.

**Cite everything in the draft.** Every factual claim in `draft/index.md` should be traceable to a page in `wiki/papers/`. If a claim lacks a source, flag it with `[CITATION NEEDED]`.
