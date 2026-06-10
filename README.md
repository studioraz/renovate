# renovate

Shared Renovate presets for StudioRaz repositories.

## How to use

Project repositories should extend the public preset only:

```json
{
  "extends": [
    "github>studioraz/renovate:main"
  ]
}
```

The public preset composes the internal presets in this repository.

## File structure

```text
studioraz/renovate
├── README.md
├── main.json
├── _default.json
├── _groupings.json
├── _safeguards.json
└── _safe-composer-updates.json
```

## Public presets

### `main.json`

Reference:

```text
github>studioraz/renovate:main
```

Main StudioRaz Renovate preset for Magento projects.

This is the preset that project repositories should use. It extends all internal presets:

```json
{
  "extends": [
    "github>studioraz/renovate:_default",
    "github>studioraz/renovate:_groupings",
    "github>studioraz/renovate:_safeguards",
    "github>studioraz/renovate:_safe-composer-updates"
  ]
}
```

## Internal presets

Files prefixed with `_` are internal building blocks. Project repositories should not extend them directly unless there is a specific reason to opt into only part of the shared configuration.

### `_default.json`

Reference:

```text
github>studioraz/renovate:_default
```

Base Renovate configuration for Magento Composer projects.

Purpose:

- Extends Renovate's base config.
- Sets the timezone to `Asia/Jerusalem`.
- Removes hourly and concurrent PR limits.
- Adds the `renovate` label to Renovate PRs.
- Enables the dependency dashboard.
- Schedules Renovate before `3am`.
- Limits Renovate scanning to `composer.json`.
- Enables the Composer manager.

### `_groupings.json`

Reference:

```text
github>studioraz/renovate:_groupings
```

Package grouping rules for related Magento modules.

Purpose:

- Groups packages that should be updated together in a single Renovate PR.
- Keeps related vendor modules aligned.
- Currently includes grouping rules for selected Amasty and Mirasvit module families.

### `_safeguards.json`

Reference:

```text
github>studioraz/renovate:_safeguards
```

Safety rules that prevent unwanted or noisy Renovate updates.

Purpose:

- Disables updates for Magento core/platform packages.
- Disables updates for `require-dev` dependencies.
- Disables Amasty package updates to avoid known private repository authorization issues.

### `_safe-composer-updates.json`

Reference:

```text
github>studioraz/renovate:_safe-composer-updates
```

Labels selected Composer package updates as safe to merge without manual testing.

Purpose:

- Matches selected Composer packages that are considered low risk.
- Adds the `safe-to-merge-without-testing` label to matching Renovate PRs.
- Applies only to Composer-managed dependencies.

## Preset naming convention

- Public presets use normal names, such as `main.json`.
- Internal presets are prefixed with `_`, such as `_default.json`.
- Project repositories should normally reference only `github>studioraz/renovate:main`.
