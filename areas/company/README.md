# Company

Use the Company Area for company direction, shared decisions, and work that spans Areas. Adapt these files to the way your company works; the starter supplies no company-specific facts or active recurring work.

| File or folder | Use it for |
| --- | --- |
| [area.yaml](area.yaml) | Area identity, title, tool policy, resources, and execution overrides. |
| [AGENTS.md](AGENTS.md) | Instructions for Tasks in this Area. |
| [docs/](docs/README.md) | Record company decisions, shared planning context, and the reasons behind them. |
| [skills/](skills/README.md) | Procedures available to Tasks in Company. |
| [autopilots/](autopilots/README.md) | Recurring or event-triggered work, when you choose to add it. |
| [metrics/](metrics/README.md) | Definitions for measurements you want to track. |

Execution inherits the [company defaults](../../README.md#choose-execution-defaults). The starter's `connector_policy: all` uses only the tools authorized for the execution; it does not grant account access.

Keep `id: company` stable when changing the title or moving the folder. This is the required root Area. Its skills apply only to Company Tasks; use root [`skills/`](../../skills/README.md) for skills shared by every Area.
