---
icon: ring-diamond
---

# Pull Changes from your org

|              | sfp-pro   | sfp (community) |
| ------------ | --------- | --------------- |
| Availability | ✅         | ❌               |
| From         | August 24 |                 |

The `sfp project:pull` command retrieves source from a Salesforce org and updates your local project files. It can pull changes based on a package, domain, or specific source path. This command is useful for synchronizing your local project with the latest changes in your Salesforce org.

### Source Tracking

Source tracking is a feature in Salesforce development that keeps track of the changes made to metadata both in your local project and in the org. When source tracking is enabled, the `project:pull` command can more efficiently retrieve only the changes made in the org since the last sync, rather than retrieving all metadata.

#### How Source Tracking Works

* Source tracking maintains a history of changes in both your local project and the Salesforce org.
* It allows sfp to determine which components have been added, modified, or deleted since the last synchronization.
* This feature is automatically enabled for scratch orgs and can be enabled for non-scratch orgs that support it.

#### Source Tracking and `project:pull`

* When pulling from a source-tracked org without specifying a package, domain, or source path, the command will use source tracking to retrieve only the changes made in the org.
* For non-source-tracked orgs or when a specific scope is provided (via `-p`, `-d`, or `-s` flags), the command will retrieve all metadata within the specified scope.
* Source tracking provides faster and more efficient retrieval of changes, especially in large projects.

#### Limitations

* Source tracking is not available for all org types. It's primarily used with scratch orgs and some sandbox orgs.
* If source tracking is not enabled or supported, the `project:pull` command will fall back to retrieving all metadata within the specified scope.

### Usage

```
sfp project:pull -o <org> [flags]
```

### Flags

| Flag                       | Description                                      | Required |
| -------------------------- | ------------------------------------------------ | -------- |
| `-o`, `--targetusername`   | Username or alias of the target org              | Yes      |
| `-p`, `--package`          | Name of the package to pull                      | No       |
| `-d`, `--domain`           | Name of the domain to pull                       | No       |
| `-s`, `--source-path`      | Path to the local source files to pull           | No       |
| `-r`, `--retrieve-path`    | Path where the retrieved source should be placed | No       |
| `-i`, `--ignore-conflicts` | Ignore conflicts during pull                     | No       |
| `--no-replacements`        | Skip text replacements during pull               | No       |
| `--replacementsoverride`   | Path to override replacements file               | No       |
| `--json`                   | Format output as JSON                            | No       |
| `--loglevel`               | Logging level                                    | No       |

### Flag Details

* The `-p`, `-d`, and `-s` flags are mutually exclusive. Use only one to specify the scope of the pull operation.
* `--ignore-conflicts`: Use this flag to override conflicts and pull changes from the org, potentially overwriting local changes.
* `--retrieve-path`: Specifies a custom location for the retrieved source files.
* `--no-replacements`: Disables automatic text replacements. By default, sfp applies reverse replacements to convert environment-specific values back to placeholders.
* `--replacementsoverride`: Specify a custom YAML file containing replacement configurations to use instead of the default.
* `--json`: When specified, the command outputs a structured JSON object with detailed information about the pull operation, including replacement details and pattern suggestions.

### Examples

Pull changes using source tracking (if available):

```
sfp project:pull -o myOrg
```

Pull changes for a specific package:

```
sfp project:pull -o myOrg -p myPackage
```

Pull changes for a specific domain:

```
sfp project:pull -o myOrg -d myDomain
```

Pull changes from a specific source path:

```
sfp project:pull -o myOrg -s force-app/main/default
```

Pull changes and ignore conflicts:

```
sfp project:pull -o myOrg -p myPackage --ignore-conflicts
```

Pull changes without applying reverse replacements:

```
sfp project:pull -o myOrg -p myPackage --no-replacements
```

Pull changes with custom replacement configuration:

```
sfp project:pull -o myOrg -p myPackage --replacementsoverride custom-replacements.yml
```

### Text Replacements (Pro Feature)

{% hint style="info" %}
**Availability**: String replacements are available from September 2025 in sfp-pro only.
{% endhint %}

The pull command automatically applies reverse text replacements to convert environment-specific values back to placeholders when retrieving source from the org. This feature helps maintain clean, environment-agnostic code in your repository.

For detailed information about string replacements, see [String Replacements](string-replacements.md).

#### How Reverse Replacements Work

When you pull changes from an org, sfp automatically:

1. **Detects known values**: Identifies environment-specific values that match your replacement configurations
2. **Converts to placeholders**: Replaces these values with their placeholder equivalents
3. **Suggests new patterns**: Detects potential patterns that could be added to your replacements

#### Quick Example

If your org contains:
```java
private static final String API_URL = 'https://api-dev.example.com';
```

After pulling, it becomes:
```java
private static final String API_URL = '%%API_ENDPOINT%%';
```

#### Pattern Detection

During pull operations, sfp analyzes retrieved code for patterns that might benefit from replacements:

```
⚠️  Potential replacements detected:

  📄 force-app/main/default/classes/APIService.cls:
     • URL detected: 'https://new-api.example.com/v2'
       New URL pattern detected. Consider adding to replacements.yml

  💡 To include these values in future replacements, update your replacements.yml file.
```

To skip reverse replacements:
```bash
sfp pull -p myPackage -o myOrg --no-replacements
```

### JSON Output

When `--json` is specified, the command outputs a JSON object with the following structure:

```json
{
  "hasError": boolean,
  "errorMessage": string,
  "files": [
    {
      "fullName": string,
      "type": string,
      "createdByName": string,
      "lastModifiedByName": string,
      "createdDate": string,
      "lastModifiedDate": string
    }
  ],
  "conflicts": [
    {
      "fullName": string,
      "type": string,
      "filePath": string,
      "state": string
    }
  ],
  "errors": [
    {
      "fileName": string,
      "problem": string
    }
  ],
  "replacements": {
    "success": boolean,
    "packageName": string,
    "filesModified": [
      {
        "path": string,
        "replacements": [
          {
            "pattern": string,
            "value": string,
            "count": number
          }
        ],
        "totalCount": number
      }
    ],
    "totalFiles": number,
    "totalReplacements": number,
    "errors": [],
    "orgAlias": string,
    "suggestions": [
      {
        "filePath": string,
        "suggestions": [
          {
            "type": string,
            "value": string,
            "message": string,
            "pattern": string
          }
        ]
      }
    ]
  }
}
```

The `replacements` field (available in sfp-pro) provides detailed information about reverse text replacements applied during the pull operation, including any pattern suggestions detected.

### Error Handling

If an error occurs during the pull operation, the command will throw an error with details about what went wrong. Use the `--json` flag to get structured error information in the output.

