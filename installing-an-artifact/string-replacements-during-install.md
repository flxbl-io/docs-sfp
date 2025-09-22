---
icon: ring-diamond
---

# String Replacements During Install

|              | sfp-pro          | sfp (community) |
| ------------ | ---------------- | --------------- |
| Availability | ✅                | ❌               |
| From         | September 2025   |                 |

During artifact installation, sfp automatically applies string replacements to convert placeholder values to environment-specific values based on the target org.

### How It Works

When installing artifacts with embedded replacement configurations:

1. **Org Detection**: Identifies the target org alias and whether it's a sandbox or production org
2. **Value Resolution**: Determines the appropriate replacement value based on org alias or defaults
3. **File Modification**: Applies replacements to matching files before deployment
4. **Deployment**: Deploys the modified components to the target org

### Environment Resolution

Replacements are resolved in this order:

1. **Exact alias match**: If the org alias matches a configured environment
2. **Default value**: For sandbox/scratch orgs when no exact match is found
3. **Error**: For production orgs without explicit configuration

### Installation Output

During installation with replacements, you'll see:

```
Text replacements for org alias dev (sandbox)
Total replacement configs 3
Modified APIService.cls with 2 replacements
Modified ConfigService.cls with 1 replacements
Replacements summary: 3 replacements in 2 files using dev environment
```

### Command Line Options

#### Disable Replacements

To skip replacements during installation:

```bash
sfp install --targetorg dev --artifactdir artifacts --no-replacements
```

#### Override Replacements

To use a custom replacement configuration file:

```bash
sfp install --targetorg dev --artifactdir artifacts --replacementsoverride custom-replacements.yml
```

### Production Deployments

Production deployments require explicit configuration for the org alias. If no configuration is found, the installation will fail:

```
ERROR: No replacement value found for production org with alias 'prod'
Production deployment requires explicit configuration for alias 'prod' in replacement 'API Endpoint'
```

### Troubleshooting

#### No Replacements Applied

* Verify the artifact was built with replacements embedded
* Check that glob patterns match your files
* Ensure the org alias has configured values

#### Wrong Values Applied

* Verify org alias with `sf org list`
* Check environment resolution order
* Ensure production orgs have explicit configuration

### Related Documentation

* [String Replacements Overview](../development/string-replacements.md)
* [String Replacements Configuration](../building-artifacts/configuring-installation-behaviour-of-a-package/string-replacements.md)