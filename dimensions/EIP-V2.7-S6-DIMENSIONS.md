# EIP V2.7 — §6 SIX DIMENSIONS

> **Status:** Source-of-Truth Build Spec
> **Version:** 2.7
> **Parent:** [§1–5 FOUNDATIONS](../foundations/EIP-V2.7-S1-S5-FOUNDATIONS.md)

---

## Overview

6 parallel state modules. Each module consumes raw market data and emits a **discrete state** (and optional sub-states/flags). All 6 dimensions are evaluated independently — no inter-dimension dependencies at this layer. Outputs feed into **L3 Scenario Match** and **L5 Trigger Evaluation**.

---

## Dim 1 — COMPRESSION

> Measures volatility contraction and identifies squeeze/release conditions and VCP patterns.

### States

| State | Description |
|-------|-------------|
| `EXPANDING` | Volatility expanding; BB and/or KC widening |
| `NORMAL` | Volatility within expected range |
| `CONTRACTING` | Volatility narrowing; setup forming |
| `SQUEEZE_ON` | BB fully inside KC — classic squeeze condition |
| `SQUEEZE_RELEASE` | BB exits KC — squeeze has released |
| `VCP_FORMING` | Volatility contraction pattern in progress |
| `VCP_READY` | VCP quality score ≥ 60; actionable setup |

### Inputs

| Input | Specification |
|-------|--------------|
| BB Width | Bollinger Bands — period: 20, std dev: 2 |
| KC Width | Keltner Channel — period: 20, multiplier: 1.5× ATR |
| ATR Percentile | ATR vs. 30-day rolling window |

### Key Rules

- `SQUEEZE_ON`: triggered when BB width < KC width (BB fully inside KC)
- `SQUEEZE_RELEASE`: triggered when BB width crosses above KC width after `SQUEEZE_ON`
- `VCP_READY`: requires `vcpQuality >= 60` (see `CoinStateSnapshot`)

---

## Dim 2 — DERIVATIVES

> Tracks open interest dynamics, funding rate positioning, and leverage sentiment.

### Primary States

| State | Description |
|-------|-------------|
| `QUIET` | OI z-score within ±0.5; funding 25th–75th percentile |
| `POSITIONING` | OI or funding moving toward extremes |
| `CROWDED_LONG` | Extreme funding (long) + high OI — longs overextended |
| `CROWDED_SHORT` | Extreme funding (short) + high OI — shorts overextended |
| `DELEVERAGING` | OI declining + funding normalizing — leverage unwinding |
| `OI_SURGE` | Sudden OI spike above threshold |

### Sub-States

**Funding Sub-State:**

| Sub-State | Condition |
|-----------|-----------|
| `FUNDING_EXTREME_LONG` | Funding rate at extreme positive |
| `FUNDING_EXTREME_SHORT` | Funding rate at extreme negative |
| `FUNDING_NEUTRAL` | Funding within normal bounds |
| `FUNDING_WARMING` | Funding trending toward extreme but not yet there |

**OI Slope:**

| Value | Meaning |
|-------|---------|
| `POSITIVE` | OI increasing |
| `NEGATIVE` | OI decreasing |
| `FLAT` | OI stable / sideways |

---

## Dim 3 — VOLUME

> Measures relative volume intensity and cross-validates with OI dynamics.

### States

| State | Threshold | Description |
|-------|-----------|-------------|
| `DEAD` | < 0.5× avg | Extremely low participation |
| `QUIET` | 0.5–0.8× avg | Below-average activity |
| `NORMAL` | 0.8–1.2× avg | Baseline activity |
| `ELEVATED` | 1.2–2.0× avg | Above-average participation |
| `SURGING` | Step-wise, 3+ bars | Sustained multi-bar volume expansion |
| `SPIKE` | Single bar extreme | One-bar volume anomaly |

### VOL_OI Interaction Flag

| Flag | Condition |
|------|-----------|
| `VOL_OI_CONFIRM` | Both volume and OI surging simultaneously |
| `VOL_OI_DIVERGE` | Volume surging but OI declining (potential distribution) |

---

## Dim 4 — STRUCTURE

> Identifies market structure regime via higher highs/lows and lower highs/lows.

### States

| State | Description |
|-------|-------------|
| `BOS_BULL` | Break of structure — bullish (2+ higher lows confirmed) |
| `BOS_BEAR` | Break of structure — bearish (2+ lower highs confirmed) |
| `CHoCH_BULL` | Change of character — bullish (first higher low after downtrend) |
| `CHoCH_BEAR` | Change of character — bearish (first lower high after uptrend) |
| `RANGE_BOUND` | No directional bias; oscillating within range |
| `OVEREXTENDED_UP` | Price z-score > 2.5 above mean — stretched to upside |
| `OVEREXTENDED_DOWN` | Price z-score < −2.5 below mean — stretched to downside |
| `NO_STRUCTURE` | Insufficient data or choppy price action; no pattern detected |

### Confirmation Flag

| Field | Type | Rule |
|-------|------|------|
| `structureConfirmed` | boolean | `true` if ≥ 3 confirmed higher lows (bull) or lower highs (bear) |

---

## Dim 5 — ORDERFLOW

> Assesses directional buying/selling pressure from taker flow.

### States

| State | Description |
|-------|-------------|
| `BUYING_PRESSURE` | Taker buy volume dominant |
| `SELLING_PRESSURE` | Taker sell volume dominant |
| `BALANCED` | Buy/sell taker ratio ≈ 0.5 |

### Proxy Flag

| Field | Type | Condition |
|-------|------|-----------|
| `orderflowProxy` | boolean | `true` when taker-side data is unavailable; estimate derived from trade flow heuristic |

### Consultant Rule ⚠️

> **Orderflow is a confidence MULTIPLIER, not a gate.**
> A signal is not blocked by non-confirming orderflow. Instead, confidence weight is adjusted proportionally. `BUYING_PRESSURE` boosts signal confidence; `SELLING_PRESSURE` reduces it; `BALANCED` is neutral.

---

## Dim 6 — PRICE ACTION

> Identifies Fair Value Gaps, Order Blocks, and liquidity pools relevant to the current price.

### States

| State | Description |
|-------|-------------|
| `PA_INACTIVE` | No significant PA structures near current price |
| `PA_FORMING` | FVG or OB identified but price not yet in range |
| `PA_READY` | FVG + liquidity pool both within 1× ATR of current price — actionable |

### Inputs

| Input | Description |
|-------|-------------|
| `fvgCount` | Number of unmitigated Fair Value Gaps |
| `obCount` | Number of active Order Blocks |
| `liquidityPoolCount` | Number of identified liquidity pools (equal highs/lows, stop clusters) |
| `nearestPAZoneDistance` | Distance to nearest PA zone expressed in ATR multiples |

### Activation Rule

`PA_READY` requires:
- At least 1 FVG **AND** at least 1 liquidity pool
- `nearestPAZoneDistance ≤ 1.0× ATR`

---

## CoinStateSnapshot — TypeScript Interface

Full snapshot type emitted after all 6 dimensions are evaluated for a given coin at a given timestamp.

```typescript
interface CoinStateSnapshot {
  symbol: string;
  timestamp: number;

  // Dim 1 — Compression
  compressionState: 'EXPANDING' | 'NORMAL' | 'CONTRACTING' | 'SQUEEZE_ON' | 'SQUEEZE_RELEASE' | 'VCP_FORMING' | 'VCP_READY';
  vcpQuality: number;           // 0–100; >= 60 required for VCP_READY
  enhancedVcp: boolean;         // true if VCP aligns with squeeze release

  // Dim 2 — Derivatives
  derivativesState: 'QUIET' | 'POSITIONING' | 'CROWDED_LONG' | 'CROWDED_SHORT' | 'DELEVERAGING' | 'OI_SURGE';
  fundingSubState: 'FUNDING_EXTREME_LONG' | 'FUNDING_EXTREME_SHORT' | 'FUNDING_NEUTRAL' | 'FUNDING_WARMING';
  oiSlope: 'POSITIVE' | 'NEGATIVE' | 'FLAT';

  // Dim 3 — Volume
  volumeState: 'DEAD' | 'QUIET' | 'NORMAL' | 'ELEVATED' | 'SURGING' | 'SPIKE';
  volOiFlag: 'VOL_OI_CONFIRM' | 'VOL_OI_DIVERGE' | null;

  // Dim 4 — Structure
  structureState: 'BOS_BULL' | 'BOS_BEAR' | 'CHoCH_BULL' | 'CHoCH_BEAR' | 'RANGE_BOUND' | 'OVEREXTENDED_UP' | 'OVEREXTENDED_DOWN' | 'NO_STRUCTURE';
  structureConfirmed: boolean;

  // Dim 5 — Orderflow
  orderflowState: 'BUYING_PRESSURE' | 'SELLING_PRESSURE' | 'BALANCED';
  orderflowProxy: boolean;

  // Dim 6 — Price Action
  priceActionState: 'PA_INACTIVE' | 'PA_FORMING' | 'PA_READY';

  // Diagnostics — raw values for traceability
  diagnostics: {
    bbWidth: number;
    kcWidth: number;
    atrPercentile: number;
    oiZScore: number;
    fundingPercentile: number;
    volumeMultiple: number;       // relative to rolling average
    priceZScore: number;
    takerBuySellRatio: number | null;
    fvgCount: number;
    obCount: number;
    liquidityPoolCount: number;
    nearestPAZoneDistance: number; // in ATR multiples
  };
}
```

---

*Last updated: 2026-04-12 | Next section: §7 onwards (Scenario Library, Trigger Definitions, FIRE Output Spec, Exit Management)*
