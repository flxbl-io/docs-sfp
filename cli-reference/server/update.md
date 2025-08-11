# Update

## `sfp server update`

Update a tenant's services to the latest version

### `sfp server update`

Update the SFP server services for a specified tenant to the latest or a specific version.

```
USAGE
  $ sfp server update -t <value> [-j] [--skip-backup] [--base-dir <value>]
    [--supabase-working-dir <value>] [--passphrase <value> [--identity-file
    <value> --ssh-connection <value>]] [-r <value>] [--docker-tag <value>]
    [--restart] [--config-file <value>] [--infisical-token <value>
    --secrets-provider infisical|aws-secretsmanager|custom] [--aws-region
    <value>] [--aws-access-key-id <value>] [--aws-secret-access-key <value>]
    [-g <value>...] [--loglevel
    trace|debug|info|warn|error|fatal|TRACE|DEBUG|INFO|WARN|ERROR|FATAL]

FLAGS
  -t, --tenant=<value>              (required) Name of the tenant to update
  -j, --[no-]json                   Output in JSON format
  -r, --cadence=<value>             [default: latest] The default cadence to use for updates
  --docker-tag=<value>              Docker image tag to deploy (overrides cadence)
  --[no-]restart                    Restart services after update
  --skip-backup                     Skip backing up current configuration
  --base-dir=<value>                [default: ./sfp-server] Base Directory which contains the sfp-server
  --supabase-working-dir=<value>    Working directory which contains supabase configuration
  --config-file=<value>             Path to JSON config file containing server configuration values
  
  SSH OPTIONS
  --ssh-connection=<value>          SSH connection string in the format user@host[:port]
  --identity-file=<value>           Path to SSH private key file
  --passphrase=<value>              Passphrase for the SSH private key if required
  
  SECRETS MANAGEMENT
  --secrets-provider=<option>       [default: custom] Secret provider to use for managing secrets
                                    <options: infisical|aws-secretsmanager|custom>
  --infisical-token=<value>         Infisical API token (required when secrets-provider is "infisical")
  --aws-region=<value>              AWS region for Secrets Manager (required when secrets-provider is "aws-secretsmanager")
  --aws-access-key-id=<value>       AWS access key ID (optional, can use instance profile)
  --aws-secret-access-key=<value>   AWS secret access key (optional, can use instance profile)
  
  OTHER OPTIONS
  -g, --logsgroupsymbol=<value>...  Symbol used by CICD platform to group/collapse logs
  --loglevel=<option>               [default: info] logging level for this command invocation
                                    <options: trace|debug|info|warn|error|fatal|TRACE|DEBUG|INFO|WARN|ERROR|FATAL>

DESCRIPTION
  Update a tenant's services to the latest version

  This command performs:
  1. Backs up current configuration (unless --skip-backup is used)
  2. Pulls latest Docker images
  3. Updates configuration files
  4. Applies database migrations if needed
  5. Optionally restarts services

  Secrets Management:
  This command supports multiple options for secrets management:
  - infisical: Use Infisical as a dedicated secrets manager
  - aws-secretsmanager: Use AWS Secrets Manager
  - custom: Use environment variables (recommended when using tools like "infisical run" or AWS CLI)

EXAMPLES
  $ sfp server update --tenant my-tenant

  $ sfp server update --tenant my-tenant --skip-backup

  $ sfp server update --tenant my-tenant --docker-tag pr-123-dev

  $ sfp server update --tenant my-tenant --restart

  $ sfp server update --tenant my-tenant --ssh-connection user@remote-server --identity-file ~/.ssh/id_rsa

  $ infisical run -- sfp server update --tenant my-app --secrets-provider custom
```

### Update Strategies

#### Latest Version (Default)
Update to the latest stable release:
```bash
sfp server update --tenant my-tenant
```

#### Specific Version
Update to a specific Docker tag:
```bash
sfp server update --tenant my-tenant --docker-tag v2.3.1
```

#### Preview/Development Version
Update to a preview build:
```bash
sfp server update --tenant my-tenant --docker-tag pr-123-dev
```

### Update Options

#### With Automatic Restart
Update and restart services automatically:
```bash
sfp server update --tenant my-tenant --restart
```

#### Skip Backup
Update without creating a backup (use with caution):
```bash
sfp server update --tenant my-tenant --skip-backup
```

### Update Process

The update process follows these steps:

1. **Backup Creation** (unless skipped)
   - Configuration files
   - Environment variables
   - Docker compose files

2. **Image Updates**
   - Pull latest Docker images
   - Verify image integrity

3. **Configuration Updates**
   - Update docker-compose.yml
   - Update Caddy configuration
   - Apply environment changes

4. **Database Migrations**
   - Check for pending migrations
   - Apply migrations if needed

5. **Service Restart** (if requested)
   - Gracefully stop services
   - Start with new configuration

### Remote Server Updates

Update a tenant on a remote server:
```bash
sfp server update --tenant my-tenant \
  --ssh-connection admin@production.example.com \
  --identity-file ~/.ssh/production_key \
  --restart
```

### Rollback Procedure

If an update fails, restore from backup:
```bash
# Backups are stored in ./sfp-server/tenants/<tenant>/backups/
cd ./sfp-server/tenants/my-tenant/backups/

# Find the latest backup
ls -la

# Restore configuration
cp backup-2024-01-15/*.yml ../
cp backup-2024-01-15/.env ../

# Restart with previous configuration
sfp server start --tenant my-tenant --restart
```

### Best Practices

#### Production Updates
1. Always create a backup (default behavior)
2. Test updates in staging first
3. Schedule updates during maintenance windows
4. Monitor services after update

```bash
# Full production update workflow
sfp server update --tenant production
sfp server status --tenant production --json
sfp server health --detailed
```

#### Zero-Downtime Updates
For minimal disruption:
```bash
# Scale up workers before update
sfp server scale --tenant my-app --normal-workers 4

# Update without restart
sfp server update --tenant my-app

# Rolling restart of workers
sfp server scale --tenant my-app --normal-workers 2
```

### Output Format

#### Standard Output
```
Updating tenant: my-tenant
✓ Created backup at ./sfp-server/tenants/my-tenant/backups/backup-2024-01-15
✓ Pulled latest Docker images
✓ Updated configuration files
✓ Applied 2 database migrations
✓ Services restarted successfully
Update completed successfully
```

#### JSON Output
```json
{
  "tenant": "my-tenant",
  "status": "success",
  "backup": "./sfp-server/tenants/my-tenant/backups/backup-2024-01-15",
  "version": {
    "previous": "v2.2.0",
    "current": "v2.3.0"
  },
  "migrations": {
    "applied": 2,
    "pending": 0
  },
  "services": "restarted",
  "timestamp": "2024-01-15T10:30:00Z"
}
```

> **Warning**: Always ensure you have a recent backup before updating production servers.

> **Note**: The `--skip-backup` flag should only be used when disk space is limited or when you have external backup procedures.