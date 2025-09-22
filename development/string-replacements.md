---
icon: ring-diamond
---

# String Replacements

|              | sfp-pro          | sfp (community) |
| ------------ | ---------------- | --------------- |
| Availability | ✅                | ❌               |
| From         | September 2025   |                 |

String replacements provide a mechanism to manage environment-specific values in your Salesforce code without modifying source files. This feature automatically replaces placeholders with appropriate values during build, install, and push operations, and converts values back to placeholders during pull operations.

String replacements complement the existing [aliasfy packages](../building-artifacts/configuring-installation-behaviour-of-a-package/aliasfy-packages/) feature. While aliasfy packages handle structural metadata differences by deploying different files per environment, string replacements handle configuration value differences within the same files, reducing duplication and maintenance overhead.

### How It Works

String replacements work across multiple sfp commands:

**Build Operations**: During `sfp build`, replacement configurations are analyzed and embedded in the artifact for later use during installation.

**Install/Deploy Operations**: During `sfp install` or `sfp deploy`, placeholders are replaced with environment-specific values based on the target org:
```
Artifact (%%API_URL%%) → Install → Target Org (https://api.example.com)
```

**Push Operations**: During `sfp push`, placeholders in your source files are replaced with environment-specific values before deployment:
```
Source File (%%API_URL%%) → Push → Target Org (https://api.example.com)
```

**Pull Operations**: During `sfp pull`, environment-specific values are converted back to placeholders:
```
Target Org (https://api.example.com) → Pull → Source File (%%API_URL%%)
```

### Configuration

Replacements are configured in a `replacements.yml` file within each package's `preDeploy` directory:

```
src/
  your-package/
    preDeploy/
      replacements.yml
    main/
      default/
        classes/
```

Example configuration:

```yaml
replacements:
  - name: "API Endpoint"
    pattern: "%%API_ENDPOINT%%"
    glob: "**/*.cls"
    environments:
      default: "https://api-sandbox.example.com"
      dev: "https://api-dev.example.com"
      staging: "https://api-staging.example.com"
      prod: "https://api.example.com"

  - name: "Support Email"
    pattern: "%%SUPPORT_EMAIL%%"
    glob: "**/*.cls"
    environments:
      default: "support-sandbox@example.com"
      dev: "support-dev@example.com"
      prod: "support@example.com"
```

The properties accepted by the configuration file are:

| Field | Required | Description |
|-------|----------|-------------|
| `name` | Yes | Human-readable name for the replacement |
| `pattern` | Yes | The placeholder pattern to replace (e.g., `%%API_URL%%`) |
| `glob` | Yes | File pattern for matching files (e.g., `**/*.cls` for all Apex classes) |
| `environments` | Yes | Map of environment aliases to replacement values |
| `environments.default` | No | Default value used for sandbox/scratch orgs (recommended) |
| `isRegex` | No | Whether the pattern is a regular expression (default: false) |

### Example Usage

#### API Configuration Example

Source file with placeholders:
```java
public class APIService {
    private static final String ENDPOINT = '%%API_ENDPOINT%%';
    private static final String API_KEY = '%%API_KEY%%';
}
```

After pushing to a dev org, the placeholders are replaced:
```java
public class APIService {
    private static final String ENDPOINT = 'https://api-dev.example.com';
    private static final String API_KEY = 'dev-key-12345';
}
```

### Pattern Detection

During pull operations, sfp automatically detects potential patterns that could be converted to replacements:

* **URLs**: Detects HTTP/HTTPS URLs
* **Email Addresses**: Identifies email patterns
* **API Keys**: Recognizes common API key formats
* **Custom Patterns**: Detects repetitive values across files

When patterns are detected, sfp provides suggestions:

```
⚠️  Potential replacements detected:

  📄 src/package/main/default/classes/APIService.cls:
     • URL detected: 'https://new-api.example.com/v2'
       New URL pattern detected. Consider adding to replacements.yml

  💡 To include these values in future replacements, update your replacements.yml file.
```

### Environment Resolution

Replacements are resolved based on the target org:

* **Exact Alias Match**: First checks for an exact match with the org alias
* **Sandbox Default**: For sandbox/scratch orgs, uses the `default` value
* **Production Requirement**: Production deployments require explicit configuration

### Org Alias Mapping

The org alias is determined from your Salesforce CLI authentication:

```bash
# Check your org aliases
sf org list

# Push with specific org alias
sfp push -o dev-sandbox -p your-package
```

### Command Support

String replacements are supported across the following sfp commands:

| Command | Support | Description |
|---------|---------|-------------|
| `sfp build` | ✅ | Analyzes and embeds replacement configurations in artifacts |
| `sfp install` | ✅ | Applies replacements during artifact installation |
| `sfp deploy` | ✅ | Applies replacements during artifact deployment |
| `sfp push` | ✅ | Applies forward replacements during source push |
| `sfp pull` | ✅ | Applies reverse replacements during source pull |
| `sfp validate` | ✅ | Applies replacements during validation |

### Command Line Options

#### Disable Replacements

```bash
# Skip replacements during install
sfp install --targetorg dev --artifactdir artifacts --no-replacements

# Skip replacements during push
sfp push -p your-package -o dev --no-replacements

# Skip replacements during pull
sfp pull -p your-package -o dev --no-replacements
```

#### Override Replacements

```bash
# Use override file during install
sfp install --targetorg dev --artifactdir artifacts --replacementsoverride custom-replacements.yml

# Use override file during push
sfp push -p your-package -o dev --replacementsoverride custom-replacements.yml

# Use override file during pull
sfp pull -p your-package -o dev --replacementsoverride custom-replacements.yml
```

### JSON Output

Both push and pull commands support JSON output with detailed replacement information:

```bash
# Get JSON output with replacement details
sfp push -p your-package -o dev --json
sfp pull -p your-package -o dev --json
```

### JSON Output Structure

```json
{
  "hasError": false,
  "replacements": {
    "success": true,
    "packageName": "your-package",
    "filesModified": [
      {
        "path": "main/default/classes/APIService.cls",
        "replacements": [
          {
            "pattern": "%%API_ENDPOINT%%",
            "value": "https://api-dev.example.com",
            "count": 1
          }
        ],
        "totalCount": 1
      }
    ],
    "totalFiles": 1,
    "totalReplacements": 1,
    "errors": [],
    "orgAlias": "dev"
  }
}
```


### Troubleshooting

#### Replacements Not Applied

* **Check File Location**: Ensure `replacements.yml` is in `preDeploy` directory
* **Verify Glob Pattern**: Test glob pattern matches your files
* **Check Org Alias**: Verify the org alias matches your configuration
* **Review Logs**: Check debug logs for replacement processing

#### Pattern Not Found

* **Case Sensitivity**: Patterns are case-sensitive
* **Special Characters**: Escape special regex characters if needed
* **File Encoding**: Ensure files are UTF-8 encoded

#### Wrong Value Applied

* **Org Detection**: Verify org alias with `sf org list`
* **Environment Priority**: Check resolution order (exact match → default)
* **Override Files**: Check if override file is being used


### Limitations

* Replacements are text-based and work with any text file format
* Binary files are not supported
* Large files may impact performance
* Regex patterns should be used carefully to avoid unintended matches


### Related Documentation

* [String Replacements Configuration](../building-artifacts/configuring-installation-behaviour-of-a-package/string-replacements.md) - Configure string replacements in packages
* [String Replacements During Install](../installing-an-artifact/string-replacements-during-install.md) - How replacements work during installation
* [`sfp push`](push-changes-to-your-org.md) - Deploy changes with replacements
* [`sfp pull`](pull-changes-from-your-org.md) - Retrieve changes with reverse replacements