# EIP V2.7 — Entry Intelligence Platform

> Source-of-truth build specification for EIP V2.7  
> Owner: Giiq Innovation Lab

---

## What is EIP?

A multi-dimensional signal engine that identifies entry-ready altcoins using a layered decision process. Operates across 5 tiers, generating FIRE signals when scenario + trigger conditions align within killzone windows.

## Repo Structure

```
eip-v2-spec/
├── foundations/          §1-§5  Purpose, Product, Architecture, Universe, Global Context
├── dimensions/           §6     Six Dimensions spec
├── scenarios/            §7-§8  Scenarios & Activity Layer
├── triggers/             §9-§10 Triggers & FIRE Output
├── engine/
│   ├── EIP-V2.7-S11-S13    §11-§13 Confidence, Sizing & Exit (Engine A)
│   └── engine-b/           Engine B — New-Listing Decision Engine
│       ├── EIP-EB-MASTER-BRIEF.md
│       ├── EIP-EB-SPEC-v0.1.md
│       ├── EIP-EB-DATA-CONTRACT.md
│       └── EIP-EB-QA-LOG.md
├── dashboard/            §14    Dashboard Display Philosophy
└── README.md
```

## Spec Index

### Engine A (EIP V2.7 Core)

| Section | Title | File |
|---------|-------|------|
| §1-§5 | Foundations (Purpose, Product, Architecture, Universe, Global Context) | `foundations/` |
| §6 | Six Dimensions | `dimensions/` |
| §7-§8 | Scenarios & Activity Layer | `scenarios/` |
| §9-§10 | Triggers & FIRE Output | `triggers/` |
| §11-§13 | Confidence, Sizing & Exit | `engine/` |
| §14 | Dashboard Display Philosophy | `dashboard/` |

### Engine B (New-Listing Decision Engine)

| Document | Description | Status |
|----------|-------------|--------|
| Master Brief | 10-layer architecture, 5 archetypes, Korea catalyst | APPROVED |
| Spec v0.1 | Full 10-section spec (§1-§10) | PENDING CALIBRATION |
| Data Contract | 14 entities, schemas, relationships, API endpoints | ALIGNED |
| Q&A Log | Cross-chat decisions, open questions, research log | IN PROGRESS |

## Engine B Overview

Engine B is a **standalone** new-listing decision engine (NOT a variant of Engine A).  
Scope: Crypto new listings only (v1), Day 0-7 lifecycle, CEX futures.

**10-Layer Architecture:** Listing Gate > Category Gate > Korea Catalyst Gate > Volume Profile > Price Action Pattern > Magnitude Classifier > Timing Window > Position Sizing > Exit Framework > Post-Trade Review

**5 Price Action Archetypes:** Spike & Fade | Slow Grind Up | V-Shape Recovery | Range-Bound | Parabolic Extension

## Core Principles

1. **Detect alignment** — not predict spikes
2. **Separate setup from trigger** — setup conditions must be met before trigger evaluation
3. **Every output traceable** — all signals map to a deterministic rule path

## Status

- Engine A (§1-§14): **COMPLETE** — all sections committed
- Engine B: **PENDING CALIBRATION** — spec committed, awaiting backtest validation

---

*Maintained by Giiq Innovation Lab*
