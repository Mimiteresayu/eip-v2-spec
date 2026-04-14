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
