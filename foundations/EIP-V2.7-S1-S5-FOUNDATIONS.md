# EIP V2.7 — FOUNDATIONS (§1–§5)

> **Status:** Source-of-Truth Build Spec  
> **Version:** 2.7  
> **Sections:** §1 Purpose · §2 Product · §3 Architecture · §4 Universe · §5 Global Context

---

## §1 PURPOSE

Source-of-truth build spec for EIP V2.7. Identifies entry-ready altcoins **NOW** using a layered decision process.

**Core Principles:**
1. **Detect alignment** — not predict spikes
2. **Separate setup from trigger** — setup conditions must be met before trigger evaluation
3. **Every output traceable** — all signals map to a deterministic rule path

---

## §2 PRODUCT

A **multi-dimensional signal engine** operating across 5 tiers, generating **FIRE signals** when a scenario + trigger align within killzone windows.

**Scope boundaries:**
- ✅ Output = signals only
- ❌ Not a portfolio manager
- ❌ Not a backtest engine
- ❌ Not an execution engine

**Output channels:**
- Telegram alerts
- Dashboard UI

---

## §3 ARCHITECTURE

7-Layer Sequential Narrowing — data flows **top-down only**, no upstream modification.

| Layer | Name | Function |
|-------|------|----------|
| L1 | Universe Management | Define and filter the tradeable universe |
| L2 | Global Context | Assess macro/BTC regime and risk state |
| L3 | Scenario Match | Match asset to qualifying scenario patterns |
| L4 | Activity Layer | Evaluate on-chain / volume / OI activity |
| L5 | Trigger Evaluation | Confirm entry trigger within killzone |
| L6 | FIRE Output | Emit signal with full rule-path trace |
| L7 | Exit Management | Monitor and close active signal positions |

> **Invariant:** No layer may modify inputs or state of any upstream layer.

---

## §4 UNIVERSE

**3-Stage Pipeline:** Full → Tradeable → Active

**5 Tiers by Market Cap:**

| Tier | Label | Market Cap Range | Notes |
|------|-------|-----------------|-------|
| 1 | MEGA | > $10B | Highest data quality requirements |
| 2 | MID | $1B – $10B | Standard thresholds |
| 3 | SMALL | $100M – $1B | Adjusted sizing |
| 4 | MICRO | $10M – $100M | Tighter timeouts, reduced sizing |
| 5 | NEW | < 30 days old | Elevated data quality scrutiny |

Tier determines: **thresholds · timeouts · sizing · data quality requirements**

---

## §5 GLOBAL CONTEXT

### BTC Regime

| Regime | Description | Scenario Multiplier |
|--------|-------------|--------------------|
| BULL | Sustained uptrend | Elevated |
| BEAR | Sustained downtrend | Reduced |
| TRANSITION | Regime change in progress | Cautious |
| RANGE | Sideways / no trend | Neutral |

### System Risk Overlay

| State | Multiplier | Trigger Condition | Behavior |
|-------|-----------|-------------------|----------|
| NORMAL | 1.0x | Default | Full signal output |
| ELEVATED | 0.5x | BTC -5% in 24h | Halved signal output |
| HIGH | 0x | Severe conditions | No new FIRE signals |
| CRISIS | Exit-only | Extreme conditions | Close active signals only |

---

*Last updated: 2026-04-12 | Next section: §6 onwards (Scenario Library, Trigger Definitions, FIRE Output Spec, Exit Management)*
