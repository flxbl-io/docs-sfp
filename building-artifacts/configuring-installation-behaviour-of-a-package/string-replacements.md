# String Replacements

<table><thead><tr><th width="229">Attribute</th><th>Type</th><th>Description</th><th>Package Types Applicable</th></tr></thead><tbody><tr><td>replacements</td><td>configuration file</td><td>Enable replacement of placeholder values with environment-specific values during installation</td><td><p></p><ul><li>source</li></ul><p></p></td></tr></tbody></table>

{% hint style="info" %}
String replacements are available only for Source Packages and require sfp-pro (available from September 2025)
{% endhint %}

String replacements enable automatic substitution of placeholder values with environment-specific values during package installation, similar to how aliasfy packages work with folder structures.

### How It Works

During build, when a package contains a `preDeploy/replacements.yml` file:

1. The configuration is analyzed and validated
2. The replacement patterns are embedded in the artifact
3. During installation, placeholders are replaced based on the target org

For example, if you have placeholders like `%%API_ENDPOINT%%` in your code, they get replaced with the appropriate value for the target environment during installation.

### Configuration

Place your `replacements.yml` file in the package's `preDeploy` directory:

```
src/
  your-package/
    preDeploy/
      replacements.yml
    main/
      default/
```

The replacement configuration will be automatically detected and processed during build.

### Package Type Requirements

String replacements are only supported for source packages. If you attempt to use replacements with other package types (unlocked, org-dependent unlocked, or data), the build will fail with an error message indicating that you must either remove the replacements.yml file or change the package type to 'source'.

### Example Configuration

```json
// Demonstrating package directory with string replacements
{
  "packageDirectories": [
    {
      "path": "src/your-package",
      "package": "your-package",
      "versionNumber": "1.0.0.NEXT"
      // No specific flag needed - replacements are auto-detected
    }
  ]
}
```

The replacements.yml file in the preDeploy folder is automatically detected and processed during build.

### When to Use String Replacements vs Aliasfy Packages

String replacements and aliasfy packages are complementary features for managing environment-specific configurations:

#### Use String Replacements For:
* **Configuration values** that change between environments (API endpoints, email addresses, feature flags)
* **Reducing duplication** when only specific values differ in otherwise identical files
* **Maintaining single source** for business logic while varying configuration
* **Code-based metadata** (Apex classes, Lightning components) with environment-specific values

#### Use Aliasfy Packages For:
* **Structural differences** between environments (different fields, objects, workflows)
* **Completely different metadata** per environment (unique page layouts, permission sets)
* **Declarative configurations** that vary significantly by environment
* **Binary metadata** that cannot be text-replaced (images, static resources)

### Migration Strategy

If you're currently using aliasfy packages with duplicated files just for value changes, consider migrating to string replacements:

1. **Identify candidates**: Find files duplicated solely for configuration value changes
2. **Consolidate files**: Create single source file with placeholder patterns
3. **Create replacements.yml**: Define environment-specific values
4. **Remove duplicates**: Delete redundant files from aliasfy folders
5. **Test thoroughly**: Validate in lower environments before production

### Example: Before and After Migration

**Before (Using Aliasfy - 3 duplicate files):**
```
src-env-specific/
  default/classes/APIService.cls     # endpoint = "https://api-dev.example.com"
  staging/classes/APIService.cls     # endpoint = "https://api-staging.example.com"
  production/classes/APIService.cls  # endpoint = "https://api.example.com"
```

**After (Using String Replacements - 1 file + config):**
```
// Single APIService.cls with placeholder
public class APIService {
    private static final String ENDPOINT = '%%API_ENDPOINT%%';
}

// preDeploy/replacements.yml
replacements:
  - name: "API Endpoint"
    pattern: "%%API_ENDPOINT%%"
    glob: "**/APIService.cls"
    environments:
      default: "https://api-dev.example.com"
      staging: "https://api-staging.example.com"
      production: "https://api.example.com"
```

{% hint style="info" %}
String replacements reduce maintenance overhead by eliminating file duplication while maintaining environment-specific configurations
{% endhint %}

### Related Documentation

* [String Replacements Overview](../../development/string-replacements.md)
* [String Replacements During Install](../../installing-an-artifact/string-replacements-during-install.md)
* [Aliasfy Packages](aliasfy-packages/)