# Support metrics

Add YAML definitions here for the Support measurements you decide to track. This starter defines no metrics and contains no measurements.

This complete illustration could be saved as `support-reviews.yaml`. Choose a metric and source that fit your work before adding a definition:

```yaml
id: support-reviews
area: support
metric: Support knowledge articles reviewed
direction: maximize
measurement_source: Record the count from the completed weekly Support review.
cadence: weekly
unit: count
```

The example describes a count, not a measured result or recommended company target. The definition becomes active after a valid publication; measurements are recorded in Melso and stay outside Git. Optional numeric targets and guardrails belong in the definition when you choose them.

Keep the metric ID stable and unique across the workspace, and set `area: support` to match this containing Area. Directions are `maximize`, `minimize`, or `maintain`; cadences are `daily`, `weekly`, `monthly`, or `quarterly`. Deleting a definition archives it without deleting its measurement history.
