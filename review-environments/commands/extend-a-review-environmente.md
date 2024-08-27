---
description: Extends the lease of a review environment assigned to a specific issue.
icon: ring-diamond
---

# Extend a Review Environmente

{% hint style="info" %}
The commands are only available in sfp-pro (August 24 onwards) and currently limited to GitHub. Using these commands requires an equivalent APP\_ID & PRIVATE\_KEY in your environment variable.
{% endhint %}

### Usage

```
sfp reviewenv extend --repository <owner/repo> --issue <issue>
```

### Options

* `--repository`: The repository path that stores the pool lock (default: current repo).
* `--issue`: The pull request number of the assigned environment to extend (required).

### Behavior

1. Locates the environment assigned to the specified issue.
2. Extends the overall validity of the environment by an additional 24 hours from the current time.

### Notes

* This command is useful when more time is needed for thorough testing or when waiting for stakeholder approval.
* It extends the overall validity of the environment, not the lease time for a specific process.
* Use judiciously to avoid unnecessarily tying up resources.
