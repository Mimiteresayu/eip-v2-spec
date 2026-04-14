# EIP-EB-SPEC-v0.1
## Engine B — New-Listing Decision Engine Specification

**Version:** 0.1  
**Status:** PENDING CALIBRATION  
**Origin:** Chat 2 (Product Spec) + Chat 11 (Research)  
**Owner:** Giiq Innovation Lab

---

## §1. Purpose & Scope

Engine B decides whether to enter a newly listed crypto token on CEX futures.  
Scope: Day 0 to Day 7 of listing lifecycle. Crypto only (v1).

**Core Question:** "Enter or skip? If enter — when, how much, when to exit?"

## §2. Input Data Requirements

| Data Point | Source | Frequency | Required? |
|------------|--------|-----------|----------|
| Listing event | Exchange API / Bitunix feed | Real-time | YES |
| Token category | Manual tag / API metadata | On listing | YES |
| Korea catalyst | Upbit/Bithumb announcement feed | Real-time | YES |
| OHLCV (1h, 4h, 1d) | TradingView / Exchange API | Streaming | YES |
| Open Interest | Coinglass API | 5-min | YES |
| Funding Rate | Coinglass API | 8h | YES |
| Liquidation data | Coinglass API | Real-time | NICE-TO-HAVE |
| Social volume | TBD | Hourly | NICE-TO-HAVE |

## §3. Layer Processing Pipeline

```
Listing Event
  │
  ├─ L1: Listing Gate (supported exchange + futures available?)
  ├─ L2: Category Gate (token type classification)
  ├─ L3: Korea Catalyst Gate (Korean exchange listing signal?)
  ├─ L4: Volume Profile (D1-3 volume shape analysis)
  ├─ L5: Price Action Pattern (archetype matching)
  ├─ L6: Magnitude Classifier (move size classification)
  ├─ L7: Timing Window (optimal entry timing)
  ├─ L8: Position Sizing (risk-calibrated allocation)
  ├─ L9: Exit Framework (TP/SL per archetype)
  └─ L10: Post-Trade Review (archetype accuracy scoring)
```

Layers L1-L3 are GATES (pass/fail). L4-L6 are CLASSIFIERS. L7-L9 are EXECUTION. L10 is FEEDBACK.

## §4. Gate Logic (L1-L3)

### L1: Listing Gate
- Token must be listed on a supported exchange
- Futures/perpetual contract must be available
- Minimum 24h since listing (avoid first-hour chaos)
- FAIL = no trade

### L2: Category Gate
- Classify token: AI | Meme | L1 | L2 | DeFi | Gaming | Infra | Other
- Each category has different archetype probability distribution
- No category is excluded in v1 — but category informs L5-L6 priors

### L3: Korea Catalyst Gate
- Check: Is token announced/listed on Upbit or Bithumb?
- If YES: flag as Korea-catalyst-active, upgrade magnitude by one tier
- If NO: proceed with base classification
- Timing window: Korea catalyst usually fires D2-D5

## §5. Classifier Logic (L4-L6)

### L4: Volume Profile
- Measure 24h volume Day 1, Day 2, Day 3
- Classify shape:
  - **Spike:** D1 >> D2 >> D3 (declining)
  - **Plateau:** D1 ≈ D2 ≈ D3 (stable)
  - **Fade:** D1 high, D2-3 collapse (>70% drop)
- Volume shape correlates with archetype

### L5: Price Action Pattern (Archetype Matching)
- Match to one of 5 archetypes (A1-A5) based on D1-D3 price action
- Confidence score: HIGH (>70%), MEDIUM (50-70%), LOW (<50%)
- LOW confidence = reduce position size or skip

### L6: Magnitude Classifier
- **Micro:** <50% total move from listing price
- **Standard:** 50-200% total move
- **Explosive:** >200% total move
- Korea catalyst: shift up one tier
- Magnitude determines position sizing in L8

## §6. Execution Logic (L7-L9)

### L7: Timing Window
| Archetype | Entry Window | Confirmation Signal |
|-----------|-------------|--------------------|
| A1 Spike & Fade | D1 peak + 4-8h | Volume decline + lower high |
| A2 Slow Grind | D2-D3 accumulation zone | Higher lows + stable volume |
| A3 V-Shape | D2-D3 reversal candle | Volume spike on recovery |
| A4 Range-Bound | NO ENTRY | Skip |
| A5 Parabolic | D1-D2 pullback | Any dip with volume support |

### L8: Position Sizing
| Magnitude | Base Size (% of risk capital) | Korea Catalyst Adj |
|-----------|-------------------------------|--------------------|
| Micro | 0.5% | +0.25% |
| Standard | 1.0% | +0.5% |
| Explosive | 1.5% | +0.75% |

Confidence modifier: HIGH = 1.0x, MEDIUM = 0.7x, LOW = 0.5x or SKIP

### L9: Exit Framework
| Archetype | Take Profit | Stop Loss | Trail Stop |
|-----------|------------|-----------|------------|
| A1 (Short) | -30% from peak | +15% from entry | 10% trailing |
| A2 (Long) | +40% from entry | -15% from entry | 15% trailing |
| A3 (Long) | +50% from reversal | -20% from entry | 12% trailing |
| A4 | N/A (skip) | N/A | N/A |
| A5 (Long) | Trail only | -10% from entry | 8% trailing |

## §7. Feedback Loop (L10)

- After trade closes, score: Did actual match predicted archetype?
- Track: Entry accuracy, magnitude accuracy, exit efficiency
- Feed back into L5 confidence calibration
- Minimum 20 trades before adjusting model weights

## §8. Data Architecture Summary

| Entity | Description | Source |
|--------|-------------|--------|
| listing_event | New token listing record | Exchange API |
| token_metadata | Category, chain, market cap | API + manual |
| korea_catalyst | Korean exchange listing signals | Upbit/Bithumb API |
| ohlcv_1h | Hourly candles | TradingView/Exchange |
| volume_profile | D1-D3 volume analysis | Computed |
| archetype_match | Pattern classification result | Engine B L5 |
| magnitude_class | Move size classification | Engine B L6 |
| trade_signal | Entry/exit recommendation | Engine B L7-L9 |
| trade_record | Executed trade log | Broker/Exchange |
| post_trade_review | Accuracy scoring | Engine B L10 |

See: EIP-EB-DATA-CONTRACT.md for full entity-relationship spec.

## §9. Integration Points

- **Engine A:** No direct dependency. Engine B is standalone.
- **Dashboard:** Engine B signals feed into §14 Dashboard Display Layer
- **Risk Manager:** Position sizes from L8 feed into global risk limits
- **Alert System:** L1-L3 gate results trigger notifications

## §10. Open Questions & Calibration Needed

| # | Question | Owner | Status |
|---|----------|-------|--------|
| Q1 | Exact volume decline threshold for L4 "Fade" | Chat 11 | OPEN |
| Q2 | Confidence score weight per layer | Chat 2 | OPEN |
| Q3 | Korea catalyst magnitude uplift — exact % | Chat 11 | OPEN |
| Q4 | Minimum OI threshold for entry | Chat 8 | OPEN |
| Q5 | Backtest sample size before going live | Chat 2 | OPEN |

---

**Document Status:** PENDING CALIBRATION  
**Next:** Backtest with 20+ tokens, calibrate thresholds, Owner final sign-off  
