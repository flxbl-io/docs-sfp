---
description: Delete one or more Salesforce sandbox orgs.
icon: ring-diamond
metaLinks:
  alternates:
    - >-
      https://app.gitbook.com/s/YLI5Ts7pWhWQV9UaBn3H/environment-management/sandbox/delete-sandbox
---

# Delete Sandbox

### Usage

```sh-session
$ sfp sandbox:delete -n <string> [flags]
$ sfp org:delete:sandbox -n <string> [flags]
```

### Flags

| Flag                                 | Required | Description                                     |
| ------------------------------------ | -------- | ----------------------------------------------- |
| `-v, --targetdevhubusername=<value>` | No       | Username or alias of the production org         |
| `-n, --name=<value>`                 | Yes      | Comma-separated list of sandbox names to delete |
| `--json`                             | No       | Format output as JSON                           |
| `--loglevel=<option>`                | No       | Logging level for this command invocation       |

### Examples

```sh-session
$ sfp sandbox:delete -n SIT -v my-sandbox-org
$ sfp sandbox:delete -n SIT,UAT,DEV -v my-sandbox-org
```
