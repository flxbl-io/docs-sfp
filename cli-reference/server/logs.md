# Logs

## `sfp server logs`

View logs for a tenant's services

### `sfp server logs`

Display and monitor logs from various services running for a tenant.

```
USAGE
  $ sfp server logs -t <value> [--json] [-s
    app|critical-worker|normal-worker|batch-worker] [-f] [--tail <value>] [-g
    <value>...] [--loglevel
    trace|debug|info|warn|error|fatal|TRACE|DEBUG|INFO|WARN|ERROR|FATAL]
    [--passphrase <value> [--identity-file <value> --ssh-connection <value>]]

FLAGS
  -t, --tenant=<value>              (required) Name of the tenant
  -s, --service=<option>            Specific service to show logs for
                                    <options: app|critical-worker|normal-worker|batch-worker>
  -f, --follow                      Follow log output in real-time
  --tail=<value>                    [default: 100] Number of lines to show from the end of logs
  
  SSH OPTIONS
  --ssh-connection=<value>          SSH connection string in the format user@host[:port]
  --identity-file=<value>           Path to SSH private key file
  --passphrase=<value>              Passphrase for the SSH private key if required
  
  OTHER OPTIONS
  --json                            Format output as json
  -g, --logsgroupsymbol=<value>...  Symbol used by CICD platform to group/collapse logs
  --loglevel=<option>               [default: info] logging level for this command invocation
                                    <options: trace|debug|info|warn|error|fatal|TRACE|DEBUG|INFO|WARN|ERROR|FATAL>

DESCRIPTION
  View logs for a tenant's services

  This command allows you to:
  - View logs from all services or a specific service
  - Follow logs in real-time
  - Retrieve historical logs
  - Filter by service type

  Available services:
  - app: Main API server logs
  - critical-worker: Critical queue worker logs
  - normal-worker: Normal queue worker logs
  - batch-worker: Batch queue worker logs

EXAMPLES
  $ sfp server logs --tenant my-app

  $ sfp server logs --tenant my-app --service app

  $ sfp server logs --tenant my-app --service normal-worker --tail 200

  $ sfp server logs --tenant my-app --follow

  $ sfp server logs --tenant my-app --ssh-connection user@remote-host --identity-file ~/.ssh/id_rsa
```

### Viewing Logs

#### All Services
View recent logs from all services:
```bash
sfp server logs --tenant my-app
```

#### Specific Service
View logs from a specific service:
```bash
# API server logs
sfp server logs --tenant my-app --service app

# Critical worker logs
sfp server logs --tenant my-app --service critical-worker

# Normal worker logs
sfp server logs --tenant my-app --service normal-worker

# Batch worker logs
sfp server logs --tenant my-app --service batch-worker
```

#### Historical Logs
View more historical logs:
```bash
# Last 500 lines
sfp server logs --tenant my-app --tail 500

# Last 1000 lines from API server
sfp server logs --tenant my-app --service app --tail 1000
```

### Real-time Monitoring

#### Follow Mode
Monitor logs in real-time as they are generated:
```bash
sfp server logs --tenant my-app --follow
```

Follow specific service:
```bash
sfp server logs --tenant my-app --service app --follow
```

### Log Format Examples

#### Standard Output
```
[2024-01-15 10:30:15] [app] INFO: Server started on port 3000
[2024-01-15 10:30:16] [app] INFO: Connected to database
[2024-01-15 10:30:17] [critical-worker] INFO: Processing authentication request
[2024-01-15 10:30:18] [normal-worker] INFO: Starting deployment task
[2024-01-15 10:30:19] [batch-worker] INFO: Running scheduled cleanup
[2024-01-15 10:30:20] [app] ERROR: Failed to process request: Connection timeout
[2024-01-15 10:30:21] [app] INFO: Retrying connection...
```

#### JSON Output
```bash
sfp server logs --tenant my-app --json
```

```json
{
  "entries": [
    {
      "timestamp": "2024-01-15T10:30:15Z",
      "service": "app",
      "level": "INFO",
      "message": "Server started on port 3000"
    },
    {
      "timestamp": "2024-01-15T10:30:16Z",
      "service": "app",
      "level": "INFO",
      "message": "Connected to database"
    },
    {
      "timestamp": "2024-01-15T10:30:20Z",
      "service": "app",
      "level": "ERROR",
      "message": "Failed to process request: Connection timeout"
    }
  ],
  "metadata": {
    "tenant": "my-app",
    "lines": 100,
    "services": ["app", "critical-worker", "normal-worker", "batch-worker"]
  }
}
```

### Remote Server Logs

View logs from a remote server:
```bash
# View recent logs
sfp server logs --tenant my-app \
  --ssh-connection admin@production.example.com \
  --identity-file ~/.ssh/production_key

# Follow logs in real-time
sfp server logs --tenant my-app \
  --ssh-connection admin@production.example.com \
  --identity-file ~/.ssh/production_key \
  --follow
```

### Troubleshooting with Logs

#### Error Investigation
```bash
# Look for errors in the last 500 lines
sfp server logs --tenant my-app --tail 500 | grep ERROR

# Check specific service for issues
sfp server logs --tenant my-app --service app --tail 200 | grep -E "ERROR|WARN"
```

#### Performance Analysis
```bash
# Check worker queue processing
sfp server logs --tenant my-app --service normal-worker --tail 100

# Monitor batch job execution
sfp server logs --tenant my-app --service batch-worker --follow
```

### Log Management Scripts

#### Log Export
```bash
#!/bin/bash
# export-logs.sh

DATE=$(date +%Y%m%d_%H%M%S)
TENANT="my-app"

# Export all logs
sfp server logs --tenant $TENANT --tail 10000 > logs_${TENANT}_${DATE}.log

# Export by service
for SERVICE in app critical-worker normal-worker batch-worker; do
  sfp server logs --tenant $TENANT --service $SERVICE --tail 1000 > logs_${TENANT}_${SERVICE}_${DATE}.log
done
```

#### Error Monitoring
```bash
#!/bin/bash
# monitor-errors.sh

sfp server logs --tenant my-app --follow | while read line; do
  if echo "$line" | grep -q "ERROR"; then
    echo "Error detected: $line"
    # Send notification
    curl -X POST https://api.pagerduty.com/incidents \
      -H "Authorization: Token token=$PAGERDUTY_TOKEN" \
      -H "Content-Type: application/json" \
      -d "{\"incident\":{\"title\":\"SFP Error: $line\"}}"
  fi
done
```

### Best Practices

1. **Regular Log Review**: Check logs daily for errors and warnings
2. **Use Follow Mode for Debugging**: Monitor in real-time when troubleshooting
3. **Export Critical Logs**: Save logs before updates or maintenance
4. **Set Up Log Aggregation**: Forward logs to centralized logging systems

> **Note**: Logs are rotated automatically to prevent disk space issues. Older logs may not be available.

> **Tip**: Use `--follow` mode combined with grep for real-time filtering of specific log patterns.