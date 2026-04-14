# Engine B — New-Listing Decision Engine
## Master Brief v0.1

**Status:** APPROVED — Owner-approved for repo commit  
**Owner:** Giiq Innovation Lab  
**Scope:** Crypto new listings only (v1). Non-crypto excluded.  
**Relationship to Engine A:** Standalone product. NOT a variant of Engine A.  
**Philosophy:** Context-first, multi-timeframe, catalyst-aware.

---

## 1. What Engine B Is

Engine B is a standalone new-listing decision engine that answers ONE question:  
> "Should I enter this newly listed token, and if yes — when, how much, and when to exit?"

It covers Day 0 through Day 7 of a token's listing lifecycle on CEX futures markets.

## 2. The 10-Layer Architecture

| Layer | Name | Purpose |
|-------|------|--------|
| L1 | Listing Gate | Is the token listed on a supported exchange with futures? |
| L2 | Category Gate | What type of token is this? (AI, Meme, L1, DeFi, Gaming, etc.) |
| L3 | Korea Catalyst Gate | Is there Korean exchange listing catalyst (Upbit/Bithumb)? |
| L4 | Volume Profile | Day 1-3 volume shape — spike, plateau, or fade? |
| L5 | Price Action Pattern | Which of the 5 archetypes does it match? |
| L6 | Magnitude Classifier | Micro (<50%), Standard (50-200%), Explosive (>200%) move? |
| L7 | Timing Window | Optimal entry timing based on archetype |
| L8 | Position Sizing | Risk-calibrated size based on magnitude + confidence |
| L9 | Exit Framework | TP/SL rules per archetype |
| L10 | Post-Trade Review | Did the trade match the predicted archetype? |

## 3. The 5 Price Action Archetypes (Day 1-7)

| # | Archetype | Pattern | Entry Logic |
|---|-----------|---------|------------|
| A1 | Spike & Fade | Sharp pump D1, steady decline D2-7 | Short after D1 peak confirmation |
| A2 | Slow Grind Up | Gradual accumulation D1-3, breakout D4-7 | Long on D2-3 accumulation |
| A3 | V-Shape Recovery | Dump D1-2, strong recovery D3-5 | Long on D2-3 reversal signal |
| A4 | Range-Bound | Tight range D1-7, no clear direction | No trade — skip |
| A5 | Parabolic Extension | Continuous pump D1-5+ | Trail stop long, no counter-trade |

## 4. Korea Catalyst Framework

Korean exchange listings (Upbit, Bithumb) create a secondary catalyst window.  
- **Pre-listing signal:** Token announced for Korean exchange listing  
- **Magnitude amplifier:** Korean listings typically add 30-80% to base move  
- **Timing:** Usually D2-D5 after initial CEX listing  
- **Action:** Upgrade magnitude classification by one tier when Korea catalyst confirmed

## 5. Research Tokens Analyzed (Chat 11)

| Token | Exchange | Listing Date | Archetype | Notes |
|-------|----------|-------------|-----------|-------|
| RAVE | Bitunix | 2025-01 | Spike & Fade | Extreme D1 spike, rapid fade |
| BASED | Bitunix | 2025-01 | Slow Grind Up | Gradual accumulation pattern |
| PRL | Bitunix | 2025-01 | V-Shape Recovery | D1-2 dump, D3+ recovery |
| EDGE | Bitunix | 2025-01 | Range-Bound | Low volatility, no clear trend |
| + 11 others | Various | Various | Mixed | See research log |

## 6. Data Sources Required

- **Bitunix:** /markets/ranking-futures/new (new listing feed)
- **Coinglass:** /currencies/{symbol} (OI, liquidation, funding)
- **TradingView:** USDT.P perpetual charts (price action, volume)
- **Exchange APIs:** Listing announcements, Korea exchange feeds

## 7. Engine B vs Engine A — Key Differences

| Dimension | Engine A | Engine B |
|-----------|----------|----------|
| Scope | Established tokens | New listings (D0-D7) |
| Data history | Months-years | Hours-days |
| Indicators | Full TA stack | Price action archetypes |
| Triggers | Technical signals | Listing event + catalyst |
| Holding period | Swing (days-weeks) | Scalp-to-swing (hours-days) |
| Risk model | Standard sizing | Magnitude-calibrated |

## 8. Spec Document Index

| Doc ID | Title | Status |
|--------|-------|--------|
| EIP-EB-SPEC-v0.1 | Full 10-Section Spec (Chat 2) | PENDING CALIBRATION |
| EIP-EB-DATA-CONTRACT | Data Architecture (Chat 8) | ALIGNED |
| EIP-EB-RESEARCH-LOG | 15-Token Research (Chat 11) | COMPLETE |
| EIP-EB-QA-LOG | Cross-Chat Q&A Log | IN PROGRESS |

## 9. Open Calibration Items

- [ ] Finalize magnitude thresholds per archetype
- [ ] Backtest Korea catalyst amplifier on 20+ tokens
- [ ] Define minimum volume threshold for L4
- [ ] Set confidence score weights for each layer
- [ ] Validate exit framework TP/SL ratios

## 10. Change Log

| Date | Change | Approved By |
|------|--------|------------|
| 2025-01 | Initial master brief created from Chat 11 research | Owner |
| 2025-01 | 15-token research complete, archetypes validated | Owner |
| 2025-01 | Committed to eip-v2-spec repo | Owner |

---

**CHANGE REQUEST #0012**  
Action: Commit Engine B Master Brief to repo  
Status: APPROVED — Owner authorized commit  
