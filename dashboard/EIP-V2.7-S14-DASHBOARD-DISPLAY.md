# EIP V2.7 §14 — DASHBOARD DISPLAY PHILOSOPHY

> **Spec Version:** 2.7  
> **Section:** §14 Dashboard Display  
> **Status:** Source-of-truth

---

## 14.1 Core Principle: Layers First, Score Second

A single confidence score **flattens critical insight**. Two coins scoring 72 can have completely different risk profiles and setup types. The dashboard must preserve the full dimensional narrative.

**Design rule:** The primary interface displays the **full layer state** of each coin. The confidence score exists only as a secondary sorting/gating/sizing tool.

This directly supports §1 Principle 3: *"Every output traceable to rule path."*

---

## 14.2 Dimension State Grid (Primary View)

Every coin in `AIMING` or `FIRE` state renders a **6-dimension state grid** showing the complete CoinStateSnapshot:

```
ETH/USDT  [AIMING > TECHNICAL_BREAKOUT]  Tier: MEGA
+---------------+---------------+---------------+
| COMPRESSION   | DERIVATIVES   | VOLUME        |
| G SQUEEZE_ON  | Y QUIET       | G SURGING     |
| VCP: 78       | OI: +0.3z     | 2.4x avg      |
+---------------+---------------+---------------+
| STRUCTURE     | ORDERFLOW     | PRICE ACTION  |
| G BOS_BULL    | G BUYING      | Y PA_FORMING  |
| confirmed Y   | proxy: false  | FVG: 2        |
+---------------+---------------+---------------+
Scenario: TECHNICAL_BREAKOUT [META_VCP_DERIV]
Whale: NEUTRAL | BTC Regime: BULL | Risk: NORMAL
Confidence: 78 (HIGH) | Size: 2.8%
```

### Color Coding

| Color | Meaning |
|-------|--------|
| **Green (G)** | Dimension state aligned with matched scenario (required or supportive pass) |
| **Yellow (Y)** | Dimension neutral or not yet aligned |
| **Red (R)** | Dimension opposing scenario direction |
| **Grey** | Dimension not applicable to this scenario |

### Grid Cell Content

Each cell shows:
1. **State label** (e.g., `SQUEEZE_ON`, `BOS_BULL`)
2. **Key diagnostic value** from CoinStateSnapshot.diagnostics (e.g., VCP quality, OI z-score, volume multiple)

---

## 14.3 Role of Confidence Score

The confidence score (§11) is **NOT** the primary decision interface. It serves three specific functions:

| Function | How Score Is Used |
|----------|------------------|
| **Ranking** | Sort multiple AIMING coins when 5+ candidates exist simultaneously |
| **Gating** | Minimum `MEDIUM` (>=55) threshold required for FIRE transition (§8.5) |
| **Sizing** | Multiplier applied to base position size (§12.2) |

The score appears as a **single line** below the dimension grid, never as the primary element.

---

## 14.4 Dashboard Views

### View 1: Global Context Bar (Always Visible)

```
BTC: BULL | Risk: NORMAL (1.0x) | Whale: NEUTRAL | Active: 12 AIMING, 3 FIRE
```

### View 2: AIMING Queue

Table of all coins in AIMING state, each row expandable to show the full dimension grid:

| Column | Content |
|--------|---------|
| Symbol | Coin + tier badge |
| Scenario | Matched scenario + tag |
| Dims | Mini 6-cell color bar (GGYGYG) for at-a-glance alignment |
| Score | Confidence score + label |
| Timer | Time remaining before AIMING timeout |
| Detail | Expandable full dimension grid |

### View 3: FIRE Signals (Active)

Full FireSignal display with:
- Dimension grid at time of FIRE (frozen snapshot)
- Entry / Stop / TP levels with current price distance
- Exit progress (TP1 hit? trailing active?)
- Audit trail link (click to see full rule path)

### View 4: Signal History

Completed signals with outcome tracking:
- Which TPs were hit
- Was it a time exit or stop exit?
- Dimension grid at entry vs exit (what changed?)

---

## 14.5 Why This Matters

| Approach | Strength | Weakness |
|----------|----------|----------|
| Score only | Fast to scan | Hides WHY, flattens edge cases, indistinguishable setups |
| Layers only | Full transparency | Slow to compare 10+ coins |
| **Layers + Score** (this spec) | Full transparency with quick ranking | Requires slightly more UI space |

The layer-first approach preserves what makes EIP valuable: you see the **state of the market** around each coin, not just a number. The score handles the operational tasks (ranking, gating, sizing) where a single number is actually appropriate.

---

> **EIP V2.7 Spec: §1–14 Complete**
