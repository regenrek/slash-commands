---
description: Find a modern, faster JavaScript/TypeScript library alternative to a baseline library
argument-hint: BASELINE_LIB=<library> DOMAIN_USE_CASE=<use-case> CANDIDATE_LIBS=<lib1,lib2,...> [SCALE=<size>] [TOP_N=<number>]
---

## Problem statement
- Goal: Find a modern, faster JavaScript/TypeScript library for $DOMAIN_USE_CASE that outperforms $BASELINE_LIB in latency and bundle size while maintaining or improving relevance/quality.
- Context: Runs in Node 18+/browser, ESM‑first, TS types, no native deps; list size $SCALE; return $TOP_N suggestions per query.
- Note: If SCALE or TOP_N are not provided, defaults to "5k–50k items" and "top-3" respectively.

## Success metrics (make these explicit)
- P95 latency/query: target <1 ms at 10k items; <5 ms at 50k items (Node laptop). Adjust as needed.
- Bundle size: <25 KB min+gzip for core (no heavy optional modules).
- Quality: NDCG@5 (or task‑specific metric) ≥ $BASELINE_LIB on the same corpus (≥1.0x).
- Features: multi‑field weights, typo tolerance, diacritics, highlight ranges, incremental updates.
- DX: ESM, TS types, active maintenance (<6 months since last release), permissive license.

## Scope and exclusions
- In‑scope: in‑memory, client/Node libraries (no servers, no external indexes).
- Out‑of‑scope: hosted/search servers unless used only for comparison; heavy NLP stacks unless used for query expansion (phase 2).

## Concrete research question to post/search
- "Which modern JS/TS libraries outperform $BASELINE_LIB for $DOMAIN_USE_CASE at $SCALE, with ESM, TS types, and <25 KB gzip bundle? Compare $CANDIDATE_LIBS by p95 latency, relevance (NDCG@5 or equivalent), bundle size, multi‑field weighting, and maintenance."

## Search queries (copy/paste)
- benchmark "$BASELINE_LIB" vs $CANDIDATE_LIBS js performance
- "$CANDIDATE_LIBS vs $BASELINE_LIB" latency relevance "typescript" "esm"
- "$CANDIDATE_LIBS" library benchmark $DOMAIN_USE_CASE
- $CANDIDATE_LIBS benchmark bundle size "diacritics" "highlight"
- $CANDIDATE_LIBS javascript benchmark "multi field"
- $CANDIDATE_LIBS js benchmark in‑memory search

## Shortlist to evaluate
Evaluate the following candidate libraries: $CANDIDATE_LIBS

(Example for fuzzy name/description search: fuzzysort, quick‑score, FlexSearch, MiniSearch, Orama/Lyra, fast‑fuzzy, match‑sorter)

## Minimal benchmark design
- Dataset: 25k items with fields relevant to the task (e.g., name, description). Combine synthetic + real.
- Queries: 100 mixed queries (typos, prefixes, mid‑word, camelCase splits, etc.).
- Procedure: warm index; run 1k queries; record mean/p95; compute NDCG@5 (or task metric) vs hand‑labeled relevancy; measure min+gzip bundle.
- Environments: Node 20, Chrome stable. Configs: defaults + one tuned weights (e.g., name:0.7, description:0.3).

## Decision rubric
- Must meet latency + bundle targets and maintain ≥ $BASELINE_LIB quality metric.
- Prefer simple multi‑field API and highlight support.
- Tie‑breaker: maintenance cadence, type safety, ESM‑first.

## Deliverables
- One‑page results table (latency, quality metric, size, features).
- Example integration snippet for our "did‑you‑mean"/ranking path.
- Recommendation + migration notes (config, weights).

---

## Example usage

Usage: `/prompts:research-better-lib BASELINE_LIB=Fuse.js DOMAIN_USE_CASE="fuzzy matching tool names + short descriptions (10–500 chars)" CANDIDATE_LIBS="fuzzysort, quick-score, FlexSearch, MiniSearch, Orama" SCALE="5k–50k items" TOP_N="top-3"`

**Resulting research question:**
"Which modern JS/TS fuzzy search libraries outperform Fuse.js on 5k–50k items for fuzzy matching tool names + short descriptions (10–500 chars), with ESM, TS types, and <25 KB gzip bundle? Please compare fuzzysort, quick‑score, FlexSearch, MiniSearch, Orama by p95 latency, relevance (NDCG@5), bundle size, multi‑field weighting, and maintenance."

