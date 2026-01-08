# Project

## `sfp server project`

Manage projects in sfp server

### Description

A **project** in sfp server represents a registered repository. The project identifier (e.g., `flxbl-io/sf-core`) is used throughout sfp to:

* Track builds, deployments, and release history
* Scope [integrations](../../project/integrations.md) to specific repositories
* Manage team access and permissions
* Link work items and commits

### Registering a Project

Before using project-scoped features, register your repository:

```bash
sfp server project register \
  --identifier "your-org/your-repo" \
  --remote-url "https://github.com/your-org/your-repo"
```

**What is the identifier?**

The identifier is typically your repository's `org/repo` format:

* GitHub: `flxbl-io/sf-core`
* GitLab: `acme-corp/salesforce-main`
* Bitbucket: `myteam/sf-project`

This identifier is used when:

* Creating project-scoped integrations
* Filtering builds and deployments
* Associating work items with repositories

### Available Commands

| Command                       | Description                  |
| ----------------------------- | ---------------------------- |
| `sfp server project register` | Register a new project       |
| `sfp server project list`     | List all registered projects |
| `sfp server project get`      | Get project details          |
| `sfp server project update`   | Update project configuration |
| `sfp server project delete`   | Remove a project             |

### Project Configuration

Projects can include:

* Repository information (URL, default branch)
* Build configurations
* Deployment targets
* Team assignments
* Access controls

### Common Use Cases

**Register a project for integration scoping:**

```bash
# Register project
sfp server project register \
  --identifier "acme/salesforce-main" \
  --remote-url "https://github.com/acme/salesforce-main"

# Now create project-scoped integrations
# See: /api-reference/integrations
```

**List registered projects:**

```bash
sfp server project list
```

### Related Topics

* [Integrations](../../project/integrations.md) - Store credentials scoped to projects
* [Repository](repository.md) - Manage repository authentication
* [Builds](builds.md) - View builds by project
