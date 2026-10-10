---
name: literature-search
description: Run a structured, citation-verified literature search with the OpenAlex MCP tools. Use when the user asks for related work, a literature review, key papers or experts in a field, citation checks, or research-trend analysis.
---

# Literature search with OpenAlex

## When to use

- Building a related-work section or literature review
- Finding seminal papers, recent reviews, or leading researchers in a field
- Checking that a reference list is real, correctly cited, and from credible venues
- Mapping research trends or comparing research areas

## Workflow

1. **Scope the question.** Restate the topic, year range, and any venue or
   field constraints. Use `autocomplete_search` to fix ambiguous names.
2. **Get oriented.** Start with `find_review_articles` and `find_seminal_papers`
   on the topic before broad keyword searches.
3. **Search precisely.** Use `search_works` (Boolean queries, year ranges,
   citation thresholds) or `search_by_topic`. To restrict to strong venues, use
   `get_top_venues_for_field` / `check_venue_quality`, then
   `search_works_in_venue`, or `list_journal_presets` + `search_in_journal_list`.
4. **Snowball.** For each core paper, use `get_work_references` (backward),
   `get_work_citations` (forward), or `get_citation_network`, plus
   `get_related_works`.
5. **People and places.** Use `search_authors_by_expertise`,
   `get_author_profile`, `get_author_collaborators`, and `search_institutions`.
6. **Landscape.** Use `analyze_topic_trends`, `compare_research_areas`,
   `get_trending_topics`, and `analyze_geographic_distribution` for
   bibliometric context.
7. **Verify and access.** Run `batch_resolve_references` on every DOI you plan
   to cite, and `find_open_access_version` for full-text links.

## Rules

- Never invent papers, DOIs, authors, or citation counts. Cite only works
  returned by the tools, with DOI or OpenAlex ID.
- Report the search queries and filters used so results are reproducible.
- OpenAlex has metadata and abstracts, not full text; say so when a claim
  needs the full paper.
- Keep page sizes small and searches targeted: each API call draws on the
  user's daily OpenAlex budget. If calls fail with 401/429, suggest checking
  the API key with `health_check`.
