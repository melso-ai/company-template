# Your company repository

Build and run your company with shared documents, instructions, and operating definitions in Git. Edit this repository in Melso, in GitHub, or in a local checkout.

This starter contains six Areas and blank documents. It has no connected accounts, product repositories, local computers, active Autopilots, or metrics. Examples in README code blocks are documentation; they do not create definitions or start work.

## Start here

1. Describe the company in [docs/company.md](docs/company.md), then add your product, market, and strategy context.
2. Review [AGENTS.md](AGENTS.md) and each Area's instructions. Replace the starter guidance with the way your company works.
3. Connect the accounts you want to use in Melso. Set any Area-specific tool choices and execution overrides.
4. Validate, commit, and publish your changes. Check the active revision before starting work that depends on them.

## Find the right file

| Path | What belongs here |
| --- | --- |
| `company.yaml` | Company name, schema version, root Area, execution defaults, and template provenance added during setup. |
| `AGENTS.md` | Instructions that apply to every Area. |
| `connected-tools.yaml` | Named tool requirements and nonsecret provider declarations. |
| `docs/` | Company-wide context, decisions, and knowledge. |
| `skills/` | Authored skills available to Tasks in every Area. |
| `areas/<area>/area.yaml` | The Area's identity, name, tool policy, resources, and optional execution overrides. |
| `areas/<area>/AGENTS.md` | Instructions for Tasks in that Area. |
| `areas/<area>/docs/` | Knowledge specific to the Area. |
| `areas/<area>/skills/` | Skills available when a Task starts in that Area. |
| `areas/<area>/autopilots/` | Autopilot YAML files, each with its own identified triggers. |
| `areas/<area>/metrics/` | Metric definitions. Measurements are recorded in Melso. |

**Root `skills/` is company-wide. `areas/company/skills/` is only for Tasks in the Company Area.** The Company Area is the root Area, but its skills do not become global skills.

The starter Areas are [Company](areas/company/README.md), [Product](areas/product/README.md), [GTM](areas/gtm/README.md), [Support](areas/support/README.md), [Finance](areas/finance/README.md), and [Legal](areas/legal/README.md). Add or adapt Areas as your work changes. Keep the required Company root Area with `id: company` and `root_area: company`.

## Edit and publish

In Melso's Repository screen, edit a file and choose Save with a commit message. Git users can clone using authenticated Melso access; a personal GitHub account is not required:

```bash
melso company clone company
cd company
melso company validate
git diff
git add docs/company.md
git commit -m "Describe the company"
git push origin HEAD:main
melso company wait --commit HEAD
```

Select the intended workspace in Melso CLI before cloning. Agents receive a separate company checkout at `MELSO_COMPANY_DIR`; their product working directory stays separate.

A valid push to `main` becomes active automatically. Melso validates the complete commit and activates its definitions centrally. You do not need to add CI configuration. Pull requests are optional: use a branch when you want review before merging into `main`. Ordinary Git conflict resolution applies when another change reaches `main` first; review both edits before publishing again.

A successful push means the commit was saved. `melso company wait` reports whether it became active, was rejected, was superseded, or timed out. The Repository screen opens `main` so you can repair invalid files. Operational screens continue showing the last active revision until a valid commit is activated.

## Fix validation errors

`melso company validate` checks the local files without contacting tool endpoints or running repository scripts. Use `melso company validate --online` to also check available connections and execution capabilities.

Errors identify the affected path. Correct the file, validate, and publish another commit. An invalid commit does not partially update the company or remove the previous active configuration.

Keep these rules in mind:

- Required root files and the Company root Area must remain present.
- Area names must be unique after normalization. Use readable lowercase IDs such as `product` or `daily-review`.
- IDs are stable identities. Changing a title or moving a file preserves its `id`. Changing an ID is a separate identity change and requires coordinated references.
- Definition IDs are unique within their type across the workspace. Trigger IDs are unique within their Autopilot. An Autopilot or metric's `area` must match the containing Area's ID.
- Removing a definition archives its active projection. Task history stays in Melso; restoring the same ID restores that identity.
- Autopilot schedules use five cron fields, with an IANA timezone in a separate `timezone` field.
- `autopilots/` and `metrics/` contain YAML definitions and explanatory `README.md` files. Other configuration files there are rejected.
- Company content must be bounded UTF-8 text. Symlinks, submodules, and Git LFS content are unsupported. Use attachments for binary media.

## Connect tools and computers

A repository declaration names a connection; the account, credentials, owner, and permission grants stay in Melso. Discover authorized connections with:

```bash
melso connections list --output json
melso runtime list --output json
melso runtime models --runtime melso-cloud --harness codex
```

For example, this is a complete `connected-tools.yaml` declaring a Slack requirement. Add it only when you want this connection, then bind `company-slack` to an authorized Slack account in Melso:

```yaml
tools:
  - id: company-slack
    kind: composio_toolkit
    provider: slack
    binding: company-slack
    description: Slack connection for company work.
```

Every starter Area has `connector_policy: all`, which allows the tools authorized for that execution. It does not grant access to another member's accounts. To select no tools, use `connector_policy: selected` with `tools: []`. To allow only declared tools, list their IDs in `tools` with the selected policy.

Missing connections keep dependent work in a Needs connection or Needs computer state; they do not invalidate an otherwise valid company revision. Changing a provider, endpoint, or requested authority requires a compatible binding. A Git commit cannot grant membership or account permissions.

## Choose execution defaults

The company starts with this complete execution policy in `company.yaml`:

```yaml
execution:
  runtime: melso-cloud
  harness: codex
  model: default
  fallbacks: []
```

`melso-cloud` means Melso's managed execution environment. Melso resolves the actual computer and an authorized provider account when work starts. Connect the required provider account in Melso. Scheduled work uses its durable execution owner; publishing a commit does not transfer that ownership.

Defaults apply in this order: **Company → Area → Autopilot → explicit Task selection**. Omitted fields inherit. An explicit value overrides its inherited value. Use `null` to clear an individual field, and `execution: {}` to inherit the whole policy. Empty strings and `execution: null` are invalid. The effective policy must still supply a usable destination, harness, and model.

An empty `fallbacks: []` disables inherited fallbacks; `fallbacks: null` also clears them. When you change the harness, inherited model and provider-specific options are cleared so they cannot carry into an incompatible harness.

`model: default` is a selection policy, not a fixed model name. Melso resolves it to a concrete supported model before dispatch and records the model, harness, runtime, company revision, and selection source in the run receipt. A new run gets the current active company revision; recovery keeps the interrupted run's revision.

For a local computer, use a readable runtime binding. This complete Area definition illustrates a local override; `my-computer` must be bound to a computer you can use before running work:

```yaml
id: product
title: Product
description: Product discovery and delivery.
connector_policy: all
execution:
  runtime: my-computer
  harness: codex
  model: default
  fallbacks: []
```

Fallback is opt-in. List up to three alternatives in the order they should be tried. For example, this policy permits a Claude alternative when the primary execution has an eligible availability failure:

```yaml
execution:
  runtime: melso-cloud
  harness: codex
  model: default
  fallbacks:
    - harness: claude
      model: default
```

Each candidate needs current authorization and stays within the run's attempt, time, and spending limits. Cancellation and permission denial do not trigger fallback. A harness switch must fence the previous attempt, preserve work, and start with an appropriate handoff. Ambiguous external side effects require recovery rather than automatic replay.

Pausing an Autopilot in Melso takes effect immediately and survives unrelated Git commits. Resuming removes the operational pause; it does not override `enabled: false` in its YAML definition.

## Add skills

Start with the [global skills guide](skills/README.md) or an Area's skills guide. A skill is a directory with a `SKILL.md` file and optional supporting references or scripts. Global and Area skills have separate qualified identities, so matching names must not silently replace each other. Instructions and skill files for a run come from the same pinned company revision.

Area scoping controls discovery, not confidentiality. Anyone who can read the repository can read its files and history.

## Keep attachments and history in the right place

Git holds documents and operating definitions. Melso keeps Tasks, messages, memberships, permissions, account connections, run history, and metric measurements in its database. Upload images, audio, videos, and other binary assets as attachments; link them from Tasks or documents as needed.

A repository document linked as a Task output identifies a specific pushed commit and path. Later edits do not change that output. Uncommitted files in an agent checkout remain drafts until committed and pushed; they are not mirrored into the Repository screen.

## Template versions

`version: 2` in `company.yaml` identifies the company schema. It is separate from the starter template's release version. Workspace setup adds the released template's source, version, and exact commit as provenance. A later template release does not overwrite your company repository; future upgrades can be reviewed as ordinary commits.
