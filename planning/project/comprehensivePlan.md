# B1: Corporate Earnings → Macroeconomic Surveillance

## Full Project Plan

---

## 1. The Business Problem

A central bank economist, a macro strategist at an asset manager, or a policy analyst at a government ministry wants to answer questions like:

- Is consumer demand strengthening or weakening across the economy right now — before GDP is published?
- Are supply chain disruptions easing or tightening, and which sectors are most affected?
- What are firms collectively saying about hiring plans, investment intentions, and inflation expectations?
- Do these text-based signals lead official statistics by one or two quarters — and can they serve as early warning indicators?

Today, answering these questions from corporate text data requires either:

- **Commercial NLP providers** (NL Analytics, used by the ECB and IMF) charging institutional-level fees for pre-processed text-based indicators across thousands of firms and dozens of countries
- **Premium news databases** (Factiva, used by Bank of Canada researchers; Refinitiv, used by academic teams) costing university or institutional subscriptions — $10K–50K+/year
- **Internal central bank resources** — the RBA, for example, uses confidential business liaison data that is not available outside the bank at all

The methodology for constructing macroeconomic indicators from corporate text is well-documented. Hassan et al. (2019, QJE) developed the keyword-in-context technique — matching topic keywords in earnings call transcripts and scoring sentiment in a ±10-word context window — originally for measuring firm-level political risk. Ruch & Taskin (2024, Oxford Bulletin of Economics and Statistics) adapted this approach to macroeconomic demand and supply topics, validating against GDP and supply chain pressure indices. The ECB published Economic Bulletin analyses in 2023, 2024, and 2025 applying similar methods at scale. The RBA published a working paper applying BERTopic to business liaison text (Gray et al., 2025). The ECB blogged about this field as recently as February 16, 2026.

But every published implementation uses proprietary data. No one has built an open-source, reproducible version using freely available data. The methodology is public; the pipeline is locked behind paywalls.

This project bridges that gap: replicate the ECB's published macroeconomic surveillance methodology using free earnings call transcripts from the FMP API, validate the constructed indicators against official statistics from FRED, Eurostat, and the ECB Data Portal, and test whether LLM-based extraction improves on the keyword approach central banks currently use.

**Resume line (draft):** "Replicated ECB macroeconomic surveillance methodology using free data: constructed text-based economic indicators from 9,000+ earnings call transcripts across 20 quarters and two geographies, achieving [r=0.XX] correlation with GDP growth and [X]-quarter leading signal. Compared keyword extraction (central bank standard) against LLM-based extraction. Deployed as interactive Streamlit dashboard with FRED/Eurostat integration."

---

## 2. Why This Problem, For This Portfolio

This project is selected for specific strategic reasons:

**Domain alignment with target roles.** Revolut operates in a macro-sensitive business — consumer lending, currency exchange, and savings products are all directly affected by economic conditions. Clarity AI monitors economic sustainability metrics. Any role involving economic analysis, risk assessment, or policy intelligence benefits from demonstrated ability to construct and validate macro indicators. This project signals: "I can build data-driven tools that monitor the economy."

**Fills a unique portfolio gap.** The existing portfolio covers gaming/entertainment (Steam), retail/CPG (Private Label), and EU regulation (L1). B1 adds macroeconomics as a domain, time series analysis and econometric validation as techniques, and financial API integration + official statistics APIs as data sources. None of these are covered by any other portfolio project. This is breadth, not redundancy.

**Reproducibility study framing is rare and valued.** Most portfolio projects build something new. This one replicates published institutional research with open tools — a category that demonstrates scientific rigour, methodological understanding, and the ability to read and implement academic papers. The framing "can free data replicate what central banks build with commercial providers?" is a question with a concrete, testable answer.

**Ties to the Private Label project indirectly.** The Private Label project analyses consumer product markets. B1's demand and supply indicators capture the macroeconomic backdrop that drives those markets — consumer confidence, supply chain conditions, input cost pressures. A hiring manager reviewing both projects sees someone who thinks at multiple levels: product-level market analysis AND economy-level trend monitoring.

**The comparison between keyword and LLM extraction is the novel methodological contribution.** Central banks use keyword matching because it's the established methodology — developed by Hassan et al. (2019) for political risk, adapted by Ruch & Taskin (2024) for macro topics — and because it scales to thousands of firms cheaply. This project can afford LLM extraction on 400–500 firms per quarter. The question "does an LLM actually improve macro indicator quality over the Hassan/Ruch-Taskin keyword approach?" has not been answered in the published literature. Whether the answer is yes or no, it's a finding.

**Verifiable outputs.** Unlike a chatbot or recommendation engine where quality is partly subjective, macroeconomic indicators are validated against official statistics. Either the text-based demand indicator correlates with GDP growth, or it doesn't. Correlation coefficients, lead/lag relationships, and Granger causality tests are concrete, reproducible, and credible.

---

## 3. Novelty Assessment

### What already exists

| Project / Publication | What it does | How B1 differs |
|---|---|---|
| **ECB Economic Bulletin boxes** (2023, 2024, 2025) | Published analysis showing text-based demand/supply indicators track GDP. Uses NL Analytics (~14,000 firms, 82 countries). No code. | B1 is open-source, reproducible, uses free data (FMP API). Adds LLM comparison. |
| **Ruch & Taskin (2024), Oxford Bull. Econ. Stat.** | Formalised methodology: keyword clusters → sentiment scoring → macro indicators. Uses Factiva (commercial). | B1 replicates their methodology with free APIs. Provides code. Adds LLM extraction as comparison. |
| **Gosselin & Taskin (2023), Bank of Canada** | Staff discussion paper applying similar methodology to Canadian firms. Factiva data. | Canadian context, proprietary data. B1 is US + Europe, open-source. |
| **IMF WEO Chapter 2 (Oct 2023)** | Uses NL Analytics for inflation expectations from corporate text. | Inflation expectations only. NL Analytics data. B1 covers 6 topic dimensions. |
| **Gray et al. (2025, RBA)** | Applies BERTopic to RBA business liaison data to construct macro indicators. | Confidential internal data (business liaison notes). Different text source entirely. B1 uses publicly downloadable earnings call transcripts. |
| **ECB blog (Feb 16, 2026)** | Blog post discussing text-based macro indicators, referencing RBA work. No code, no data. | B1 is the open-source implementation the blog implies should exist. |
| **Hassan, Hollander, van Lent & Tahoun (2019, QJE) — firmlevelrisk.com** | Keyword-in-context methodology (±10 word window, Loughran-McDonald sentiment) applied to earnings calls for 11,000+ firms in 81 countries. Measures **political risk**, not macro demand/supply. Replication code on Harvard Dataverse. Published dataset. | Same keyword-in-context engine, but **different topic dimensions** entirely. Hassan et al. measure political risk exposure per firm. B1 adapts the methodology (following Ruch & Taskin's extension) to **macroeconomic** demand/supply/inflation topics, then **aggregates to economy-wide indicators** validated against GDP — a step Hassan et al. never take. We cite Hassan et al. as the methodological origin of the keyword-in-context approach that Ruch & Taskin later applied to macro topics. |
| **Dozens of "earnings call NLP" GitHub repos** | Company-level extraction: sentiment per firm, guidance extraction, stock signal. | Fundamentally different unit of analysis. These ask "what did Company X say?" B1 asks "what are 500 companies collectively saying about the economy?" None aggregate to macro indicators. None validate against GDP/PMI. |

### What does NOT exist (confirmed via GitHub, Kaggle, Medium, Google Scholar, arXiv search)

- **Zero** GitHub repos constructing macroeconomic indicators from aggregate earnings call text
- **Zero** open-source implementations of the ECB/Ruch-Taskin keyword methodology
- **Zero** portfolio projects with the "reproducibility study of central bank methodology" framing
- **Zero** comparisons of keyword vs. LLM extraction for macro indicator quality
- **Zero** studies on "minimum firm count needed for reliable text-based macro indicator"

### Why the gap persists

The methodological lineage is clear: Hassan et al. (2019) developed the keyword-in-context approach for political risk → Ruch & Taskin (2024) adapted it to macroeconomic demand/supply topics → the ECB operationalised it for real-time economic monitoring. But at every stage, the implementation depends on commercial data. Hassan et al. use Factiva transcripts. Ruch & Taskin use Factiva. The ECB uses NL Analytics (commercial, institutional pricing). Academics use Factiva (university subscription). The methodology is published but the data pipeline is locked behind paywalls. FMP's free tier — 250 requests/day, 8,000+ US companies, earnings call transcripts as structured JSON — creates the first practical opportunity to replicate this with open tools.

### Our differentiation (4 layers)

1. **Open-source replication of institutional methodology:** Published ECB/Ruch-Taskin approach, implemented with free data and open code.
2. **Two-geography design:** US as robust proof-of-concept (400+ firms), Europe as generalisability test (60–100 firms) — the comparison itself is a contribution.
3. **Keyword vs. LLM extraction comparison:** Novel methodological question with practical implications for anyone building text-based economic indicators.
4. **Sample-size sensitivity analysis:** "How many firms do you need for a reliable text-based macro indicator?" — directly useful, never published.

---

## 4. Data Sources

### Primary: Earnings Call Transcripts (FMP API)

**Access method:** Financial Modeling Prep (FMP) REST API, free tier.
- **Rate limit:** 250 requests/day
- **Bandwidth limit:** 500 MB / 30 days (free tier). A transcript JSON response is typically 50–200 KB depending on call length (~100 KB median for S&P 500 companies). For the 8Q core subset: 3,200 transcripts × 100 KB = ~320 MB (fits in one 30-day window). For the full 20Q corpus: ~9,000 transcripts × 100 KB = ~900 MB (requires phasing downloads across 2+ months, which the phased timeline already accommodates). Longer calls (2+ hours, common for large banks) can produce 200–300 KB responses. **Mitigation:** Download in monthly batches, monitor cumulative bandwidth, deprioritise the longest transcripts if approaching the monthly limit.
- **Coverage:** 8,000+ US publicly traded companies; European companies available via ADR/US listing tickers
- **Format:** JSON with full transcript text, separated by speaker
- **Historical depth:** 10+ years for major companies
- **Python access:** Simple `requests` calls; `fmpsdk` package available but thin

**Alternative transcript sources (tested, documented as backup):**

| Source | Coverage | Free tier | Rate limit | Notes |
|---|---|---|---|---|
| **Finnhub** | US, UK, European, Australian, Canadian companies | 60 calls/minute | Very generous | Full transcript endpoint documented as "US companies only" in SDK, but metadata endpoint covers European companies. Coverage claims conflict across documentation — **requires empirical testing during pre-commitment.** If Finnhub covers transcripts broadly, it eliminates the FMP rate-limit bottleneck entirely (the full 9,000+ transcript corpus in hours, not weeks). |
| **API Ninjas** | 8,000+ companies (US focus) | Free tier available | Not documented | Returns transcript as single string (no speaker separation). Includes summary and guidance fields. Useful backup if FMP quality is poor. |
| **EarningsCall.biz** | 5,000+ companies | 2 companies free (AAPL, MSFT) | Paid for broader access | Good Python library (`earningscall`), supports speaker separation and word timestamps. Not viable for free bulk download. |

**Decision:** FMP is the primary source because its free tier covers the required volume with adequate bandwidth. Finnhub is tested during pre-commitment as a potential accelerator (especially for European transcripts). API Ninjas is a documented fallback. The plan works with FMP alone; alternatives are risk mitigation.

**Scope for this project — two geographies:**

| Geography | Companies | Quarters | Transcripts | API days needed |
|---|---|---|---|---|
| **US (primary)** | ~400–500 S&P 500 constituents | 20 (Q1 2020 – Q4 2024) | ~8,000–10,000 | ~32–40 days |
| **Europe (secondary)** | ~60–100 ADR-listed European large caps | Same | 1,200–2,000 | ~5–8 days |
| **Total** | | | **~9,200–12,000** | **~37–48 days** |

**⚠️ Revised timeline:** The 20-quarter, full-corpus download takes 5–7 weeks at 250 requests/day. This makes the download pipeline the project's critical path. **Mitigation:** Start downloading immediately during pre-commitment and continue through all project phases. The download runs as a background job while coding proceeds on already-downloaded data. Additionally, Finnhub (if European transcript access is confirmed during pre-commitment) could dramatically accelerate the timeline — 60 calls/minute would complete the same volume in hours.

**Phased download strategy:**
1. **Immediate (pre-commitment + Week 1):** Download 8 core quarters (Q1 2022 – Q4 2023) for both geographies = ~3,700–4,800 transcripts = ~15–19 days. This provides enough data to build and validate all pipelines.
2. **Background (Weeks 2–5):** Continue downloading remaining 12 quarters (Q1 2020 – Q1 2022, Q1 2024 – Q4 2024) while development proceeds. Data arrives incrementally and is integrated as it becomes available.
3. **Completion:** Full 20-quarter corpus is complete by Week 4–5, enabling final validation with the full time series.

**Why 20 quarters (not 8) as the primary plan:** With n=8 quarterly observations, the critical Pearson r for statistical significance at p<0.05 (two-tailed) is approximately 0.71 — even a correlation of r=0.6 would not be significant. This severely limits what can be claimed from the analysis. With n=20, the critical r drops to approximately 0.44, making correlations of r=0.5+ statistically meaningful. Granger causality tests also require adequate degrees of freedom. The 20-quarter window (Q1 2020 – Q4 2024) spans the COVID crash, post-COVID recovery, the 2022 inflation shock, rate-hiking cycle, and normalisation — providing rich macroeconomic variation for the indicators to capture. The 8-quarter core subset (Q1 2022 – Q4 2023) serves as the development/debugging dataset, not the final evaluation period.

**European company universe (expected):**

| Sector | Companies (ADR tickers) | Count |
|---|---|---|
| Tech/Industrial | ASML, SAP, Siemens (SIEGY), Infineon (IFNNY), Philips (PHG), Nokia (NOK), Ericsson (ERIC), ABB | ~8 |
| Financials | ING, Deutsche Bank (DB), UBS, BNP (BNPQY), Barclays (BCS), HSBC | ~6 |
| Consumer/Luxury | Nestlé (NSRGY), Unilever (UL), LVMH (LVMUY), Diageo (DEO), AB InBev (BUD), Danone (DANOY) | ~6 |
| Energy/Materials | Shell (SHEL), TotalEnergies (TTE), BP, ArcelorMittal (MT), Rio Tinto (RIO) | ~5 |
| Pharma/Health | AstraZeneca (AZN), Novartis (NVS), Novo Nordisk (NVO), GSK, Sanofi (SNY), Roche (RHHBY) | ~6 |
| Telecom/Utilities | Vodafone (VOD), Enel (ENLAY), Iberdrola (IBDRY) | ~3 |
| **Known total** | | **~34** |
| **Expected total (incl. smaller ADRs)** | | **60–100** |

**⚠️ Empirical verification required:** The exact European count needs testing against the FMP API before commitment (see Section 6, Q1).

### Secondary: Official Economic Statistics (Validation Targets)

All free, all with Python API access:

| Source | Statistics | Access | Python package |
|---|---|---|---|
| **FRED** (Federal Reserve Bank of St. Louis) | US GDP growth, unemployment, industrial production, CPI, consumer sentiment, ISM Manufacturing PMI (series NAPM) | Free API key | `fredapi` |
| **Eurostat** | Euro area GDP, employment, industrial production, business confidence | Free, no key needed | `eurostat` |
| **ECB Data Portal** | HICP inflation, bank lending surveys, financial conditions index | Free, no key needed | `ecbdata` / `sdmx` |
| **European Commission** | Economic Sentiment Indicator (ESI), sector confidence indices | Free download | CSV/Excel |

**Why multiple validation targets matter:** A text-based demand indicator that correlates with GDP growth could be spurious. If it also correlates with ISM PMI (different methodology, monthly vs. quarterly) and consumer sentiment (survey-based), the signal is more credible. Each validation target tests a different aspect of the indicator.

### Tertiary: Reference Lexicons

| Resource | Purpose | Access |
|---|---|---|
| **Loughran-McDonald Financial Sentiment Lexicon** | Domain-specific positive/negative word lists for financial text. Standard in financial NLP since 2011. Used in ±10-word context windows following Hassan et al. (2019). | Free download from Notre Dame |
| **ECB published keyword clusters** | Demand, supply, inflation, hiring, investment, financing keyword lists from ECB Economic Bulletin boxes | Extracted from published ECB analyses (free) |
| **Hassan et al. (2019) replication data** | Political risk keyword lists, methodology documentation, pre-computed firm-level indices. Available on Harvard Dataverse. Useful as methodological reference for the keyword-in-context approach, though the topic dimensions differ. | Harvard Dataverse (free) / firmlevelrisk.com |

---

## 5. What the System Builds

### Architecture Overview

```
┌─────────────────────────────────────────────────────────────┐
│              1. Transcript Acquisition Pipeline              │
│                                                             │
│  FMP API → raw JSON → cleaned text                          │
│  Per transcript:                                            │
│    - Separate prepared remarks from Q&A                     │
│    - Extract speaker roles (CEO, CFO, analyst)              │
│    - Metadata: ticker, GICS sector, HQ country, quarter     │
│  Rate-limited: 250/day, batched over ~15–19 days            │
│  Output: data/raw/transcripts/{ticker}/{quarter}.json       │
└──────────────────────────┬──────────────────────────────────┘
                           │
                           ▼
┌─────────────────────────────────────────────────────────────┐
│          2A. Keyword Extraction (ECB Baseline)              │
│                                                             │
│  For each transcript, for each topic dimension:             │
│    demand | supply | inflation | hiring | investment |      │
│    financing                                                │
│                                                             │
│  1. Find keyword hits from ECB-derived clusters             │
│  2. Score ±10-word context window with Loughran-McDonald     │
│     (r=10 matches Hassan et al. 2019 / Ruch & Taskin 2024)  │
│  3. Compute per-firm topic sentiment score                  │
│                                                             │
│  No LLM. Pure text processing. Deterministic.               │
│  This is the approach central banks actually use.           │
└──────────────────────────┬──────────────────────────────────┘
                           │
                           ▼
┌─────────────────────────────────────────────────────────────┐
│          2B. LLM Structured Extraction (Comparison)         │
│                                                             │
│  For each transcript (or sampled subset), extract:          │
│  {                                                          │
│    "company": "AAPL",                                       │
│    "quarter": "Q3 2023",                                    │
│    "signals": [                                             │
│      {                                                      │
│        "topic": "demand",                                   │
│        "direction": "positive",                             │
│        "confidence": 0.85,                                  │
│        "evidence": "iPhone demand exceeded expectations..." │
│      }, ...                                                 │
│    ]                                                        │
│  }                                                          │
│                                                             │
│  Same 6 topic dimensions as Method A.                       │
│  Enables direct comparison: same input, same output schema, │
│  different extraction method.                               │
└──────────────────────────┬──────────────────────────────────┘
                           │
                           ▼
┌─────────────────────────────────────────────────────────────┐
│              3. Aggregation Engine                           │
│                                                             │
│  Per-firm scores → per-sector → economy-wide quarterly      │
│  indices.                                                   │
│                                                             │
│  Aggregation:                                               │
│    - Equally weighted (baseline)                            │
│    - Market-cap weighted (if available, robustness check)   │
│    - Sector-weighted (match GDP sectoral composition)       │
│                                                             │
│  Z-score each index against its historical mean/stdev.      │
│  Output: 6 topic × 2 methods × 2 geographies = 24 time     │
│  series of quarterly indicator values.                      │
└──────────────────────────┬──────────────────────────────────┘
                           │
                           ▼
┌─────────────────────────────────────────────────────────────┐
│              4. Validation Framework                         │
│                                                             │
│  For each constructed indicator:                            │
│    - Correlation with matched official statistic            │
│    - Lead/lag analysis (does text lead GDP by 0/1/2 Q?)     │
│    - Granger causality tests                                │
│    - Sector decomposition                                   │
│    - Visual overlay plots (ECB Economic Bulletin style)     │
│                                                             │
│  For the method comparison:                                 │
│    - Keyword vs. LLM correlation with same target           │
│    - Cost per indicator point (API $ vs. compute time)      │
│                                                             │
│  For sample size sensitivity:                               │
│    - Re-run at N = 25, 50, 100, 200, 400+ firms            │
│    - Correlation degradation curve                          │
│    - Compare against natural European sample (N ≈ 60–100)   │
└──────────────────────────┬──────────────────────────────────┘
                           │
                           ▼
┌─────────────────────────────────────────────────────────────┐
│              5. Output: Dashboard + API                      │
│                                                             │
│  Streamlit dashboard:                                       │
│    - Select geography, topic, method, time range            │
│    - Interactive overlay: text indicator vs. official stat   │
│    - Method comparison panel                                │
│    - Sample size sensitivity curve                          │
│    - Sector drill-down                                      │
│                                                             │
│  FastAPI:                                                   │
│    - GET /indicator?geography=US&topic=demand&method=keyword │
│    - GET /validation?indicator=demand&target=gdp_growth     │
└─────────────────────────────────────────────────────────────┘
```

### Why This Architecture, Not the "Chatbot" or "Company Screener" Alternative

The naive approach — "ask an LLM to summarise what companies are saying about the economy" — is the default in every financial NLP tutorial. We reject it deliberately:

| | Company-level extraction (L2 approach, discarded) | Aggregate macro indicator (B1 approach) |
|---|---|---|
| **Unit of analysis** | Individual firm | The economy |
| **Output** | "Apple said demand was strong" | "Demand sentiment across 450 S&P 500 firms rose 0.8σ above historical mean in Q3" |
| **Validation** | Hard — how do you verify a company summary? | Concrete — correlate with GDP, PMI, employment data |
| **Novelty** | Saturated (dozens of GitHub repos) | Zero existing implementations |
| **Audience** | Equity analysts | Macro strategists, central bankers, policy analysts |
| **Demonstrates** | "I can call an LLM on earnings calls" | "I can construct and validate economic indicators from unstructured text" |

The keyword extraction baseline is included deliberately. A project that only uses an LLM looks like "I called an API." A project that replicates a published central bank methodology with deterministic keyword matching, THEN compares LLM extraction against it, demonstrates understanding of the research landscape and methodological rigour.

### Technology Choices (Justified by the Task)

| Tool | Purpose | Why this tool |
|---|---|---|
| **`requests` + rate limiter** | FMP API transcript download | Simple REST API, no SDK needed. Rate limiting is the engineering challenge. |
| **`pandas` + `numpy`** | Tabular data: firm-quarter scores, aggregation, time series | Standard and appropriate. The data IS tabular after extraction. |
| **Loughran-McDonald lexicon + regex/spaCy** | Keyword extraction + context-window sentiment scoring (r=10 words, matching Hassan et al. 2019 / Ruch & Taskin 2024) | This is what the ECB uses. Replicating their approach means using their tools. |
| **OpenAI/Anthropic API** (Structured Outputs) | LLM extraction (Method B comparison) | Structured JSON output for typed signal extraction. The comparison requires an LLM. |
| **`fredapi` / `eurostat` / `sdmx`** | Download official statistics for validation | Standard Python packages for these APIs. No alternative. |
| **`statsmodels`** | Granger causality, correlation analysis, lead/lag testing | Standard econometrics library. Appropriate for the validation framework. |
| **`matplotlib` / `plotly`** | Time series visualisation (ECB-style overlay plots) | The visual overlay of text indicator vs. official statistic is the key output. |
| **Streamlit** | Interactive dashboard | Standard for portfolio project demos. Appropriate for this use case. |
| **FastAPI** | API endpoint for programmatic access | Lightweight, standard. |
| **Docker** | Reproducible deployment | Standard. |

**What this is NOT:**

- Not a stock prediction project. Validation is against GDP/PMI/employment, never stock prices. The framing is macroeconomic surveillance, not investment signal.
- Not a chatbot. No conversation, no open-ended queries. Input: parameter selection. Output: time series indicators with validation statistics.
- Not a company-level analysis tool. Individual firm signals are intermediate steps, not outputs. The unit of analysis is the economy.
- Not an LLM showcase. The keyword baseline is the primary methodology. The LLM comparison is one experiment, not the centrepiece. If the LLM adds nothing over keywords, that's a finding.

---

## 6. Technical Challenges Worth Documenting

These are the hard problems that make this project portfolio-worthy — not just "I scraped some data and made a chart":

### 6a. Rate-Limited Data Acquisition at Scale

Downloading 9,000–12,000 transcripts (full 20-quarter corpus) at 250 requests/day requires a disciplined pipeline: batched downloading with exponential backoff, progress checkpointing (resume after interruption), deduplication, and metadata tracking. The download phase runs 5–7 weeks — longer than most project phases, making the download pipeline the project's critical path. This is a real data engineering constraint — not a limitation to hide, but a design challenge to document. The phased approach (8Q core data first, remaining 12Q in background) ensures pipeline development is never blocked by data acquisition.

**Approach:** Build an idempotent downloader: tracks what's been fetched in a SQLite manifest, skips already-downloaded transcripts, handles API errors gracefully. Runs as a background job. The manifest doubles as the corpus metadata index.

**Blog-worthy insight:** "I spent two weeks just downloading data — here's the engineering I built to make that reliable."

### 6b. Keyword Cluster Design and Calibration

The ECB publishes keyword clusters for demand, supply, and inflation — but not the full implementation detail. Adapting their published clusters to earnings call text (which differs from news text in tone, vocabulary, and structure) requires calibration:

- Earnings calls use management-speak ("headwinds," "tailwinds," "run rate," "pipeline") that maps to economic concepts but doesn't match generic keyword lists
- The context window size parameter (r) for sentiment scoring needs validation: the literature standard is r=10 words (Hassan et al. 2019, Ruch & Taskin 2024), but earnings calls may have different sentence structure than the political-risk transcripts where r=10 was calibrated. Testing r=5, 10, 15, 20 and measuring impact on indicator quality is a straightforward sensitivity check.
- Some keywords are ambiguous: "demand" in "we see strong demand" is positive, but "demand" in "demand for regulatory compliance" is irrelevant

**Approach:** Start with ECB published clusters and r=10 (literature standard). Manually review 50 keyword hits to validate window size and add domain-specific synonyms. Test r=5, 10, 15, 20 — report which produces the strongest correlation with official statistics. Document every cluster modification with rationale.

**Blog-worthy insight:** "Translating central bank keyword lists to earnings call language — what 'demand' actually means in a CEO's prepared remarks."

### 6c. Aggregation Methodology

Moving from per-firm scores to an economy-wide indicator requires aggregation choices with measurable consequences:

- **Equal weighting** treats Apple and a small-cap retailer the same. Simple, transparent, but small firms contribute disproportionately.
- **Market-cap weighting** matches how GDP is constructed (larger firms contribute more to economic output). More defensible, but requires market cap data (available from FMP).
- **Sector weighting** matches the sectoral composition of GDP. The information technology sector is ~30% of S&P 500 market cap but only ~10% of GDP. Adjusting for this could improve correlation with GDP but adds complexity.

**Approach:** Implement all three. Compare correlations. Report which aggregation method produces the strongest validation against official statistics. This comparison is itself a result.

**Blog-worthy insight:** "Does it matter how you weight 500 companies? Surprisingly, yes — and the best method depends on which macro variable you're tracking."

### 6d. The European Coverage Problem

The European indicator uses 60–100 firms available via ADR listings. These are almost exclusively large-cap multinationals — Nestlé, ASML, Shell, LVMH. They are not representative of the European economy in two distinct ways:

**Sample composition bias:** No SMEs, no domestic-only firms, heavy sector skew toward energy, pharma, and luxury goods. This is the obvious problem and is partially addressed by the sample-size sensitivity analysis.

**Content bias (subtler and more important):** ADR-listed European companies report on US calendars, in English, and their earnings calls often emphasise US and global operations. When Nestlé's CEO discusses "strong demand," they may be describing US or Asian markets, not Europe. The "European" macro indicator constructed from ADR transcripts may actually track **global** or **US** conditions better than European domestic conditions. This is testable: if the European text indicator correlates more strongly with US GDP than with Euro area GDP, the content bias dominates. If it correlates more strongly with Euro area GDP, the European signal survives despite the content mixing.

This is not a problem to solve — it's a problem to **measure**. The sample-size sensitivity analysis (subsample the US data to 60–100 firms, measure correlation degradation) quantifies how much signal is lost from the small sample. The European content-bias analysis then adds a second dimension: even at equivalent sample sizes, does the European indicator track its own economy or the US economy?

**Blog-worthy insight:** "How many firms do you need for a reliable text-based macro indicator? I tested this systematically — and discovered that for European ADR companies, the bigger issue isn't sample size but *whose* economy they're actually describing."

### 6e. LLM Extraction at Scale — Cost and Consistency

Running LLM structured extraction on 9,000+ transcripts (full 20Q corpus) is non-trivial:

- Each transcript is 5,000–15,000 words. At ~4 tokens/word, that's 20K–60K input tokens per transcript.
- With a structured output schema requiring 6 topic dimensions, each call produces ~500–1,000 output tokens.
- At Claude Sonnet pricing (~$3/M input, ~$15/M output), processing 9,000 transcripts ≈ $500–1,500 in API costs — prohibitive.
- At GPT-4o-mini pricing (~$0.15/M input, ~$0.60/M output), the same ≈ $25–75 — manageable.

**Approach:** Use a cheaper model (GPT-4o-mini or Haiku) for the bulk extraction. Run a validation sample (100 transcripts) through a stronger model to measure quality degradation. Report the cost-per-indicator-point for each method: "Keyword extraction: $0/quarter. LLM extraction (4o-mini): $X/quarter. LLM extraction (Sonnet): $Y/quarter."

This cost comparison is the kind of production-relevant analysis hiring managers value.

**Optional Method C — FinBERT sentiment (middle ground):** A natural intermediate between dictionary lookup (Method A) and full LLM extraction (Method B) is using FinBERT or a similar domain-specific transformer for sentiment scoring within the keyword context windows. This replaces *only* the Loughran-McDonald sentiment step while keeping the keyword identification from Method A. It's cheap (local inference, no API cost), fast (GPU batch inference), and would give a three-way comparison: dictionary sentiment vs. FinBERT sentiment vs. LLM extraction. This is not essential for the project but adds a talking point about the spectrum from rule-based to ML-based extraction with minimal additional effort (~1 day implementation).

### 6f. Lead/Lag Identification

The central bank value proposition is that text indicators lead official statistics (earnings calls report Q3 results in October; GDP for Q3 is published in January). Measuring this lead requires care:

- With 20 quarterly observations (primary plan), standard cross-correlation and Granger causality tests have reasonable but not overwhelming statistical power. At n=20, we have ~16–18 effective degrees of freedom after lag adjustments — enough for basic Granger tests but not for multivariate controls.
- Stationarity assumptions for Granger causality may not hold over a 5-year window that includes the COVID structural break
- The "lead" may be partly mechanical: corporate reporting calendars are offset from statistical publication calendars. A 1-quarter lead could reflect timing, not information.

**Approach:** Report cross-correlation at lags -2 to +2, Granger causality tests with appropriate caveats about the short series and structural break, and visual overlay plots (which remain the most persuasive output for non-technical reviewers). For the 8-quarter development subset, report only visual comparisons and directional alignment — formal statistical tests are not meaningful at n=8. The 20-quarter full analysis is where quantitative validation claims are grounded.

---

## 7. Evaluation Framework

This is where the project separates from "I scraped some earnings calls and plotted a chart." The evaluation has five layers, each answering a different question:

### Layer 0: Data Quality Audit

**Question:** Is the raw data clean enough to extract meaningful signals?

**Metrics:**
- Transcript completeness: what fraction have full text vs. truncated/missing sections?
- Language quality: what fraction are in English vs. partly in other languages (relevant for European companies)?
- Temporal coverage: for how many company-quarters do we have transcripts? What's the missing-data pattern?
- Speaker separation quality: can we reliably identify CEO/CFO prepared remarks vs. analyst Q&A?

**Presentation:** Summary statistics table + data quality report. Honest about any gaps.

### Layer 1: Keyword Extraction Validation

**Question:** Do the keyword clusters capture economically relevant mentions?

**Method:**
- Manually review 200 keyword hits (across topics and sectors): classify each as relevant/irrelevant/ambiguous
- Compute precision per keyword cluster
- Identify systematic false positive patterns (e.g., "demand" in non-economic contexts)
- Compare sentiment scoring accuracy against manual labels on the same 200 hits

**Target:** Keyword precision > 75% (i.e., at least 3 in 4 keyword hits are genuinely about the economic topic).

### Layer 2: Indicator-Level Validation Against Official Statistics

**Question:** Do the constructed indicators track real macroeconomic variables?

**Metrics (per indicator × per geography):**

| Validation test | What it measures | Statistical tool |
|---|---|---|
| **Pearson/Spearman correlation** | Do the indicator and official statistic move together? | `scipy.stats.pearsonr`, `spearmanr` |
| **Lead/lag correlation** | Does the text indicator lead by 0, 1, or 2 quarters? | Cross-correlation at lags -2 to +2 |
| **Granger causality** | Does the text indicator have predictive information beyond the official stat's own history? | `statsmodels.tsa.stattools.grangercausalitytests` |
| **Visual overlay** | Do the time series look similar? (The most persuasive output for non-technical reviewers.) | `matplotlib` dual-axis plot |

**Indicator × target pairings:**

| Text indicator | Validation target (US) | Validation target (Europe) |
|---|---|---|
| Demand sentiment | GDP growth, ISM PMI, consumer sentiment | GDP growth, ESI, Euro PMI |
| Supply sentiment | ISM supplier deliveries, industrial production | Industrial production, ESI industry component |
| Inflation sentiment | CPI, PCE deflator | HICP |
| Hiring sentiment | Nonfarm payrolls, unemployment rate | Employment growth |
| Investment sentiment | Business fixed investment (GDP component) | Gross fixed capital formation |
| Financing sentiment | Fed funds rate, credit conditions | ECB lending survey, financial conditions |

**Target:** With n=20 quarterly observations, the critical r for significance at p<0.05 (two-tailed) is approximately 0.44. At least 3 of 6 US indicators should achieve |r| > 0.5 with their primary validation target — i.e., not just statistically significant but economically meaningful. European indicators expected weaker due to both smaller sample size and content bias (Section 6d) — the sample-size analysis and content-bias analysis explain why quantitatively.

**Statistical power note:** Ruch & Taskin (2024) report correlation of 0.86 between their supply sentiment index and the Global Supply Chain Pressure Index. The ECB's published charts show strong visual alignment. If the methodology works at all with 400+ S&P 500 firms, |r| > 0.5 for the strongest indicators is a conservative target. If no indicator exceeds this threshold, the finding is that free-tier data at this scale is insufficient — which is itself informative.

### Layer 3: Method Comparison (Keyword vs. LLM)

**Question:** Does LLM extraction produce better macro indicators than keyword matching?

**Method:**
- For each of the 6 topic dimensions, compare the keyword-based indicator's correlation with official statistics against the LLM-based indicator's correlation
- Test statistical significance of the difference (bootstrap confidence intervals on correlation difference)
- Report cost per indicator point for each method
- Report processing time per transcript

**Presentation:** Side-by-side table:

| Topic | Keyword r(GDP) | LLM r(GDP) | Δ | Keyword cost | LLM cost |
|---|---|---|---|---|---|
| Demand | [X] | [Y] | [Z] | $0 | $[N] |
| ... | | | | | |

**Possible outcomes and what they mean:**
- "LLM improves correlation by 0.1+ across topics" → LLM adds genuine value for this task
- "LLM matches keyword within noise" → Keywords are sufficient; the ECB's approach is already good
- "LLM improves on some topics but not others" → Most interesting — identifies WHERE language understanding matters

All three are valid results. The framing is "which method works better and when?" not "prove the LLM is superior."

**Optional extension — FinBERT as Method C:** If time permits, replace the Loughran-McDonald dictionary sentiment step with FinBERT sentiment scoring within the same keyword context windows. This gives a three-way comparison (dictionary → FinBERT → full LLM) along the spectrum from rule-based to ML-based extraction. Low additional effort (~1 day), adds a talking point about the cost-accuracy frontier.

### Layer 4: Sample-Size Sensitivity Analysis

**Question:** How many firms do you need for a reliable text-based macro indicator?

**Method:**
1. Start with full US sample (~400–500 firms per quarter)
2. Randomly subsample to N = 200, 100, 75, 50, 25 firms
3. For each N, re-compute the indicator (keyword method) and its correlation with GDP
4. Repeat each subsample 100 times (bootstrap) to get confidence intervals
5. Plot the degradation curve: N (x-axis) vs. |r| with GDP (y-axis) with 95% CI band
6. Overlay the European sample (N ≈ 60–100) on the same curve

**Presentation:** The key chart of the entire project — a curve showing how indicator reliability degrades with fewer firms, with the European natural sample marked.

**Why this matters:** This has never been published. Central banks work with hundreds or thousands of firms. Smaller institutions, startups, or researchers with limited data budgets need to know: is 50 firms enough? 100? This answers it.

### Layer 5: Failure Mode Analysis

**Question:** When the indicator fails to track official statistics, WHY?

**Categories:**
- **Sector composition mismatch:** S&P 500 sector weights ≠ GDP sector weights. Tech-heavy index may miss manufacturing signal.
- **Large-firm bias:** Only publicly traded firms speak on earnings calls. Misses SMEs, private companies, government sector.
- **Reporting lag heterogeneity:** Not all firms report in the same month within a quarter. Early reporters may not be representative.
- **Keyword false positives:** "Supply" in "money supply" or "supply of new products" doesn't mean supply chains.
- **Sentiment scoring errors:** Loughran-McDonald lexicon misclassifies domain-specific language.
- **LLM extraction inconsistency:** Same phrasing, different LLM classification across runs.
- **Structural breaks:** COVID-19 period (Q1–Q2 2020) may distort all indicators simultaneously. Tested by running analysis with and without these quarters.
- **European content bias:** ADR-listed European firms discussing global/US operations rather than European domestic conditions. Tested by comparing European indicator correlation with Euro area GDP vs. US GDP.
- **Context window sensitivity:** The r=10 word window (from Hassan et al.) may be suboptimal for some macro topic keywords vs. political risk keywords.

**Presentation:** Error breakdown with specific examples. For each failure mode, explain what would fix it (better keywords, more firms, sector reweighting, etc.). This is the section that shows maturity.

---

## 8. Deployment Plan

### Streamlit Dashboard (Primary)

An interactive dashboard with four main views:

**View 1: Indicator Explorer (main view)**
- Select: geography (US / Europe), topic (demand / supply / inflation / hiring / investment / financing), method (keyword / LLM), time range
- Display: dual-axis plot — text indicator (left axis, blue) overlaid on official statistic (right axis, grey)
- Statistics sidebar: correlation coefficient, p-value, optimal lag, Granger causality result
- Below chart: raw data table (quarter, indicator value, official stat value)

**View 2: Method Comparison**
- Side-by-side: keyword indicator vs. LLM indicator for the same topic
- Difference chart: where do the methods diverge most?
- Cost comparison table: $ per indicator point per method

**View 3: Sample Size Sensitivity**
- Interactive curve: drag slider to select N (number of firms)
- Shows correlation with GDP at each sample size, with confidence interval
- European sample marked on the curve
- Annotation: "With [N] firms, the indicator explains [X]% of GDP variation"

**View 4: Sector Drill-Down**
- Decompose the economy-wide indicator by GICS sector
- Show which sectors are driving aggregate sentiment shifts
- Compare sector-level text indicators against sector-specific official statistics (e.g., tech sentiment vs. tech output)

**Design goal:** A hiring manager opens the dashboard, clicks "US → Demand → Keyword," and immediately sees the text-based demand indicator overlaid on GDP growth with a correlation of r = [X]. No configuration, no waiting, no prompt engineering.

### FastAPI Endpoints (Secondary)

```
GET /indicator?geography=US&topic=demand&method=keyword&start=2022Q1&end=2023Q4

→ 200 OK
{
  "indicator": "demand",
  "geography": "US",
  "method": "keyword",
  "time_series": [
    {"quarter": "2022Q1", "value": 0.82, "n_firms": 456},
    {"quarter": "2022Q2", "value": -0.34, "n_firms": 461},
    ...
  ],
  "metadata": {
    "keyword_cluster": ["demand", "consumer spending", "orders", ...],
    "aggregation": "equal_weight",
    "z_score_baseline": "2020Q1-2024Q4"
  }
}
```

```
GET /validation?indicator=demand&geography=US&target=gdp_growth

→ 200 OK
{
  "indicator": "demand",
  "target": "gdp_growth",
  "geography": "US",
  "correlation": {"pearson": 0.73, "spearman": 0.68, "p_value": 0.002},
  "optimal_lag": 1,
  "granger_causality": {"lag_1": {"f_stat": 5.2, "p_value": 0.04}},
  "time_series": {
    "indicator": [...],
    "target": [...]
  }
}
```

```
GET /sensitivity?topic=demand&geography=US

→ 200 OK
{
  "topic": "demand",
  "geography": "US",
  "curve": [
    {"n_firms": 25, "mean_correlation": 0.31, "ci_lower": 0.12, "ci_upper": 0.48},
    {"n_firms": 50, "mean_correlation": 0.49, "ci_lower": 0.35, "ci_upper": 0.61},
    ...
  ],
  "european_natural_sample": {"n_firms": 78, "correlation": 0.42}
}
```

### Docker + Render (Production)

- `Dockerfile` using `python:3.11-slim`
- Pre-computed indicators + validation statistics baked into the image (no live API calls needed at demo time)
- Environment variable for LLM API key (only needed if re-running extraction)
- Deploy to Render free tier for a live demo URL
- Health check endpoint at `/health`

---

## 9. Project Structure

```
macro-surveillance-earnings/
├── README.md                          # Business-first, < 500 words
├── Dockerfile
├── docker-compose.yml
├── pyproject.toml
├── .gitignore
├── .env.example
│
├── data/
│   ├── raw/                           # Transcript JSONs (gitignored — too large)
│   │   └── transcripts/
│   │       ├── US/                    # {ticker}/{quarter}.json
│   │       └── EU/
│   ├── processed/                     # Cleaned, parsed transcripts
│   │   ├── corpus_manifest.db         # SQLite: what's downloaded, metadata
│   │   ├── firm_quarter_scores/       # Per-firm extraction results
│   │   └── aggregated_indicators/     # Quarterly time series (checked in — small)
│   ├── validation/                    # Official statistics from FRED/Eurostat/ECB
│   │   ├── us_macro.parquet
│   │   └── eu_macro.parquet
│   ├── lexicons/                      # Loughran-McDonald, ECB keyword clusters
│   │   ├── loughran_mcdonald.csv
│   │   └── keyword_clusters.json      # ECB-derived + calibrated clusters
│   └── sample/                        # 20 transcripts for reviewers to run
│
├── src/
│   ├── acquisition/
│   │   ├── fmp_downloader.py          # Rate-limited transcript fetcher (primary source)
│   │   ├── finnhub_downloader.py      # Alternative transcript source (if pre-commitment confirms access)
│   │   ├── manifest.py                # SQLite corpus manifest (idempotent tracking)
│   │   ├── transcript_parser.py       # JSON → cleaned text, speaker separation
│   │   └── company_universe.py        # S&P 500 list, European ADR list, GICS sectors
│   ├── extraction/
│   │   ├── keyword_extractor.py       # ECB-style keyword matching + context windows (r=10 baseline)
│   │   ├── sentiment_scorer.py        # Loughran-McDonald scoring within windows
│   │   ├── finbert_scorer.py          # Optional: FinBERT sentiment within keyword windows (Method C)
│   │   ├── llm_extractor.py           # Structured LLM extraction (Method B)
│   │   └── schemas.py                 # Pydantic models for extraction output
│   ├── aggregation/
│   │   ├── aggregator.py              # Firm → sector → economy. Equal/cap/sector weights.
│   │   ├── z_scorer.py                # Historical normalisation
│   │   └── time_series.py             # Quarterly indicator construction
│   ├── validation/
│   │   ├── official_stats.py          # FRED, Eurostat, ECB data downloaders
│   │   ├── correlation.py             # Pearson, Spearman, cross-correlation at lags
│   │   ├── granger.py                 # Granger causality tests
│   │   ├── sensitivity.py             # Sample-size subsampling + bootstrap
│   │   └── window_sensitivity.py      # Context window r=5,10,15,20 comparison
│   ├── pipeline.py                    # End-to-end orchestration
│   └── api.py                         # FastAPI endpoints
│
├── app/
│   └── streamlit_app.py               # Dashboard with 4 views
│
├── evaluation/
│   ├── data_quality_audit.py          # Layer 0: transcript completeness, language, coverage
│   ├── keyword_validation.py          # Layer 1: manual review of keyword hits
│   ├── indicator_validation.py        # Layer 2: correlation with official stats
│   ├── method_comparison.py           # Layer 3: keyword vs. LLM
│   ├── sensitivity_analysis.py        # Layer 4: sample size degradation curve
│   └── failure_analysis.py            # Layer 5: categorise and document failures
│
├── notebooks/
│   ├── 01_data_exploration.ipynb      # Corpus statistics, transcript structure
│   ├── 02_keyword_calibration.ipynb   # Keyword cluster development + manual review
│   ├── 03_indicator_construction.ipynb # Build indicators, first visual overlays
│   ├── 04_validation_results.ipynb    # Full correlation/Granger analysis
│   ├── 05_method_comparison.ipynb     # Keyword vs. LLM head-to-head
│   └── 06_sensitivity_analysis.ipynb  # Sample size curves
│
├── tests/
│   ├── test_downloader.py
│   ├── test_keyword_extractor.py
│   ├── test_sentiment_scorer.py
│   ├── test_aggregator.py
│   ├── test_validation.py
│   └── test_pipeline.py
│
└── docs/
    ├── architecture.md                # Architecture diagram + design decisions
    ├── keyword_clusters.md            # Full keyword lists with calibration notes
    ├── data_quality_report.md         # Transcript coverage and quality statistics
    └── evaluation_results.md          # Full results + failure analysis
```

---

## 10. Communication Plan

### README Structure (< 500 words)

1. **One-liner:** "Open-source replication of ECB macroeconomic surveillance: text-based economic indicators from 9,000+ earnings call transcripts across 20 quarters, validated against GDP, PMI, and employment data."
2. **The problem:** Central banks build text-based economic indicators from corporate communications — but using commercial data costing institutional-level fees. The methodology is published; the pipeline isn't. This project bridges the gap.
3. **What this builds:** A pipeline that downloads earnings call transcripts (FMP API), extracts macroeconomic signals using both ECB-style keyword matching and LLM structured extraction, aggregates into economy-wide quarterly indicators, and validates against official statistics from FRED/Eurostat/ECB. Two geographies: US (400+ firms, robust) and Europe (60–100 firms, frontier test).
4. **Key results:** Demand indicator correlation with GDP [r = X], optimal lead [N quarters], keyword vs. LLM comparison finding, minimum firm count for reliable indicator [N]. (Numbers filled in after evaluation.)
5. **Try it:** Link to deployed Streamlit dashboard.
6. **Tech stack:** Python, FMP API, FRED/Eurostat/ECB APIs, Loughran-McDonald lexicon, OpenAI/Anthropic (comparison only), Streamlit, FastAPI, Docker.
7. **How to run:** `docker compose up` or `pip install -e . && python -m src.pipeline --sample` (runs on the 20 included sample transcripts).

### Blog Post Outline

**Title:** "What 9,000 Earnings Calls Tell Us About the Economy — and Why Central Banks Are Listening"

1. **Hook:** "The ECB published a blog post three days before I started this project, describing how they use NLP on corporate text to monitor the economy. Their data costs institutional fees. I replicated their approach with a free API and Python."
2. **The methodological lineage:** Hassan et al. (2019) built keyword-in-context analysis for political risk → Ruch & Taskin (2024) adapted it to macroeconomic demand/supply → the ECB operationalised it for real-time monitoring. Every step used proprietary data. I replicated the final step with free APIs.
3. **The data engineering challenge:** Downloading 9,000+ transcripts at 250/day over 5+ weeks. Building an idempotent pipeline. Parsing prepared remarks from Q&A. The European coverage problem — both sample size and content bias.
4. **Keyword calibration:** Translating ECB keyword clusters to earnings call language. What "demand" means in a CEO's mouth. Manual validation of 200 keyword hits.
5. **The results:** Visual overlays of text indicators vs. GDP/PMI. Which topics worked best. Where the US and European indicators diverged.
6. **Does the LLM help?** The head-to-head comparison. Cost per indicator point. When language understanding matters and when keywords suffice.
7. **How many firms do you need?** The sample-size sensitivity curve. Practical implications for smaller-scale implementations.
8. **Honest limitations:** Large-firm bias, English-only text, European content bias, 95% fewer firms than the ECB's commercial data, correlation ≠ causation.
9. **What I'd do next:** Real-time indicator updates, integration with Bloomberg for live monitoring, expansion to additional text sources (10-K filings, central bank minutes).

**Publish on:** Medium/TDS + LinkedIn.

### Interview Talking Points (STAR Format)

**S:** Central banks including the ECB, Bank of Canada, and RBA are increasingly using NLP on corporate text to construct macroeconomic indicators — early-warning signals for GDP, inflation, and employment. But they all rely on commercial data providers charging institutional-level fees, so their published methodology has never been replicated with open tools.

**T:** Build an open-source pipeline replicating the ECB's published methodology using free data, validate the constructed indicators against official statistics, and test whether LLM extraction improves on the keyword approach central banks use.

**A:** Built a rate-limited download pipeline that acquired 9,000+ quarterly earnings call transcripts from the FMP API over 20 quarters (Q1 2020 – Q4 2024) across two geographies (US: 450 S&P 500 firms, Europe: 80 ADR-listed large caps). Implemented the keyword-in-context methodology from Hassan et al. (2019) / Ruch & Taskin (2024) — demand, supply, inflation, hiring, investment, financing clusters with Loughran-McDonald sentiment scoring in ±10-word context windows — and an LLM structured extraction comparison. Aggregated per-firm scores into economy-wide quarterly indicators using three weighting schemes. Validated against FRED (US) and Eurostat/ECB (Europe) official statistics using correlation analysis, lead/lag testing, and Granger causality across 20 quarterly observations. Conducted bootstrap sample-size sensitivity analysis at N = 25 to 450 firms. Tested European content bias (ADR companies describing global vs. domestic conditions). Deployed as a Streamlit dashboard with four interactive views.

**R:** US demand sentiment index correlated at r = [X] with quarterly GDP growth (p < [Y]), with [N]-quarter leading signal. [X] of 6 topic indicators achieved |r| > 0.5 against their primary validation targets. European indicator correlated at r = [X] (weaker, consistent with the sample-size sensitivity curve showing signal emerges above ~[N] firms). LLM extraction [did/did not] significantly improve indicator quality over keywords — the finding was [X]. The sample-size sensitivity analysis is the first open-source study of "how many firms are needed for a reliable text-based macro indicator."

### Resume Line (Final Draft After Results)

"Replicated ECB macroeconomic surveillance methodology using free data: constructed text-based economic indicators from 9,000+ earnings call transcripts across 20 quarters, achieving r=[X] correlation with GDP growth and [N]Q leading signal. Compared keyword vs. LLM extraction; [finding]. Deployed as interactive Streamlit dashboard."

---

## 11. Scope and Timeline

### Pre-Commitment: Empirical Tests (4–5 hours, before Week 1)

Before committing to the full project:

- [ ] **Q1 (1 hr):** Sign up for FMP API key. Call the earnings transcript symbol list endpoint. Count European ADR-listed companies with transcript availability. **Go/no-go threshold:** ≥ 40 European companies.
- [ ] **Q1b (30 min):** Sign up for Finnhub free API key. Test transcript list endpoint for 5 European companies (ASML, NESN, SHEL, AZN, SAP). Check if full transcripts are accessible on the free tier. If yes, this dramatically accelerates data acquisition. Document results either way.
- [ ] **Q2 (1 hr):** Download 10 sample transcripts (5 US, 5 European) from FMP. Check text quality, speaker separation, word count, language. Measure JSON response sizes for bandwidth estimation. **Go/no-go threshold:** Transcripts are clean, in English, ≥ 3,000 words. Average response size × total transcripts < 500 MB (FMP bandwidth limit).
- [ ] **Q3 (2–3 hr):** Run ECB demand keyword list (with r=10 context window) on 20 S&P 500 transcripts for one quarter. Compute rough demand sentiment. Compare direction against known GDP growth for that quarter. **Go/no-go threshold:** Directional alignment (positive sentiment ↔ positive GDP surprise, or vice versa).

### Phase 1: Data Acquisition + Keyword Baseline (Weeks 1–2)

- [ ] Build rate-limited FMP download pipeline with SQLite manifest (+ Finnhub integration if pre-commitment confirms access)
- [ ] **Start downloading** S&P 500 transcripts — 8 core quarters first (Q1 2022 – Q4 2023), then continue to 20Q in background
- [ ] **Start downloading** European ADR company transcripts (same phased approach)
- [ ] Implement transcript parser: JSON → cleaned text, speaker separation, metadata
- [ ] Download official statistics: FRED (US, including ISM PMI series NAPM), Eurostat + ECB Data Portal (Europe)
- [ ] Implement keyword extraction (ECB-derived clusters, r=10 baseline)
- [ ] Implement Loughran-McDonald context-window sentiment scoring
- [ ] Calibrate keyword clusters: manual review of 200 hits, adjust clusters. Test r=5, 10, 15, 20.
- [ ] Aggregate firm → sector → economy-wide quarterly indicators (8-quarter subset)
- [ ] Z-score normalisation
- [ ] First validation: correlation + visual overlay of demand indicator vs. GDP (8-quarter visual check — no formal statistics)
- [ ] **Milestone:** One working indicator (US demand) with visual alignment against GDP growth. Download pipeline running in background.

### Phase 2: Full Indicator Suite + LLM Comparison (Week 3)

- [ ] Build all 6 topic indicators (US, keyword method) — using 8-quarter data available
- [ ] Validate each against primary target statistic (visual + preliminary correlation)
- [ ] Implement LLM structured extraction pipeline
- [ ] Run LLM extraction on US corpus (or cost-constrained sample)
- [ ] Build LLM-based indicators for 6 topics
- [ ] Head-to-head comparison: keyword vs. LLM per topic
- [ ] Build European indicators (keyword method)
- [ ] European content-bias test: correlate European text indicator with both Euro area GDP and US GDP
- [ ] Validate European indicators against Eurostat/ECB targets
- [ ] **Background download continues** — 20Q corpus approaching completion
- [ ] **Milestone:** Method comparison table, European results (including content-bias finding)

### Phase 3: Full 20Q Validation + Sensitivity Analysis (Week 4)

- [ ] **20Q corpus should be complete or nearly complete** — re-run all indicators on full 20-quarter time series
- [ ] Formal statistical validation: Pearson/Spearman correlation, p-values, confidence intervals (now meaningful at n=20)
- [ ] Implement bootstrap sample-size sensitivity analysis
- [ ] Run subsampling at N = 25, 50, 75, 100, 200, 400+
- [ ] Plot degradation curve with confidence intervals
- [ ] Overlay European natural sample
- [ ] Lead/lag analysis across all indicators (cross-correlation at lags -2 to +2)
- [ ] Granger causality tests (with stationarity checks and structural-break caveats for COVID period)
- [ ] Aggregation method comparison (equal vs. cap-weighted vs. sector-weighted)
- [ ] Context-window sensitivity: report correlation at r=5, 10, 15, 20
- [ ] Full failure mode analysis
- [ ] **Milestone:** All evaluation layers complete on full 20-quarter data, results documented

### Phase 4: Deployment + Communication (Week 5)

- [ ] Build Streamlit dashboard (4 views)
- [ ] Build FastAPI endpoints
- [ ] Dockerise
- [ ] Deploy to Render
- [ ] Write README (< 500 words)
- [ ] Write blog post
- [ ] Record 2-min demo video (screen capture of dashboard)
- [ ] Prepare interview talking points
- [ ] Final code cleanup: docstrings, type hints, linting, tests
- [ ] **Milestone:** Live demo URL, published blog post

### Total: 5 weeks (download pipeline runs as background job from Week 1 through Week 4)

**Critical path:** Data download. If FMP rate limit (250/day) is the only source, the 20Q corpus takes 5–7 weeks to download — longer than the project itself. Mitigations: (1) start downloading during pre-commitment, (2) build pipeline to process data incrementally, (3) Finnhub as accelerator if European access confirmed. The 8Q core subset provides enough data for all pipeline development; the 20Q full corpus is needed only for Phase 3 statistical validation.

---

## 12. Estimated Costs

| Item | Cost |
|---|---|
| FMP API (free tier) | $0 |
| Finnhub API (free tier, if used for European acceleration) | $0 |
| FRED API | $0 |
| Eurostat / ECB Data Portal | $0 |
| Loughran-McDonald lexicon | $0 |
| LLM extraction — GPT-4o-mini (full 20Q corpus, ~8,000–10,000 transcripts) | ~$25–75 |
| LLM extraction — validation sample on stronger model (100 transcripts) | ~$5–15 |
| Render deployment (free tier) | $0 |
| **Total** | **~$30–90** |

**Note:** If LLM comparison is dropped (keyword-only project), cost drops to $0. If LLM extraction is run on only the 8Q subset (~3,200 transcripts), cost is ~$10–30.

---

## 13. Risks and Mitigations

| Risk | Likelihood | Impact | Mitigation |
|---|---|---|---|
| US indicator doesn't correlate with GDP | Low | Fatal | ECB published correlations validate the methodology works at scale. With 400+ firms across 20 quarters, signal should emerge. If it doesn't with free data, that's also a publishable finding — document why. |
| European indicator is too noisy | Medium | Medium | Expected. The sample-size sensitivity analysis and content-bias analysis explain *why* quantitatively. US indicator is the primary result. European analysis is labelled "frontier test," not "main finding." |
| FMP European coverage < 40 companies | Medium | Medium | Empirical test (Q1) catches this before commitment. Finnhub tested as alternative European source. Fallback: US-only project with documented European coverage appendix. |
| FMP transcript quality is poor for European companies | Medium | Medium | Empirical test (Q2) catches this early. Fallback: restrict European analysis to companies that conduct calls in English (most large-cap ADR-listed firms do). |
| FMP bandwidth limit (500 MB/30 days) binds | Medium | Medium | Measured during pre-commitment (Q2: record response sizes). At ~100KB average × 9,000 transcripts = ~900 MB total, this exceeds a single 30-day window. **Mitigation:** Phase downloads across 2–3 months (the phased timeline already accommodates this). Alternatively, Finnhub has no documented bandwidth limit. |
| 20Q download takes longer than project timeline | Medium | Medium | Start download during pre-commitment. Phased approach: 8Q core data (15–19 days) provides enough to build all pipelines. Remaining 12Q downloaded in background. Finnhub as accelerator if available. Worst case: report results on 8Q (with appropriate statistical caveats) and 20Q as an appendix if download completes late. |
| LLM extraction doesn't improve over keyword baseline | Medium | Low | Valid negative result. "The ECB's keyword approach is already sufficient for macro indicators" is a useful finding. Frame as cost-effectiveness analysis. |
| Quick keyword test (Q3) shows zero signal | Low | Fatal | Methodology is proven at institutional scale. If it fails at portfolio scale with 20 firms, investigate whether the issue is sample size, keyword calibration, or data quality before abandoning. |
| Scope creeps beyond 5 weeks | Medium | Medium | Hard-scope: 20 quarters of data, 6 topic dimensions, 2 geographies, 2 methods. No expansions until core analysis is complete. |
| Project looks like "stock prediction" | Low | High | Framing discipline: validation is against GDP/PMI/employment, never stock prices. Blog title and README make "macroeconomic surveillance" framing explicit. No stock tickers in the dashboard. |
| COVID structural break distorts indicators | Medium | Medium | The 20-quarter window (Q1 2020 – Q4 2024) intentionally includes COVID. Test indicators with and without Q1–Q2 2020 outliers. Document whether excluding the crash period changes results — this is itself a finding about structural break sensitivity. |

---

## 14. Limitations (Pre-Written)

These will appear in the README, blog post, and evaluation documentation:

**Data limitations:**
- "Free-tier transcript data covers primarily large-cap publicly traded firms. The indicator misses SMEs, private companies, government sector, and non-profit organisations — collectively a substantial share of GDP."
- "European coverage via ADR listings is biased in two ways: (1) **sample composition** — only ~60–100 large-cap multinationals, no SMEs or domestic-only firms; (2) **content bias** — these companies report on US calendars, in English, and often emphasise global rather than European domestic operations. The European indicator may track global or US conditions better than Euro area conditions. This is tested explicitly by comparing correlations with Euro area GDP vs. US GDP."
- "Earnings call transcripts are in English. Companies reporting in other languages are excluded, creating coverage gaps in non-Anglophone markets."
- "With 20 quarterly observations (Q1 2020 – Q4 2024), correlation estimates have moderate confidence intervals and Granger causality tests have limited degrees of freedom. The time series includes the COVID structural break (Q1–Q2 2020), which may distort all indicators simultaneously. Results are reported both with and without the COVID quarters."

**Coverage gap — what's lost vs. the ECB's commercial data:**
- "The ECB uses NL Analytics covering ~14,000 firms across 82 countries. Ruch & Taskin (2024) use Factiva with firms headquartered in 80 countries. This project uses ~500 US firms + ~80 European ADR firms — a 95%+ reduction in coverage."
- "However, the S&P 500 represents ~80% of US equity market capitalisation and includes the largest firms across all major sectors. If the methodology works at all with free data, the question is whether 500 large firms carry enough signal — or whether the long tail of thousands of smaller firms adds meaningful information. If 500 firms suffice for the US indicator, that's itself a finding about redundancy in large corporate text corpora."
- "For Europe, the coverage gap is more severe: ~80 ADR-listed multinationals cannot represent a diverse, 27-country economy. The European indicator should be interpreted as 'what large European multinationals say about their business conditions' — not as a comprehensive Euro area economic sentiment measure."

**Methodological limitations:**
- "Keyword clusters are calibrated to S&P 500 earnings call language. They may not generalise to other text sources (news articles, central bank minutes, social media) without re-calibration."
- "Equal-weighted aggregation treats a $3T company the same as a $5B company. Market-cap weighting is tested as an alternative, but neither perfectly matches GDP composition."
- "Correlation between text indicators and GDP does not establish causation. Both may be driven by the same underlying economic conditions without the text providing additional predictive information."
- "The context window parameter (r=10 words) is taken from the literature (Hassan et al. 2019). Sensitivity to r=5, 10, 15, 20 is tested, but the optimal window may differ for macro topic keywords vs. political risk keywords."

**What a production system would do differently:**
- "With NL Analytics or Factiva, the corpus would include 14,000+ firms across 80+ countries — dramatically stronger signal."
- "With access to Factiva or Refinitiv, the corpus could include analyst reports, news articles, and conference presentations — not just quarterly earnings calls."
- "A production system would update indicators in near-real-time as earnings calls occur, rather than in quarterly batches."
- "With longer time series (10+ years), out-of-sample backtesting could establish genuine forecasting ability, not just in-sample correlation."

---

## 15. References and Prior Art to Cite

- **Hassan, Hollander, van Lent & Tahoun (2019), Quarterly Journal of Economics:** "Firm-Level Political Risk: Measurement and Effects." Developed the keyword-in-context methodology (±10 word window, Loughran-McDonald sentiment) applied to earnings call transcripts. 11,000+ firms, 81 countries. Replication data on Harvard Dataverse, pre-computed indices at firmlevelrisk.com. **Methodological origin** of the approach Ruch & Taskin later adapted to macro topics.
- **Baker, Bloom & Davis (2016), Quarterly Journal of Economics:** "Measuring Economic Policy Uncertainty." The Economic Policy Uncertainty (EPU) index that inspired the keyword-based measurement approach in Hassan et al. and Ruch & Taskin.
- **ECB Economic Bulletin, Box 7 (2023):** "Using text data to assess economic developments in the euro area." Published correlation between text-based demand indicators and GDP.
- **ECB Economic Bulletin, Box 4 (2024):** Updated analysis showing supply chain text indicators tracked post-COVID normalisation.
- **ECB blog (Feb 16, 2026):** "Text as data: new approaches to economic monitoring." Most recent institutional endorsement of the methodology.
- **Ruch & Taskin (2024), Oxford Bulletin of Economics and Statistics:** "Global Demand and Supply Sentiment: Evidence From Earnings Calls." Formalised the methodology this project replicates. Explicitly cites Hassan et al. (2020 working paper version) and Baker et al. (2016) as methodological basis. Uses Factiva data. Reports r=0.86 with Global Supply Chain Pressure Index.
- **Gosselin & Taskin (2023), Bank of Canada Staff Discussion Paper:** Extended methodology to Canadian context using Factiva.
- **Gray, Leung, Manokaran (2025, RBA):** Applied BERTopic to business liaison data. Different text source but validates the general approach.
- **IMF World Economic Outlook, Ch. 2 (Oct 2023):** Used NL Analytics for inflation expectations. Validates institutional interest.
- **Loughran & McDonald (2011), Journal of Finance:** "When Is a Liability Not a Liability? Textual Analysis, Dictionaries, and 10-Ks." The standard financial sentiment lexicon used in Method A.

---

## 16. Checklist (From Portfolio Principles Reference)

### Business Framing
- [x] Clear business question in first sentence ("Can open-source tools replicate central bank macro surveillance?")
- [x] Identified stakeholder (central bank economists, macro strategists, policy analysts)
- [x] Quantified cost of current solution (NL Analytics institutional fees, Factiva $10K–50K+/year)
- [x] Specific, actionable output (quarterly macro indicators validated against official statistics)

### Technical Execution
- [x] Real-world data (FMP earnings call transcripts, FRED/Eurostat/ECB official statistics)
- [x] Multiple data sources joined together (FMP + FRED + Eurostat + ECB + Loughran-McDonald)
- [x] Substantial data cleaning with documented decisions (transcript parsing, keyword calibration, speaker separation)
- [x] Appropriate technique choice, justified (keyword extraction replicates ECB methodology; LLM is comparison, not default)
- [x] Proper evaluation with multiple metrics (correlation, lead/lag, Granger causality, sensitivity analysis)
- [x] Honest limitation documentation (pre-written in Section 14)

### Modern Stack
- [x] Evaluation framework for model outputs (5-layer framework, Layers 0–5)
- [x] Cost analysis (keyword $0 vs. LLM $X per indicator point — Section 6e, Layer 3)
- [x] Failure mode documentation (7 categorised failure types — Layer 5)
- [x] Comparison against sensible baselines (keyword baseline IS the ECB's method; LLM compared against it)

### Deployment & Engineering
- [x] Interactive demo (Streamlit dashboard with 4 views)
- [x] API endpoint (FastAPI — /indicator, /validation, /sensitivity)
- [x] Docker + cloud deployment (Render free tier)
- [x] Professional code structure (src/, tests/, docs/, evaluation/)
- [x] Version control with meaningful commit history planned

### Communication
- [x] README plan (< 500 words, business-first, with key findings)
- [x] Blog post outline ("What 3,000 Earnings Calls Tell Us About the Economy")
- [x] Resume line drafted
- [x] Interview talking points (STAR format)
- [x] Architecture diagram included

### Novelty
- [x] Searched GitHub, Kaggle, Medium, Google Scholar, arXiv
- [x] Framing distinct from existing projects (reproducibility study, not financial NLP)
- [x] Combination of data sources/techniques is original (FMP + FRED/Eurostat + keyword/LLM comparison)
- [x] Not replicable by following a tutorial (no tutorial exists for aggregate-text-to-macro-indicator)
- [x] Prior art acknowledged and differentiation stated (Section 3)

---

*This document is the complete plan for B1. Implementation begins with the empirical pre-commitment tests (Section 11), followed by Phase 1 (Weeks 1–2).*
