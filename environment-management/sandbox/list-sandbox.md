---
description: >-
  This command lists sandboxes and their statuses from the specified production
  org. You can filter the results by sandbox name and status.
icon: ring-diamond
---

# List Sandbox

|              | sfp-pro      | sfp (community) |
| ------------ | ------------ | --------------- |
| Availability | ✅            | ❌               |
| From         | September 24 |                 |

### Usage

```sh-session
$ sfp sandbox:list [flags]
$ sfp sandbox:status [flags]
```

### Flags

| Flag                                 | Required | Description                                        |
| ------------------------------------ | -------- | -------------------------------------------------- |
| `-v, --targetdevhubusername=<value>` | No       | Username or alias of the production org            |
| `-n, --name=<value>`                 | No       | Filter results by sandbox name (max 10 characters) |
| `-s, --status=<value>`               | No       | Filter results by sandbox status                   |
| `--json`                             | No       | Format output as JSON                              |
| `--loglevel=<option>`                | No       | Logging level for this command invocation          |

### Status Options

Available status options: 'Pending', 'Processing', 'Completed', 'Deleted', 'Deleting', 'Discarding', 'Locked', 'Locking', 'Sampling', 'Stopped', 'Suspended', 'Activating'

### Examples

```sh-session
$ sfp sandbox:list --name mySandbox1 -v myDevHub
$ sfp sandbox:status --status Completed -v myDevHub
```
