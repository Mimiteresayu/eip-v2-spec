# EIP V2.7 §§11–13 — CONFIDENCE SCORING, POSITION SIZING & EXIT MANAGEMENT

> **Spec Version:** 2.7  
> **Sections:** §11 Confidence Scoring, §12 Position Sizing, §13 Exit Management  
> **Status:** Source-of-truth

---

## §11 CONFIDENCE SCORING

### 11.1 Base Score

```
base_confidence = 50
```

### 11.2 Scoring Components

| Component | Condition | Points |
|-----------|-----------|--------|
| **Scenario Quality** | All required + >= 2 supportive dims pass | +20 |
| | All required + 1 supportive dim passes | +10 |
| | Only required dims pass | +0 |
| **VCP Bonus** | `META_VCP_DERIV` tag | +15 |
| | `META_VCP` tag | +10 |
| | Otherwise | +0 |
| **Volume-OI Interaction** | `VOL_OI_CONFIRM` | +10 |
| | `VOL_OI_DIVERGE` | -10 |
| **OrderFlow Alignment** | Aligned with scenario direction | +5 |
| | Opposed to scenario direction | -5 |
| **Timeframe Confirmation** | `confirmedTimeframes = 2` (HTF + MTF) | +10 |
| | `confirmedTimeframes = 1` | +0 |
| | `confirmedTimeframes = 0` | -5 |
| **Structure Confirmation** | `structureConfirmed = true` (>= 3 HLs/LHs) | +5 |
| **Tier Discount** | Tier = `NEW` AND listing age < 14 days | -10 |

### 11.3 Global Context Multiplier

After summing all components:

```
final_confidence = (base + component_sum) * scenarioMultiplier
```

Where `scenarioMultiplier` is from §5.5 (BTC Regime table).

### 11.4 Confidence Labels

| Label | Score Range |
|-------|-------------|
| **HIGHEST** | >= 85 |
| **HIGH** | >= 70 |
| **MEDIUM** | >= 55 |
| **LOW** | < 55 |

---

## §12 POSITION SIZING

### 12.1 Base Size by Scenario Tag

| Scenario Tag | Base Size |
|--------------|----------|
| `META_VCP_DERIV` | 3.0% |
| `META_VCP` | 2.5% |
| `TECHNICAL_BREAKOUT` (STANDARD) | 2.0% |
| `LEVERAGE_SQUEEZE` | 2.0% |
| `MEAN_REVERSION` | 1.5% |

### 12.2 Confidence Multiplier

| Confidence | Multiplier |
|------------|------------|
| HIGHEST | 1.2x |
| HIGH | 1.0x |
| MEDIUM | 0.7x |
| LOW | 0.5x |

### 12.3 System Risk Multiplier

| Risk Level | Multiplier | Note |
|------------|------------|------|
| NORMAL | 1.0x | |
| ELEVATED | 0.5x | |
| HIGH | 0.0x | No new positions |
| CRISIS | 0.0x | Exit only |

### 12.4 Final Calculation

```
final_size = base_size * confidence_multiplier * risk_multiplier
```

> **Hard Cap:** Maximum single position = **5% of portfolio**, regardless of calculation output.

---

## §13 EXIT MANAGEMENT

### 13.1 Take-Profit Structure

| Level | Distance | Exit % |
|-------|----------|--------|
| **TP1** | 1.5x ATR | 40% of position |
| **TP2** | 2.5x ATR | 30% of position |
| **TP3** | Trailing stop | 30% (remaining) |

### 13.2 Trailing Stop

- **Activation:** After TP1 is hit
- **Trail distance:** 1x ATR from highest point post-entry
- Applies to remaining position (TP3 portion)

### 13.3 Time-Based Exit

If no TP hit within hold period, exit at market:

| Tier | Max Hold Period |
|------|-----------------|
| MEGA | 8 hours |
| MID | 6 hours |
| SMALL | 4 hours |
| MICRO | 3 hours |
| NEW | 2 hours |

### 13.4 Emergency Exit: LIQUIDATION_CASCADE

If system risk escalates to `HIGH` or `CRISIS` while a position is open: **immediate market exit** of all open positions.

### 13.5 Stop Loss

- Set at **trigger-specific level** (defined per trigger in §9)
- **Hard stop** — no averaging down, no manual override

---

> **EIP V2.7 Spec Complete: §1–13**
