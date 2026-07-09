# Signal Audit

**Assignment:** ML-06  Signal and Myth Audit

## Lane

**Core Lane:** Search & Discoverability

---

# Objective

Before training a predictive model, we investigated whether commonly assumed SEO signals are actually supported by the available data.

Rather than accepting common SEO advice as fact, each signal was evaluated using exploratory data analysis (EDA).

---

# Dataset Overview

## Unit of Analysis

Each row represents one website content page.

## Features Examined

- Search Volume
- Impressions
- CTR (Click Through Rate)
- Average Position
- Content Type

---

# Distribution Summary

The dataset contains pages with a wide range of search performance.

Observations include:

- Many pages receive relatively few impressions.
- CTR varies considerably across pages.
- Search positions range from top-ranking pages to lower-ranking results.
- Search volume is highly skewed, with a small number of keywords receiving most searches.

These distributions suggest that the dataset is not normally distributed and contains several outliers.

---

# Signal Test 1

## Myth

> Higher search volume always produces more page impressions.

### Test

Correlation analysis between:

- Search Volume
- Impressions

### Result

Correlation:

```
0.001
```

### Verdict

**FALSE**

### Interpretation

The observed correlation is effectively zero.

Although higher search volume may create greater opportunity, pages targeting high-volume keywords do not automatically receive more impressions.

Ranking position, competition, and content quality likely influence impressions more strongly than search volume alone.

---

# Signal Test 2

## Myth

> Better ranking position is associated with higher CTR.

### Test

Average CTR grouped by ranking position.

### Result

Pages appearing nearer the top of search results generally showed higher click-through rates.

However, the relationship was not perfectly consistent across all positions.

### Verdict

**CONFIRMED**

### Interpretation

The data supports the expectation that better rankings generally improve CTR, although page title quality, search intent, and competition may also influence user clicks.

---

# Signal Test 3

## Myth

> Different content types have identical click behaviour.

### Test

Average CTR grouped by content type.

### Result

CTR differed across content categories.

Some content types consistently achieved higher average CTR than others.

### Verdict

**MIXED**

### Interpretation

Content type appears to influence click behaviour, but the effect is not uniform.

Other variables such as keyword intent and ranking position are also likely contributors.

---

# Overall Findings

| Signal | Verdict |
|---------|----------|
| Search Volume → Impressions | FALSE |
| Better Position → Higher CTR | CONFIRMED |
| Content Type → CTR | MIXED |

---

# Practical Implications

The exploratory analysis suggests that simple SEO assumptions do not always hold.

In particular:

- Search volume alone is a poor predictor of page visibility.
- Ranking position remains an important signal for CTR.
- Content characteristics contribute to performance but should not be interpreted in isolation.

These observations justify using multiple features together within a machine learning model rather than relying on individual heuristic rules.

---

# Limitations

The analysis identifies statistical associations rather than causal relationships.

Observed patterns may also be affected by:

- Search engine algorithm updates
- Seasonal search demand
- User behaviour
- Competition
- Website-specific factors

Therefore, these findings should be interpreted as evidence supporting future modeling rather than definitive explanations of search performance.

---

# Conclusion

The signal audit indicates that some commonly accepted SEO beliefs are supported by the available data, while others are not.

Negative findings are valuable because they discourage oversimplified rules and motivate the use of evidence-based machine learning models that consider multiple interacting signals.