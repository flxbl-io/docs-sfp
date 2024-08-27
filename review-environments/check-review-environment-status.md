---
description: >-
  Checks the status of review environments assigned to a specific pull
  request/issue.
icon: ring-diamond
---

# Check Review Environment Status

{% hint style="info" %}
The commands are only available in sfp-pro (August 24 onwards) and currently limited to GitHub. Using these commands requires an equivalent APP\_ID & PRIVATE\_KEY in your environment variable.
{% endhint %}

### Usage

```
sfp reviewenv check --repository <owner/repo> --issue <issue> [--pool <pool>] [--poolType <type>] [--branch <branch>]
```

### Options

* `--repository`: The repository path that stores the pool lock (default: current repo).
* `--issue`: The pull request number to check for assigned environments (required).
* `--pool`: The name of the pool to filter by (optional).
* `--poolType`: The type of the pool to filter by, either `sandbox` or `scratchorg` (optional).
* `--branch`: The pull request branch to filter by (optional).

### Behavior

1. Searches the repository's stored environment data for environments associated with the specified issue.
2. Returns details of any found environments, including:
   * Environment name or username
   * Environment type (sandbox or scratch org)
   * Pool the environment belongs to
   * Associated pull request branch
   * Environment status and expiration date

### Notes

* This command is typically used as needed, not within the regular pull request workflow.
* It's useful for verifying environment details or troubleshooting.

