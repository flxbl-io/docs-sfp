---
description: >-
  This command logs in to a specified sandbox org using the authentication
  details of the user who requested  the creation of  sandbox
icon: ring-diamond
---

# Login to Sandbox

|              | sfp-pro   | sfp (community) |
| ------------ | --------- | --------------- |
| Availability | ✅         | ❌               |
| From         | August 24 |                 |



### Usage

```sh-session
$ sfp sandbox:login -n <string> [flags]
$ sfp org:login:sandbox -n <string> [flags]
```

### Flags

| Flag                                 | Required | Description                                          |
| ------------------------------------ | -------- | ---------------------------------------------------- |
| `-v, --targetdevhubusername=<value>` | No       | Username or alias of the production org              |
| `-n, --name=<value>`                 | Yes      | Name of the sandbox to log in to (max 10 characters) |
| `-a, --alias=<value>`                | No       | Alias to set for the authenticated sandbox org       |
| `-w, --write-file`                   | No       | Write login details to a file (org-info.json)        |
| `--json`                             | No       | Format output as JSON                                |
| `--loglevel=<option>`                | No       | Logging level for this command invocation            |

### Examples

```sh-session
$ sfp sandbox:login --name mySandbox1 -v myDevHub
$ sfp sandbox:login --name mySandbox1 -v myDevHub --alias mySandboxAlias --write-file
```
