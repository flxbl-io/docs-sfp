# Org Authentication

This page covers how to authenticate Salesforce orgs for use with sfp commands. With sfp-pro, the recommended approach is using environments via sfp-server, but direct org authentication is also available.

## Recommended: Environment-based Authentication

The easiest way to authenticate orgs in sfp-pro:

```bash
# Authenticate with sfp-server first
sfp server auth login --provider github

# Get environment with authentication
# Option 1: Using auth-type (no lock required for read-only access)
sfp server environment get \
  --name UAT \
  --repository myorg/salesforce-app \
  --auth-type accessToken \
  --authenticate

# Option 2: With locking for exclusive access (CI/CD pipelines)
sfp server environment lock --name UAT --repository myorg/salesforce-app --reason "Deployment"
# Returns: ticket_id

sfp server environment get \
  --name UAT \
  --repository myorg/salesforce-app \
  --lock-ticket-id <ticket_id> \
  --authenticate

# Now use the org
sfp install --targetorg UAT --artifactdir ./artifacts

# Release the lock when done
sfp server environment unlock --name UAT --repository myorg/salesforce-app --lock-ticket-id <ticket_id>
```

See [Environments](environments.md) for full details.

## Direct Org Authentication

When you need to authenticate orgs directly without environments.

### From Registered Orgs (Server)

Access orgs registered with sfp-server:

```bash
# Login using a specific username
sfp server org login --username admin@production.com --alias production

# Login to the default DevHub
sfp server org login --default-devhub --alias devhub

# Set as default DevHub during login
sfp server org login --default-devhub --alias devhub --set-default-dev-hub

# List available orgs
sfp org list
```

### From SFDX Auth URL

Authenticate using an SFDX Auth URL file:

```bash
# From JSON file containing sfdxAuthUrl
sfp org login --url-file ./authfile.json --alias myOrg

# From plain text file
sfp org login --url-file ./authurl.txt --alias myOrg

# Set as default org
sfp org login --url-file ./authfile.json --alias myOrg --set-default

# Set as DevHub
sfp org login --url-file ./authfile.json --alias devhub --set-default-dev-hub
```

### From Access Token

For short-lived operations with an existing access token:

```bash
sfp org login \
  --access-token $ACCESS_TOKEN \
  --instance-url https://myorg.my.salesforce.com \
  --alias myOrg
```

{% hint style="info" %}
Access tokens expire (typically 2 hours). Use environment-based or SFDX Auth URL authentication for longer operations.
{% endhint %}

## Managing Authenticated Orgs

### List Orgs

```bash
# List local orgs
sfp org list

# Include server orgs (requires server authentication)
sfp org list --include-server-only

# Filter by type
sfp org list --type devhub
sfp org list --type sandbox
sfp org list --type scratch
```

### Open an Org

```bash
# Open in browser
sfp org open --targetorg myOrg

# Open a specific page
sfp org open --targetorg myOrg --path /lightning/setup/SetupOneHome/home
```

### Clean Up

```bash
# Remove inactive org authorizations
sfp org list --clean
```

## Using Authenticated Orgs

Once authenticated, reference orgs in sfp commands by alias:

### Validation

```bash
sfp validate \
  --targetorg myOrg \
  --devhubusername devhub \
  --pools scratch-pool
```

### Deployment

```bash
# Install artifacts
sfp install --targetorg myOrg --artifactdir ./artifacts

# Release
sfp release --targetorg myOrg --releasedefinition ./release.yaml
```

### Building

```bash
# Build packages (DevHub required for unlocked packages)
sfp build --devhubusername devhub --branch main
```

## Authentication for Different Org Types

### Production Orgs

```bash
# Via environment (recommended)
sfp server environment get --name Production --repository myorg/app --auth-type accessToken --authenticate

# Direct registration
sfp server org register --targetorg production
```

### Sandboxes

```bash
# Via JIT authentication (recommended)
sfp server org register-sandbox --sandbox-name uat --production-username admin@prod.com

# Access via environment
sfp server environment get --name UAT --repository myorg/app --auth-type accessToken --authenticate
```

### DevHub

```bash
# Register as DevHub
sfp server org register --targetorg devhub --is-devhub --is-default

# Access for pool operations
sfp server org login --default-devhub --alias devhub --set-default-dev-hub
```

### Scratch Orgs

Scratch orgs are typically fetched from pools:

```bash
# Fetch from pool
sfp pool fetch --tag dev --alias scratchOrg --set-default

# The org is automatically authenticated locally
sfp org open --targetorg scratchOrg
```

## CI/CD Integration

### With sfp-server (Recommended)

```yaml
jobs:
  deploy:
    runs-on: ubuntu-latest
    env:
      SFP_SERVER_URL: ${{ secrets.SFP_SERVER_URL }}
      SFP_SERVER_TOKEN: ${{ secrets.SFP_SERVER_TOKEN }}
    steps:
      - name: Authenticate Environment
        run: |
          sfp server environment get \
            --name Production \
            --repository ${{ github.repository }} \
            --auth-type accessToken \
            --authenticate

      - name: Deploy
        run: sfp install --targetorg Production --artifactdir ./artifacts
```

### With Locking (For Exclusive Access)

```yaml
jobs:
  deploy:
    runs-on: ubuntu-latest
    env:
      SFP_SERVER_URL: ${{ secrets.SFP_SERVER_URL }}
      SFP_SERVER_TOKEN: ${{ secrets.SFP_SERVER_TOKEN }}
    steps:
      - name: Lock Environment
        id: lock
        run: |
          RESULT=$(sfp server environment lock \
            --name Production \
            --repository ${{ github.repository }} \
            --reason "Release ${{ github.sha }}" \
            --json)
          echo "ticket_id=$(echo $RESULT | jq -r '.ticketId')" >> $GITHUB_OUTPUT

      - name: Authenticate Environment
        run: |
          sfp server environment get \
            --name Production \
            --repository ${{ github.repository }} \
            --lock-ticket-id ${{ steps.lock.outputs.ticket_id }} \
            --authenticate

      - name: Deploy
        run: sfp install --targetorg Production --artifactdir ./artifacts

      - name: Unlock Environment
        if: always()
        run: |
          sfp server environment unlock \
            --name Production \
            --repository ${{ github.repository }} \
            --lock-ticket-id ${{ steps.lock.outputs.ticket_id }}
```

### Without sfp-server

See [Community Edition Authentication](community-edition.md).

## Auth Type Selection

When retrieving orgs from the server, choose your auth type:

```bash
# Short-lived access token (default, more secure)
sfp server environment get --name UAT --repository myorg/app --auth-type accessToken --authenticate

# Long-lived SFDX Auth URL (for extended operations)
sfp server environment get --name UAT --repository myorg/app --auth-type sfdxAuthUrl --authenticate
```

| Auth Type | Lifetime | Best For |
|-----------|----------|----------|
| `accessToken` | ~2 hours | Short deployments, better security |
| `sfdxAuthUrl` | Until revoked | Long-running pipelines, pools |

## Troubleshooting

### "Authentication failed"

* Verify org credentials are still valid
* Check if user is active in the org
* Try re-registering the org with sfp-server

### "Org not found"

```bash
# Check local orgs
sfp org list

# Check server orgs
sfp server org list
```

### "Unable to get credentials"

* Verify you have owner or application role
* Check your server authentication: `sfp server auth display`

### "--authenticate requires --lock-ticket-id or --auth-type"

When using `--authenticate` with `sfp server environment get`, you must also specify either:
* `--auth-type accessToken` or `--auth-type sfdxAuthUrl` for read-only access
* `--lock-ticket-id <ticket>` for exclusive access with locking

## Related Topics

* [Environments](environments.md) - Environment-based authentication
* [Server Authentication](server-authentication.md) - Login to sfp-server
* [Org Registration](org-registration.md) - Register orgs with server
* [SFDX Auth URL](sfdx-auth-url.md) - Auth URL format
