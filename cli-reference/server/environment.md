# Environment

## `sfp server environment`

Manage environments in the SFP server

### Commands

* [`sfp server environment create`](#sfp-server-environment-create) - Create a new environment
* [`sfp server environment list`](#sfp-server-environment-list) - List environments with filtering options
* [`sfp server environment get`](#sfp-server-environment-get) - Get detailed environment information
* [`sfp server environment lock`](#sfp-server-environment-lock) - Lock environment for exclusive access
* [`sfp server environment unlock`](#sfp-server-environment-unlock) - Unlock environment
* [`sfp server environment delete`](#sfp-server-environment-delete) - Delete an environment

---

### `sfp server environment create`

Create a new environment in the server.

```
USAGE
  $ sfp server environment create -n <value> [--json] [-t <value>] [-d <value>]
    [--sfdx-auth-url <value>] [--repository <value>] [-e <value> | -a <value>]
    [--sfp-server-url <value>] [-g <value>...] [--loglevel
    trace|debug|info|warn|error|fatal|TRACE|DEBUG|INFO|WARN|ERROR|FATAL]

FLAGS
  -n, --name=<value>                (required) Name of the environment
  -t, --type=<value>                Environment type (production, staging, dev, sandbox)
  -d, --description=<value>         Description of the environment
  --sfdx-auth-url=<value>           SFDX auth URL for the Salesforce org
  --repository=<value>              Repository identifier (e.g., owner/repo)
  
  AUTHENTICATION
  -e, --email=<value>               Email address for authenticated CLI user
  -a, --application-token=<value>   Application token for authentication
  --sfp-server-url=<value>          URL of the SFP server
  
  OTHER OPTIONS
  --json                            Format output as json
  -g, --logsgroupsymbol=<value>...  Symbol used by CICD platform to group/collapse logs
  --loglevel=<option>               [default: info] logging level
                                    <options: trace|debug|info|warn|error|fatal|TRACE|DEBUG|INFO|WARN|ERROR|FATAL>

DESCRIPTION
  Create a new environment in the server

  Environments represent Salesforce orgs (production, sandbox, scratch orgs)
  that can be targets for deployments and releases.

EXAMPLES
  $ sfp server environment create --name production --type production

  $ sfp server environment create --name staging --type staging --description "Staging environment for UAT"

  $ sfp server environment create --name dev-sandbox --type sandbox --sfdx-auth-url force://...

  $ sfp server environment create --name qa-env --type sandbox --repository myorg/myrepo
```

---

### `sfp server environment list`

List environments from the SFP server with optional filtering.

```
USAGE
  $ sfp server environment list [--json] [--repository <value>] [-e <value> | -t
    <value>] [--sfp-server-url <value>] [-g <value>...] [--loglevel
    trace|debug|info|warn|error|fatal|TRACE|DEBUG|INFO|WARN|ERROR|FATAL]
    [--type <value>] [--status available|locked] [--matrix]

FLAGS
  --repository=<value>              Repository identifier (e.g., owner/repo)
  --type=<value>                    Filter by environment type
  --status=<option>                 Filter by status
                                    <options: available|locked>
  --matrix                          Output as CI/CD matrix format
  
  AUTHENTICATION
  -e, --email=<value>               Email address for authenticated CLI user
  -t, --application-token=<value>   Application token for authentication
  --sfp-server-url=<value>          URL of the SFP server
  
  OTHER OPTIONS
  --json                            Format output as json
  -g, --logsgroupsymbol=<value>...  Symbol used by CICD platform to group/collapse logs
  --loglevel=<option>               [default: info] logging level
                                    <options: trace|debug|info|warn|error|fatal|TRACE|DEBUG|INFO|WARN|ERROR|FATAL>

DESCRIPTION
  List environments from the SFP server with optional filtering for CI/CD matrix jobs

EXAMPLES
  $ sfp server environment list

  $ sfp server environment list --type production

  $ sfp server environment list --status available

  $ sfp server environment list --matrix --type staging

  $ sfp server environment list --repository myorg/myrepo --json
```

#### Output Formats

**Standard Output:**
```
Environments:

production
  Type: production
  Status: Available
  Last Deployed: 2024-01-15T10:30:00Z
  Repository: myorg/myrepo

staging
  Type: staging
  Status: Locked (by user@example.com)
  Lock Expires: 2024-01-15T11:30:00Z
  Last Deployed: 2024-01-14T15:45:00Z
  Repository: myorg/myrepo

dev-sandbox-01
  Type: sandbox
  Status: Available
  Last Deployed: 2024-01-13T09:00:00Z
  Repository: myorg/myrepo
```

**Matrix Output (for CI/CD):**
```json
{
  "include": [
    {"environment": "staging", "type": "staging"},
    {"environment": "dev-sandbox-01", "type": "sandbox"}
  ]
}
```

---

### `sfp server environment get`

Get detailed information about a specific environment.

```
USAGE
  $ sfp server environment get -n <value> [--json] [--lock-with-ticket <value>]
    [--repository <value>] [-e <value> | -t <value>] [--sfp-server-url <value>]
    [-g <value>...] [--loglevel
    trace|debug|info|warn|error|fatal|TRACE|DEBUG|INFO|WARN|ERROR|FATAL]

FLAGS
  -n, --name=<value>                (required) Name of the environment
  --lock-with-ticket=<value>        Lock ticket to retrieve SFDX auth URL
  --repository=<value>              Repository identifier (e.g., owner/repo)
  
  AUTHENTICATION
  -e, --email=<value>               Email address for authenticated CLI user
  -t, --application-token=<value>   Application token for authentication
  --sfp-server-url=<value>          URL of the SFP server
  
  OTHER OPTIONS
  --json                            Format output as json
  -g, --logsgroupsymbol=<value>...  Symbol used by CICD platform to group/collapse logs
  --loglevel=<option>               [default: info] logging level
                                    <options: trace|debug|info|warn|error|fatal|TRACE|DEBUG|INFO|WARN|ERROR|FATAL>

DESCRIPTION
  Get detailed information about a specific environment, optionally retrieving SFDX auth URL with a lock ticket

EXAMPLES
  $ sfp server environment get --name production

  $ sfp server environment get --name staging --lock-with-ticket ticket_abc123

  $ sfp server environment get --name dev-sandbox --repository myorg/myrepo --json
```

---

### `sfp server environment lock`

Lock an environment for exclusive access.

```
USAGE
  $ sfp server environment lock -n <value> [--json] [--wait] [--timeout <value>]
    [--repository <value>] [-e <value> | -t <value>] [--sfp-server-url <value>]
    [-g <value>...] [--loglevel
    trace|debug|info|warn|error|fatal|TRACE|DEBUG|INFO|WARN|ERROR|FATAL]

FLAGS
  -n, --name=<value>                (required) Name of the environment to lock
  --wait                            Wait for environment to become available
  --timeout=<value>                 [default: 30] Timeout in minutes when waiting
  --repository=<value>              Repository identifier (e.g., owner/repo)
  
  AUTHENTICATION
  -e, --email=<value>               Email address for authenticated CLI user
  -t, --application-token=<value>   Application token for authentication
  --sfp-server-url=<value>          URL of the SFP server
  
  OTHER OPTIONS
  --json                            Format output as json
  -g, --logsgroupsymbol=<value>...  Symbol used by CICD platform to group/collapse logs
  --loglevel=<option>               [default: info] logging level
                                    <options: trace|debug|info|warn|error|fatal|TRACE|DEBUG|INFO|WARN|ERROR|FATAL>

DESCRIPTION
  Lock an environment for exclusive access with optional wait functionality

  Locking ensures exclusive access to an environment during deployments
  or maintenance operations. The lock includes a ticket ID that must be
  used to unlock the environment.

EXAMPLES
  $ sfp server environment lock --name production

  $ sfp server environment lock --name staging --wait --timeout 60

  $ sfp server environment lock --name dev-sandbox --repository myorg/myrepo
```

**Output:**
```
Environment locked successfully
Environment: production
Lock Ticket: ticket_xyz789
Expires: 2024-01-15T11:30:00Z
```

---

### `sfp server environment unlock`

Unlock an environment using the ticket ID.

```
USAGE
  $ sfp server environment unlock -n <value> -i <value> [--json] [--repository <value>]
    [-e <value> | -t <value>] [--sfp-server-url <value>] [-g <value>...]
    [--loglevel trace|debug|info|warn|error|fatal|TRACE|DEBUG|INFO|WARN|ERROR|FATAL]

FLAGS
  -n, --name=<value>                (required) Name of the environment to unlock
  -i, --ticket=<value>              (required) Lock ticket ID obtained from lock command
  --repository=<value>              Repository identifier (e.g., owner/repo)
  
  AUTHENTICATION
  -e, --email=<value>               Email address for authenticated CLI user
  -t, --application-token=<value>   Application token for authentication
  --sfp-server-url=<value>          URL of the SFP server
  
  OTHER OPTIONS
  --json                            Format output as json
  -g, --logsgroupsymbol=<value>...  Symbol used by CICD platform to group/collapse logs
  --loglevel=<option>               [default: info] logging level
                                    <options: trace|debug|info|warn|error|fatal|TRACE|DEBUG|INFO|WARN|ERROR|FATAL>

DESCRIPTION
  Unlock an environment using the ticket ID obtained from the lock command

EXAMPLES
  $ sfp server environment unlock --name production --ticket ticket_xyz789

  $ sfp server environment unlock --name staging --ticket ticket_abc123 --repository myorg/myrepo
```

---

### `sfp server environment delete`

Delete an environment from the server.

```
USAGE
  $ sfp server environment delete -n <value> [--json] [--force] [--repository <value>]
    [-e <value> | -t <value>] [--sfp-server-url <value>] [-g <value>...]
    [--loglevel trace|debug|info|warn|error|fatal|TRACE|DEBUG|INFO|WARN|ERROR|FATAL]

FLAGS
  -n, --name=<value>                (required) Name of the environment to delete
  --force                           Force deletion without confirmation
  --repository=<value>              Repository identifier (e.g., owner/repo)
  
  AUTHENTICATION
  -e, --email=<value>               Email address for authenticated CLI user
  -t, --application-token=<value>   Application token for authentication
  --sfp-server-url=<value>          URL of the SFP server
  
  OTHER OPTIONS
  --json                            Format output as json
  -g, --logsgroupsymbol=<value>...  Symbol used by CICD platform to group/collapse logs
  --loglevel=<option>               [default: info] logging level
                                    <options: trace|debug|info|warn|error|fatal|TRACE|DEBUG|INFO|WARN|ERROR|FATAL>

DESCRIPTION
  Delete an environment from the server

  This permanently removes the environment configuration from the server.
  The underlying Salesforce org is not affected.

EXAMPLES
  $ sfp server environment delete --name old-sandbox

  $ sfp server environment delete --name deprecated-env --force

  $ sfp server environment delete --name test-env --repository myorg/myrepo
```

### Use Cases

#### CI/CD Pipeline with Environment Locking

```yaml
# GitHub Actions example
jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - name: Lock Environment
        id: lock
        run: |
          LOCK_OUTPUT=$(sfp server environment lock --name staging --wait --json)
          echo "ticket=$(echo $LOCK_OUTPUT | jq -r '.ticket')" >> $GITHUB_OUTPUT
      
      - name: Deploy to Environment
        run: |
          # Deployment commands here
          sfp release --environment staging
      
      - name: Unlock Environment
        if: always()
        run: |
          sfp server environment unlock \
            --name staging \
            --ticket ${{ steps.lock.outputs.ticket }}
```

#### Environment Matrix for Parallel Testing

```yaml
jobs:
  get-environments:
    runs-on: ubuntu-latest
    outputs:
      matrix: ${{ steps.get-envs.outputs.matrix }}
    steps:
      - id: get-envs
        run: |
          MATRIX=$(sfp server environment list --type sandbox --status available --matrix)
          echo "matrix=$MATRIX" >> $GITHUB_OUTPUT
  
  test:
    needs: get-environments
    strategy:
      matrix: ${{ fromJson(needs.get-environments.outputs.matrix) }}
    runs-on: ubuntu-latest
    steps:
      - name: Run Tests
        run: |
          echo "Testing on ${{ matrix.environment }}"
```

### Best Practices

1. **Always Lock for Deployments**: Lock environments during deployments to prevent conflicts
2. **Use Wait with Timeout**: In CI/CD, use `--wait` with appropriate timeout values
3. **Always Unlock**: Ensure environments are unlocked in finally/always blocks
4. **Document Environment Types**: Maintain clear naming conventions for environment types
5. **Regular Cleanup**: Remove unused environments to keep the list manageable

> **Note**: Environment locks have automatic expiration to prevent indefinite locks from failed processes.

> **Warning**: Deleting an environment only removes it from the SFP server. The actual Salesforce org remains unchanged.