---
icon: ring-diamond
---

# Server Management Overview

|              | sfp-pro   | sfp (community) |
| ------------ | --------- | --------------- |
| Availability | ✅         | ❌               |
| From         | August 25 |                 |

The SFP server provides comprehensive CLI commands for managing all aspects of your server deployment. This page provides an overview of available commands organized by their primary use cases.

## Quick Start

The typical workflow for setting up and managing an SFP server:

1. **Initialize**: [`sfp server init`](../cli-reference/server/init.md) - Set up a new server instance
2. **Start**: [`sfp server start`](../cli-reference/server/start.md) - Launch the server services
3. **Monitor**: [`sfp server status`](../cli-reference/server/status.md) and [`sfp server health`](../cli-reference/server/health.md) - Check server health
4. **Manage**: Use various commands to manage environments, users, and resources
5. **Update**: [`sfp server update`](../cli-reference/server/update.md) - Keep the server up to date

## Command Categories

### &#x20;Lifecycle Management

Essential commands for managing the server lifecycle:

| Command                                       | Purpose                          | Common Usage                                  |
| --------------------------------------------- | -------------------------------- | --------------------------------------------- |
| [`init`](../cli-reference/server/init.md)     | Initialize a new server instance | `sfp server init --tenant my-app --mode prod` |
| [`start`](../cli-reference/server/start.md)   | Start server services            | `sfp server start --tenant my-app --daemon`   |
| [`stop`](../cli-reference/server/stop.md)     | Stop server services             | `sfp server stop --tenant my-app`             |
| [`status`](../cli-reference/server/status.md) | Check service status             | `sfp server status --tenant my-app`           |
| [`update`](../cli-reference/server/update.md) | Update to latest version         | `sfp server update --tenant my-app`           |

### 📊 Monitoring & Operations

Commands for monitoring and operational management:

| Command                                       | Purpose                       | Common Usage                                          |
| --------------------------------------------- | ----------------------------- | ----------------------------------------------------- |
| [`health`](../cli-reference/server/health.md) | Health checks and diagnostics | `sfp server health --detailed`                        |
| [`logs`](../cli-reference/server/logs.md)     | View and monitor logs         | `sfp server logs --tenant my-app --follow`            |
| [`scale`](../cli-reference/server/scale.md)   | Scale worker services         | `sfp server scale --tenant my-app --normal-workers 3` |

### Authentication & Security

Manage authentication and security:

| Command                                                             | Purpose             | Common Usage                                            |
| ------------------------------------------------------------------- | ------------------- | ------------------------------------------------------- |
| [`auth`](../cli-reference/server/auth.md)                           | User authentication | `sfp server auth login --email user@example.com`        |
| [`application-token`](../cli-reference/server/application-token.md) | Manage API tokens   | `sfp server application-token create --name "CI Token"` |
| [`user`](../cli-reference/server/user.md)                           | User management     | `sfp server user list`                                  |

### Environment Management

Manage deployment environments:

| Command                                                 | Purpose                | Common Usage                                      |
| ------------------------------------------------------- | ---------------------- | ------------------------------------------------- |
| [`environment`](../cli-reference/server/environment.md) | Manage environments    | `sfp server environment create --name production` |
| [`org`](../cli-reference/server/org.md)                 | Manage Salesforce orgs | `sfp server org list`                             |
| [`pool`](../cli-reference/server/pool.md)               | Manage org pools       | `sfp server pool list`                            |
| [`review-envs`](../cli-reference/server/review-envs.md) | Review environments    | `sfp server review-envs list`                     |

### Build & Release Management

Commands for managing builds and releases:

| Command                                                             | Purpose            | Common Usage                                         |
| ------------------------------------------------------------------- | ------------------ | ---------------------------------------------------- |
| [`builds`](../cli-reference/server/builds.md)                       | View build history | `sfp server builds list --repository myorg/myrepo`   |
| [`releasedefinition`](../cli-reference/server/releasedefinition.md) | Generate releases  | `sfp server releasedefinition generate -n MyRelease` |
| [`artifacts`](../cli-reference/server/artifacts.md)                 | Manage artifacts   | `sfp server artifacts list`                          |

### Configuration & Integration

Configuration and integration management:

| Command                                               | Purpose                | Common Usage                |
| ----------------------------------------------------- | ---------------------- | --------------------------- |
| [`repository`](../cli-reference/server/repository.md) | Repository connections | `sfp server repository add` |
| [`webhook`](../cli-reference/server/webhook.md)       | Webhook configuration  | `sfp server webhook create` |
| [`project`](../cli-reference/server/project.md)       | Project management     | `sfp server project list`   |

### Data Storage

Data and configuration storage:

| Command                                             | Purpose           | Common Usage               |
| --------------------------------------------------- | ----------------- | -------------------------- |
| [`doc-store`](../cli-reference/server/doc-store.md) | Document storage  | `sfp server doc-store get` |
| [`key-value`](../cli-reference/server/key-value.md) | Key-value storage | `sfp server key-value set` |

## Common Workflows

### Initial Setup

```bash
# 1. Initialize the server
sfp server init --tenant my-app --mode prod --domain example.com

# 2. Authenticate
sfp server auth login --email admin@example.com

# 3. Start the server
sfp server start --tenant my-app --daemon

# 4. Verify health
sfp server health --detailed
```

### Daily Operations

```bash
# Check server status
sfp server status --tenant my-app

# Monitor logs
sfp server logs --tenant my-app --service app --follow

# Check build status
sfp server builds list --repository myorg/myrepo --days 1

# View environment status
sfp server environment list --status available
```

### Deployment Workflow

```bash
# 1. Lock environment
TICKET=$(sfp server environment lock --name production --json | jq -r '.ticket')

# 2. Deploy (your deployment commands)
sfp release --environment production

# 3. Unlock environment
sfp server environment unlock --name production --ticket $TICKET
```

### Maintenance Operations

```bash
# Scale down for maintenance
sfp server scale --tenant my-app --normal-workers 1 --batch-workers 0

# Update server
sfp server update --tenant my-app

# Scale back up
sfp server scale --tenant my-app --normal-workers 3 --batch-workers 2
```

## Authentication Options

Most server commands support multiple authentication methods:

### User Authentication

```bash
# Login once
sfp server auth login --email user@example.com

# Commands use stored token
sfp server environment list
```

### Application Token (CI/CD)

```bash
# Set token in environment
export SFP_APPLICATION_TOKEN="sfp_app_..."

# Commands use token automatically
sfp server builds list --repository myorg/myrepo
```

## Remote Server Management

All lifecycle commands support managing remote servers via SSH:

```bash
# Remote server operations
sfp server start --tenant my-app \
  --ssh-connection admin@server.example.com \
  --identity-file ~/.ssh/id_rsa

sfp server logs --tenant my-app \
  --ssh-connection admin@server.example.com \
  --identity-file ~/.ssh/id_rsa \
  --follow
```

## Best Practices

1. **Lock Environments During Deployments**: Prevent concurrent deployments with environment locking
2. **Monitor Health Regularly**: Set up automated health checks using `sfp server health`
3. **Scale Appropriately**: Adjust worker counts based on your workload
4. **Keep Servers Updated**: Regularly update to get latest features and security fixes
5. **Use Application Tokens for CI/CD**: Create dedicated tokens for automated processes
6. **Review Logs Regularly**: Monitor logs for errors and performance issues

## Troubleshooting

### Server Won't Start

```bash
# Check status
sfp server status --tenant my-app

# View recent logs
sfp server logs --tenant my-app --tail 500

# Check health with details
sfp server health --detailed
```

### Authentication Issues

```bash
# Clear and re-authenticate
sfp server auth clear --all
sfp server auth login --email user@example.com

# Verify authentication
sfp server health --check-auth
```

### Performance Issues

```bash
# Check worker status
sfp server status --tenant my-app --json | jq '.workers'

# Adjust scaling
sfp server scale --tenant my-app --critical-workers 2 --normal-workers 4
```

## Next Steps

* **Detailed Command Reference**: See the [CLI Reference](../cli-reference/server/) for complete command documentation
* **Architecture Overview**: Learn about the [server architecture](sfp-pro-server-architecture-overview.md)
* **Installation Guide**: Follow the [installation guide](installing-sfp-server/) for detailed setup instructions
* **API Reference**: Explore the [API documentation](../api-reference/) for programmatic access

> **Note**: All server commands require sfp-pro. Community edition users should upgrade to access server functionality.

> **Tip**: Use `--json` flag with commands for scripting and automation purposes.
