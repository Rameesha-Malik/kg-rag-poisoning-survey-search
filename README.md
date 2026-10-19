# Search and screening record for the survey "Defending Retrieval-Augmented Generation Against Knowledge Poisoning: A Survey Focused on Knowledge-Graph-Based RAG and Provenance Verification"

This repository holds the script, the logs and the screening decisions behind the Survey Method section of the survey. Its purpose is to let a reader check the numbers of the PRISMA flow diagram and repeat or audit the search. It does not contain the text of the paper or the papers themselves.

**Authors:** Rameesha and Dr Anwar Shah, Department of Artificial Intelligence and Data Science, FAST-NUCES, Islamabad.
**Status:** draft under supervisor review.

## At a glance

| Item | Value |
|---|---|
| Search window | 1 January 2024 to 31 October 2026 |
| Date of the main run | 19 October 2026 |
| Sources searched by script | arXiv, Crossref, OpenAlex, with citation chasing in OpenAlex |
| Other sources | Records collected by hand from IEEE Xplore, ACM Digital Library, SpringerLink and ScienceDirect; five PDFs supplied directly; a list of 50 surveys; a supplementary Crossref run on survey titles for four publisher DOI prefixes |
| Unique records | 1,925 |
| Records examined at title and abstract level | 796 |
| Included after screening | 25 |
| Reporting standard | PRISMA 2020 |

## Repository contents

| File | Content |
|---|---|
| `code/collect_papers.py` | Python script (standard library only). Queries arXiv, Crossref and OpenAlex, merges duplicates, scores each record, assigns tier A, B or C, optionally follows citations (`--snowball`) and writes the logs. |
| `code/title_sweep.py` | Title check of records that were not screened in the first pass. |
| `data/search_log.csv` | One row per query: source, query text, date window, hits reported and retrieved; one row per seed for citation chasing. |
| `data/prisma_counts.json` | The counts used in the PRISMA flow diagram. |
| `data/publisher_surveys_search_log.csv` | Queries of the supplementary Crossref run on survey titles (10 queries for each of 4 publisher DOI prefixes). |
| `data/merge_log_*.csv` | How the hand-collected publisher records, the survey list and the publisher survey run were merged into the main set (duplicate or added). |
| `data/screening_decisions.csv` | All 1,925 unique records: tier, score, title, year, venue, DOI or arXiv identifier, source, decision (approve, maybe, reject, not screened) and the reason. Abstracts are not included. |
| `data/approved_list.csv` | The 25 included works with group and reason. |
| `data/manual_search_log.csv` | Template for the manual searches (Google Scholar, IEEE Xplore). |

## Papers and full data archive (Hugging Face)

The collected papers and the working data are kept in a separate Hugging Face dataset, because they are too large for GitHub and because the papers belong to their publishers and authors:

**https://huggingface.co/datasets/rameesha146/survey-KG-RAG**

It contains one archive, `KG_RAG_Survey_Full_Archive.zip` (about 1.55 GB). The archive supports the reading and checking of the survey. The papers in it are the property of their publishers and authors and are provided for the review of this work only; please do not redistribute them. Access to the dataset is restricted to the reviewers of this work. The records in this repository carry titles, identifiers and decisions only, so every paper can be found from its DOI or arXiv identifier.

## How the scores and tiers work

The script scores the title and abstract of each record with keyword patterns (a graph or knowledge-graph term, a RAG term, an attack term, and a defence, provenance or verification term) and assigns a tier.

- **Tier A:** a graph term, a RAG term and a poisoning, provenance, verification or integrity term, and no term that points to an unrelated application.
- **Tier B:** a graph or RAG term, an attack or integrity term and a score of at least four.
- **Tier C:** all other records.

The score only orders the work. It does not decide inclusion.

## Screening

Each screened record was judged against four criteria, which are given in the Survey Method section of the paper. Records were labelled approve, maybe or reject, and the reason is recorded for each in `data/screening_decisions.csv`. The first-pass decisions were made with the help of an AI assistant (Claude, Anthropic) acting on these criteria. The decisions were then reviewed manually, which changed four of them. There was no second independent reviewer. Records marked "not screened" (1,129 of them) were not examined beyond a title check.

## How to repeat the search

```
python code/collect_papers.py --since 2024-01-01 --until 2026-10-31 --out results --snowball
```

A new run will return different records, because arXiv, Crossref and OpenAlex change over time. The logs in `data/` are the record of what was actually retrieved in the main run. The optional environment variable `OPENALEX_MAILTO` can be set to an e-mail address for polite access to OpenAlex.

## Limits of the record

- The search covered arXiv, Crossref and OpenAlex, with 50 records collected by hand from four publishers. Scopus and Web of Science were not used.
- Screening was done once, without a second independent reviewer.
- Screening is based on titles and abstracts; the full text was read only for the included works.
- Most of the included work is recent and much of it is in preprint form, so the status of each preprint should be checked again before publication.

## How to cite

Citation details will be added when the paper is published.
