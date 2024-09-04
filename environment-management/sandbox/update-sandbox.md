---
description: This command updates the configuration of an existing sandbox or refreshes it.
icon: ring-diamond
---

# Update Sandbox

|              | sfp-pro   | sfp (community) |
| ------------ | --------- | --------------- |
| Availability | ✅         | ❌               |
| From         | August 24 |                 |

### Usage

```sh-session
$ sfp sandbox:update [flags]
$ sfp org:update:sandbox [flags]
```

### Flags

| Flag                                 | Required                    | Description                               |
| ------------------------------------ | --------------------------- | ----------------------------------------- |
| `-v, --targetdevhubusername=<value>` | No                          | Username or alias of the production org   |
| `-n, --name=<value>`                 | Yes (if no definition file) | Name of the sandbox to update             |
| `-f, --definition-file=<value>`      | No                          | Path to a sandbox definition file         |
| `--json`                             | No                          | Format output as JSON                     |
| `--loglevel=<option>`                | No                          | Logging level for this command invocation |

### Examples

```sh-session
$ sfp sandbox:update -o MyDevHub -n my-sandbox
$ sfp sandbox:update -o MyDevHub -f config/sandbox-def.json
```

### Sandbox Definition File

If using a definition file, create a JSON file (default location: `config/sandbox-def.json`) with the following structure:

```json
{
  "sandboxName": "MySandbox",
  "description": "Updated sandbox description",
  "apexClassId": "01p...",
  "autoActivate": true,
  "copyChatter": true,
  "historyDays": 0,
  "licenseType": "DEVELOPER",
  "templateId": "0TT..."
}
```

Note: When using a definition file, the `--name` flag is ignored.
