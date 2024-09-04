---
description: Create a new Salesforce sandbox org.
icon: ring-diamond
---

# Create Sandbox

|              | sfp-pro   | sfp (community) |
| ------------ | --------- | --------------- |
| Availability | ✅         | ❌               |
| From         | August 24 |                 |

\
This command creates a new sandbox org from a source org (production or another sandbox). You can specify the sandbox configuration using command-line options or a definition file.

### Usage

```sh-session
$ sfp sandbox:create -v <string> -n <string> -s <string> [flags]
$ sfp org:create:sandbox -v <string> -n <string> -s <string> [flags]
```

### Flags

| Flag                                 | Required                    | Description                               |
| ------------------------------------ | --------------------------- | ----------------------------------------- |
| `-v, --targetdevhubusername=<value>` | No                          | Username or alias of the production org   |
| `-n, --name=<value>`                 | Yes (if no definition file) | Name of the sandbox to create             |
| `-s, --sourcesandbox=<value>`        | No                          | Name of the source sandbox to clone       |
| `-f, --definition-file=<value>`      | No                          | Path to a sandbox definition file         |
| `-d, --description=<value>`          | No                          | Description of the sandbox                |
| `--json`                             | No                          | Format output as JSON                     |
| `--loglevel=<option>`                | No                          | Logging level for this command invocation |

### Examples

```sh-session
$ sfp sandbox:create -v MyDevHub -n MySandbox1 -s SourceSandbox
```

### Sandbox Definition File

If using a definition file, create a JSON file (default location: `config/sandbox-def.json`) with the following structure:

```json
{
  "sandboxName": "MySandbox",
  "description": "My sandbox description",
  "sourceSandbox": "SourceSandboxName",
  "apexClassId": "01p...",
  "autoActivate": true,
  "copyChatter": true,
  "copyArchivedActivities": true,
  "licenseType": "Developer",
  "templateId": "0TT...",
  "historyDays": 0
}
```

Note: When using a definition file, the `--name` and `--sourcesandbox` flags are ignored.
