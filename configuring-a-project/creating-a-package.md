---
description: All packages start out as directory in your repo!
---

# Creating a package

A package is a collection of metadata grouped together in a directory, and defined by an entry in your sfdx-project.json (Project Manifest).

```json
// A sample sfdx-project.json with a package
{
  "packageDirectories": [
    {
      "path": "src/my-package",
      "package": "my-package",
      "versionNumber": "1.0.0.NEXT"
    }
  ]
}
```

Each package in sfp must have the following attributes as the minimum:

| Attribute          | Required | Description                                                      |
| ------------------ | -------- | ---------------------------------------------------------------- |
| path               | yes      | Path to the directory that contains the contents of the package |
| package            | yes      | The name of the package                                         |
| versionNumber      | yes      | The version number of the package                               |
| versionDescription | no       | Description for a particular version of the package             |

{% hint style="info" %}
sfp will not consider any entries in your sfdx-project.json for its operations if it is missing 'package' or 'versionNumber' attribute.
{% endhint %}

## Package Types

By default, sfp treats all entries in sfdx-project.json as [Source Packages](../concepts/supported-package-types/source-packages.md). You can create different types of packages depending on your needs:

| Package Type | sfp-pro | sfp (community) | Description |
| ------------ | ------- | --------------- | ----------- |
| Source Package | ✅ | Manual | Default package type for deploying metadata |
| Unlocked Package | ✅ | SF CLI | Versioned, upgradeable package |
| Org-Dependent Unlocked | ✅ | SF CLI | Unlocked package with org dependencies |
| Data Package | ✅ | Manual | Package for data migration |
| Diff Package | ✅ | Manual | Package containing only changed components |

## Creating Packages with sfp-pro

### Source Package

Create a source package using the sfp-pro CLI:

```bash
sfp package create source -n "my-source-package" -r "src/my-package"

# With domain (for organizing packages)
sfp package create source -n "my-source-package" -r "src/my-package" --domain
```

**Flags:**
- `-n, --name` (required): Package name
- `-r, --path` (required): Directory path for the package
- `-d, --description`: Package description
- `--domain`: Mark package as a domain package
- `--no-insert`: Don't insert into sfdx-project.json automatically
- `--insert-after`: Insert after a specific package

### Unlocked Package

Create an unlocked package with automatic DevHub registration:

```bash
sfp package create unlocked -n "my-unlocked-package" -r "src/my-package" -v devhub

# Org-dependent unlocked package
sfp package create unlocked -n "my-package" -r "src/my-package" --org-dependent -v devhub

# With namespace
sfp package create unlocked -n "my-package" -r "src/my-package" --no-namespace -v devhub
```

**Flags:**
- `-n, --name` (required): Package name
- `-r, --path` (required): Directory path for the package
- `-v, --targetdevhubusername`: DevHub alias/username
- `--org-dependent`: Create org-dependent unlocked package
- `--no-namespace`: Create without namespace
- `-d, --description`: Package description
- `--error-notification-username`: Username for error notifications
- `--domain`: Mark package as a domain package

### Data Package

Create a data package for data migration:

```bash
sfp package create data -n "my-data-package" -r "data/my-data-package"
```

**Flags:**
- `-n, --name` (required): Package name
- `-r, --path` (required): Directory path for the package
- `-d, --description`: Package description
- `--domain`: Mark package as a domain package

{% hint style="info" %}
Ensure your data package directory contains an export.json and the required CSV files. See [Data Packages](../concepts/supported-package-types/data-packages.md) for details.
{% endhint %}

### Diff Package

Create a diff package to track changes from a baseline:

```bash
sfp package create diff -n "my-diff-package" -r "src/my-diff-package" -c "baseline-commit-id" -v devhub
```

**Flags:**
- `-n, --name` (required): Package name
- `-r, --path` (required): Directory path for the package
- `-c, --commit-id`: Baseline commit ID
- `-v, --targetdevhubusername`: DevHub alias/username
- `-d, --description`: Package description
- `--domain`: Mark package as a domain package

## Creating Packages for Community Edition

For sfp community edition users, packages need to be created manually or using Salesforce CLI.

### Source Package (Manual)

1. Create a directory for your package
2. Add an entry to your `sfdx-project.json`:

```json
{
  "packageDirectories": [
    {
      "path": "src/my-source-package",
      "package": "my-source-package",
      "versionNumber": "1.0.0.NEXT"
    }
  ]
}
```

### Unlocked Package (Using SF CLI)

1. Ensure your `sfdx-project.json` contains an entry for the package with `path`, `package`, and `versionNumber`
2. Create the package using Salesforce CLI:

```bash
# Standard unlocked package
sf package create --name my-package --package-type Unlocked --no-namespace -v devhub

# Org-dependent unlocked package
sf package create --name my-package --package-type Unlocked --org-dependent --no-namespace -v devhub
```

3. Commit the updated `sfdx-project.json` with the new package ID in `packageAliases`

### Data Package (Manual)

1. Create a directory for your data package
2. Add the required export.json and CSV files
3. Add an entry to `sfdx-project.json` with `type: "data"`:

```json
{
  "path": "data/my-data-package",
  "package": "my-data-package",
  "versionNumber": "1.0.0.NEXT",
  "type": "data"
}
```

### Diff Package (Manual)

1. Create a directory for your diff package
2. Add an entry to `sfdx-project.json` with `type: "diff"`:

```json
{
  "path": "src/my-diff-package",
  "package": "my-diff-package",
  "versionNumber": "1.0.0.NEXT",
  "type": "diff"
}
```

3. Create a record in `SfpowerscriptsArtifact2__c` object in your DevHub with:
   - Package name
   - Initial version number
   - Baseline commit ID

## Best Practices

1. **Use descriptive names**: Package names should clearly indicate their purpose
2. **Organize by domain**: Group related packages using domains
3. **Version consistently**: Use semantic versioning (MAJOR.MINOR.PATCH)
4. **Document packages**: Add meaningful version descriptions
5. **Choose the right type**:
   - Source packages for most metadata
   - Unlocked packages for distributed, versioned components
   - Data packages for reference data
   - Diff packages for selective deployments

## Next Steps

- [Defining a Domain](defining-a-domain.md) - Organize packages into domains
- [Building Artifacts](../building-artifacts/overview.md) - Build deployable artifacts from packages
- [Package Types](../concepts/supported-package-types/) - Learn more about different package types