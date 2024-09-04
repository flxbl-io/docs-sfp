---
icon: ring-diamond
description: Removes the assignment of a review environment from an issue.
---

# Unassign a Review Environment

{% hint style="info" %}
The commands are only available in sfp-pro (August 24 onwards) and currently limited to GitHub. Using these commands requires an equivalent APP\_ID & PRIVATE\_KEY in your environment variable.
{% endhint %}

### Usage

```
sfp reviewenv unassign --repository <owner/repo> --issue <issue> [--returntopool]
```

### Options

* `--repository`: The repository path that stores the pool lock (default: current repo).
* `--issue`: The pull request number to assign the environment to, or a unique id that will be used subsequently to identify (required).
* `--returntopool`: If set to true, the environment will be returned to the pool for reuse. If false or not set, it will be marked as expired.

### Behavior

1. Locates the environment assigned to the specified issue.
2. Removes the assignment of the environment from the issue.
3. Based on the `--returntopool` flag:
   * If true: Marks the environment as available for reuse within the pool.
   * If false or not set: Marks the environment as expired, to be picked up by Pool commands for deletion.

### Notes

* This command should be used when a review environment is no longer needed for an issue.
* Consider carefully whether to return the environment to the pool or mark it as expired based on your project's needs and the state of the environment.
