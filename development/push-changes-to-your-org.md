---
icon: ring-diamond
---

# Push Changes to your org

|              | sfp-pro   | sfp (community) |
| ------------ | --------- | --------------- |
| Availability | ✅         | ❌               |
| From         | August 24 |                 |



The `sfp project:push` command deploys source from your local project to a specified Salesforce org. It can push changes based on a package, domain, or specific source path. This command is useful for deploying local changes to your Salesforce org.

### Usage

```
sfp project:push -o <org> [flags]
```

### Flags

| Flag                       | Description                            | Required |
| -------------------------- | -------------------------------------- | -------- |
| `-o`, `--targetusername`   | Username or alias of the target org    | Yes      |
| `-p`, `--package`          | Name of the package to push            | No       |
| `-d`, `--domain`           | Name of the domain to push             | No       |
| `-s`, `--source-path`      | Path to the local source files to push | No       |
| `-i`, `--ignore-conflicts` | Ignore conflicts during push           | No       |
| `--json`                   | Format output as JSON                  | No       |
| `--loglevel`               | Logging level                          | No       |

### Flag Details

* The `-p`, `-d`, and `-s` flags are mutually exclusive. Use only one to specify the scope of the push operation.
* `--ignore-conflicts`: Use this flag to override conflicts and push changes to the org, potentially overwriting org metadata.
* `--json`: When specified, the command outputs a structured JSON object with detailed information about the push operation.

### Source Tracking

Source tracking is a feature that keeps track of the changes made to metadata both in your local project and in the org. When source tracking is enabled, the `project:push` command can more efficiently deploy only the changes made locally since the last sync, rather than deploying all metadata.

#### How Source Tracking Works with `project:push`

* When pushing to a source-tracked org without specifying a package, domain, or source path, the command will use source tracking to deploy only the local changes.
* For non-source-tracked orgs or when a specific scope is provided (via `-p`, `-d`, or `-s` flags), the command will deploy all metadata within the specified scope.
* Source tracking provides faster and more efficient deployment of changes, especially in large projects.

#### Limitations

* Source tracking is not available for all org types. It's primarily used with scratch orgs and some sandbox orgs.
* If source tracking is not enabled or supported, the `project:push` command will fall back to deploying all metadata within the specified scope.

### Examples

Push changes using source tracking (if available):

```
sfp project:push -o myOrg
```

Push changes for a specific package:

```
sfp project:push -o myOrg -p myPackage
```

Push changes for a specific domain:

```
sfp project:push -o myOrg -d myDomain
```

Push changes from a specific source path:

```
sfp project:push -o myOrg -s force-app/main/default
```

Push changes and ignore conflicts:

```
sfp project:push -o myOrg -i
```

### JSON Output

When `--json` is specified, the command outputs a JSON object with the following structure:

```json
{
  "hasError": boolean,
  "errorMessage": string,
  "errors": [
    {
      "Name": string,
      "Type": string,
      "Status": string,
      "Message": string
    }
  ],
  "conflicts": [
    {
      "fullName": string,
      "type": string,
      "filePath": string,
      "state": string
    }
  ]
}
```

### Error Handling

If an error occurs during the push operation, the command will throw an error with details about what went wrong. Use the `--json` flag to get structured error information in the output.

