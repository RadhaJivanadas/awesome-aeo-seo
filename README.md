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

## Research & Papers

Controlled experiments, large-scale audits, and empirical studies on AI citation behavior. Methodology notes are included where sample size or experimental design affects how far findings generalize.

### Citation Drivers & Content Signals

- [What Gets Cited: Competitive GEO in AI Answer Engines](https://arxiv.org/abs/2605.25517) - 252,000-trial controlled experiment across six LLMs evaluating 18 content factors in head-to-head source competition (Vishwakarma et al., 2026); topical relevance and list position dominate, explicit pricing and recent timestamps provide consistent marginal lift, and formatting changes alone have minimal impact - directly falsifying a common GEO assumption.

- [Discovered Labs: What Drives AI Citations](https://discoveredlabs.com/research/what-drives-ai-citations) - ~2 million prompt-engine-citation data points across 10,000 cited URLs with 60+ attributes per page and nine robustness validation checks; prompt-content alignment is the dominant signal (β=+0.37, ~3× larger than competing signals), domain authority SHAP 0.38 vs. 0.06 for the strongest page-level feature, FAQ sections and TLDR blocks are positive, listicle reviews are negative, Core Web Vitals and schema show no independent citation signal, and engine recency preferences diverge: Claude cites median 5.1-month content, ChatGPT cites median 8.0-month content.

- [From Citation Selection to Citation Absorption](https://arxiv.org/abs/2604.25707) - Distinguishes being cited from having cited content actually influence the generated answer (Zhang et al., 2026); across 21,000+ citations on ChatGPT, Gemini, and Perplexity, ChatGPT cites fewer sources but absorbs more per citation, while high-absorption pages share longer content, strong structural hierarchy, and rich evidence including definitions, numerical facts, and comparisons.

- [From Experience to Skill: MAGEO](https://arxiv.org/abs/2604.19516) - Multi-agent framework that learns and reuses engine-specific editing strategies across documents rather than optimizing each instance from scratch (Wu et al., ACL 2026); introduces the DSV-CF metric jointly measuring semantic visibility and citation fidelity, revealing that pure visibility gains can hurt attribution accuracy - a finding absent from single-metric GEO benchmarks.

- [Authority Signals in AI Cited Health Sources](https://arxiv.org/abs/2601.17109) - Author credentials, institutional affiliation, peer review signals, and digital authority signals (domain trust, backlinks) each independently influence ChatGPT citation selection (Jacques et al., 2026); the authority signal framework generalizes to any domain where LLMs act as trust proxies, providing an actionable practitioner checklist.

- [Algorithmic Trust and Compliance in Regulated Industries](https://arxiv.org/abs/2603.12282) - In regulated sectors, compliance signals (regulatory licenses, standards body certifications) function as authority multipliers that LLMs treat as trust proxies (Oruesagasti, 2026); verifiable regulatory compliance displayed on-page is a high-leverage AEO signal in financial services, healthcare, and adjacent domains.

### Scale Audits & Brand Visibility

- [Generative Engine Optimization at Scale: Measuring Brand Visibility](https://arxiv.org/abs/2606.20065) - 100K+ AI search responses across 100+ brands mapping a three-tier visibility hierarchy (Kumar, 2026); best-of listicles account for ~21% of citations (highest of any format), YouTube and Reddit dominate non-corporate cited sources, brand maturity strongly predicts visibility tier, and sentiment framing fluctuates 6.7× more than mention frequency.

- [AI Brand Authority Index: 2.9 Million Brands by Associative Embeddedness in Gemini](https://dejan.ai/blog/brands/) - Graph-based brand visibility study: 200,000 recall surveys seed 5,892 brands, association queries grow the graph to 2.9 million nodes, Personalized PageRank with seed-weighted teleportation computes authority scores; key finding is that non-seed brands (not directly recalled) rank highest through "dense intersections of associations from high-authority seeds" rather than direct recall - establishing network centrality in brand association graphs as a measurable AEO signal distinct from direct name recall.

- [Prominence-Stratified Failure Modes](https://arxiv.org/abs/2605.27439) - 37,000-run audit across 215 commercial queries and 533 brands (Jack et al., 2026) showing L4/L5 brands are invisible in 48–52% of all runs while L1 category leaders win only 25–41% of available recommendation slots - optimization strategy must be calibrated to brand tier, not applied uniformly.

- [Generative Engine Optimization: How to Dominate AI Search](https://arxiv.org/abs/2509.08919) - Systematic large-scale study documenting an overwhelming earned-media bias across ChatGPT, Perplexity, and Gemini (Chen et al., 2025); third-party authoritative mentions far outperform owned content, and engine-specific and language-specific strategies are necessary because no single cross-platform approach transfers reliably.

- [The Discovery Gap: Product Hunt Startups in LLM Discovery](https://arxiv.org/abs/2601.00912) - LLMs recognize products by name at 99% but surface them in organic discovery queries at only 3–8% (Sharma, 2026); traditional SEO signals - referring domains and community rank - predicted Perplexity visibility better than any AI-specific tactic, making SEO foundation a prerequisite for LLM discoverability rather than an alternative to it.

### Causal Inference & Methodology

- [Disentangling AEO from Platform Growth](https://arxiv.org/abs/2606.04362) - Natural experiment using untreated pages as controls isolates true causal AEO effect from ChatGPT's underlying platform growth (Watanabe and Nakayashiki, 2026); genuine AEO lift was 1.82× ChatGPT referral (95% CI: 1.31–2.54, p=0.001) while site-wide ChatGPT traffic grew 5.7× from platform expansion alone - the canonical methodological warning for AEO attribution.

- [Quantifying Uncertainty in AI Visibility](https://arxiv.org/abs/2603.08924) - AI search citation distributions follow power laws with high variance, making single-run visibility metrics statistically unreliable for decision-making (Sielinski, 2026); bootstrap confidence intervals show many apparent performance differences fall within measurement noise, with minimum sample size guidance published for Perplexity, SearchGPT, and Gemini.

### Evaluation Frameworks and Methodology

- [HELM: Holistic Evaluation of Language Models](https://github.com/stanford-crfm/helm) - Stanford CRFM's open evaluation framework covering 100+ scenarios and 50+ metrics with calibrated uncertainty estimates; the methodological template for longitudinal citation rate studies and controlled AEO experiments requiring statistical rigor beyond single-run visibility scores.

- [A Survey of Cross-Lingual Word Embedding Models](https://arxiv.org/abs/1706.04902) - Sebastian Ruder's foundational survey of cross-lingual representation learning (Ruder et al., 2017); the methodology reference for researchers studying how citation probability varies across languages or regional engine deployments - the largest empirical gap in the current GEO literature.

- [Survey of Hallucination in Natural Language Generation](https://arxiv.org/abs/2202.03629) - Comprehensive taxonomy of hallucination types, detection methods, and mitigation strategies (Ji et al., 2022); explains the parametric knowledge threshold that determines when LLMs bypass live retrieval entirely, making this the mechanistic basis for targeting knowledge-gap content in AEO strategy.

- [Lil'Log: Extrinsic Hallucination in LLMs](https://lilianweng.github.io/posts/2024-07-07-hallucination/) - Lilian Weng's technical analysis of hallucination in production retrieval systems covering attribution failure modes, grounding methods, and retrieval-augmented mitigation patterns; directly explains the citation decay dynamics that occur as engine retrieval models update and parametric coverage of a topic expands.

### Emerging Optimization Surfaces

- [ALAS: Autonomous Learning Agent for Self-Updating Language Models](https://arxiv.org/abs/2508.15805) - Autonomous pipeline continuously fine-tuning LLMs on post-cutoff web content raises post-cutoff accuracy from 15% to 90% on evolving topics (Atreja, 2025); appearing as a cited source in continuous-learning pipelines - not just in live retrieval - is an emerging AEO surface where high-quality, stable, citable content gets embedded into model weights over time.

---

## AEO & GEO Fundamentals

Conceptual and architectural grounding for why AI search is different from keyword-based search. Read this section before the tooling and tactics sections.

- [GEO: Generative Engine Optimization](https://arxiv.org/abs/2311.09735) - The paper (Aggarwal et al., KDD 2024) that named the field and established the vocabulary: nine optimization strategies evaluated against the GEO-bench dataset showing up to 40% visibility gains, with statistics and cited sources performing best overall and domain-specific variation determining which strategies apply where.

- [C-SEO Bench: Does Conversational SEO Work?](https://arxiv.org/abs/2506.11097) - First systematic benchmark of conversational SEO methods across domains and competitive saturation scenarios (Puerto et al., NeurIPS 2025); most C-SEO methods are ineffective or actively harmful to ranking, and gains collapse to zero-sum as competitors converge on the same tactics - the required methodological counterweight before investing in any specific GEO tactic.

- [Is AEO Different to SEO, or Is It All One Big Grift?](https://discoveredlabs.com/blog/is-aeo-geo-different-to-seo) - Makes the architectural case that the distinction is real based on retrieval and synthesis mechanics rather than ranking mechanics, explaining why the same page can rank #1 organically while being invisible to AI Overviews.

- [Position: GEO Creates Underexamined Risks](https://arxiv.org/abs/2606.12439) - Formalizes three structural governance risks (Wen et al., ICML 2026 position track): concentrated influence from retrieval systems favoring established players, undisclosed commercial embedding in AI answers, and academic-industry measurement misalignment where controlled-setting benchmarks understate production deployment risks.

- [Retrieval-Augmented Generation for Large Language Models: A Survey](https://arxiv.org/abs/2312.10997) - Comprehensive survey of Naive, Advanced, and Modular RAG paradigms (Gao et al., 2023) covering chunking strategy, embedding similarity, reranking, and prompt injection - directly explains which content properties get a document retrieved and which get filtered before reaching the LLM context window.

- [Great Disappearance Acts: Generative Search and Shadow Banning](https://arxiv.org/abs/2606.11216) - Structural argument that AI search creates zero-click at a deeper level than featured snippets (Friedmann, 2026) - content is consumed by the synthesis layer without attribution traffic - shifting the appropriate success metric from referral visits to brand impression and citation presence.

- [Commercialized Generative AI: Ethics of Native Advertising in Conversational Search](https://arxiv.org/abs/2310.04892) - Demonstrates that LLMs can blend sponsored content into AI-generated answers indistinguishably from organic responses (Zelch et al., 2023); as paid inclusion enters AI answers, organic authority signals - earned media, established backlink profiles, factual recurrence - become the primary differentiators between paid and unpaid AI visibility.

---
