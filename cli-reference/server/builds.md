# Builds

## `sfp server builds`

View and manage build information from the SFP server

### Commands

* [`sfp server builds list`](#sfp-server-builds-list) - List recent builds sorted by commit time

---

### `sfp server builds list`

List recent builds from the SFP server sorted by commit time.

```
USAGE
  $ sfp server builds list [--json] [--repository <value>] [-e <value> | -t
    <value>] [--sfp-server-url <value>] [-g <value>...] [--loglevel
    trace|debug|info|warn|error|fatal|TRACE|DEBUG|INFO|WARN|ERROR|FATAL]
    [--limit <value>] [-d <value>] [-s InProgress|Completed|Failed] [--days
    <value>]

FLAGS
  --repository=<value>              Repository identifier (e.g., owner/repo)
  --limit=<value>                   [default: 20] Number of builds to display
  -d, --domain=<value>              Filter builds by domain
  -s, --status=<option>             Filter builds by status
                                    <options: InProgress|Completed|Failed>
  --days=<value>                    Show builds from the last N days
  
  AUTHENTICATION
  -e, --email=<value>               Email address for authenticated CLI user
  -t, --application-token=<value>   Application token for authentication (CI/CD)
  --sfp-server-url=<value>          URL of the SFP server
  
  OTHER OPTIONS
  --json                            Format output as json
  -g, --logsgroupsymbol=<value>...  Symbol used by CICD platform to group/collapse logs
  --loglevel=<option>               [default: info] logging level for this command invocation
                                    <options: trace|debug|info|warn|error|fatal|TRACE|DEBUG|INFO|WARN|ERROR|FATAL>

DESCRIPTION
  List recent builds from the SFP server sorted by commit time

  Displays build information including:
  - Build ID and number
  - Repository and branch
  - Commit SHA and message
  - Build status (InProgress, Completed, Failed)
  - Duration
  - Triggered by
  - Timestamp

EXAMPLES
  $ sfp server builds list --repository myorg/myrepo

  $ sfp server builds list --repository myorg/myrepo --limit 10

  $ sfp server builds list --repository myorg/myrepo --domain core

  $ sfp server builds list --repository myorg/myrepo --status Failed

  $ sfp server builds list --repository myorg/myrepo --days 7

  $ sfp server builds list --repository myorg/myrepo --status InProgress --json
```

### Output Formats

#### Standard Output

```
Recent Builds for myorg/myrepo:

#1234 | main | abc123def
  Status: Completed ✓
  Duration: 15m 32s
  Triggered: 2 hours ago by user@example.com
  Commit: "feat: Add new authentication module"
  Domain: core
  
#1233 | feature/update-deps | def456ghi
  Status: Failed ✗
  Duration: 8m 12s
  Triggered: 4 hours ago by GitHub Actions
  Commit: "chore: Update dependencies"
  Domain: ui
  Error: Package validation failed
  
#1232 | main | ghi789jkl
  Status: InProgress ⟳
  Duration: 3m 45s (running)
  Triggered: 5 minutes ago by Jenkins
  Commit: "fix: Resolve deployment issue"
  Domain: core
```

#### JSON Output

```json
{
  "repository": "myorg/myrepo",
  "builds": [
    {
      "id": "build_1234",
      "number": 1234,
      "branch": "main",
      "commit": {
        "sha": "abc123def456",
        "message": "feat: Add new authentication module",
        "author": "developer@example.com",
        "timestamp": "2024-01-15T14:30:00Z"
      },
      "status": "Completed",
      "domain": "core",
      "duration": 932,
      "startTime": "2024-01-15T14:30:00Z",
      "endTime": "2024-01-15T14:45:32Z",
      "triggeredBy": "user@example.com",
      "artifacts": [
        "core-package-1.2.3",
        "auth-module-2.0.0"
      ]
    },
    {
      "id": "build_1233",
      "number": 1233,
      "branch": "feature/update-deps",
      "commit": {
        "sha": "def456ghi789",
        "message": "chore: Update dependencies",
        "author": "developer@example.com",
        "timestamp": "2024-01-15T12:00:00Z"
      },
      "status": "Failed",
      "domain": "ui",
      "duration": 492,
      "startTime": "2024-01-15T12:00:00Z",
      "endTime": "2024-01-15T12:08:12Z",
      "triggeredBy": "GitHub Actions",
      "error": "Package validation failed"
    }
  ],
  "metadata": {
    "total": 45,
    "displayed": 20,
    "filters": {
      "repository": "myorg/myrepo",
      "status": null,
      "domain": null,
      "days": null
    }
  }
}
```

### Filtering Options

#### By Status

```bash
# Show only failed builds
sfp server builds list --repository myorg/myrepo --status Failed

# Show in-progress builds
sfp server builds list --repository myorg/myrepo --status InProgress

# Show completed builds
sfp server builds list --repository myorg/myrepo --status Completed
```

#### By Domain

```bash
# Show builds for specific domain
sfp server builds list --repository myorg/myrepo --domain core

# Combine with status filter
sfp server builds list --repository myorg/myrepo --domain ui --status Failed
```

#### By Time Period

```bash
# Last 7 days
sfp server builds list --repository myorg/myrepo --days 7

# Last 30 days
sfp server builds list --repository myorg/myrepo --days 30

# Today's builds
sfp server builds list --repository myorg/myrepo --days 1
```

### Use Cases

#### Build Monitoring Dashboard

```bash
#!/bin/bash
# build-monitor.sh

# Check for failed builds in the last day
FAILED_BUILDS=$(sfp server builds list \
  --repository myorg/myrepo \
  --status Failed \
  --days 1 \
  --json | jq '.builds | length')

if [ "$FAILED_BUILDS" -gt 0 ]; then
  echo "⚠️  $FAILED_BUILDS failed builds in the last 24 hours"
  sfp server builds list \
    --repository myorg/myrepo \
    --status Failed \
    --days 1
fi
```

#### CI/CD Integration

```yaml
# GitHub Actions - Check recent build status
- name: Check Recent Builds
  run: |
    # List recent builds
    sfp server builds list \
      --repository ${{ github.repository }} \
      --limit 5
    
    # Check for failures
    FAILURES=$(sfp server builds list \
      --repository ${{ github.repository }} \
      --status Failed \
      --days 1 \
      --json | jq '.builds | length')
    
    if [ "$FAILURES" -gt 0 ]; then
      echo "::warning::Recent build failures detected"
    fi
```

#### Build History Analysis

```bash
# Analyze build success rate
sfp server builds list --repository myorg/myrepo --days 30 --json | \
  jq '{
    total: .builds | length,
    completed: [.builds[] | select(.status == "Completed")] | length,
    failed: [.builds[] | select(.status == "Failed")] | length,
    success_rate: (([.builds[] | select(.status == "Completed")] | length) / (.builds | length) * 100 | tostring + "%")
  }'
```

### Build Information Details

Each build entry includes:

- **Build Identifier**: Unique build ID and sequential number
- **Repository Info**: Repository name and branch
- **Commit Details**: SHA, message, author, and timestamp
- **Status**: Current state (InProgress, Completed, Failed)
- **Domain**: Build domain/category
- **Duration**: Time taken or elapsed time for running builds
- **Trigger Source**: User, CI/CD system, or scheduled trigger
- **Artifacts**: List of generated artifacts (if completed)
- **Error Details**: Error message for failed builds

### Best Practices

1. **Regular Monitoring**: Check builds regularly for failures
2. **Use Filters**: Filter by domain or status to focus on relevant builds
3. **Automate Checks**: Integrate build checks into CI/CD pipelines
4. **Track Trends**: Monitor build success rates over time

> **Note**: Build history is retained based on server configuration. Older builds may be archived or removed.

> **Tip**: Use the `--json` flag with `jq` for advanced filtering and analysis of build data.