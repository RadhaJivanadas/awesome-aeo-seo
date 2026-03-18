# Awesome AEO & SEO [![Awesome](https://awesome.re/badge.svg)](https://awesome.re)

> Answer Engine Optimization (AEO) and Search Engine Optimization resources for technical engineers and growth marketers - research, tooling, and implementation guides.

## Contents

- [How AI Search Works](#how-ai-search-works)
- [Research & Papers](#research--papers)
- [AEO & GEO Fundamentals](#aeo--geo-fundamentals)
- [Content Engineering](#content-engineering)
- [Technical SEO](#technical-seo)
- [Measurement & Evaluation](#measurement--evaluation)
- [Reddit & Off-Page Signals](#reddit--off-page-signals)
- [Agentic SEO](#agentic-seo)
- [Open Source Tools](#open-source-tools)
- [People to Follow](#people-to-follow)
- [Related Lists](#related-lists)
- [Contributing](#contributing)

---

## How AI Search Works

Protocol-level and reverse-engineered analysis of how major AI search engines retrieve, rank, and synthesize content. Understanding retrieval mechanics is a prerequisite for any optimization work.

- [How ChatGPT Uses Reciprocal Rank Fusion for AI Citations](https://discoveredlabs.com/blog/how-chatgpt-uses-reciprocal-rank-fusion-for-ai-citations) - Covers ChatGPT's hybrid BM25 + vector retrieval pipeline fused via Reciprocal Rank Fusion with the scoring formula `1/(rank+k)` at `k=60`, including extracted code-level parameters (`rrf_alpha: 1`, `rrf_input_threshold: 0`) and a worked numerical example showing consistent mid-list presence (score 0.0462) outperforms a #1 ranking that appears in only one retrieval method (0.0164) - the clearest existing explanation of why retrieval consistency across methods matters more than raw authority for citation selection.

- [How Google AI Overviews Works](https://discoveredlabs.com/blog/how-google-ai-overviews-works) - Reverse-engineered five-phase pipeline (495ms initial HTML, async `/folsrch` endpoint, 200ms server-side retrieval, 1,200ms LLM synthesis, HTML fragment injection) with `mlro`/`mrc` routing token analysis; passage-level retrieval is scored independently of organic rank - a page's position in blue-link results does not predict AI Overview citation probability.

- [How Google AI Mode Works](https://discoveredlabs.com/blog/how-google-ai-mode-works) - Documents 816 simultaneous active experiments, five distinct backend routing paths, and a 6.5-second average response, showing why content must survive multiple routing configurations - not a single retrieval pass - to be reliably cited across AI Mode sessions.

- [How Gemini Works: AI Agent Architecture Deep Dive](https://discoveredlabs.com/blog/how-gemini-works-ai-agent-architecture-deepdive) - Protocol-level analysis of Gemini's BatchExecute RPC system cataloging 227 RPC calls across 28 methods per session, 138 active feature flags, six query routing modes (Default, Agent, Deep Research, Deep Think, Canvas, Gempix), and 161 long-polling Signaler connections - documents how structured data and entity relationships directly influence citation decisions at the session level.

- [Google AI Overviews: Official Indexing and Control Documentation](https://developers.google.com/search/docs/appearance/ai-overviews) - Google's canonical reference establishing that no special schema.org markup exists beyond standard Search eligibility and that exclusion is controlled via `robots.txt`, `nosnippet`, `data-nosnippet`, and `max-snippet` - authoritative on the absence of a privileged technical channel for AI Overview inclusion.

- [How AI Search Grounding Actually Works: Google vs OpenAI vs Anthropic](https://dejan.ai/blog/grounding/) - Direct comparison of grounding behavior across three platforms on the same query: Google retrieves 7 pages and cites all 7 (1:1 ratio), OpenAI retrieves 39 but cites only 2 (20:1 filtering), Anthropic reads 14 and cites 9 with visibility into 5 rejected sources; each wraps snippets differently (redirect-wrapped readable URLs, 100-200 word sliding windows, encrypted snippets requiring a second pass); establishes that "AI mentioned us" is only meaningful within a specific platform's citation architecture.

- [How Search Grounding Biased an LLM Against YouTube](https://dejan.ai/blog/how-search-grounding-biased-an-llm-against-youtube/) - Documents a case where grounding pulled from affiliate-driven vendor blogs rather than primary documentation, causing Claude to rank paid webinar platforms above the free incumbent; the mechanism: "retrieval does not correct the model's prior - it confirms it"; directly implies that brand presence in affiliate-independent, primary documentation is a stronger long-term AEO asset than presence in review aggregators.

- [LLM Knowledge Cutoff Dates: Every Major Model Fact-Checked](https://metehan.ai/articles/llm-knowledge-cutoff-dates) - Verified cutoff dates across GPT-4o (Oct 2023), GPT-5 variants (Sep 2024–Dec 2025), Claude Fable 5 and Opus 4.8 (Jan 2026), Gemini 3.5 Flash (Jan 2025 - stale despite May 2026 release), Qwen, Kimi, and MiniMax; the authoritative quick-reference for understanding which topics fall inside vs. outside parametric knowledge, directly determining where live-web AEO has leverage vs. where training data coverage makes retrieval unlikely.

- [Perplexity API Documentation](https://docs.perplexity.ai) - Covers the Agent API (third-party model access with web search tools, JSON schema structured responses, domain filtering), Search API (real-time ranked results with `search_domain_filter` and `search_recency_filter` parameters), and Embeddings API - the practical integration reference for engineers querying Perplexity in RAG pipelines or testing citation behavior programmatically.

- [LiveBrowseComp: Are Search Agents Searching, or Just Verifying?](https://arxiv.org/abs/2605.28721) - AI search agents answer ~45% of questions without triggering a search (Fan et al., 2026); queries are internally generated from model hypotheses rather than user-supplied queries, meaning content must match the model's natural framing of a topic - optimizing for the language a model would use to query your subject area matters as much as keyword targeting.

- [Navigating Uncertainty: Optimizing API Dependency for Hallucination Reduction](https://arxiv.org/abs/2401.01780) - LLMs trigger external web retrieval for only 62% of queries (Erbacher et al., 2024); topics well-covered in training data bypass live retrieval entirely, confining live-web AEO leverage to niche, emerging, and fast-changing domains where parametric knowledge is insufficient.

- [Uncertainty-Aware Web-Conditioned Scientific Fact-Checking](https://arxiv.org/abs/2604.11036) - Retrieval fires only when internal confidence on a specific atomic claim is insufficient (Vinod and Erk, 2026); to be cited, content must address the exact gap in the LLM's parametric knowledge - granular, verifiable claims on under-trained topics maximize citation probability in production retrieval pipelines.

- [How ChatGPT Shopping Works and Where Ads Fit In](https://discoveredlabs.com/blog/how-chatgpt-shopping-works-and-where-ads-fit-in) - Traffic inspection of 1.1 million product tracking parameters reverse-engineers ChatGPT's shopping infrastructure: the "Sonic" client-side query classifier (0.1ms execution), `connector_openai_shopping` architecture, Google Shopping API integration with `srsltid=AfmBOo*` tracking markers, and product ranking factors; documents zero current advertising infrastructure with full technical insertion points mapped for future monetization.

- [How Google AI Overviews Ads Work Today and Where They're Heading](https://discoveredlabs.com/blog/how-google-ai-overviews-ads-work-today-and-where-theyre-heading) - Network traffic analysis revealing AI Overview ad architecture: AI content renders 7ms before ad tracking fires, a unified `adview_query_id` links impressions to specific queries, and separate slot systems (`SLOT_TYPE_ABOVE_FEED` vs `SLOT_TYPE_AD_BREAK_REQUEST`) currently exclude ads from AI-generated content while staging infrastructure for future monetization.

- [How Google AI Mode Ads Work Today](https://discoveredlabs.com/blog/how-google-ai-mode-ads-work-today-and-what-they-might-look-like-tomorrow) - Analysis of 547 network flows from AI Mode sessions: ad auctions run and complete in the background within 60 milliseconds despite ads not yet displaying; documents AsyncDataService timing patterns, `adview_query_id` attribution chains, and shopping ad code patterns confirming the full monetization infrastructure is built and awaiting activation.

- [How Gemini Ads Work Today](https://discoveredlabs.com/blog/how-gemini-ads-work-today-and-what-they-look-like-when-activated) - Eight-layer traffic inspection revealing five dormant advertising SDKs (qads, ada, adrc, qapid, adcgm3) calling Google's OneGoogle Ads RPC endpoint and returning null responses; documents Shopify and Google Merchant Center commerce integrations alongside 5,000+ feature flags providing granular ad rollout control.

---
