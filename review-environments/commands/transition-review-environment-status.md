---
description: Updates the status of a review environment assigned to a specific issue.
icon: ring-diamond
---

# Transition Review Environment Status

{% hint style="info" %}
The commands are only available in sfp-pro (August 24 onwards) and currently limited to GitHub. Using these commands requires an equivalent APP\_ID & PRIVATE\_KEY in your environment variable.
{% endhint %}

### Usage

```
sfp reviewenv transition --repository <owner/repo> --issue <issue> --status <status>
```

### Options

* `--repository`: The repository path that stores the pool lock (default: current repo).
* `--issue`: The pull request number to assign the environment to, or a unique id that will be used subsequently to identify (required).
* `--status`: The status to transition the review environment to (required). Options: 'InUse', 'Available', 'Expired'

### Behavior

1. Locates the environment assigned to the specified issue.
2. Updates the status of the environment to the specified status.

### Status Meanings

* 'InUse': The environment is currently being used by automated checks or another automated process.
* 'Available': The environment is available for reuse by another automation within the same issue's context.
* 'Expired': The environment will be picked up by Pool commands for deletion.

### Notes

* This command doesn't reflect the state of the pull request, but rather the current usage state of the environment within the issue's context.
* Transitioning to 'Available' before the lease expires allows for efficient reuse within the same issue.
