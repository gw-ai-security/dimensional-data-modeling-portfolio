# Baseline Metrics

> **Status: next active validation artifact.** Populate this file before structural changes to the Nightmare model.

The purpose is to protect known business results while the model is being redesigned. A model that looks cleaner but changes important totals without explanation is not validated.

## Baseline table

| Metric | Business definition | Grain / aggregation rule | Baseline value | Source / method | Notes |
|---|---|---|---:|---|---|
| Total Sales | TBD after source assessment | TBD | | | |
| Total Orders | TBD after source assessment | TBD | | | |
| Total Customers | TBD after source assessment | TBD | | | |

Add all business-critical measures discovered during the source-model audit. Do not assume the three starter metrics above are sufficient.

## Baseline rules

- define the metric before recording its number;
- note the grain/aggregation logic, especially for counts and repeated higher-grain measures;
- capture the value before remodeling;
- record how the value was obtained so it can be reproduced;
- if the original model is already wrong, document that explicitly rather than treating a bad number as truth.

## Reconciliation workflow

```text
Original model / source
→ record baseline and definition
→ remodel
→ recompute same metric
→ compare
→ explain any intentional difference
```

## Minimum evidence before major remodeling

- [ ] key business measures identified
- [ ] definitions written
- [ ] grain/aggregation assumptions documented
- [ ] baseline values recorded
- [ ] source/method recorded
- [ ] known source-model errors separated from trusted baselines