# Status

## `sfp server status`

Check the status of a tenant's services

### `sfp server status`

Display the current operational status of all services for a specified tenant.

```
USAGE
  $ sfp server status -t <value> [--json] [--passphrase <value>
    [--identity-file <value> --ssh-connection <value>]] [-g <value>...]
    [--loglevel
    trace|debug|info|warn|error|fatal|TRACE|DEBUG|INFO|WARN|ERROR|FATAL]

FLAGS
  -t, --tenant=<value>              (required) Name of the tenant to check
  --json                            Output status in JSON format
  
  SSH OPTIONS
  --ssh-connection=<value>          SSH connection string in the format user@host[:port]
  --identity-file=<value>           Path to SSH private key file
  --passphrase=<value>              Passphrase for the SSH private key if required
  
  OTHER OPTIONS
  -g, --logsgroupsymbol=<value>...  Symbol used by CICD platform to group/collapse logs
  --loglevel=<option>               [default: info] logging level for this command invocation
                                    <options: trace|debug|info|warn|error|fatal|TRACE|DEBUG|INFO|WARN|ERROR|FATAL>

DESCRIPTION
  Check the status of a tenant's services

  This command provides information about:
  - Service health status (running/stopped/error)
  - Container status
  - Resource usage
  - Uptime information

EXAMPLES
  $ sfp server status --tenant my-app

  $ sfp server status --tenant my-app --json

  $ sfp server status --tenant my-app --ssh-connection user@example.com --identity-file ~/.ssh/id_rsa
```

### Output Information

The status command provides comprehensive information about:

- **API Server**: Main application server status
- **Workers**: Status of critical, normal, and batch workers
- **Supporting Services**: Redis and Caddy status
- **Resource Usage**: Memory and CPU utilization
- **Uptime**: How long services have been running

### Output Formats

#### Standard Output
```
Tenant: my-app
Status: Running

Services:
  ✓ API Server         Running  (Uptime: 2d 14h 32m)
  ✓ Critical Workers   Running  (3 instances)
  ✓ Normal Workers     Running  (2 instances)
  ✓ Batch Workers      Running  (1 instance)
  ✓ Redis              Running  
  ✓ Caddy              Running  

Resource Usage:
  CPU:    23%
  Memory: 1.2GB / 4GB

Last Health Check: 2024-01-15T10:30:00Z
```

#### JSON Output
```json
{
  "tenant": "my-app",
  "status": "running",
  "services": {
    "api": {
      "status": "running",
      "uptime": "2d 14h 32m",
      "health": "healthy"
    },
    "critical-worker": {
      "status": "running",
      "instances": 3,
      "health": "healthy"
    },
    "normal-worker": {
      "status": "running",
      "instances": 2,
      "health": "healthy"
    },
    "batch-worker": {
      "status": "running",
      "instances": 1,
      "health": "healthy"
    },
    "redis": {
      "status": "running",
      "health": "healthy"
    },
    "caddy": {
      "status": "running",
      "health": "healthy"
    }
  },
  "resources": {
    "cpu": "23%",
    "memory": {
      "used": "1.2GB",
      "total": "4GB",
      "percentage": "30%"
    }
  },
  "lastHealthCheck": "2024-01-15T10:30:00Z"
}
```

### Remote Server Management

Check status of a tenant on a remote server:
```bash
sfp server status --tenant my-app \
  --ssh-connection admin@production.example.com \
  --identity-file ~/.ssh/production_key
```

### Use Cases

#### Monitoring Script
```bash
#!/bin/bash
STATUS=$(sfp server status --tenant my-app --json | jq -r '.status')

if [ "$STATUS" != "running" ]; then
  echo "Alert: Server is not running!"
  # Send notification
fi
```

#### Health Check Integration
```bash
# For use with monitoring systems
sfp server status --tenant my-app --json | \
  jq '{status: .status, healthy: (.services | to_entries | all(.value.health == "healthy"))}'
```

### Status Values

- **running**: All services are operational
- **partial**: Some services are running, others are stopped
- **stopped**: All services are stopped
- **error**: One or more services have encountered errors
- **unknown**: Unable to determine status

> **Note**: Use the JSON output format for integration with monitoring and automation systems.

> **Tip**: Combine with `sfp server health` for more detailed health information.