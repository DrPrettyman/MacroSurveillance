# Corporate Text → Macroeconomic Indicator: Validation Assessment

## The Idea

**Business question:** "Can aggregate NLP signals from corporate earnings calls predict or nowcast official macroeconomic statistics — GDP growth, employment, inflation — before the official numbers are published?"

**Who pays for this today:** Central banks (ECB, RBA, Bank of Canada, Fed), international organisations (IMF, World Bank), economic consultancies, sovereign wealth funds, macro hedge funds. NL Analytics (the commercial provider the ECB uses) charges institutional rates for exactly this service.

**The framing difference from L2:** L2 asked "what did Company X say?" (company-level extraction → investment signal). This asks "what are 200 companies *collectively* saying about the economy?" (aggregate text → macroeconomic indicator). The unit of analysis flips from the firm to the economy.

---

## Validation Check 1: Novelty — ✅ PASS (Strong)

### The academic/institutional landscape

This is an active research area *at central banks*, not in the open-source community:

| Institution | Publication | What they did |
|---|---|---|
| **ECB** | Economic Bulletin boxes (2023, 2024, 2025) | Keyword-based risk/sentiment indices from ~563 euro area firms' earnings calls. Validated against GDP, PMI, bank lending surveys. Uses NL Analytics (commercial). |
| **Bank of Canada / World Bank** | Ruch & Taskin (2024), Oxford Bulletin of Economics and Statistics | Demand/supply sentiment from earnings calls using NLP. Validated with structural Bayesian VAR. Uses Factiva (commercial). |
| **IMF** | World Economic Outlook Ch. 2 (Oct 2023) | Firm-level inflation expectations extracted from earnings calls. Used NL Analytics. |
| **RBA** | Gray et al. (Aug 2025), arXiv 2506.18505 | AI-powered tool for central bank liaison intelligence. BERTopic + FinBERT + numerical extraction. Nowcasting wages growth. Internal data. |
| **ECB** | Blog post (Feb 16, 2026 — 2 days ago) | "Using AI to transform the ECB's Corporate Telephone Survey." References RBA work. States NLP techniques are reshaping central bank analysis. |

### The open-source landscape

**Zero GitHub projects replicate this approach.** Searched:
- `"earnings call" macroeconomic indicator` — nothing
- `"earnings call" aggregate sentiment GDP nowcast` — nothing
- GitHub Topics: `macroeconomic-indicators`, `nowcasting`, `earnings-calls` — no overlap

Every single earnings call NLP project on GitHub does: company-level sentiment → stock price prediction. Nobody has built: aggregate corporate text → macroeconomic indicator.

The NY Fed's nowcasting code is on GitHub, but it uses traditional macro variables (PMI, employment), not text data.

### Why the gap exists

The central banks all use commercial data providers (NL Analytics: 14,000 firms, 82 countries; Factiva: similar scale). The methodology is published in academic papers, but the data pipeline isn't replicable with free tools. This creates a reproducibility gap that a portfolio project could fill.

**Novelty verdict:** Strong. The framing (macroeconomic surveillance, not stock prediction) is genuinely untouched in the portfolio/open-source world. The methodology is documented in published research but never implemented openly.

---

## Validation Check 2: Data Availability — ⚠️ PARTIAL PASS

### Earnings call transcripts (input)

**FMP API (free tier):**
- 250 requests/day, 500MB/month bandwidth
- Coverage: 8,000+ US companies + international companies with US presence
- European companies available: those with US ADR listings (Siemens [SIEGY], ASML [ASML], Nestlé [NSRGY], SAP [SAP], TotalEnergies [TTE], Shell [SHEL], Unilever [UL], AstraZeneca [AZN], Novartis [NVS], etc.)
- Practical yield for European large-caps: ~50-80 companies with reliable quarterly transcripts

**The scale problem:** The ECB uses ~563 euro area firms per quarter. FMP free tier would yield ~50-80. That's an order of magnitude less. The aggregate signal might be too noisy with 50 companies to correlate meaningfully with macro indicators.

**Possible mitigations:**
- Focus on the largest companies (higher economic footprint per firm)
- Use sector-level aggregation within sectors where coverage is better
- Include US companies and construct a US indicator (much better coverage) as the primary analysis, with a European indicator as a secondary exercise
- Be honest about the scale limitation — document it as the main constraint

### Macroeconomic validation data (ground truth)

**Excellent availability:**
- Eurostat API (Python `eurostat` package): quarterly GDP, employment, industrial production, business confidence
- ECB Data Portal (`ecbdata` package): lending surveys, HICP inflation, financial conditions
- European Commission ESI (Economic Sentiment Indicator): monthly, directly comparable
- S&P Global PMI: publicly available headline figures
- All free, all well-documented, all available via Python APIs

### Loughran-McDonald lexicon

Freely available. The standard financial sentiment lexicon, same one used by ECB/Ruch & Taskin.

### Data verdict

Validation data: excellent. Transcript data: workable but thin for European companies. The honest approach would be to build a US indicator (where coverage is deep) and a European indicator (where coverage is thin), documenting the performance difference as a function of sample size.

---

## Validation Check 3: Methodology — ✅ PASS

The ECB/Ruch & Taskin methodology is well-documented and reproducible:

1. **Topic identification:** Define keyword clusters (synonym sets) for economic themes: demand, supply, investment, financing conditions, inflation, hiring, geopolitics
2. **Sentence-level extraction:** For each transcript, identify sentences containing topic keywords
3. **Sentiment scoring:** Around each topic mention, score sentiment using Loughran-McDonald lexicon (positive terms minus negative terms in a window around the mention)
4. **Aggregation:** Aggregate per-firm sentiment into quarterly sector-level and economy-level indices (z-scored)
5. **Validation:** Correlate constructed indices against official statistics. Test lead/lag relationships.

**The LLM enhancement opportunity:** The ECB uses keyword matching because they need it to run at scale (6,000+ firms, automated, reproducible). For a portfolio project with 50-200 transcripts, we can compare:
- **Baseline:** Keyword matching + Loughran-McDonald (replicating ECB approach)
- **LLM extraction:** Structured extraction using LangChain — extract (topic, sentiment, confidence, specific_figures) tuples from each paragraph

This comparison IS the novel contribution: "Does LLM-based extraction produce better macro indicators than the keyword approach central banks currently use?"

---

## Validation Check 4: Portfolio Fit — ⚠️ MIXED

### Strengths
- **EU focus:** Validation against Eurostat/ECB data. European company transcripts where available.
- **Novel domain:** Macroeconomics/policy — not covered by Steam (gaming), Private Label (retail), or L1 (regulation)
- **Target company relevance:** Revolut (macro conditions), Clarity AI (economic monitoring), any policy-adjacent fintech
- **Verifiable evaluation:** Correlation with GDP, lead/lag analysis, comparison against published ECB charts

### Concerns

**Framework fit is awkward.** This isn't cleanly a "LangChain project" or a "PyTorch project." The core contribution is:
1. Data engineering: transcript collection, NLP extraction pipeline
2. Time series construction: aggregating text signals into quarterly indicators
3. Econometric validation: correlation analysis, lead/lag testing, sector decomposition

The LangChain/PyTorch component (whichever is used for extraction) is an input to the real work, not the centrepiece. A hiring manager scanning the repo might see "correlation analysis against GDP" and think "data analyst project" rather than "ML engineering project."

**L1 already fills the LangChain slot.** If this uses LangChain for extraction, it overlaps with L1 on technique (structured extraction from specialized text). The portfolio would have two LangChain extraction projects and no PyTorch project.

**Scale limitation is hard to hide.** "I replicated the ECB's methodology with 50 companies instead of 563" invites the question: "and did it work?" If the signal is noisy (likely, with 50 companies), the project's key deliverable — the macro indicator — may not be convincing.

**Scope is large.** The ECB has dedicated teams for this. The pipeline involves: API integration, text processing, NLP extraction (keyword AND LLM), time series construction, multiple macro data source integration, statistical validation, sector decomposition. This is 5-6 weeks minimum, and the marginal effort goes into econometric analysis rather than ML engineering.

---

## Validation Check 5: Evaluation Story — ✅ PASS

This is where the idea genuinely shines. The evaluation is inherently strong because macroeconomic statistics are published ground truth:

- **Correlation:** Does the constructed text index co-move with GDP growth / PMI / employment?
- **Lead time:** Does the text signal lead the official statistic? (Earnings calls happen before GDP is published — this is the whole value proposition for central banks.)
- **Sector decomposition:** Does manufacturing sentiment track industrial production? Does services sentiment track services PMI?
- **Method comparison:** Keyword extraction vs. LLM extraction — which produces indices that correlate better with official statistics?
- **Sample size sensitivity:** How does indicator quality degrade as we reduce the number of firms? (This turns the scale limitation into a feature — a documented analysis of "how many firms do you need for a reliable text-based macro indicator?")

---

## Overall Verdict

| Check | Result | Notes |
|---|---|---|
| Novelty | ✅ Strong | Zero portfolio projects. Active central bank research area. |
| Data availability | ⚠️ Partial | Validation data excellent. Transcript coverage thin for Europe. |
| Methodology | ✅ Documented | ECB/Bank of Canada papers provide replicable methodology. |
| Portfolio fit | ⚠️ Mixed | Novel domain, but framework fit is awkward and overlaps L1 on technique. |
| Evaluation | ✅ Strong | Validated against published official statistics. |
| Scope | ⚠️ Large | 5-6 weeks, heavy on econometrics rather than ML engineering. |

### The honest assessment

This is a genuinely novel and intellectually interesting idea with strong academic backing. It would make an excellent research project or a standout portfolio piece for someone targeting central bank / economic research / policy analytics roles.

**For this specific portfolio, it has problems:**

1. **It doesn't fill the PyTorch gap.** The portfolio needs a PyTorch project (P1 or P2). This idea, whether implemented with keyword matching or LangChain, doesn't address that need.

2. **It overlaps L1 on technique.** Both L1 and this project would use LangChain for structured extraction from specialized text. The portfolio would demonstrate the same technical skill twice.

3. **The core contribution is econometric, not ML.** The novel part — "does this text index predict GDP?" — is a time series correlation exercise. The NLP extraction is an established technique applied to this data. This makes it a strong data science project but a weak demonstration of modern ML/AI framework skills, which is the stated portfolio gap.

4. **The scale limitation may undermine the headline result.** If the 50-company indicator doesn't correlate well with GDP (plausible), the project's central claim falls apart. The "sample size sensitivity analysis" partially rescues this, but it's risky.

---

## Recommendation

**TABLE THIS IDEA. Don't build it now. Focus on P1/P2 validation for the PyTorch slot.**

The macroeconomic surveillance idea is worth keeping in reserve for two scenarios:

1. **Fifth portfolio project** — if, after building L1 + a PyTorch project, there's appetite for a fifth project that demonstrates a different flavour of data science (econometrics + NLP rather than ML engineering)

2. **Alternative career targeting** — if the job search shifts toward central banks, economic consultancies, or policy-focused fintech (Clarity AI, some roles at the ECB/BIS/IMF), this project becomes highly relevant and could replace one of the others

3. **Blog post without full project** — the research findings alone ("Here's what central banks are doing with earnings call NLP, here's the open data gap, here's a proof of concept") could make a strong blog post that demonstrates domain knowledge without the full 5-week build

### Immediate next step

Validate P1 (Fine-Tune vs. Prompt Benchmark) and P2 (Custom Embeddings for Vertical Search) with the same rigour applied to L1 and L2. The portfolio needs one PyTorch project, and that's the decision that actually matters now.
