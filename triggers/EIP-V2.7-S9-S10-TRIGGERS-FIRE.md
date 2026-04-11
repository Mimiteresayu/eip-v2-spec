# EIP V2.7 §§9–10 — TRIGGER MODULES & FIRE OUTPUT

> **Spec Version:** 2.7  
> **Sections:** §9 Trigger Modules, §10 FIRE Output  
> **Status:** Source-of-truth

---

## §9 TRIGGER MODULES

5 trigger types. Each evaluated **only** when the coin is in `AIMING` state.

### 9.1 Trigger Definitions

#### T1 — SQUEEZE_RELEASE

| Field | Value |
|-------|-------|
| **Condition** | BB exits KC after `SQUEEZE_ON` |
| **Valid Scenarios** | `TECHNICAL_BREAKOUT` only |
| **Confidence** | `HIGH` with `VCP_READY`, `MEDIUM` otherwise |
| **Entry** | Release bar close |
| **Stop** | Opposite squeeze range |
| **Target** | 1.5–2.5x ATR |

#### T2 — VCP_BREAKOUT

| Field | Value |
|-------|-------|
| **Condition** | Price breaks pivot level on `VCP_READY` (quality >= 60) with RVOL >= 1.5 |
| **Valid Scenarios** | `TECHNICAL_BREAKOUT` |
| **Entry** | Breakout bar close |
| **Stop** | Last contraction low |
| **Target** | Measured move |

#### T3 — DISPLACEMENT

| Field | Value |
|-------|-------|
| **Condition** | Single candle: body >= 2x ATR, body/range >= 0.7, RVOL >= 2.0 |
| **Valid Scenarios** | `TECHNICAL_BREAKOUT`, `LEVERAGE_SQUEEZE` |
| **Entry** | FVG retest (see consultant rule below) |
| **Stop** | Displacement candle midpoint |
| **Target** | Extension of displacement move |

> ⚠️ **Consultant Rule (DISPLACEMENT):**  
> Do **NOT** fire immediately on displacement detection.  
> **Primary:** Wait for FVG retest before FIRE.  
> **Fallback:** If price extends > 2x ATR beyond displacement without retest within 15 min, FIRE with **50% position size**.

#### T4 — SWEEP_REVERSAL

| Field | Value |
|-------|-------|
| **Condition** | Price penetrates swing H/L by >= 0.5x ATR, then reverses. 15 min candle close back inside swept level. OrderFlow must show reversal. |
| **Valid Scenarios** | `MEAN_REVERSION`, `LEVERAGE_SQUEEZE` |
| **Entry** | Reversal confirmation candle close |
| **Stop** | Beyond sweep extreme |
| **Target** | Mean / prior structure level |

#### T5 — FUNDING_FLIP

| Field | Value |
|-------|-------|
| **Condition** | Funding rate crosses from extreme to neutral while OI remains high |
| **Valid Scenarios** | `LEVERAGE_SQUEEZE` only |
| **Entry** | Flip confirmation candle |
| **Stop** | Pre-flip extreme |
| **Target** | Squeeze resolution level |

### 9.2 Trigger–Scenario Compatibility Matrix

| Trigger | TECHNICAL_BREAKOUT | LEVERAGE_SQUEEZE | MEAN_REVERSION |
|---------|:-:|:-:|:-:|
| SQUEEZE_RELEASE | ✅ | — | — |
| VCP_BREAKOUT | ✅ | — | — |
| DISPLACEMENT | ✅ | ✅ | — |
| SWEEP_REVERSAL | — | ✅ | ✅ |
| FUNDING_FLIP | — | ✅ | — |

### 9.3 Per-Tier Thresholds

All trigger-specific numeric thresholds (RVOL multipliers, ATR multiples, quality floors) are stored in `TierConfig` and vary by tier (MEGA through NEW).

---

## §10 FIRE OUTPUT

### 10.1 FireSignal Contract

```typescript
interface FireSignal {
  symbol:          string;           // e.g. "ETHUSDT"
  tier:            Tier;             // MEGA | MID | SMALL | MICRO | NEW
  direction:       'LONG' | 'SHORT';
  scenario:        ScenarioType;     // TECHNICAL_BREAKOUT | LEVERAGE_SQUEEZE | MEAN_REVERSION
  scenarioTag:     string;           // META_VCP_DERIV | META_VCP | STANDARD | ...
  trigger:         TriggerType;      // SQUEEZE_RELEASE | VCP_BREAKOUT | DISPLACEMENT | SWEEP_REVERSAL | FUNDING_FLIP
  confidence:      'HIGHEST' | 'HIGH' | 'MEDIUM' | 'LOW';
  pSpike:          number;           // probability % of spike
  entry:           number;           // entry price
  stopLoss:        number;           // stop-loss price
  tp1:             number;           // target 1
  tp2:             number;           // target 2
  tp3:             number;           // target 3
  riskRewardRatio: number;           // R:R
  killzone:        string;           // active killzone window
  positionSize:    number;           // position size %
  dominantDriver:  string;           // primary catalyst
  auditTrail: {
    dimensionSnapshots: CoinStateSnapshot;  // full 6-dim state at FIRE time
    triggerEvidence:    Record<string, any>; // raw trigger data
  };
  expiresAt:       Date;             // signal expiry timestamp
}
```

### 10.2 Telegram Alert Format

```
🔥 FIRE SIGNAL
────────────────
Symbol:      {symbol} {direction}
Scenario:    {scenario} [{scenarioTag}]
Trigger:     {trigger}
Confidence:  {confidence}
────────────────
Entry:       {entry}
Stop:        {stopLoss}
TP1:         {tp1}
TP2:         {tp2}
TP3:         {tp3}
R:R:         {riskRewardRatio}
────────────────
Driver:      {dominantDriver}
```

---

> **Next:** §11 Exit Management
