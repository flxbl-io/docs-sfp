# Health

## `sfp server health`

Check the health and connectivity of the remote SFP server

### `sfp server health`

Verify the health status of the SFP server and its components.

```
USAGE
  $ sfp server health [--json] [-d] [--check-auth] [--repository <value>]
    [-e <value> | -t <value>] [--sfp-server-url <value>] [-g <value>...]
    [--loglevel
    trace|debug|info|warn|error|fatal|TRACE|DEBUG|INFO|WARN|ERROR|FATAL]

FLAGS
  -d, --detailed                    Show detailed health status including all components
  --check-auth                      Validate authentication token with the server
  --repository=<value>              Repository identifier (e.g., owner/repo)
  --sfp-server-url=<value>          URL of the SFP server
  
  AUTHENTICATION
  -e, --email=<value>               Email address for authenticated CLI user
  -t, --application-token=<value>   Application token for authentication (for CI/CD)
  
  OTHER OPTIONS
  --json                            Format output as json
  -g, --logsgroupsymbol=<value>...  Symbol used by CICD platform to group/collapse logs
  --loglevel=<option>               [default: info] logging level for this command invocation
                                    <options: trace|debug|info|warn|error|fatal|TRACE|DEBUG|INFO|WARN|ERROR|FATAL>

DESCRIPTION
  Check the health and connectivity of the remote SFP server

  This command verifies:
  - Server connectivity
  - API endpoint availability
  - Database connection
  - Redis connection
  - Worker queue status
  - Authentication validity (with --check-auth)

EXAMPLES
  $ sfp server health

  $ sfp server health --detailed

  $ sfp server health --check-auth

  $ sfp server health --check-auth --detailed

  $ sfp server health --json

  $ sfp server health --sfp-server-url https://sfp.example.com
```

### Health Check Levels

#### Basic Health Check
Quick connectivity and basic service check:
```bash
sfp server health
```

Output:
```
SFP Server Health Check
✓ Server is reachable
✓ API is responding
✓ Database connection is healthy
✓ Redis is operational
Status: HEALTHY
```

#### Detailed Health Check
Comprehensive status of all components:
```bash
sfp server health --detailed
```

Output:
```
SFP Server Health Check - Detailed

Core Services:
  ✓ API Server         Healthy  (Response time: 45ms)
  ✓ Database           Healthy  (Connections: 12/100)
  ✓ Redis              Healthy  (Memory: 256MB)
  
Worker Queues:
  ✓ Critical Queue     Active   (0 pending, 0 failed)
  ✓ Normal Queue       Active   (3 pending, 0 failed)
  ✓ Batch Queue        Active   (15 pending, 2 failed)

System Resources:
  CPU Usage:           23%
  Memory Usage:        2.1GB / 8GB
  Disk Usage:          45GB / 100GB
  
Version Information:
  Server Version:      v2.3.0
  API Version:         v1
  Database Version:    14.2
  
Status: HEALTHY
Last Check: 2024-01-15T10:30:00Z
```

### Authentication Validation

#### Check Authentication
Verify that your authentication credentials are valid:
```bash
sfp server health --check-auth
```

With email authentication:
```bash
sfp server health --check-auth --email user@example.com
```

With application token:
```bash
sfp server health --check-auth --application-token your-token
```

### Output Formats

#### JSON Output
For integration with monitoring systems:
```bash
sfp server health --detailed --json
```

```json
{
  "status": "healthy",
  "timestamp": "2024-01-15T10:30:00Z",
  "components": {
    "api": {
      "status": "healthy",
      "responseTime": 45
    },
    "database": {
      "status": "healthy",
      "connections": {
        "active": 12,
        "max": 100
      }
    },
    "redis": {
      "status": "healthy",
      "memory": "256MB"
    },
    "queues": {
      "critical": {
        "status": "active",
        "pending": 0,
        "failed": 0
      },
      "normal": {
        "status": "active",
        "pending": 3,
        "failed": 0
      },
      "batch": {
        "status": "active",
        "pending": 15,
        "failed": 2
      }
    }
  },
  "resources": {
    "cpu": "23%",
    "memory": {
      "used": "2.1GB",
      "total": "8GB",
      "percentage": "26%"
    },
    "disk": {
      "used": "45GB",
      "total": "100GB",
      "percentage": "45%"
    }
  },
  "version": {
    "server": "v2.3.0",
    "api": "v1",
    "database": "14.2"
  }
}
```

### Integration Examples

#### Monitoring Script
```bash
#!/bin/bash
# health-monitor.sh

HEALTH=$(sfp server health --json | jq -r '.status')

if [ "$HEALTH" != "healthy" ]; then
  echo "Server unhealthy, sending alert..."
  # Send alert to PagerDuty, Slack, etc.
  curl -X POST https://hooks.slack.com/services/YOUR/WEBHOOK/URL \
    -H 'Content-Type: application/json' \
    -d '{"text":"SFP Server is unhealthy!"}'
fi
```

#### CI/CD Pipeline Check
```yaml
# GitHub Actions example
- name: Check SFP Server Health
  run: |
    sfp server health --check-auth --detailed
  env:
    SFP_SERVER_URL: ${{ secrets.SFP_SERVER_URL }}
    SFP_APPLICATION_TOKEN: ${{ secrets.SFP_TOKEN }}
```

### Health Status Values

- **healthy**: All components operational
- **degraded**: Some non-critical components have issues
- **unhealthy**: Critical components are failing
- **unknown**: Unable to determine health status

### Configuration

Set default server URL via environment variable:
```bash
export SFP_SERVER_URL=https://sfp.example.com
```

Or via config:
```bash
sfp config:set server-url https://sfp.example.com
```

> **Note**: Regular health checks are recommended for production environments to ensure service availability.

> **Tip**: Use `--detailed` flag when troubleshooting to get comprehensive diagnostic information.