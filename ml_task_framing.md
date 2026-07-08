# ML Task Framing

**Assignment:** ML-03 - Frame Your Lane as an ML Task

## Chosen Lane

**Core Lane:** Search & Discoverability

---

# Problem Statement

Content teams manage hundreds or thousands of web pages, making it difficult to identify which pages are likely to lose search visibility. Instead of reviewing every page manually, the goal is to use machine learning to identify pages that may require attention before their search performance declines.

---

# ML Task Type

**Classification**

The task is to classify each content page into one of two categories:

- **Likely to decline in search visibility**
- **Not likely to decline in search visibility**

Although the model may output a probability, the underlying task is binary classification.

---

# Target / Proxy

The desired target (future visibility loss) cannot be observed at prediction time, so a proxy is used.

**Target Variable:**

A page is considered **declining** if its impressions during the most recent time window are significantly lower than those in the previous time window.

Example:

```
is_declining = 1
if

current_impressions < 0.8 × previous_impressions
```

Otherwise,

```
is_declining = 0
```

This proxy provides a measurable training target while approximating future performance loss.

---

# Input Features

Possible features include:

- Historical impressions
- Historical clicks
- Click-through rate (CTR)
- Average search position
- Search volume
- Query diversity
- Share of branded vs. non-branded queries
- Top query concentration
- Engagement metrics
- Historical performance trends

These signals together describe the search performance of each content page.

---

# Model Output

For every page, the model produces:

- Probability of future decline
- Predicted class (Declining / Not Declining)

The prediction should support human decision-making rather than replace it.

---

# Success Metric

The primary evaluation metric is:

**Precision**

A high precision score means that when the model predicts a page is at risk, it is usually correct. This helps reduce unnecessary reviews of healthy pages.

Additional metrics include:

- Recall
- F1 Score
- Confusion Matrix

These metrics provide a more complete picture of model performance.

---

# Action Supported

The model helps content and SEO teams prioritize work.

If a page is predicted to be at high risk, the team may:

- Update outdated content
- Improve keyword targeting
- Refresh metadata
- Strengthen internal linking
- Investigate ranking issues
- Monitor the page more closely

Rather than reviewing every page equally, resources can be focused on the pages most likely to benefit from intervention.

---

# Why This Is an ML Problem

A fixed rule such as:

> "Review every page with CTR below 3%"

or

> "Review every page ranked below position 10"

does not account for the interaction of multiple factors.

Search performance depends on combinations of variables, including:

- CTR
- Search position
- Search demand
- Historical trends
- Query distribution
- User behavior

Machine learning can learn these complex relationships from historical data and assign a risk score based on multiple signals simultaneously.

---

# Why a Fixed Rule Is Not Enough

Simple rules are easy to understand but often generate many false positives and false negatives.

For example:

- A low CTR may be normal for some queries.
- A page with a good ranking may still lose impressions because search demand changes.
- Two pages with similar rankings may perform very differently because of differences in content quality or user intent.

Machine learning adapts to these interactions more effectively than manually defined thresholds.

---

# Risks and Limitations

The model should be treated as a decision-support tool rather than an automated decision-maker.

Predictions may be affected by factors outside the available data, including:

- Search engine algorithm updates
- Seasonal search behavior
- Competitor activity
- Breaking news or trending topics

Regular retraining and monitoring are necessary to maintain model quality.

---

# Summary

| Item | Description |
|------|-------------|
| **Lane** | Search & Discoverability |
| **ML Task** | Binary Classification |
| **Target** | Predict whether a page is likely to decline in search visibility |
| **Proxy** | Significant drop in impressions compared to the previous period |
| **Success Metric** | Precision (supported by Recall and F1 Score) |
| **Output** | Risk probability and predicted class |
| **Business Action** | Prioritize pages for SEO and content optimization |
| **Why ML** | Learns complex relationships among multiple search signals that fixed rules cannot capture effectively |