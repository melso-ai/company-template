# GTM Autopilots

Add an Autopilot YAML file here when you want recurring or event-triggered GTM work. This starter contains no Autopilots.

The complete example below could be saved as `weekly-gtm-review.yaml`. It is disabled at both the Autopilot and trigger levels. Review its instructions, schedule, and execution owner before enabling it.

```yaml
id: weekly-gtm-review
area: gtm
title: Weekly GTM review
prompt: |
  Review the GTM documents relevant to this week's work.
  Summarize changes, open questions, and suggested next steps.
  Link to the evidence and distinguish facts from assumptions.
enabled: false
issue_title_template: "GTM review — {{date}}"
execution:
  runtime: melso-cloud
  harness: codex
  model: default
  fallbacks: []
triggers:
  - id: monday-morning
    kind: schedule
    enabled: false
    label: Monday morning
    cron_expression: "0 9 * * 1"
    timezone: Etc/UTC
```

The example runs at 09:00 on Mondays in UTC only after both enablement values are changed to `true`, required connections are ready, and the definition is active. Cron uses exactly five fields; keep the IANA timezone in its separate field.

Keep the Autopilot ID stable and unique across the workspace. Trigger IDs are unique within their Autopilot. Set `area: gtm` to match this containing Area. Omit execution fields to inherit the [company and Area defaults](../../../README.md#choose-execution-defaults).

Publishing a definition does not choose a new execution owner or grant account access. Use Melso to bind the authorized owner and required accounts. The operational pause in Melso remains independent of configured enablement in Git.
