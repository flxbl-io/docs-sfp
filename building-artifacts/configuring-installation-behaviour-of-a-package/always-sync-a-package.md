# Always sync a package during validation

| Attribute  | Type    | Description                                                                                                                                                                                                  | Package Types Applicable                                               |
| ---------- | ------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ---------------------------------------------------------------------- |
| alwaysSync | boolean | During validation, automatically includes this package when any other package in the same domain is impacted. Useful for config/settings packages that must stay synchronized with their domain. | <ul><li>unlocked</li><li>org-dependent unlocked</li><li>source</li><li>data</li></ul> |

|              | sfp-pro      | sfp (community) |
| ------------ | ------------ | --------------- |
| Availability | ✅            | ❌               |
| From         | November 25  |                 |



To ensure a package is always included during validation when its domain is impacted, add **`alwaysSync`** as a property to your package descriptor:

```json
{
  "packageDirectories": [
    {
      "path": "src/frameworks/framework-core",
      "package": "framework-core",
      "versionNumber": "1.0.0.NEXT"
    },
    {
      "path": "src/frameworks/framework-config",
      "package": "framework-config",
      "versionNumber": "1.0.0.NEXT",
      "alwaysSync": true
    }
  ]
}
```

When `framework-core` is changed and validation is triggered, `framework-config` will automatically be included because it belongs to the same domain and has `alwaysSync: true`.

This also works across domains when using `dependencyOn` in release configs. If Domain B has a `dependencyOn` referencing a package from Domain A, and that package changes, all `alwaysSync` packages in Domain A will be included.
