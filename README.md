# OpenAlex Research — Cursor plugin

<img src="assets/logo.png" width="96" alt="OpenAlex Research logo">

Search and analyze the [OpenAlex](https://openalex.org) scholarly graph (480M+ works,
authors, institutions, venues, topics) directly from Cursor's agent. Useful for
literature reviews, related-work sections, citation checking, finding experts, and
research-trend analysis.

This plugin packages the open-source MCP server
[`openalex-research-mcp`](https://github.com/oksure/openalex-research-mcp)
(npm, MIT license, by [@oksure](https://github.com/oksure)) plus a
`literature-search` skill with a recommended workflow. It is not affiliated with
or endorsed by OpenAlex / OurResearch.

## What's included

| Component | Path | Purpose |
| --- | --- | --- |
| MCP server | `mcp.json` | Runs `npx -y openalex-research-mcp@0.5.0` (stdio) |
| Skill | `skills/literature-search/SKILL.md` | Step-by-step literature-search workflow using the tools below |
| Logo | `assets/logo.svg`, `assets/logo.png` | Marketplace icon |

## Tools (31, from openalex-research-mcp v0.5.0)

**Works & search**
- `search_works` — search works with Boolean queries, year ranges, citation thresholds, venue and institution filters
- `get_work` — full details for a work by OpenAlex ID or DOI (all authors, affiliations, abstract, etc.)
- `get_related_works` — works related by shared topics, citations, and references
- `search_by_topic` — search within research topics/domains, optionally venue-filtered
- `autocomplete_search` — fast typeahead for works, authors, institutions, and other entities
- `get_entity` — any OpenAlex entity by ID (works, authors, sources, institutions, topics, publishers, funders)

**Citation analysis**
- `get_work_citations` — works citing a given work (forward citations)
- `get_work_references` — works referenced by a given work (backward citations)
- `get_citation_network` — immediate citing + referenced works
- `get_top_cited_works` — most-cited works matching criteria
- `find_seminal_papers` — highly cited foundational papers (published 5+ years ago)
- `find_review_articles` — reviews, systematic reviews, meta-analyses
- `batch_resolve_references` — resolve a list of DOIs/IDs in one call to validate a reference list
- `find_open_access_version` — free full-text locations (arXiv, bioRxiv, repositories)

**Venues & credibility**
- `search_sources` — journals/conferences sorted by h-index
- `get_top_venues_for_field` — top venues for a field
- `check_venue_quality` — h-index, impact and indexing status of a venue
- `search_works_in_venue` — papers in a specific journal/conference
- `list_journal_presets` / `search_in_journal_list` — search named lists such as UTD24, FT50, AJG 4*

**Authors & institutions**
- `search_authors` — authors by name, with h-index, citations, affiliations
- `search_authors_by_expertise` — leading researchers in a topic
- `get_author_profile` — h-index, i10-index, top/recent works, main topics
- `get_author_works` — an author's publications over time
- `get_author_collaborators` — co-authorship network
- `search_institutions` — institutions by output, citations, location

**Trends & landscape**
- `analyze_topic_trends` — publications per year for a topic
- `compare_research_areas` — volume and citation comparison across topics
- `get_trending_topics` — fast-growing research areas
- `analyze_geographic_distribution` — countries/institutions most active on a topic

**Diagnostics**
- `health_check` — server status, cache, and whether the API key / email are configured

## Setup

Requirements: Node.js 18+ (the server is fetched with `npx` on first use).

1. Get a free OpenAlex API key at <https://openalex.org/settings/api>.
   Since February 2026 OpenAlex expects an API key for anything beyond light demo
   use; each key includes $1/day of free usage
   ([announcement](https://blog.openalex.org/openalex-api-new-features-and-usage-based-pricing/),
   [authentication docs](https://help.openalex.org/api/authentication/)).
2. Install the plugin, then set its variables in the Cursor dashboard under
   **Plugins → Configure**:

| Variable | Required | Description |
| --- | --- | --- |
| `OPENALEX_API_KEY` | Yes | Your OpenAlex API key |
| `OPENALEX_EMAIL` | No | Contact email added to the request User-Agent (the old `mailto` polite pool is no longer honored by OpenAlex) |

The plugin stores no secrets; `mcp.json` only contains `${OPENALEX_API_KEY}` /
`${OPENALEX_EMAIL}` placeholders.

3. Ask the agent something like:
   - "Find the most-cited review articles on digital twins in construction since 2019."
   - "Who are the leading researchers in BIM-based construction robotics, and where do they work?"
   - "Check that these DOIs exist and tell me which venues they were published in: …"
   - "Show the publication trend for 'automated rebar inspection' 2015–2025."

### Local testing

Copy this folder to `~/.cursor/plugins/local/openalex-research`, run
**Developer: Reload Window**, and confirm the `openalex` MCP server and the
`literature-search` skill appear in Customize.

## Notes and limits

- OpenAlex provides metadata and abstracts, not full text.
- Every tool call consumes your OpenAlex daily budget; HTTP 429 means the budget
  or the 100 req/s limit was hit.
- The server version is pinned to `0.5.0` for reproducibility; bump it in
  `mcp.json` to pick up upstream updates.

## 中文说明

本插件把开源 MCP 服务器 [`openalex-research-mcp`](https://github.com/oksure/openalex-research-mcp)
（MIT 许可，作者 @oksure）打包为 Cursor 插件，可在 Cursor 中直接检索 OpenAlex
学术数据库（4.8 亿+ 文献）：文献检索、引文分析（前向/后向引用）、作者与机构、
期刊质量评估、研究趋势与地理分布等，共 31 个工具，并附带一个“文献检索”技能
（`literature-search`），给出综述/相关工作检索的推荐流程。

**配置：** 需要 Node.js 18+。在 <https://openalex.org/settings/api> 免费申请 API
key（自 2026 年 2 月起 OpenAlex 要求使用 key，每个 key 每天有 1 美元免费额度），
安装插件后在 Cursor 后台 **Plugins → Configure** 中填写 `OPENALEX_API_KEY`
（必填）和 `OPENALEX_EMAIL`（可选）。插件本身不包含任何密钥。

**注意：** OpenAlex 只提供元数据和摘要，不提供全文；请只引用工具实际返回的文献并核对 DOI。

## Attribution & license

- MCP server: [oksure/openalex-research-mcp](https://github.com/oksure/openalex-research-mcp),
  MIT License, Copyright (c) 2025 OpenAlex MCP Server. It is downloaded from npm at
  runtime and not redistributed in this repository.
- Data: [OpenAlex](https://openalex.org) by OurResearch (data released under CC0).
  "OpenAlex" is used here only to describe compatibility; the logo in `assets/` is an
  original design, not the OpenAlex logo.
- This plugin's own files (manifest, skill, README, logo): MIT License, see `LICENSE`.
