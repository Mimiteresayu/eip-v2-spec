# EIP-EB-DATA-CONTRACT
## Engine B — Data Architecture & Entity Contract

**Version:** 0.1  
**Status:** ALIGNED (Chat 8 feasibility confirmed)  
**Origin:** Chat 8 (Data Architecture) + Chat 11 (Research)  
**Owner:** Giiq Innovation Lab

---

## 1. Entity Overview (14 Entities)

| # | Entity | Type | Source | Layer |
|---|--------|------|--------|-------|
| 1 | listing_event | Raw | Exchange API | L1 |
| 2 | exchange_support | Config | Manual | L1 |
| 3 | token_metadata | Raw | API + Manual | L2 |
| 4 | category_tag | Derived | Classification | L2 |
| 5 | korea_catalyst | Raw | Upbit/Bithumb API | L3 |
| 6 | ohlcv_1h | Raw | TradingView/Exchange | L4-L5 |
| 7 | ohlcv_4h | Raw | TradingView/Exchange | L4-L5 |
| 8 | ohlcv_1d | Raw | TradingView/Exchange | L4-L5 |
| 9 | open_interest | Raw | Coinglass API | L4 |
| 10 | funding_rate | Raw | Coinglass API | L4 |
| 11 | volume_profile | Derived | Computed from OHLCV | L4 |
| 12 | archetype_match | Derived | Engine B L5 output | L5-L6 |
| 13 | trade_signal | Derived | Engine B L7-L9 output | L7-L9 |
| 14 | post_trade_review | Derived | Engine B L10 output | L10 |

## 2. Entity Definitions

### E1: listing_event
```
listing_event {
  id: UUID
  token_symbol: STRING
  exchange: STRING
  listing_timestamp: DATETIME
  contract_type: ENUM(perpetual, quarterly)
  base_price: DECIMAL
  detected_at: DATETIME
}
```

### E2: exchange_support
```
exchange_support {
  exchange_id: STRING
  name: STRING
  futures_available: BOOLEAN
  api_endpoint: STRING
  status: ENUM(active, deprecated)
}
```

### E3: token_metadata
```
token_metadata {
  token_symbol: STRING
  full_name: STRING
  chain: STRING
  category: STRING
  market_cap_at_listing: DECIMAL
  circulating_supply: DECIMAL
}
```

### E4: category_tag
```
category_tag {
  token_symbol: STRING
  category: ENUM(AI, Meme, L1, L2, DeFi, Gaming, Infra, Other)
  confidence: DECIMAL
  tagged_by: ENUM(manual, auto)
}
```

### E5: korea_catalyst
```
korea_catalyst {
  token_symbol: STRING
  korean_exchange: ENUM(Upbit, Bithumb)
  announcement_date: DATETIME
  listing_date: DATETIME
  catalyst_active: BOOLEAN
  magnitude_uplift: DECIMAL
}
```

### E6-E8: ohlcv (1h, 4h, 1d)
```
ohlcv {
  token_symbol: STRING
  exchange: STRING
  timeframe: ENUM(1h, 4h, 1d)
  timestamp: DATETIME
  open: DECIMAL
  high: DECIMAL
  low: DECIMAL
  close: DECIMAL
  volume: DECIMAL
}
```

### E9: open_interest
```
open_interest {
  token_symbol: STRING
  exchange: STRING
  timestamp: DATETIME
  oi_value: DECIMAL
  oi_change_pct: DECIMAL
}
```

### E10: funding_rate
```
funding_rate {
  token_symbol: STRING
  exchange: STRING
  timestamp: DATETIME
  rate: DECIMAL
  next_funding_time: DATETIME
}
```

### E11: volume_profile
```
volume_profile {
  token_symbol: STRING
  day_number: INT (1-7)
  volume_24h: DECIMAL
  volume_change_pct: DECIMAL
  shape_classification: ENUM(spike, plateau, fade)
}
```

### E12: archetype_match
```
archetype_match {
  token_symbol: STRING
  archetype: ENUM(A1, A2, A3, A4, A5)
  confidence_score: DECIMAL
  magnitude_class: ENUM(micro, standard, explosive)
  korea_catalyst_active: BOOLEAN
  matched_at: DATETIME
}
```

### E13: trade_signal
```
trade_signal {
  id: UUID
  token_symbol: STRING
  direction: ENUM(long, short, skip)
  entry_price: DECIMAL
  entry_window_start: DATETIME
  entry_window_end: DATETIME
  position_size_pct: DECIMAL
  take_profit: DECIMAL
  stop_loss: DECIMAL
  trail_stop_pct: DECIMAL
  archetype: STRING
  generated_at: DATETIME
}
```

### E14: post_trade_review
```
post_trade_review {
  trade_signal_id: UUID
  actual_archetype: ENUM(A1, A2, A3, A4, A5)
  predicted_correct: BOOLEAN
  entry_accuracy_pct: DECIMAL
  magnitude_accuracy_pct: DECIMAL
  exit_efficiency_pct: DECIMAL
  pnl_pct: DECIMAL
  reviewed_at: DATETIME
}
```

## 3. Entity Relationships

```
listing_event (1) --- (1) token_metadata
listing_event (1) --- (0..1) korea_catalyst
listing_event (1) --- (N) ohlcv
listing_event (1) --- (N) open_interest
listing_event (1) --- (N) funding_rate
listing_event (1) --- (1) volume_profile
listing_event (1) --- (1) archetype_match
archetype_match (1) --- (0..1) trade_signal
trade_signal (1) --- (0..1) post_trade_review
```

## 4. Data Flow

```
[Exchange API] --> listing_event --> L1 Gate
[Manual/API] --> token_metadata --> L2 Gate
[Upbit/Bithumb] --> korea_catalyst --> L3 Gate
[TradingView] --> ohlcv --> volume_profile --> L4
[Coinglass] --> open_interest + funding_rate --> L4
ohlcv --> archetype_match --> L5-L6
archetype_match --> trade_signal --> L7-L9
trade_signal --> post_trade_review --> L10
```

## 5. Storage Requirements

| Entity | Retention | Est. Volume (per token) |
|--------|-----------|------------------------|
| listing_event | Permanent | 1 record |
| ohlcv_1h | 30 days | ~168 records/7d |
| ohlcv_4h | 90 days | ~42 records/7d |
| ohlcv_1d | Permanent | ~7 records/7d |
| open_interest | 30 days | ~2016 records/7d (5-min) |
| trade_signal | Permanent | 0-1 per token |
| post_trade_review | Permanent | 0-1 per token |

## 6. API Endpoints Required

| Source | Endpoint | Data | Rate Limit |
|--------|----------|------|------------|
| Bitunix | /markets/ranking-futures/new | New listings | TBD |
| Coinglass | /api/futures/openInterest | OI data | 30 req/min |
| Coinglass | /api/futures/funding | Funding rate | 30 req/min |
| TradingView | Chart data (websocket) | OHLCV | Streaming |
| Upbit | Announcement RSS/API | Korea catalyst | Polling 5-min |
| Bithumb | Announcement RSS/API | Korea catalyst | Polling 5-min |

---

**Document Status:** ALIGNED  
**Feasibility:** Confirmed by Chat 8 data architecture review  
