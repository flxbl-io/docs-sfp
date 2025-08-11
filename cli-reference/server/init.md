# Init

## `sfp server init`

Initialize a new SFP server instance

### `sfp server init`

Initialize a new SFP server instance by creating the necessary directory structure, generating configuration files, and setting up the database.

```
USAGE
  $ sfp server init --tenant <value> [--mode dev|prod] [--domain <value>] 
    [--worker-counts <value>] [--base-dir <value>] [--secrets-provider 
    custom|infisical] [--infisical-token <value>] [--infisical-workspace 
    <value>] [--interactive] [--force] [--config-file <value>] [--loglevel
    trace|debug|info|warn|error|fatal|TRACE|DEBUG|INFO|WARN|ERROR|FATAL]

FLAGS
  --tenant=<value>                 (required) Name of the tenant (lowercase alphanumeric with hyphens)
  --mode=<option>                  [default: prod] Server mode
                                   <options: dev|prod>
  --domain=<value>                 Domain name for the server (required in prod mode)
  --worker-counts=<value>          [default: 1,1,1] Number of workers for critical,normal,batch queues (comma separated)
  --base-dir=<value>               [default: ./sfp-server] Base directory for the server
  --secrets-provider=<option>      Type of secrets provider to use
                                   <options: custom|infisical>
  --infisical-token=<value>        Authentication token for Infisical
  --infisical-workspace=<value>    Workspace ID in Infisical
  --interactive                    Run in interactive mode to prompt for secrets
  --force                          Overwrite existing tenant configuration if it exists
  --config-file=<value>            Path to JSON config file containing server configuration
  --loglevel=<option>              [default: info] logging level for this command invocation
                                   <options: trace|debug|info|warn|error|fatal|TRACE|DEBUG|INFO|WARN|ERROR|FATAL>

DESCRIPTION
  Initialize a new SFP server instance

  This command will:
  1. Create a directory structure for your tenant at ./sfp-server/tenants/<tenant-name>
  2. Generate Docker Compose and Caddy configuration files
  3. Collect required secrets (prompting if needed)
  4. Initialize the Supabase database with required schema
  5. Start the server containers
  6. Create a default admin user

EXAMPLES
  $ sfp server init --tenant my-app

  $ sfp server init --tenant my-app --mode dev --interactive

  $ sfp server init --tenant my-app --mode prod --domain example.com --worker-counts 2,3,1

  $ sfp server init --tenant my-app --config-file ./server-config.json

  $ sfp server init --tenant my-app --force
```

### Configuration File Format

When using `--config-file`, provide a JSON file with the following structure:

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
    "AUTH_SUPABASE_ANON_KEY": "your-anon-key"
  }
}
```

### Post-Initialization

After successful initialization, the command outputs:
* Tenant URL and access details
* Available management commands
* Service status information

You can then manage the server using other `sfp server` commands like `start`, `stop`, `status`, and `update`.

> **Note**: For production deployments, ensure you have configured your domain DNS settings and have necessary SSL certificates before initialization.

> **Warning**: The `--force` flag will overwrite existing configurations. Use with caution in production environments.