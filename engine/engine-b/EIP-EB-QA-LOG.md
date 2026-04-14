# EIP-EB-QA-LOG
## Engine B — Cross-Chat Q&A Log

**Status:** IN PROGRESS  
**Purpose:** Track all questions, decisions, and answers across Chat 2, 5, 8, 11  
**Owner:** Giiq Innovation Lab

---

## Decision Log

| # | Question | Raised By | Answer | Decided By | Date |
|---|----------|-----------|--------|------------|------|
| D1 | Should Engine B be a variant of Engine A? | Chat 2 | NO — Engine B is standalone, separate product | Owner | 2025-01 |
| D2 | Should we use current screener as baseline for Engine B? | Chat 11 | NO — totally different logic, dropped | Owner | 2025-01 |
| D3 | Should new coin listings be separated from other altcoin analysis? | Chat 11 | YES — Engine B handles new listings only | Owner | 2025-01 |
| D4 | Crypto only or include non-crypto in v1? | Chat 2 | Crypto only v1. Non-crypto excluded. | Owner | 2025-01 |
| D5 | Where to store canonical specs? | Chat 11 | GitHub repo: eip-v2-spec | Owner | 2025-01 |
| D6 | Is Korea catalyst a separate layer? | Chat 11 | YES — L3 Korea Catalyst Gate | Owner + Chat 11 | 2025-01 |

## Open Questions (Pending Calibration)

| # | Question | Owner Chat | Status |
|---|----------|-----------|--------|
| Q1 | Exact volume decline threshold for L4 "Fade" classification | Chat 11 | OPEN |
| Q2 | Confidence score weight distribution per layer | Chat 2 | OPEN |
| Q3 | Korea catalyst magnitude uplift exact percentage | Chat 11 | OPEN |
| Q4 | Minimum OI threshold for entry signal | Chat 8 | OPEN |
| Q5 | Backtest sample size before live deployment | Chat 2 | OPEN |
| Q6 | How to handle tokens listed on multiple exchanges simultaneously | Chat 8 | OPEN |
| Q7 | Social volume data source selection | Chat 8 | OPEN |

## Cross-Chat Package Tracker

| Package | From | To | Content | Status |
|---------|------|----|---------|--------|
| Chat 5 Package | Chat 11 | Chat 5 | Product-gate review of Engine B (Output A-F) | SENT |
| Chat 2 Spec | Chat 11 | Chat 2 | EIP-EB-SPEC-v0.1, 10 sections + questions | SENT |
| Chat 8 Package | Chat 11 | Chat 8 | Data architecture feasibility, 14 entities | SENT |
| Chat 8 Reply | Chat 8 | Chat 11 | Data contract alignment confirmed | RECEIVED |

## Research Log (Chat 11)

| # | Token | Exchange | Archetype | Magnitude | Korea Catalyst | Notes |
|---|-------|----------|-----------|-----------|---------------|-------|
| 1 | RAVE | Bitunix | A1 Spike & Fade | Explosive | No | Extreme D1 spike |
| 2 | BASED | Bitunix | A2 Slow Grind | Standard | No | Gradual accumulation |
| 3 | PRL | Bitunix | A3 V-Shape | Standard | No | D1-2 dump, D3 recovery |
| 4 | EDGE | Bitunix | A4 Range-Bound | Micro | No | Low volatility |
| 5-15 | Various | Various | Mixed | Mixed | Mixed | See Bitunix research screenshots |

## Change Request Log

| CR # | Description | Status |
|------|-------------|--------|
| CR-0012 | Commit Engine B Master Brief to repo | APPROVED |
| CR-0013 | Commit Engine B full spec package (4 files) | APPROVED |

---

**Last Updated:** 2025-01  
**Next Update:** After calibration backtest results from Chat 11  

---

## Calibration Research Update (2026-04-15)

### Bitunix New Listings Feed (20 tokens captured)

| # | Token | Listed | Category | 24h Vol | OI | 24h Chg | Trend |
|---|-------|--------|----------|---------|-----|---------|-------|
| 1 | UP | 2026/04/10 | Crypto | -- | -- | -- | Flat (too new) |
| 2 | PRL | 2026/04/02 | Crypto | -- | -- | -- | Flat (too new) |
| 3 | NATGAS | 2026/04/01 | Commodity | -- | -- | -- | EXCLUDED (non-crypto) |
| 4 | BZ | 2026/04/01 | Commodity | -- | -- | -- | EXCLUDED (non-crypto) |
| 5 | BASED | 2026/03/30 | Crypto | $4.04M | -- | +4.37% | Green up |
| 6 | BSB | 2026/03/26 | Crypto | $620.87K | -- | +5.87% | Green up |
| 7 | CL | 2026/03/24 | Commodity | $44.41M | -- | -5.04% | EXCLUDED (non-crypto) |
| 8 | CFG | 2026/03/17 | Crypto | $611.88K | -- | +3.41% | Green up |
| 9 | COPPER | 2026/03/06 | Commodity | $281.58K | -- | +1.21% | EXCLUDED (non-crypto) |
| 10 | MANTRA | 2026/03/04 | Crypto | $17.89M | $7.35M | -0.38% | Range/Fade |
| 11 | KATU | 2026/03/03 | Crypto | $238.43K | -- | -0.99% | Red fade |
| 12 | ROBO | 2026/02/27 | Crypto | $37.44M | $14.92M | -7.39% | Red down |
| 13 | OPN | 2026/02/24 | Crypto | $409.61K | -- | +4.36% | Green up |
| 14 | MEGA | 2026/02/13 | Crypto | $66.20K | -- | +0.60% | Green up |
| 15 | AZTEC | 2026/02/12 | Crypto | $12.37M | $10.47M | +5.40% | Green up |
| 16 | ESP | 2026/02/12 | Crypto | $427.28K | -- | -0.67% | Red fade |
| 17 | XAU | 2026/02/10 | Commodity | $84.37M | -- | +1.68% | EXCLUDED (non-crypto) |
| 18 | TRIA | 2026/02/09 | Crypto | $427.08K | -- | +1.46% | Green up |
| 19 | ZAMA | 2026/02/04 | Crypto | $441.56M | $35.37M | +34.96% | Strong green |
| 20 | XPD | 2026/02/03 | Commodity | $465.83K | -- | +1.20% | EXCLUDED (non-crypto) |

**Key Finding:** Bitunix now lists commodity pairs (NATGAS, BZ, CL, COPPER, XAU, XPD). Engine B v1 excludes non-crypto.
**Crypto tokens for analysis:** 14 out of 20 (6 commodity excluded)

### Deep Analysis (Coinglass + TradingView)

#### ZAMA (Zama) — Listed 2026/02/04
- **Coinglass:** $0.034, Futures Vol $441.56M, OI $35.37M, MCap $73.95M
- **Performance:** 24h +34.69%, 7d +56.47%, 30d +51.57%, All -45.06%
- **TradingView Chart:** D1 extreme spike to $0.23 (from ~$0.06 base), immediate crash to $0.06, then steady fade to $0.02 over 40 days, recent recovery to $0.034
- **Archetype:** A1 Spike & Fade (D0-D7 view)
- **Magnitude:** Explosive (>200% D1 intra-day spike)
- **Calibration Note:** D1 wick was 280%+ from close, confirming Explosive classification. Late-stage recovery (D40+) does not change D0-D7 archetype.

#### ROBO (Fabric Protocol) — Listed 2026/02/27
- **Coinglass:** $0.0194, Futures Vol $37.44M, OI $14.92M, MCap $43.22M
- **Performance:** 24h -7.71%, 7d +19.57%, 30d -51.86%, All -47.77%
- **TradingView Chart:** D1 spike to $0.065, D2-5 fade to $0.04, D5-10 crash to $0.015, brief dead-cat bounce to $0.04, then resumed fade
- **Archetype:** A1 Spike & Fade
- **Magnitude:** Explosive (listing spike 0.065 to current 0.019 = -70% from peak)
- **Calibration Note:** Dead-cat bounce on D10-15 is a trap. The D0-D7 pattern is unambiguously A1.

#### MANTRA — Listed 2026/03/04
- **Coinglass:** $0.01053, Futures Vol $17.89M, OI $7.35M, MCap $51.45M
- **Performance:** 24h -0.38%, 7d +0.35%, 30d -31.57%, All -32.54%
- **TradingView Chart:** Listed ~$0.011, brief spike, then steady fade to $0.0105 range
- **Archetype:** A1 Spike & Fade / A4 Range-Bound (borderline)
- **Magnitude:** Micro (<50% total move)
- **Calibration Note:** Low-magnitude A1 faders can look like A4 after D7. Need a magnitude threshold to distinguish: if All-time move <30%, classify as A4 not A1.

#### AZTEC (Aztec) — Listed 2026/02/12
- **Coinglass:** $0.02207, Futures Vol $12.37M, OI $10.47M, MCap $63.32M
- **Performance:** 24h +5.35%, 7d +18.82%, 30d -5.67%, All +14.12%
- **TradingView Chart:** D1 spike to $0.04, crash to $0.018 (D1-5), consolidated $0.018-0.024 (D5-15), then slow recovery with breakout
- **Archetype:** A1 > A3 Hybrid (Spike then V-Shape Recovery)
- **Magnitude:** Standard (50-200% initial move)
- **Calibration Note:** CRITICAL FINDING — Some tokens exhibit MULTI-PHASE archetypes. D0-D3 = A1, but D3-D7+ transitions to A3. Need to define: primary archetype (D0-D3) vs secondary archetype (D3-D7).

### Calibration Insights

| # | Insight | Impact on Spec | Status |
|---|---------|---------------|--------|
| C1 | Commodity pairs now on Bitunix — L1 gate needs exchange-type filter | Add instrument_type filter to L1 | NEW |
| C2 | A1 Spike & Fade dominates new listings (3 of 4 deep-analyzed = A1) | A1 should be default prior in L5 | NEW |
| C3 | Multi-phase archetypes exist (A1 > A3 transition) | Need primary + secondary archetype fields in L5 | NEW |
| C4 | Low-magnitude A1 looks like A4 after D7 | Set magnitude threshold: <30% all-time = reclassify as A4 | NEW |
| C5 | Dead-cat bounces are traps in A1 faders | L9 exit rules need anti-bounce filter | NEW |
| C6 | D1 wick-to-close ratio is key magnitude signal | Add wick_ratio field to L6 classifier | NEW |
| C7 | OI/volume ratio correlates with archetype persistence | High OI tokens sustain pattern longer | NEW |

### Updated Open Questions

| # | Question | Owner Chat | Status |
|---|----------|-----------|--------|
| Q8 | Should L1 gate filter by instrument type (crypto vs commodity)? | Chat 11 | PROPOSED — YES |
| Q9 | How to handle multi-phase archetype transitions (A1>A3)? | Chat 11 | OPEN |
| Q10 | What magnitude threshold separates A1-fade from A4-range? | Chat 11 | PROPOSED — 30% all-time move |
| Q11 | Should L5 output primary + secondary archetype? | Chat 2 | OPEN |
| Q12 | Dead-cat bounce detection for L9 anti-trap rules? | Chat 11 | OPEN |

### Updated Change Request Log

| CR # | Description | Status |
|------|-------------|--------|
| CR-0014 | Update README with full spec index | APPROVED |
| CR-0015 | Update Q&A log with calibration research (Apr 2026) | PENDING OWNER APPROVAL |

---

**Last Updated:** 2026-04-15  
**Next Update:** After Owner reviews calibration insights C1-C7 and Q8-Q12  
