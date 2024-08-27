---
icon: ring-diamond
---

# Overview

Review environments are a crucial component in the CI/CD workflow of flxbl projects. They provide isolated, ephemeral environments (either scratch orgs or sandboxes) for validating deployments, running tests, and performing acceptance testing during the pull request process.

### Key Benefits

1. **Isolation**: Each pull request gets its own environment, preventing conflicts between different features or bug fixes.
2. **Reproducibility**: Environments are created from consistent pool configurations, ensuring predictable testing conditions.
3. **Early Validation**: Catch deployment issues early in the development process.
4. **Automated Testing**: Run tests against review environments to verify changes and prevent regressions.
5. **Collaboration**: Provide a shared space for team members to perform acceptance testing and provide feedback.
6. **Compliance**: Enforce governance policies and organizational standards before merging changes.

### How It Works

1. **Environment Pools**: Review environments are managed in pools, which can be either sandbox or scratch org pools.
2. **Fetching**: When a pull request is opened or updated, a review environment is fetched from the appropriate pool. If an environment is already assigned to the issue, it may be reused if its lease hasn't expired.
3. **Usage**: The environment is populated with the pull request changes and used for automated checks, manual testing, and review processes.
4. **Leasing**: Environments are leased for specific durations to manage resource usage efficiently. While an environment is valid for 24 hours, it can be leased for shorter periods for specific processes.
5. **Status Management**: Throughout its lifecycle, an environment's status can be transitioned between 'InUse', 'Available', and 'Expired' to reflect its current state and availability.
6. **Extension**: If needed, an environment's overall validity can be extended beyond the initial 24-hour period.
7. **Unassignment**: When no longer needed, environments are unassigned from issues and either returned to the pool or marked for deletion.

This lifecycle is managed through a set of `sfp reviewenv` commands, which automate the process of fetching, checking, transitioning, extending, and unassigning review environments.
