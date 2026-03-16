---
metaLinks:
  alternates:
    - >-
      https://app.gitbook.com/s/YLI5Ts7pWhWQV9UaBn3H/development/development-environment
---

# Development Environment

In a Flxbl project powered by sfp, development follows a structured, iterative workflow designed for team collaboration and continuous delivery. This section guides you through the complete development lifecycle - from setting up your environment to submitting code for review.

## Prerequisites

**DevHub Access is Required** for sfp development:

* Building any type of package (source, unlocked, data)
* Creating and managing scratch orgs
* Resolving package dependencies

Ensure your Salesforce user has DevHub access by following [Salesforce's DevHub setup guide](https://developer.salesforce.com/docs/atlas.en-us.sfdx_dev.meta/sfdx_dev/sfdx_setup_add_free_license_users.htm).

## The Development Flow

Development in an sfp project follows these key principles:

1. **Isolated Environments**: Each developer works in their own sandbox or scratch org
2. **Source-Driven Development**: All changes are tracked in version control
3. **Iterative Cycles**: Developers continuously pull, modify, and push changes
4. **Automated Validation**: Pull requests trigger comprehensive validation
5. **Review Environments**: Stakeholders can test changes before merge

## How Developers Work

Developers in an sfp project typically follow this pattern:

### Starting a Sprint or Feature

* **Fetch a fresh environment** from a pre-prepared pool
  * Scratch orgs: Can be fetched per story for maximum isolation
  * Sandboxes: Usually fetched at sprint start for longer-running work
* **Pull the latest metadata** to sync with the org
* **Create a feature branch** for version control

### Daily Development Cycle

* **Pull changes** from the org to stay synchronized
* **Make modifications** to metadata and code
* **Push changes** back to the org for testing
* **Run tests locally** to validate functionality
* **Build artifacts** to ensure packages are valid

### Completing Work

* **Create a pull request** with your changes
* **Automated validation** runs via CI/CD pipeline
* **Review environment** is created for acceptance testing
* **Merge** after approval and successful validation

## Environment Management

sfp provides environment management through server-managed pools:

```bash
# Fetch an org from the pool (scratch or sandbox)
sfp pool fetch --tag dev-pool

# The same command works for both scratch orgs and sandboxes
# Pool type is configured at the server level
```

Pool provisioning and replenishment is handled by the sfp server. This means developers spend less time waiting for environment creation and more time coding.

## Source Synchronization

The foundation of sfp development is bidirectional source synchronization:

### Pull Command

Retrieves changes from your org to local source:

* Automatically handles text replacement reversals
* Detects conflicts and provides resolution options
* Suggests new replacements for hardcoded values

### Push Command

Deploys local changes to your org:

* Applies environment-specific text replacements
* Handles destructive changes automatically
* Supports various test execution levels

## What's Next?

For a complete walkthrough of the development workflow with detailed commands and examples:

{% content-ref url="development-workflow.md" %}
[development-workflow.md](development-workflow.md)
{% endcontent-ref %}

For specific development tasks:

{% content-ref url="pull-changes-from-your-org.md" %}
[pull-changes-from-your-org.md](pull-changes-from-your-org.md)
{% endcontent-ref %}

{% content-ref url="push-changes-to-your-org.md" %}
[push-changes-to-your-org.md](push-changes-to-your-org.md)
{% endcontent-ref %}

For managing environments:

{% content-ref url="../environment-management/pools/" %}
[pools](../environment-management/pools/)
{% endcontent-ref %}
