---
icon: ring-diamond
---

# Initializing SFP server

|              | sfp-pro     | sfp (community) |
| ------------ | ----------- | --------------- |
| Availability | ✅           | ❌               |
| From         | December 24 |                 |

The `sfp server init` command is used to set up and configure an SFP server instance. This command creates the necessary directory structure, generates configuration files, and initializes the database.

#### Basic Usage

```bash
sfp server init --tenant my-app
```

This command will:

1. Create a directory structure for your tenant at `./sfp-server/tenants/my-app`
2. Generate Docker Compose and Caddy configuration files
3. Collect required secrets (prompting if needed)
4. Initialize the Supabase database with required schema
5. Start the server containers
6. Create a default admin user

#### Command Options

**Required Parameters**

* `--tenant`: Name of the tenant (required, must be lowercase alphanumeric with hyphens)

**Environment Configuration**

* `--mode`: Server mode, either 'dev' or 'prod' (default: 'prod')
* `--domain`: Domain name for the server (required in prod mode)

**Infrastructure Configuration**

* `--worker-counts`: Number of workers for critical,normal,batch queues (comma separated, default: 1,1,1)
* `--base-dir`: Base directory for the server (default: './sfp-server')

**Secrets Configuration**

* `--secrets-provider`: Type of secrets provider to use (options: 'custom', 'infisical')
* `--infisical-token`: Authentication token for Infisical
* `--infisical-workspace`: Workspace ID in Infisical

**Other Options**

* `--interactive`: Run in interactive mode to prompt for secrets (default: false)
* `--force`: Overwrite existing tenant configuration if it exists
* `--config-file`: Path to JSON config file containing server configuration

#### Example Usage

**Development Mode**

```bash
sfp server init --tenant my-app --mode dev --interactive
```

**Production Mode with Domain**

```bash
sfp server init --tenant my-app --mode prod --domain example.com --worker-counts 2,3,1
```

**Using Configuration File**

```bash
sfp server init --tenant my-app --config-file ./server-config.json
```

Example configuration file:

```json
{
  "domain": "example.com",
  "workerCounts": "2,3,1",
  "secrets": {
    "DOCKER_REGISTRY": "ghcr.io",
    "DOCKER_REGISTRY_TOKEN": "your-token",
    "SUPABASE_DB_URL": "postgresql://postgres:password@localhost:5432/postgres",
    "SUPABASE_URL": "https://your-project.supabase.co",
    "SUPABASE_SERVICE_KEY": "your-service-key",
    "SUPABASE_ANON_KEY": "your-anon-key",
    "SUPABASE_JWT_SECRET": "your-jwt-secret",
    "SUPABASE_ENCRYPTION_KEY": "your-encryption-key",
    "GITHUB_TOKEN": "your-github-token",
    "AUTH_USE_GLOBAL_AUTH": "false",
    "AUTH_SUPABASE_URL": "https://your-project.supabase.co",
    "AUTH_SUPABASE_ANON_KEY": "your-anon-key",
  }
}
```

### Post-Initialization

After successful initialization, the command outputs:

* Tenant URL and access details
* Available management commands
* Service status information

You can manage the initialized server using other `sfp server` commands:

```bash
sfp server logs <tenant>     # View server logs
sfp server status <tenant>   # Check server status
sfp server stop <tenant>     # Stop server
sfp server update <tenant>   # Update server configuration
```

> **Note**: For production deployments, ensure you have configured your domain DNS settings and have necessary SSL certificates before initialization.

> **Warning**: The `--force` flag will overwrite existing configurations. Use with caution in production environments.
