# Company metrics

Add YAML definitions here for the Company measurements you decide to track. This starter defines no metrics and contains no measurements.

This complete illustration could be saved as `company-reviews.yaml`. Choose a metric and source that fit your work before adding a definition:

```yaml
id: company-reviews
area: company
metric: Company decisions reviewed
direction: maximize
measurement_source: Record the count from the completed weekly Company review.
cadence: weekly
unit: count
```

The example describes a count, not a measured result or recommended company target. The definition becomes active after a valid publication; measurements are recorded in Melso and stay outside Git. Optional numeric targets and guardrails belong in the definition when you choose them.

Keep the metric ID stable and unique across the workspace, and set `area: company` to match this containing Area. Directions are `maximize`, `minimize`, or `maintain`; cadences are `daily`, `weekly`, `monthly`, or `quarterly`. Deleting a definition archives it without deleting its measurement history.
