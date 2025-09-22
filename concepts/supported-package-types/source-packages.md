# Source Packages

|              | sfp-pro | sfp (community) |
| ------------ | ------- | --------------- |
| Availability | ✅       | ✅               |

Source Packages is an sfp feature that provides a flexible alternative to native unlocked packages for metadata deployment and organization.

## Understanding Source Packages

Source Packages are metadata deployments from a Salesforce perspective - they are groups of components that are deployed to an org as a unit. Unlike Unlocked packages which are **First Class** Salesforce deployment constructs with lifecycle governance (versioning, component locking, automated dependency validation), source packages provide more flexibility at the cost of some safety guarantees.

## When to Use Source Packages

While we generally recommend using unlocked packages over source packages for production code, source packages excel in several scenarios:

### Ideal Use Cases

* **Application Configuration**: Configuring applications delivered by managed packages (e.g., changes to help text, field descriptions)
* **Org-Specific Metadata**: Global or org-specific components (queues, profiles, permission sets, custom settings)
* **Environment-Specific Configuration**: Components that vary significantly across environments
* **Composite UI Layouts**: Complex UI configurations that don't package well
* **Rapid Development**: When iteration speed is more important than package versioning
* **Large Monolithic Applications**: When breaking into unlocked packages is not feasible
* **Starting Package Development**: Teams beginning their journey to package-based development

### Technical Constraints Favoring Source Packages

* [Metadata not supported by Unlocked Packages](https://developer.salesforce.com/docs/metadata-coverage)
* Known bugs or limitations with unlocked package deployment
* Unlocked Package validation taking too long (consider org-dependent as alternative)
* Need for destructive changes support
* Requirement for environment-specific text replacements

## Source Packages vs Unlocked Packages

### Advantages of Source Packages

* **Deployment Speed**: Significantly faster deployment times (no package creation/validation overhead)
* **Testing Control**: Flexible test execution - can skip tests in sandboxes for faster iterations (see [Testing Behavior](#testing-behavior))
* **Environment Management**: Support for aliasified folders and text replacements for environment-specific configurations
* **Destructive Changes**: Full support for pre and post-destructive changes
* **Metadata Coverage**: Supports all metadata types, including those not supported by unlocked packages
* **No Version Constraints**: No need to manage package versions or dependencies at the platform level
* **Quick Fixes**: Can deploy hotfixes immediately without package creation
* **Development Flexibility**: Easier to refactor and reorganize code

### Disadvantages of Source Packages

* **No Component Locking**: Salesforce org is unaware of package boundaries - components can be overwritten by other packages
* **No Platform Validation**: Dependencies are not validated by Salesforce
* **No Automated Rollback**: Cannot easily rollback to previous versions
* **Manual Destructive Changes**: Must be explicitly managed through pre/post-destructive folders
* **No Package Lifecycle**: No built-in versioning, upgrade, or deprecation paths
* **Dependency Risks**: Dependencies only validated at deployment time
* **Production Risk**: Higher risk of deployment failures in production

### Common Override Scenarios

Metadata components that commonly get overridden across packages:
* [Custom Labels](https://developer.salesforce.com/docs/atlas.en-us.api_meta.meta/api_meta/meta_customlabels.htm)
* Profiles and Permission Sets
* Custom Settings
* Global Value Sets

## Advanced Features (sfp-pro)

### Environment-Specific Configuration

Source packages in sfp-pro support two powerful mechanisms for managing environment-specific differences:

#### 1. Aliasified Packages

Deploy different metadata variants based on org aliases:

```
src-env-specific/
└── main/
    ├── default/        # Fallback for sandboxes
    │   └── classes/
    ├── dev/           # Dev-specific metadata
    │   └── classes/
    └── prod/          # Production metadata
        └── classes/
```

See [Aliasified Packages](../../building-artifacts/configuring-installation-behaviour-of-a-package/aliasfy-packages/) for details.

#### 2. Text Replacements

Replace configuration values during deployment without file duplication:

```yaml
# preDeploy/replacements.yml
replacements:
  - name: "API Endpoint"
    glob: "**/*.cls"
    pattern: "%%API_URL%%"
    environments:
      default: "https://api.dev.example.com"
      prod: "https://api.example.com"
```

See [String Replacements](../../building-artifacts/configuring-installation-behaviour-of-a-package/string-replacements.md) for details.

### Destructive Changes Support

Source packages fully support destructive changes through dedicated folders:

* **pre-destructive/**: Components deleted before deployment
* **post-destructive/**: Components deleted after deployment

```
my-package/
├── main/
│   └── default/
├── pre-destructive/     # Deleted before main deployment
│   └── objects/
└── post-destructive/    # Deleted after main deployment
    └── classes/
```

Destructive changes are automatically detected and processed during package installation.

### Testing Behavior

Source packages have intelligent test execution control:

* **Development/Sandbox**: Tests skipped by default for faster iterations
* **Production**: Tests always run (Salesforce requirement)
* **Override Options**:
  * `sfp install --runtests`: Force test execution
  * Package-level `skipTesting` in sfdx-project.json

This provides significant performance improvements during development while maintaining production safety.

## Dependency Management

**Source packages can depend on other unlocked packages or managed packages**. Dependencies are validated at deployment time, meaning the dependent metadata must already exist in the target org.

For development in scratch orgs, you can add dependencies to enable automatic installation:

```json
{
  "packageDirectories": [
    {
      "path": "src/my-package",
      "package": "my-package",
      "versionNumber": "1.0.0.NEXT",
      "dependencies": [
        {"package": "another-source-package"},
        {"package": "unlocked-package@1.2.3"},
        {"subscriberPackageVersionId": "04t..."}
      ]
    }
  ]
}
```

sfp commands like `prepare` and `validate` will automatically install dependencies before deploying the source package.

## Best Practices

1. **Use unlocked packages for shared libraries** that need version control and component protection
2. **Use source packages for environment-specific configuration** and org-specific metadata
3. **Organize large source packages** into logical domains for better maintainability
4. **Leverage aliasified packages** for structural differences between environments
5. **Use text replacements** for configuration values that change across environments
6. **Document destructive changes** clearly in your release notes
7. **Test in lower environments** before production deployment
8. **Consider org-dependent unlocked packages** as a middle ground when validation time is a concern

## Migration Paths

### From Unpackaged Metadata
1. Identify logical groupings of metadata
2. Create source package entries in sfdx-project.json
3. Move metadata into package directories
4. Define dependencies between packages
5. Test deployment order

### To Unlocked Packages
1. Start with source packages to organize metadata
2. Identify stable components suitable for packaging
3. Gradually convert source packages to unlocked packages
4. Keep environment-specific components as source packages