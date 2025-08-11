# Review Environments

## `sfp server review-envs`

Manage review environments in sfp server

### Description

The review-envs commands provide functionality for managing ephemeral review environments (also known as preview environments) that are automatically created for pull requests and feature branches.

### Available Commands

* Create review environments
* List active review environments
* Fetch review environment details
* Extend review environment lifetime
* Transition environment status
* Unassign review environments
* Delete review environments

### Review Environment Lifecycle

1. **Creation**: Automatically or manually created for PRs
2. **Active**: Available for testing and review
3. **Extended**: Lifetime extended if more time needed
4. **Expired**: Marked for cleanup after timeout
5. **Deleted**: Resources reclaimed

### Common Use Cases

- Automatic environment provisioning for PRs
- Feature branch testing
- UAT and review processes
- Isolated testing environments
- Demo environments

### Integration with CI/CD

Review environments can be automatically:
- Created when PRs are opened
- Updated when commits are pushed
- Deleted when PRs are merged or closed

> **Note**: Detailed command documentation coming soon.

> **Best Practice**: Configure automatic cleanup policies to manage resource usage efficiently.