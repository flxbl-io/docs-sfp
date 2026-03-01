---
icon: puzzle-piece
---

# Integrations

Integrations connect sfp to external services for source control, work item tracking, artifact management, and notifications. Credentials are encrypted at rest using AES-256 and can be scoped globally or per project.

## Supported Providers

| Provider                             | Auth Types              | Use Case                           |
| ------------------------------------ | ----------------------- | ---------------------------------- |
| [Azure DevOps](azure-devops.md)      | Service Principal, PAT  | Source control, builds, work items |
| GitHub                               | PAT, OAuth, GitHub App  | Source control, PRs, status checks |
| GitLab                               | PAT                     | Source control, merge requests     |
| Jira                                 | Basic Auth, OAuth       | Work item tracking                 |
| JFrog                                | API Key                 | Artifact registry                  |
| Slack                                | Webhook                 | Notifications                      |

## Credential Management

Integrations support two scoping models:

* **Global** (`isGlobal: true`): A single set of credentials shared across all projects. Suitable when one service account covers the entire organization.
* **Project-scoped** (`projects: [...]`): Credentials assigned to specific repositories. Use when teams require isolated access or different service accounts per project.

During operations, sfp resolves credentials by checking project-scoped integrations first, then falling back to global integrations.

## Further Reading

* [Integrations API Reference](../api-reference/integrations.md) — REST endpoints for managing integrations programmatically.
* [Azure DevOps Setup Guide](azure-devops.md) — Step-by-step Service Principal and PAT configuration.
