# Scale

## `sfp server scale`

Scale worker services for a tenant

### `sfp server scale`

Adjust the number of worker instances for different queue priorities to handle varying workloads.

```
USAGE
  $ sfp server scale -t <value> [--json] [--passphrase <value>
    [--identity-file <value> --ssh-connection <value>]] [--critical-workers
    <value>] [--normal-workers <value>] [--batch-workers <value>] [-g
    <value>...] [--loglevel
    trace|debug|info|warn|error|fatal|TRACE|DEBUG|INFO|WARN|ERROR|FATAL]

FLAGS
  -t, --tenant=<value>              (required) Name of the tenant
  --critical-workers=<value>        Number of critical queue workers (1-10)
  --normal-workers=<value>          Number of normal queue workers (1-10)
  --batch-workers=<value>           Number of batch queue workers (1-10)
  
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
  Scale worker services for a tenant

  Worker Types:
  - Critical Workers: Handle high-priority, time-sensitive tasks (auth, critical operations)
  - Normal Workers: Process standard operations and deployments
  - Batch Workers: Execute background jobs, scheduled tasks, and non-urgent operations

  Each worker type can be scaled independently from 1 to 10 instances.

EXAMPLES
  $ sfp server scale --tenant my-app --critical-workers 2

  $ sfp server scale --tenant my-app --normal-workers 3

  $ sfp server scale --tenant my-app --batch-workers 2

  $ sfp server scale --tenant my-app --critical-workers 2 --normal-workers 3 --batch-workers 2

  $ sfp server scale --tenant my-app --critical-workers 2 --ssh-connection user@example.com --identity-file ~/.ssh/id_rsa
```

### Worker Types

#### Critical Workers
Handle high-priority, time-sensitive operations:
- Authentication requests
- Security operations
- System-critical tasks
- Real-time notifications

```bash
sfp server scale --tenant my-app --critical-workers 3
```

#### Normal Workers
Process standard operations:
- Package deployments
- Build operations
- Standard API requests
- Regular task processing

```bash
sfp server scale --tenant my-app --normal-workers 5
```

#### Batch Workers
Execute background and scheduled tasks:
- Report generation
- Data cleanup
- Scheduled maintenance
- Long-running operations

```bash
sfp server scale --tenant my-app --batch-workers 2
```

### Scaling Strategies

#### Scale Up for High Load
Increase workers during peak times:
```bash
# Scale up all worker types
sfp server scale --tenant production \
  --critical-workers 5 \
  --normal-workers 8 \
  --batch-workers 4
```

#### Scale Down for Maintenance
Reduce workers for maintenance:
```bash
# Minimal configuration
sfp server scale --tenant production \
  --critical-workers 1 \
  --normal-workers 1 \
  --batch-workers 0
```

#### Gradual Scaling
Scale workers gradually to test capacity:
```bash
# Step 1: Increase normal workers
sfp server scale --tenant my-app --normal-workers 3

# Step 2: Monitor performance
sfp server status --tenant my-app

# Step 3: Adjust if needed
sfp server scale --tenant my-app --normal-workers 5
```

### Output Formats

#### Standard Output
```
Scaling workers for tenant: my-app

Current Configuration:
  Critical Workers: 1 → 3
  Normal Workers:   2 → 5
  Batch Workers:    1 → 2

✓ Scaled critical workers to 3
✓ Scaled normal workers to 5
✓ Scaled batch workers to 2

New Configuration:
  Critical Workers: 3 (running)
  Normal Workers:   5 (running)
  Batch Workers:    2 (running)

Scaling completed successfully
```

#### JSON Output
```json
{
  "tenant": "my-app",
  "status": "success",
  "previous": {
    "critical": 1,
    "normal": 2,
    "batch": 1
  },
  "current": {
    "critical": 3,
    "normal": 5,
    "batch": 2
  },
  "workers": {
    "critical": {
      "requested": 3,
      "running": 3,
      "status": "healthy"
    },
    "normal": {
      "requested": 5,
      "running": 5,
      "status": "healthy"
    },
    "batch": {
      "requested": 2,
      "running": 2,
      "status": "healthy"
    }
  },
  "timestamp": "2024-01-15T10:30:00Z"
}
```

### Remote Server Scaling

Scale workers on a remote server:
```bash
sfp server scale --tenant production \
  --critical-workers 3 \
  --ssh-connection admin@production.example.com \
  --identity-file ~/.ssh/production_key
```

### Scaling Recommendations

#### Based on Load Patterns

**Light Load** (< 100 operations/hour):
```bash
sfp server scale --tenant my-app \
  --critical-workers 1 \
  --normal-workers 1 \
  --batch-workers 1
```

**Medium Load** (100-1000 operations/hour):
```bash
sfp server scale --tenant my-app \
  --critical-workers 2 \
  --normal-workers 3 \
  --batch-workers 2
```

**Heavy Load** (> 1000 operations/hour):
```bash
sfp server scale --tenant my-app \
  --critical-workers 4 \
  --normal-workers 6 \
  --batch-workers 3
```

### Auto-Scaling Script

```bash
#!/bin/bash
# auto-scale.sh

TENANT="my-app"
THRESHOLD_HIGH=80
THRESHOLD_LOW=20

# Get current queue sizes
QUEUE_STATUS=$(sfp server status --tenant $TENANT --json)
NORMAL_QUEUE=$(echo $QUEUE_STATUS | jq '.queues.normal.pending')

if [ $NORMAL_QUEUE -gt $THRESHOLD_HIGH ]; then
  echo "High load detected, scaling up..."
  sfp server scale --tenant $TENANT --normal-workers 5
elif [ $NORMAL_QUEUE -lt $THRESHOLD_LOW ]; then
  echo "Low load detected, scaling down..."
  sfp server scale --tenant $TENANT --normal-workers 2
fi
```

### Performance Monitoring

Monitor worker performance after scaling:
```bash
# Check worker status
sfp server status --tenant my-app --json | jq '.workers'

# Monitor queue sizes
sfp server logs --tenant my-app --service normal-worker --tail 50

# Check resource usage
sfp server health --detailed
```

### Best Practices

1. **Start Conservative**: Begin with fewer workers and scale up as needed
2. **Monitor After Scaling**: Always check status after scaling operations
3. **Consider Queue Depth**: Scale based on queue backlog, not just time
4. **Balance Worker Types**: Ensure appropriate ratio between worker types
5. **Plan for Peaks**: Pre-scale before expected high-load periods

### Troubleshooting

#### Workers Not Starting
```bash
# Check logs for errors
sfp server logs --tenant my-app --service critical-worker --tail 100

# Verify resources
sfp server health --detailed
```

#### High Memory Usage
```bash
# Scale down workers
sfp server scale --tenant my-app \
  --critical-workers 1 \
  --normal-workers 2 \
  --batch-workers 1
```

> **Note**: Worker scaling changes take effect immediately but may take a few moments for all instances to be fully operational.

> **Warning**: Scaling down workers will gracefully shutdown excess instances, but any running tasks will be completed first.