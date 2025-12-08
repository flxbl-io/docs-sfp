---
icon: ring-diamond
---

# Managing Environment-Specific Metadata

{% hint style="info" %}
Aliasify: **sfp** (community) and **sfp-pro**. Aliasfy merge mode: **sfp-pro**. String replacements: **sfp-pro** with sfp-server.
{% endhint %}

This guide explains how to handle environment-specific metadata in sfp with two complementary features:

- **Aliasified packages**: deliver different files per environment (structural differences).
- **String replacements**: substitute values inside shared files per environment (configuration differences).

Use them together to minimize duplication while keeping environments consistent.

## Feature Availability

| Feature | Community (sfp) | sfp-pro |
| --- | --- | --- |
| Aliasify packages | ✅ | ✅ |
| Aliasify merge mode (default inheritance) | ❌ | ✅ |
| String replacements | ❌ | ✅ (requires sfp-server) |

## When to Use Each

| Scenario | Use | Why |
| --- | --- | --- |
| Different layouts/fields/permissions by environment | Aliasify | Deploy different files per environment alias |
| Same file, different values (URLs, keys, emails) | String replacements | Substitute placeholders per environment |
| Both structure and values differ | Combine | Separate structure; vary values within shared files |

## Environment Resolution

- **Org alias drives selection** for both features.
- **Aliasfy**: deploys the folder matching the alias; falls back to `default` (required for sandboxes/scratch). Merge mode can inherit from `default`.
- **String replacements**: exact alias match first; otherwise uses `default` (recommended for sandboxes). Production values should be explicit.

## Aliasify (Structural Differences)

1. Mark the source package with `aliasfy: true` (or `aliasfy.mergeMode: true` for inheritance).
2. Create subfolders for each environment alias plus `default/`.
3. Build/install: only the matching alias folder deploys; merge mode blends `default` into alias folders.

Example structure (Sites/Networks differing by environment):
```
src-env-specific/
  default/
    networks/InvestorPortal.network-meta.xml
    sites/InvestorPortal.site-meta.xml
  staging/
    networks/InvestorPortal.network-meta.xml        # UAT routing and domains
    sites/InvestorPortal.site-meta.xml              # staging URLs/login settings
  production/
    networks/InvestorPortal.network-meta.xml        # prod domains/security
    sites/InvestorPortal.site-meta.xml              # prod login and redirects
```

### Merge Mode vs Standard Mode

| Mode | Default folder required | Build output | Install behavior | Push/Pull behavior |
| --- | --- | --- | --- | --- |
| Standard (`aliasfy: true`) | Yes for sandboxes; prod requires alias | Only alias folder (no inheritance) | Deploys alias folder; if missing uses `default` (sandboxes only) | Push/Pull: typically ignore alias folders; default often force-ignored |
| Merge (`aliasfy.mergeMode: true`) | Recommended | Alias folders inherit from `default` during build | Deploys alias folder (with inherited default); if missing uses `default` | Push/Pull use `default` folder; do **not** force-ignore `default` |

Notes:
- If neither alias nor `default` exists, install fails.
- In merge mode, keep `default` in version control and out of `.forceIgnore` to allow push/pull.
- Binary differences still require aliasfy folders (string replacements are text-only).

## String Replacements (Value Differences)

1. Add `preDeploy/replacements.yml` to the source package (sfp-pro).
2. Define patterns, globs, and per-environment values.
3. Build/install/push/pull/validate: placeholders are substituted based on target org alias; pull reverses values back to placeholders.

Example `preDeploy/replacements.yml`:
```yaml
replacements:
  - name: "API Endpoint"
    pattern: "%%API_ENDPOINT%%"
    glob: "**/*.cls"
    environments:
      default: "https://api-sandbox.example.com"
      staging: "https://api-staging.example.com"
      production: "https://api.example.com"
```

Configuration fields:
- `name`: human-readable label.
- `pattern`: placeholder (supports regex with `isRegex: true`).
- `glob`: files to target (e.g., `**/*.cls`).
- `environments`: map of alias → value; include `default` for sandboxes, explicit production entries.

Flow by command:
- **build**: validates and embeds replacement config into artifact.
- **install/deploy**: applies forward replacements for the target alias.
- **push**: applies forward replacements before deployment.
- **pull**: applies reverse replacements, restoring placeholders into source.
- **validate**: applies replacements during validation.

## Using Both Together

- Keep structural differences in aliasfy folders; keep shared code single-sourced with placeholders for value differences.
- Migration path: start with aliasfy duplicates → consolidate shared files → add replacements → remove duplicates.
- For large packages, combine: aliasfy separates components; replacements handle values inside shared components.

## CI/CD and Governance Tips

- Standardize org aliases (e.g., `dev`, `staging`, `prod`) to match folder names and replacement keys.
- Always include `default` values for sandboxes/scratch; specify production explicitly.
- Flags: `--no-replacements` to skip replacements; `--replacementsoverride` to supply an override file during install/push/pull.
- Validate glob patterns locally to ensure replacements target the right files.

## Troubleshooting

- **Aliasify**: missing `default`/alias folder → add required folder; merge mode + push/pull → ensure `default` is not force-ignored; missing alias → install uses `default` (fails if absent).
- **String replacements**: pattern not found → verify placeholder and glob; wrong value → confirm org alias and environments map; skipped → check `--no-replacements` not set and package is source type.

## Related References

- Aliasify Packages: `building-artifacts/configuring-installation-behaviour-of-a-package/aliasfy-packages/`
- Aliasify Merge Mode: `building-artifacts/configuring-installation-behaviour-of-a-package/aliasfy-packages/aliasfy-packages-merge-mode.md`
- String Replacements: `building-artifacts/configuring-installation-behaviour-of-a-package/string-replacements.md`
- String Replacements (workflow): `development/string-replacements.md`
