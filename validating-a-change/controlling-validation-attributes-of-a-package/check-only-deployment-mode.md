---
icon: clipboard-check
---

# Check-Only Deployment Mode

|              | sfp-pro     | sfp (community) |
| ------------ | ----------- | --------------- |
| Availability | ✅           | ❌               |
| From         | February 26 |                 |

| Attribute        | Type     | Description                                                                                                                                                                                                             | Package Types Applicable                                                              |
| ---------------- | -------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------- |
| checkOnlyAgainst | string[] | Array of org aliases where this package should be validated using check-only (validate-only) deployment. When the target org matches an alias in this array, a check-only deployment is performed; otherwise skipped. | <ul><li>unlocked</li><li>org-dependent unlocked</li><li>source</li><li>diff</li></ul> |

Certain packages cannot be validated in pooled review environments (scratch orgs or sandboxes) - for example, Data Cloud packages that require specific provisioning, or integration-heavy packages that depend on connected apps and named credentials not available in pools.

The `checkOnlyAgainst` attribute allows these packages to be validated against persistent orgs using Salesforce's check-only deployment, which verifies deployability without committing changes to the target org.

{% hint style="info" %}
This attribute only affects the `sfp validate org` command. It has no effect during deploy, release, or install stages.
{% endhint %}

```json
{
  "packageDirectories": [
    {
      "package": "dc-connector",
      "path": "src/dc-connector",
      "type": "source",
      "checkOnlyAgainst": ["review"]
    }
  ]
}
```

When running `sfp validate org --targetorg datacloud-dev --releaseconfig config/dc-domain.yaml`, the package will be validated using check-only deployment against the specified org.

## Behavior

When using `sfp validate org` with a `--releaseconfig`:

- If `--targetorg` matches one of the aliases in `checkOnlyAgainst` → Check-only deployment
- If `--targetorg` does NOT match any alias → Package is skipped

When using `sfp validate pool`, packages with `checkOnlyAgainst` are always skipped regardless of pool alias.

## Requirements

### Release Config Required

The feature only works when `--releaseconfig` is provided. This is required to validate the single-package domain constraint.

### Single Package Per Domain

Domains containing a package with `checkOnlyAgainst` must have exactly one package. This avoids dependency ordering complexity.

### Org Authentication

The org alias in `--targetorg` must be pre-authenticated in your CI environment.
