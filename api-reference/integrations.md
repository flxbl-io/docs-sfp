# Integrations

Integrations allow you to securely store and manage credentials for external services like **Jira** and **GitHub**. These credentials are encrypted at rest using AES-256 and can be scoped to specific projects or made globally available.

## Key Concepts

### What is an Integration?

An integration is a secure credential store that connects sfp server to external services. For example:
- **Jira integration**: Stores your Jira API credentials so sfp can fetch work items, link commits to issues, and track deployment status
- **GitHub integration**: Stores GitHub tokens for repository access, PR comments, and status checks

### What is a Project?

A **project** in sfp server represents a registered repository (e.g., `flxbl-io/sf-core`). Projects are the foundation for:
- Tracking builds and deployments
- Scoping integrations to specific repositories
- Managing team access and permissions

Before creating project-scoped integrations, you must first register your project:

```bash
# Register a project via CLI
sfp server project register --identifier "your-org/your-repo" --remote-url "https://github.com/your-org/your-repo"
```

Or via API:
```bash
curl -X POST https://your-server/sfp/api/projects \
  -H "Authorization: Bearer $TOKEN" \
  -H "Content-Type: application/json" \
  -d '{
    "identifier": "your-org/your-repo",
    "remoteUrl": "https://github.com/your-org/your-repo"
  }'
```

### Global vs Project-Scoped Integrations

| Scope | Use Case | Example |
|-------|----------|---------|
| **Global** (`isGlobal: true`) | Single set of credentials shared across all projects | Company-wide Jira instance |
| **Project-scoped** (`projects: [...]`) | Credentials specific to certain repositories | Per-repo GitHub App tokens |

**When to use global integrations:**
- You have one Jira instance for all projects
- You want a fallback credential when no project-specific one exists

**When to use project-scoped integrations:**
- Different teams use different Jira projects
- You need separate GitHub tokens per repository
- Security requires credential isolation between projects

## Authentication Types

| Type | Provider | Use Case |
|------|----------|----------|
| `pat` | GitHub | Personal Access Token for API access |
| `oauth` | GitHub, Jira | OAuth flow for user-delegated access |
| `app` | GitHub | GitHub App installation token |
| `basic_auth` | Jira | Email + API token (recommended for Jira Cloud) |

## API Reference

### Create Integration

Register a new integration with encrypted credentials.

```
POST /sfp/api/integrations
```

**Request Body:**

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `provider` | string | Yes | Service provider: `github` or `jira` |
| `authType` | string | Yes | Authentication type: `pat`, `oauth`, `app`, or `basic_auth` |
| `credentials` | object | Yes | Provider-specific credentials (see examples below) |
| `isGlobal` | boolean | No | Set `true` for global integration |
| `projects` | string[] | No | Project identifiers (required if not global) |
| `config` | object | No | Provider-specific configuration |

**Example: Global Jira Integration**

```bash
curl -X POST https://your-server/sfp/api/integrations \
  -H "Authorization: Bearer $TOKEN" \
  -H "Content-Type: application/json" \
  -d '{
    "provider": "jira",
    "authType": "basic_auth",
    "credentials": {
      "base_url": "https://your-company.atlassian.net",
      "username": "your-email@company.com",
      "api_token": "your-jira-api-token"
    },
    "isGlobal": true
  }'
```

**Example: Project-Scoped GitHub Integration**

```bash
curl -X POST https://your-server/sfp/api/integrations \
  -H "Authorization: Bearer $TOKEN" \
  -H "Content-Type: application/json" \
  -d '{
    "provider": "github",
    "authType": "pat",
    "credentials": {
      "token": "ghp_xxxxxxxxxxxx"
    },
    "projects": ["your-org/sf-core", "your-org/sf-sales"]
  }'
```

### Get Credentials

Retrieve stored credentials for use in automation. All access is audited.

```
GET /sfp/api/integrations/credentials?provider={provider}&project={project}
```

**Query Parameters:**

| Parameter | Required | Description |
|-----------|----------|-------------|
| `provider` | No | Filter by provider (`github`, `jira`) |
| `project` | No | Filter by project identifier |

**Example:**

```bash
# Get Jira credentials for a specific project
curl "https://your-server/sfp/api/integrations/credentials?provider=jira&project=your-org/sf-core" \
  -H "Authorization: Bearer $TOKEN"
```

**Response:**

```json
[
  {
    "integration_id": "uuid-here",
    "provider": "jira",
    "auth_type": "basic_auth",
    "credentials": {
      "base_url": "https://your-company.atlassian.net",
      "username": "your-email@company.com",
      "api_token": "your-jira-api-token"
    }
  }
]
```

## Complete Workflow Example

Here's a typical setup workflow for a new team:

### 1. Register Your Project

```bash
sfp server project register \
  --identifier "acme-corp/salesforce-main" \
  --remote-url "https://github.com/acme-corp/salesforce-main"
```

### 2. Create a Global Jira Integration

Since most teams share a single Jira instance:

```bash
curl -X POST https://your-server/sfp/api/integrations \
  -H "Authorization: Bearer $TOKEN" \
  -H "Content-Type: application/json" \
  -d '{
    "provider": "jira",
    "authType": "basic_auth",
    "credentials": {
      "base_url": "https://acme.atlassian.net",
      "username": "sfp-service@acme.com",
      "api_token": "ATATT3xFfGF0..."
    },
    "isGlobal": true
  }'
```

### 3. Create Project-Scoped GitHub Integration

For repository-specific access:

```bash
curl -X POST https://your-server/sfp/api/integrations \
  -H "Authorization: Bearer $TOKEN" \
  -H "Content-Type: application/json" \
  -d '{
    "provider": "github",
    "authType": "pat",
    "credentials": {
      "token": "ghp_xxxx"
    },
    "projects": ["acme-corp/salesforce-main"]
  }'
```

### 4. Use in Your Pipeline

Once configured, sfp commands automatically use the stored credentials:

```bash
# Work items are fetched using stored Jira credentials
sfp work-items list --project "acme-corp/salesforce-main"
```

## Security Considerations

- **Encryption**: All credentials are encrypted at rest using AES-256 via Supabase pgcrypto
- **Audit Logging**: Every credential access is logged with actor information
- **Least Privilege**: Use project-scoped integrations when possible to limit blast radius
- **Token Rotation**: Regularly rotate API tokens and update integrations accordingly

## Troubleshooting

### "Either isGlobal or projects must be specified"

You must specify either `isGlobal: true` OR provide a `projects` array. You cannot omit both.

### "Project 'xyz' not found"

The project identifier must match an existing registered project. List projects first:

```bash
sfp server project list
```

### "No integration found for project=xyz"

Either no integration exists for that project, or the integration is not scoped to include it. Check if a global integration exists or create a project-scoped one.
