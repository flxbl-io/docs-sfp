---
icon: ring-diamond
description: >-
  Fetches a review environment from a specified pool and assigns it to a pull
  request/issue.
---

# Fetch a Review Environment

{% hint style="info" %}
The commands are only available in sfp-pro (August 24 onwards) and currently limited to GitHub. Using these commands requires an equivalent APP\_ID & PRIVATE\_KEY in your environment variable.
{% endhint %}

### Usage

```
sfp reviewenv fetch --repository <owner/repo> --pool <pool> --poolType <type> --branch <branch> --issue <issue> [--devhubAlias <alias>] [--wait <minutes>] [--leaseFor <minutes>]
```

### Options

* `--repository`: The repository path that stores the pool lock (default: current repo).
* `--pool`: The name of the pool to fetch the review environment from (required).
* `--poolType`: The type of the pool, either `sandbox` or `scratchorg` (required).
* `--branch`: The pull request branch to fetch the environment for (required).
* `--issue`: The pull request number to assign the environment to, or a unique id that will be used subsequently to identify  (required).
* `--devhubAlias`: The DevHub alias associated with the pool (default: `devhub`).
* `--wait`: Time in minutes to wait for an environment to become available (default: 20).
* `--leaseFor`: Time in minutes typically this environment is leased for during  similar operations

### Behavior

1. Checks if an environment is already assigned to the issue.
2. If assigned, verifies if the previous lease has expired based on the `leaseFor` duration. If a job has not released the environment within the earlier mentioned leaseFor, the new job will be provided the environment
3. If no environment is assigned or the environment assigned to the issue has expired, fetches a new environment from the pool.
4. Automatically marks the fetched or reassigned environment as "InUse".
5. Waits for up to the specified `wait` time if no environment is immediately available.

### Notes

* The environment remains valid for 24 hours from assignment, regardless of the `leaseFor` duration.
* The `leaseFor` parameter determines how long the current process can use the environment before it becomes available for reassignment for a different job within the same issue.
