# Stop

## `sfp server stop`

Stop a tenant's services

### `sfp server stop`

Gracefully shut down the services for a specified tenant.

```
USAGE
  $ sfp server stop -t <value> [--json] [--passphrase <value>
    [--identity-file <value> --ssh-connection <value>]] [-f] [-g <value>...]
    [--loglevel
    trace|debug|info|warn|error|fatal|TRACE|DEBUG|INFO|WARN|ERROR|FATAL]

FLAGS
  -t, --tenant=<value>              (required) Name of the tenant to stop
  -f, --force                       Force stop even if services are in use
  --json                            Format output as json
  
  SSH OPTIONS
  --ssh-connection=<value>          SSH connection string in the format user@host[:port]
  --identity-file=<value>           Path to SSH private key file
  --passphrase=<value>              Passphrase for the SSH private key if required
  
  OTHER OPTIONS
  -g, --logsgroupsymbol=<value>...  Symbol used by CICD platform to group/collapse logs
  --loglevel=<option>               [default: info] logging level for this command invocation
                                    <options: trace|debug|info|warn|error|fatal|TRACE|DEBUG|INFO|WARN|ERROR|FATAL>

DESCRIPTION
  Stop a tenant's services

  This command gracefully shuts down all services for the specified tenant including:
  - API server
  - Critical workers
  - Normal workers
  - Batch workers
  - Supporting services (Redis, Caddy)

EXAMPLES
  $ sfp server stop --tenant my-app

  $ sfp server stop --tenant my-app --force

  $ sfp server stop --tenant my-app --ssh-connection user@example.com --identity-file ~/.ssh/id_rsa

  $ sfp server stop --tenant my-app --json
```

### Stop Modes

#### Graceful Stop (Default)
Allows services to complete current operations before shutting down:
```bash
sfp server stop --tenant my-app
```

#### Force Stop
Immediately terminates all services without waiting for operations to complete:
```bash
sfp server stop --tenant my-app --force
```

### Remote Server Management

Stop a tenant on a remote server via SSH:
```bash
sfp server stop --tenant my-app \
  --ssh-connection admin@production.example.com \
  --identity-file ~/.ssh/production_key
```

### Best Practices

#### Maintenance Window Shutdown
For planned maintenance, scale down workers before stopping:
```bash
# Scale down workers first
sfp server scale --tenant my-app --batch-workers 0
sfp server scale --tenant my-app --normal-workers 0

# Then stop the server
sfp server stop --tenant my-app
```

#### Emergency Shutdown
When immediate shutdown is required:
```bash
sfp server stop --tenant my-app --force
```

### Output Format

#### Standard Output
```
Stopping services for tenant: my-app
✓ Stopped API server
✓ Stopped critical workers
✓ Stopped normal workers
✓ Stopped batch workers
✓ All services stopped successfully
```

#### JSON Output
```json
{
  "tenant": "my-app",
  "status": "stopped",
  "services": {
    "api": "stopped",
    "critical-worker": "stopped",
    "normal-worker": "stopped",
    "batch-worker": "stopped",
    "redis": "stopped",
    "caddy": "stopped"
  },
  "timestamp": "2024-01-15T10:30:00Z"
}
```

> **Warning**: The `--force` flag should be used with caution as it may interrupt running operations and could lead to data inconsistency.

> **Note**: Always ensure proper backup procedures are in place before stopping production servers.