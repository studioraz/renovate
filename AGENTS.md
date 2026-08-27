<!-- FOR AI AGENTS - Human readability is a side effect, not a goal -->
<!-- Managed by agent: keep sections and order; edit content, not structure -->
<!-- Last updated: 2026-08-27 | Last verified: 2026-08-27 -->

# AGENTS.md

**Precedence:** the **closest `AGENTS.md`** to the files you're changing wins. Root holds global defaults only.

## Purpose
This repository hosts shared **Renovate presets** for StudioRaz Magento 2 and Mage-OS projects. It contains JSON preset configuration files consumed downstream via Renovate's `github>studioraz/renovate:...` preset syntax.

## Commands
> Source: JSON linting / validation via available toolchains or Renovate CLI

<!-- AGENTS-GENERATED:START commands -->
| Task | Command | ~Time |
|------|---------|-------|
| Validate JSON | `python3 -m json.tool <file>.json > /dev/null` | ~1s |
| Validate Renovate Config | `npx --yes --package renovate renovate-config-validator` | ~5s |
<!-- AGENTS-GENERATED:END commands -->

## Response Style
- Answer first, elaborate only if needed. No sycophantic openers ("Great question!", "Absolutely!").
- For yes/no or status questions, lead with the answer.
- Skip preamble. Match response length to task complexity.

## Workflow
1. **Before editing**: Check which preset file is responsible for the rule (safeguards, groupings, safe updates, default).
2. **After each change**: Validate JSON syntax with `python3 -m json.tool` or validate with `renovate-config-validator`.
3. **Before committing**: Ensure JSON files are strictly valid and follow Renovate preset schema specifications (`$schema: "https://docs.renovatebot.com/renovate-schema.json"`).
4. **Before claiming done**: Verify that modified JSON parses without error.

## File Map
<!-- AGENTS-GENERATED:START filemap -->
```
README.md                    -> Documentation and usage instructions for downstream consumers
main.json                    -> Public entrypoint preset (github>studioraz/renovate:main) composing internal presets
_default.json                -> Base Renovate config (timezone, schedule, limits, labels, composer manager)
_groupings.json              -> Package grouping rules for vendor modules (Amasty, Mirasvit, MageWorx)
_safeguards.json             -> Safety rules (disable core Magento/Mage-OS upgrades, dev deps, auth-restricted packages)
_safe-composer-updates.json  -> Whitelist of low-risk Composer packages tagged with 'safe-to-merge-without-testing'
```
<!-- AGENTS-GENERATED:END filemap -->

## Golden Samples (follow these patterns)
<!-- AGENTS-GENERATED:START golden-samples -->
| For | Reference | Key patterns |
|-----|-----------|--------------|
| Public preset | `main.json` | Extends internal `_*.json` presets via `github>studioraz/renovate:_<name>` |
| Package grouping rule | `_groupings.json` | `packageRules` with `description`, `matchPackageNames`, `groupName` |
| Safeguard/Ignore rule | `_safeguards.json` | `packageRules` with `description`, `matchPackageNames` / `matchPackagePatterns` / `matchDepTypes`, `"enabled": false` |
| Safe updates label | `_safe-composer-updates.json` | `packageRules` with `matchManagers: ["composer"]`, `matchPackageNames`, `addLabels: ["safe-to-merge-without-testing"]` |
<!-- AGENTS-GENERATED:END golden-samples -->

## Heuristics (quick decisions)
<!-- AGENTS-GENERATED:START heuristics -->
| When | Do |
|------|-----|
| Adding a new public preset | Add without `_` prefix, document in `README.md` |
| Adding an internal building block | Name with `_` prefix (e.g., `_new-rule.json`) and extend it inside `main.json` |
| Adding related module packages | Group them under a unified `groupName` in `_groupings.json` |
| Adding low-risk utility modules | Append package name to `_safe-composer-updates.json` |
| Blocking unsafe/breaking packages | Add to `_safeguards.json` with `"enabled": false` |
<!-- AGENTS-GENERATED:END heuristics -->

## Key Decisions
<!-- AGENTS-GENERATED:START key-decisions -->
- **Preset Naming**: Public presets use standard filenames (`main.json`); internal composition modules use `_` prefix (`_default.json`, `_groupings.json`, etc.).
- **Downstream Consumers**: Downstream repos should only extend `github>studioraz/renovate:main` unless a specific partial preset is explicitly needed.
- **Pure Configuration Repo**: No build scripts or runtime code; purely declarative JSON presets.
<!-- AGENTS-GENERATED:END key-decisions -->

## Boundaries

### Always Do
- Ensure all JSON files conform to Renovate schema and pass JSON syntax checks.
- Add descriptive `description` keys to all rules in `packageRules`.
- Group both base modules and their Hyvä compatibility companion modules together.
- Use atomic commits and conventional commit messages.

### Ask First
- Changing schedule, timezone, or default PR limits in `_default.json`.
- Modifying or removing packages from `_safeguards.json`.
- Adding new public entrypoint presets.

### Never Do
- Never commit malformed JSON.
- Never enable automated updates for core Magento/Mage-OS platform packages in `_safeguards.json`.
- Never expose internal presets without the `_` prefix convention.

## Terminology
| Term | Means |
|------|-------|
| Renovate Preset | Reusable, shareable Renovate configuration hosted on GitHub or npm |
| Public Preset | Presets meant to be referenced directly by consumer repositories (e.g. `main.json`) |
| Internal Preset | Modular building block prefixed with `_` consumed internally by `main.json` |
| Safe to Merge | Packages that have proven zero regression risk, labeled for auto/expedited merge |

## When instructions conflict
The nearest `AGENTS.md` wins. Explicit user prompts override files.
