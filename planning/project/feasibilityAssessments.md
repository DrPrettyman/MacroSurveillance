# B1: Corporate Earnings → Macroeconomic Surveillance

## Deep Feasibility Assessment

---

## 1. The Reframed Idea

**Business question:** "Can open-source NLP tools and free data replicate the macroeconomic surveillance indicators that central banks build with commercial datasets costing institutional-level fees?"

**Who does this today:** The ECB (using NL Analytics, ~14,000 firms, 82 countries), the Bank of Canada (using Factiva), the IMF (NL Analytics), the RBA (using internal liaison data + BERTopic). All use commercial or proprietary data. None have published reproducible open-source implementations.

**The framing difference from L2 (discarded):** L2 asked "what did Company X say?" (company-level extraction → stock signal). B1 asks "what are 500 companies *collectively* saying about the economy?" (aggregate text → macroeconomic indicator). The unit of analysis is the economy, not the firm.

**Why this framing matters for the portfolio:** This is not a "financial NLP" project (saturated). It's a **reproducibility study of central bank research methodology** — a category with zero existing portfolio projects, active institutional interest (the ECB blogged about this on Feb 16, 2026 — three days ago), and a clear gap between published methodology and open-source implementation.

---

## 2. Data Strategy — The Two-Geography Approach

The initial assessment flagged data scale as the critical risk: the ECB uses ~563 euro area firms per quarter, while FMP free tier yields ~50-80 European companies. This led to a "table it" recommendation.

**The fix:** Don't try to be an EU-only project. Build two indicators:

### Geography 1: United States (primary analysis)

**Source:** FMP API free tier (250 requests/day)
- Coverage: 8,000+ US publicly traded companies with earnings transcripts
- S&P 500 companies: essentially complete coverage
- Historical depth: 10+ years of transcripts available
- Practical corpus: ~400-500 S&P 500 companies × 8 quarters = 3,200-4,000 transcripts
- **API budget:** 3,200 transcripts = 3,200 API calls. At 250/day, that's 13 days of downloading. Totally workable.

**Validation data (all free):**
- FRED API: quarterly GDP growth, unemployment, industrial production, CPI
- ISM PMI: publicly available headline figures (monthly)
- University of Michigan Consumer Sentiment: publicly available
- Conference Board Leading Economic Index: published monthly

**Why this works:** With 400-500 companies per quarter, the aggregate signal should be strong. The US economy is well-measured with timely statistics, providing solid validation targets. This is where the methodology proves (or disproves) itself.

### Geography 2: Europe (secondary analysis)

**Source:** FMP API — European companies with US ADR listings or US exchange presence.

**Known European companies on FMP** (all have ADRs or direct US listings):
- **Tech/Industrial:** ASML (ASML), SAP (SAP), Siemens (SIEGY), Infineon (IFNNY), Philips (PHG), Nokia (NOK), Ericsson (ERIC), ABB (ABB)
- **Financial:** ING (ING), Deutsche Bank (DB), UBS (UBS), Credit Suisse (now UBS), BNP (BNPQY), Barclays (BCS), HSBC (HSBC)
- **Consumer/Luxury:** Nestlé (NSRGY), Unilever (UL), LVMH (LVMUY), Diageo (DEO), Anheuser-Busch InBev (BUD), Danone (DANOY)
- **Energy/Materials:** Shell (SHEL), TotalEnergies (TTE), BP (BP), ArcelorMittal (MT), Rio Tinto (RIO)
- **Pharma/Health:** AstraZeneca (AZN), Novartis (NVS), Novo Nordisk (NVO), GSK (GSK), Sanofi (SNY), Roche (RHHBY)
- **Telecom/Utilities:** Vodafone (VOD), Enel (ENLAY), Iberdrola (IBDRY)

**Estimated practical yield:** 60-100 European-headquartered companies with FMP transcripts available. This needs empirical verification (see Section 6), but the large-cap European coverage through ADR listings is wider than initially assumed.

**Validation data (all free):**
- Eurostat API (`eurostat` Python package): quarterly GDP, employment, industrial production, business confidence
- ECB Data Portal (`ecbdata` Python package): lending surveys, HICP inflation, financial conditions
- European Commission ESI (Economic Sentiment Indicator): monthly
- S&P Global Euro Area PMI: publicly available headline figures

### Why Two Geographies Is Better Than One

The comparison itself becomes a research contribution:

1. **Signal vs. noise:** Does the indicator work better with 400 firms (US) than with 80 firms (Europe)? At what sample size does the signal emerge?
2. **Cross-geography validation:** Does US corporate sentiment predict Euro area GDP? (The ECB's own data shows global risk perceptions correlate across regions.)
3. **Data availability frontier:** Explicitly documents what free data can and cannot replicate — honest about the boundary between open-source and institutional capabilities.
4. **Portfolio coherence:** The European validation against Eurostat/ECB data maintains the EU connection. The US analysis is the robust proof-of-concept.

---

## 3. Methodology — What Gets Built

### Step 1: Transcript Acquisition Pipeline

```
FMP API → raw JSON transcripts → cleaned text
  - Download S&P 500 transcripts (Q1 2020 – Q4 2025, ~24 quarters)
  - Download European ADR-listed company transcripts (same period)
  - Parse: separate prepared remarks from Q&A
  - Metadata: company ticker, sector (GICS), headquarters country, quarter, date
```

### Step 2: Two Extraction Methods (Compared)

**Method A — ECB/Ruch-Taskin keyword approach (baseline):**

Replicate the published methodology:
- Define keyword clusters for economic themes:
  - **Demand:** demand, consumer spending, orders, bookings, sales volume, customer traffic
  - **Supply:** supply chain, bottleneck, supply disruption, supply shortage, supply constraint, supply tightness, inventory shortage
  - **Inflation:** inflation, price increases, cost pressure, input costs, wage pressure
  - **Hiring/Labour:** hiring, recruitment, headcount, workforce, labour shortage, talent
  - **Investment:** capital expenditure, capex, investment, expansion, capacity
  - **Financing conditions:** interest rate, credit conditions, refinancing, borrowing cost
  - (ECB's published clusters provide the starting point; Loughran-McDonald financial lexicon supplements)
- For each keyword hit, score sentiment in a ±N-word window using Loughran-McDonald positive/negative word lists
- Aggregate per-firm → per-sector → economy-wide quarterly indices
- Z-score each index against its historical mean/stdev

**Method B — LLM structured extraction:**

For each transcript (or sampled subset), use an LLM to extract structured signals:
```json
{
  "company": "ASML",
  "quarter": "Q3 2024",
  "economic_signals": [
    {
      "topic": "demand",
      "sentiment": "positive",
      "confidence": 0.85,
      "evidence": "Strong order pipeline...",
      "specific_figure": null
    },
    {
      "topic": "supply_chain",
      "sentiment": "negative", 
      "confidence": 0.72,
      "evidence": "Lead times remain extended...",
      "specific_figure": "18-month lead times"
    }
  ]
}
```
- Same topic categories as Method A
- Aggregate the extracted signals into the same quarterly indices
- Compare: does LLM extraction produce indices that correlate better with GDP/PMI than keyword matching?

**Why the comparison matters:** This IS the novel methodological contribution. The ECB uses keyword matching because it scales to 6,000 firms. A portfolio project with 400-500 firms can afford LLM extraction. The question — "does the LLM actually improve the macro signal?" — has not been answered in the published literature.

### Step 3: Macroeconomic Validation

For each constructed indicator (demand, supply, inflation, hiring, investment, financing):

- **Correlation analysis:** Pearson/Spearman correlation with corresponding official statistics (GDP growth, PMI, employment change, CPI)
- **Lead/lag analysis:** Does the text indicator lead the official statistic by 0, 1, or 2 quarters? (This is the central bank value proposition — earnings calls happen before GDP is published.)
- **Sector decomposition:** Does manufacturing sector sentiment track industrial production? Does services sentiment track services PMI?
- **Granger causality tests:** Does the text indicator Granger-cause the official statistic?
- **Visual overlay:** Plot the constructed indices against official time series (this is what the ECB publishes in their Economic Bulletin boxes — reproducing this visual style is compelling)

### Step 4: Sample Size Sensitivity Analysis

The novel contribution that turns the data limitation into a feature:

- Start with the full US sample (~400-500 firms per quarter)
- Randomly subsample to 200, 100, 50, 25 firms
- Re-compute the indicator at each sample size
- Measure how correlation with GDP/PMI degrades as N shrinks
- Compare against the European sample (which is naturally at N ≈ 60-80)
- **Result:** A curve showing "how many firms do you need for a reliable text-based macro indicator?" — this has direct practical value for anyone trying to build this with limited data.

---

## 4. Novelty — Deeper Check

### What exists (confirmed from prior research)

| What | Where | How B1 differs |
|---|---|---|
| ECB Economic Bulletin boxes (2023, 2024, 2025) | Published analysis, not code | B1 is open-source, reproducible, uses free data |
| Ruch & Taskin (2024, Oxford Bull. Econ. Stat.) | Academic paper, Factiva data | B1 uses free APIs, provides code, adds LLM comparison |
| Gosselin & Taskin (2023, Bank of Canada) | Staff discussion paper | Canadian context, proprietary data |
| IMF WEO Ch. 2 (Oct 2023) | Chapter in report | Inflation expectations only, NL Analytics data |
| RBA Gray et al. (2025, arXiv) | Research paper + internal data | Business liaison data (confidential), not earnings calls via API |
| ECB blog (Feb 16, 2026) | Blog post | References RBA work, no code, no open data |

### What does NOT exist (confirmed)

- **Zero** GitHub repos doing: aggregate earnings call text → macroeconomic indicator
- **Zero** open-source implementations of the ECB/Ruch-Taskin methodology
- **Zero** portfolio projects with this framing
- **Zero** comparisons of keyword vs. LLM extraction for macro indicator quality
- **Zero** analysis of "minimum firm count for reliable text-based macro indicator"

### Why the gap persists

Central banks use NL Analytics (commercial, institutional pricing). Academics use Factiva (university subscription). The methodology is published but the data pipeline is locked behind paywalls. FMP's free tier creates the first opportunity to replicate this with open tools.

---

## 5. Revised Portfolio Fit

With the "portfolio is not limited to 4 projects" constraint removed:

| Concern (from initial assessment) | Status now |
|---|---|
| "Doesn't fill PyTorch gap" | **Irrelevant.** P1 and/or P2 fill that gap independently. |
| "Overlaps L1 on LangChain technique" | **Partially addressed.** B1's LLM component is optional/comparative — the core contribution is the data engineering + econometric validation pipeline, not the extraction chain. The LLM comparison is one of several analyses, not the centrepiece. |
| "Core contribution is econometric, not ML" | **Reframed as a strength.** The portfolio now demonstrates *range* — classical ML (Steam), data engineering (Private Label), RAG/legal NLP (L1), fine-tuning (P1/P2), AND applied econometrics. This is breadth, not redundancy. |
| "Scale limitation may undermine headline result" | **Addressed by two-geography approach.** US indicator (400+ firms) should produce strong correlations. European indicator documents the boundary. Sample size sensitivity analysis turns the limitation into a methodological contribution. |

### What B1 uniquely adds to the portfolio

| Dimension | B1 contribution | Already covered by |
|---|---|---|
| Domain: macroeconomics/policy | ✅ New | Not covered by Steam, PL, L1, or P1/P2 |
| Technique: time series analysis | ✅ New | Not covered |
| Technique: econometric validation | ✅ New | Not covered |
| Data: financial API integration | ✅ New | Not covered (PL uses web scraping, L1 uses EUR-Lex) |
| Data: official statistics APIs (Eurostat, ECB, FRED) | ✅ New | Not covered |
| Skill: reproducible research | ✅ New (replicating institutional methodology) | Not covered |
| Target companies: macro-relevant | ✅ Revolut (macro conditions), Clarity AI (economic monitoring) | Partial overlap with L1 on EU regulatory angle |

### What B1 does NOT uniquely add

- LLM extraction technique (overlap with L1)
- LangChain usage (overlap with L1)
- Professional code structure (all projects should have this)

**Assessment:** B1 adds meaningful breadth to the portfolio. The overlap with L1 is limited to the LLM extraction component, which is one experiment within B1 — not the centrepiece.

---

## 6. Open Questions Requiring Empirical Testing

Before committing to a full project plan, three things need empirical verification that can't be resolved by desk research alone:

### Q1: How many European companies actually have FMP transcripts?

**Test:** Sign up for FMP free API key. Call the `Available Earnings Transcript Symbols` endpoint. Filter to known European ADR tickers. Count.

**Expected outcome:** 60-100 companies. If fewer than 40, the European analysis may need to be scoped down to a "proof of concept" rather than a full indicator.

**Time:** 1 hour.

### Q2: What's the actual transcript quality and structure from FMP?

**Test:** Download 10 sample transcripts (5 US, 5 European). Check: Is the text clean? Are prepared remarks separated from Q&A? Is there speaker identification? How long are they (word count)?

**Expected outcome:** FMP transcripts are reportedly JSON with full text, 5,000-15,000 words each. But quality for European companies (which may be transcribed from calls conducted partially in non-English languages) needs checking.

**Time:** 1 hour.

### Q3: Does a quick keyword test on 20 transcripts show any signal at all?

**Test:** Take 20 S&P 500 Q3 2024 transcripts. Apply the ECB's published demand keyword list. Count mentions. Compare the "demand sentiment" you compute against known Q3 2024 US GDP growth.

**Expected outcome:** Even with 20 companies, there should be directional alignment if the methodology is sound. If 20 transcripts produce pure noise, the project's feasibility is in question.

**Time:** 2-3 hours.

**Total pre-commitment test time: ~4-5 hours.**

---

## 7. Risk Assessment (Updated)

| Risk | Likelihood | Impact | Mitigation |
|---|---|---|---|
| US indicator doesn't correlate with GDP | Low | Fatal | The methodology is proven (ECB published correlations). With 400+ firms, the signal should emerge. If it doesn't, document why — that's also a result. |
| European indicator is too noisy | Medium | Medium | Expected and documented. The sample-size sensitivity analysis explains *why* it's noisy. US indicator is the primary result. |
| FMP transcripts have quality issues for European companies | Medium | Medium | Empirical test (Q2 above) catches this early. Fallback: restrict European analysis to companies that conduct calls in English. |
| LLM extraction doesn't improve over keyword baseline | Medium | Low | This is a valid negative result. "The ECB's keyword approach is already sufficient" is a finding. |
| Scope creeps to 6+ weeks | Medium | Medium | Hard-scope: 24 quarters of data (2020-2025), 6 topic dimensions, 2 geographies. No extensions without completing base analysis first. |
| Looks like "stock prediction" if poorly framed | Low | High | Framing is macroeconomic surveillance, not investment signal. Validation is against GDP/PMI, never stock prices. Blog title makes this explicit. |

---

## 8. Scope and Timeline (Estimated)

### Phase 1: Data Collection + Keyword Baseline (Weeks 1-2)
- [ ] FMP API integration: download S&P 500 transcripts (2020-2025)
- [ ] FMP API: download European ADR company transcripts (same period)
- [ ] Official statistics pipeline: FRED, Eurostat, ECB Data Portal integration
- [ ] Text preprocessing: cleaning, section parsing, metadata extraction
- [ ] Implement keyword extraction (ECB methodology)
- [ ] Implement Loughran-McDonald sentiment scoring
- [ ] Compute quarterly indicators (US)
- [ ] First validation: correlation + visual overlay against GDP/PMI

### Phase 2: LLM Comparison + European Analysis (Week 3)
- [ ] Implement LLM structured extraction (sample of transcripts)
- [ ] Compute LLM-based indicators
- [ ] Compare keyword vs. LLM indicator quality
- [ ] Compute European indicators (keyword method)
- [ ] European validation against Eurostat/ECB
- [ ] Sample size sensitivity analysis

### Phase 3: Deployment + Communication (Weeks 4-5)
- [ ] Streamlit dashboard: interactive indicator explorer
  - Select geography (US / Europe), topic (demand / supply / inflation / ...), time range
  - Overlay against official statistics
  - Method comparison (keyword vs. LLM)
  - Sample size sensitivity curve
- [ ] FastAPI endpoint: `/indicator?geography=US&topic=demand&method=keyword`
- [ ] Docker + deploy
- [ ] Write README, blog post, prepare interview talking points
- [ ] Full evaluation documentation

### Total: 5 weeks

---

## 9. Communication Plan (Draft)

### Resume line
"Replicated ECB macroeconomic surveillance methodology using free data: constructed text-based economic indicators from 3,200+ earnings call transcripts, achieving [r=0.XX] correlation with GDP growth. Compared keyword extraction (central bank standard) against LLM-based extraction; keyword approach matched LLM quality at 1/100th the cost. Deployed as interactive Streamlit dashboard."

### Blog post title
"What 3,000 Earnings Calls Tell Us About the Economy — and Why Central Banks Are Listening"

### Interview talking points (STAR)

**S:** Central banks (ECB, Bank of Canada, RBA) are increasingly using NLP on corporate earnings calls to construct macroeconomic indicators — but they rely on commercial data providers that charge institutional fees. Their methodology is published but not reproducible with open tools.

**T:** Replicate the ECB's published methodology using free data (FMP API, Eurostat, ECB Data Portal) and test whether LLM-based extraction improves on the keyword approach central banks use.

**A:** Built a pipeline that downloads 3,200+ quarterly earnings call transcripts via FMP, applies both keyword-based extraction (replicating ECB Economic Bulletin methodology) and LLM structured extraction, aggregates into economy-wide quarterly sentiment indices, and validates against official GDP/PMI/employment statistics. Conducted analysis for both US (400+ firms, deep coverage) and Europe (60-80 firms, testing the data frontier). Deployed as a Streamlit dashboard with Eurostat/ECB/FRED integration.

**R:** US demand sentiment index correlated at r=[X] with quarterly GDP growth, with [X]-quarter lead time. European indicator correlated at r=[X] (weaker, as expected with fewer firms). Sample size analysis showed signal emerges above ~[N] firms per quarter. LLM extraction [did/did not] significantly improve indicator quality over keywords — the ECB's approach is [efficient/limited]. The sample-size sensitivity analysis is the first open-source study of "how many firms do you need for a reliable text-based macro indicator."

---

## 10. Decision Framework

### Build B1 if:
- Empirical tests (Section 6) confirm: ≥50 European companies on FMP, transcript quality is acceptable, and 20-company keyword test shows directional signal
- The project genuinely excites you (portfolio principle: "passion is detectable")
- You're comfortable with the 5-week investment given other portfolio priorities

### Don't build B1 if:
- FMP European coverage is <30 companies (European analysis becomes too thin to be credible)
- Quick keyword test on 20 transcripts shows zero signal (methodology may not work at this scale)
- P1/P2 validation reveals a more compelling alternative that fills a bigger portfolio gap

### Downscale B1 if:
- Time is tight: drop the LLM comparison, do keyword-only. The ECB replication + validation is valuable on its own. This cuts the project to 3-4 weeks.
- European data is thin: do US-only with a documented "European coverage" appendix showing what data exists and doesn't.

---

## 11. Comparison to Initial Assessment

| Dimension | Initial assessment (Feb 19) | Updated assessment |
|---|---|---|
| **Portfolio fit** | ⚠️ "Doesn't fill PyTorch gap, overlaps L1" | ✅ Irrelevant now — portfolio not size-limited. B1 adds unique dimensions (econometrics, time series, macro domain). |
| **Data scale** | ⚠️ "50 companies vs. ECB's 563" | ✅ **Reframed.** US primary (400+), European secondary (60-80). Sample-size sensitivity analysis turns limitation into contribution. |
| **Framework identity** | ⚠️ "Not cleanly LangChain or PyTorch" | ✅ **Reframed.** B1 is a data science / applied economics project. Not every project needs to demonstrate a specific framework. Portfolio needs range, not uniformity. |
| **Scope** | ⚠️ "5-6 weeks, heavy on econometrics" | ⚠️ Still true. 5 weeks. Can be reduced to 3-4 by dropping LLM comparison. |
| **Novelty** | ✅ Strong | ✅ Still strong. Nothing has changed in 3 days. |
| **Evaluation** | ✅ Strong | ✅ Still strong. |

---

*This is the deep feasibility assessment for B1. Next step: run the empirical tests in Section 6 (4-5 hours) to confirm data availability before committing to a full project plan.*
