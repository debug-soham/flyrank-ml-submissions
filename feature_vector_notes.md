# Feature Vector Notes

**Assignment:** ML-05 - Feature Vector and Leakage/Privacy Check

## Lane

**Core Lane:** Search & Discoverability

---

# Objective

The goal of this project is to predict whether a content page is likely to experience a decline in search visibility. The feature vector contains historical search performance signals that are available before making a prediction.

---

# Unit of Analysis

Each row represents **one content page (URL/content item)**.

The model predicts the future performance of each page independently.

---

# Feature Vector

| Feature | Meaning | Engineered | Available at Prediction Time | Safe to Use |
|----------|---------|------------|------------------------------|-------------|
| `imp_prev45` | Impressions during the previous 45-day period | No | Yes | ✅ |
| `clk_last45` | Clicks during the latest available observation window | No | Yes | ✅ |
| `pos_last45` | Average search position | No | Yes | ✅ |
| `visible_queries` | Number of queries generating impressions | Yes | Yes | ✅ |
| `rare_share` | Percentage of low-frequency queries | Yes | Yes | ✅ |
| `anon_share` | Share of anonymous or hidden queries | Yes | Yes | ✅ |
| `top_query_share` | Percentage of traffic from the top query | Yes | Yes | ✅ |
| `clicks_per_impression` | Click-through ratio (`clicks ÷ impressions`) | Yes | Yes | ✅ |

---

# Engineered Features

Several features are derived from raw warehouse data rather than stored directly.

### clicks_per_impression

Formula:

```
clicks_per_impression =
clicks / impressions
```

Purpose:

Measures how effectively impressions convert into clicks.

---

### rare_share

Represents the proportion of search traffic coming from uncommon search queries.

Purpose:

May indicate whether traffic depends on long-tail searches.

---

### top_query_share

Measures how dependent a page is on a single search query.

Higher values may indicate less diversified search traffic.

---

### anon_share

Represents the proportion of hidden or anonymous search queries.

This may indicate limited visibility into query distribution.

---

# Categorical Handling

The selected feature vector contains primarily numerical variables.

No categorical encoding is required in the baseline model.

If future versions include categorical fields (such as content category or page type), they should be encoded using techniques such as one-hot encoding before training.

---

# Missing Values

Potential missing values may occur because:

- pages have insufficient historical data
- some query statistics are unavailable
- divisions produce undefined values (for example, zero impressions)

Handling strategy:

- remove rows missing required training features
- replace undefined ratios with zero where appropriate
- verify missingness before training

Example:

```
clicks_per_impression = 0

if impressions = 0
```

---

# Prediction-Time Availability

Only features that are known before prediction should be used.

Safe examples include:

- historical impressions
- historical clicks
- CTR
- average position
- historical query statistics

Features generated after the prediction period must not be included.

---

# Approximate Dataset Size

Each row corresponds to one content page.

The exact row count depends on filtering criteria such as minimum historical impressions and available reporting history.

Only pages with sufficient historical observations are included in training.

---

# Leakage and Privacy Check

## Private Identifiers

| Field | Use? | Reason |
|------|------|--------|
| `client_hash_id` | ❌ | Identifier only; not predictive |
| `content_hash_id` | ❌ | Identifier only; risk of memorization |
| URL | ❌ | May reveal customer information |
| Raw search queries | ❌ | May contain sensitive information |

These fields should never be used as default model features.

---

## Future-Window Leakage

Unsafe examples:

- future impressions
- future clicks
- future CTR
- future ranking
- any statistics calculated after the prediction date

These directly reveal the outcome being predicted and would artificially inflate model performance.

---

## Product-Flag Leakage

The following should **not** be used as training features if present:

- recommendation flags
- internal product decisions
- optimization status
- manual review labels
- intervention flags

These variables already encode business decisions and may leak target information.

---

## health_score

If a derived field such as `health_score` exists, it should **not** be used by default.

Reason:

It may already summarize several predictive signals or include internal business logic, causing hidden leakage.

Instead, use the underlying measurable features.

---

# Causal Overclaims

The model predicts **risk**, not cause.

Incorrect statements include:

- "Low CTR causes traffic loss."
- "Improving position guarantees higher impressions."
- "The model proves why rankings dropped."

Appropriate wording:

- "The model identifies patterns associated with historical declines."
- "Predictions estimate future risk using historical data."
- "The output supports prioritization, not causal conclusions."

---

# Safe Default Feature Set

The following features are appropriate for model training:

- imp_prev45
- clk_last45
- pos_last45
- visible_queries
- rare_share
- anon_share
- top_query_share
- clicks_per_impression

These features are historical, measurable   before prediction, and do not expose private identifiers.

---

# Summary

| Requirement | Status |
|------------|--------|
| Feature meanings explained | ✅ |
| Engineered features documented | ✅ |
| Missing values discussed | ✅ |
| Categorical handling explained | ✅ |
| Prediction-time availability checked | ✅ |
| Private identifiers identified | ✅ |
| Future leakage identified | ✅ |
| Product-flag leakage identified | ✅ |
| health_score discussed | ✅ |
| Causal overclaims avoided | ✅ |