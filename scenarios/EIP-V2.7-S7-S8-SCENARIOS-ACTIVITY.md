# EIP V2.7 §§7–8 — SCENARIO MATCHING & ACTIVITY LAYER

> **Spec Version:** 2.7  
> **Sections:** §7 Scenario Matching, §8 Activity Layer  
> **Status:** Source-of-truth

---

## §7 SCENARIO MATCHING

3 scenarios mapped from the 6-dimension state vector (see §6).

### 7.1 Scenario Definitions

| # | Scenario | Required Dimensions | Supportive Dimensions | Tags |
|---|----------|--------------------|-----------------------|------|
| 1 | **TECHNICAL_BREAKOUT** | Compression (`SQUEEZE_ON` \| `VCP_READY`), Structure (`BOS_BULL` \| `BOS_BEAR`), Volume (`>= ELEVATED`) | Derivatives, OrderFlow, PriceAction | `META_VCP_DERIV` (VCP + derivatives), `META_VCP` (VCP only), `STANDARD` |
| 2 | **LEVERAGE_SQUEEZE** | Derivatives (`CROWDED_LONG` \| `CROWDED_SHORT`), Volume (`>= ELEVATED`), Structure (`OVEREXTENDED_UP` \| `OVEREXTENDED_DOWN` \| `CHoCH`) | Compression, OrderFlow, PriceAction | — |
| 3 | **MEAN_REVERSION** | Structure (`OVEREXTENDED_UP` \| `OVEREXTENDED_DOWN`), PriceAction (`PA_READY`), OrderFlow (opposing current trend) | Derivatives, Volume, Compression | — |

### 7.2 Matcher Output

The scenario matcher returns **ranked candidates** with a full audit trail showing which dimensions passed/failed for each scenario evaluated.

```
ScenarioMatchResult {
  scenario:       ScenarioType;       // TECHNICAL_BREAKOUT | LEVERAGE_SQUEEZE | MEAN_REVERSION
  matched:        boolean;
  requiredDims:   DimResult[];        // pass/fail per required dimension
  supportiveDims: DimResult[];        // pass/fail per supportive dimension
  tags:           string[];           // e.g. ['META_VCP_DERIV']
  rank:           number;             // candidate ranking
}
```

---

## §8 ACTIVITY LAYER

### 8.1 Lifecycle States

```
IDLE  ──>  AIMING  ──>  FIRE  ──>  (Exit Management)
 ```

| State | Description |
|-------|-------------|
| **IDLE** | Default state. Coin monitored but no scenario alignment detected. |
| **AIMING** | Activity score threshold met — weighted sum of dimension alignment matches scenario requirements. |
| **FIRE** | AIMING + trigger module fires + confidence >= `MEDIUM` + within killzone window. |

### 8.2 Activity Score

```
activityScore = SUM( dimensionWeight[i] * stateMatchesScenario[i] )
```

AIMING activates when `activityScore >= threshold` (threshold varies by scenario).

### 8.3 AIMING Timeout by Tier

| Tier | Timeout |
|------|---------|
| MEGA | 4 hours |
| MID | 3 hours |
| SMALL | 2 hours |
| MICRO | 1.5 hours |
| NEW | 1 hour |

### 8.4 Cancellation Conditions (AIMING → IDLE)

| # | Condition |
|---|-----------|
| 1 | Compression returns to `NORMAL` or `EXPANDING` |
| 2 | Structure breaks against scenario direction (CHoCH opposite) |
| 3 | Timeout reached (per tier table above) |
| 4 | System risk escalates to `HIGH` or `CRISIS` |
| 5 | Volume drops to `DEAD` or `QUIET` |

### 8.5 FIRE Transition Rule

**All** of the following must be true simultaneously:

1. Coin is in `AIMING` state
2. Trigger module fires (see §9 — future section)
3. Confidence >= `MEDIUM`
4. Current time is within a **killzone window**

---

> **Next:** §9 Trigger Definitions, §10 FIRE Output Spec, §11 Exit Management
