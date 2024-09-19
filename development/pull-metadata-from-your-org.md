# Pull Metadata from your org

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
| `--json`                   | Format output as JSON                            | No       |
| `--loglevel`               | Logging level                                    | No       |

### Flag Details

* The `-p`, `-d`, and `-s` flags are mutually exclusive. Use only one to specify the scope of the pull operation.
* `--ignore-conflicts`: Use this flag to override conflicts and pull changes from the org, potentially overwriting local changes.
* `--retrieve-path`: Specifies a custom location for the retrieved source files.
* `--json`: When specified, the command outputs a structured JSON object with detailed information about the pull operation.

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
  ]
}
```

### Error Handling

If an error occurs during the pull operation, the command will throw an error with details about what went wrong. Use the `--json` flag to get structured error information in the output.

