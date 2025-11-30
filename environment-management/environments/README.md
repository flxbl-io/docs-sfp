# Environments

{% hint style="info" %}
This feature requires **sfp-pro** with sfp-server
{% endhint %}

Environments are the central concept in sfp-server's credential management. An environment links a repository branch to a Salesforce org, providing controlled access to credentials for team members and CI/CD pipelines.

## What is an Environment?

An environment represents a deployment target - the combination of:

* **Repository**: Which codebase (e.g., `myorg/salesforce-app`)
* **Branch**: Which code branch (e.g., `main`, `develop`)
* **Salesforce Org**: Which org to deploy to
* **Category**: The environment type (dev, test, release)

```
Environment: "UAT"
├── Repository: myorg/salesforce-app
├── Branch: release/v2.0
├── Salesforce Org: admin@uat.sandbox.com
├── Category: test
└── Metadata: { "region": "US", "owner": "qa-team" }
```

## Environment Categories

| Category | Purpose | Typical Use |
|----------|---------|-------------|
| `dev` | Development | Feature development, local testing |
| `test` | Testing | UAT, SIT, QA environments |
| `snapshot` | Snapshots | Point-in-time environment copies |
| `release` | Production | Production and staging releases |

## Complete Setup: From Org to Environment

Before creating environments, you need to register your Salesforce orgs with sfp-server. Here's the complete flow:

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                    Environment Setup Flow                                    │
├─────────────────────────────────────────────────────────────────────────────┤
│                                                                              │
│   STEP 1: Register Salesforce Orgs                                           │
│   ────────────────────────────────                                           │
│                                                                              │
│   # Production org (requires local auth first)                               │
│   $ sf org login web --alias production                                      │
│   $ sfp server org register --targetorg production                           │
│                                                                              │
│   # DevHub (if using scratch orgs)                                           │
│   $ sf org login web --alias devhub                                          │
│   $ sfp server org register --targetorg devhub --is-devhub --is-default      │
│                                                                              │
│   # Sandboxes (option A: JIT - recommended)                                  │
│   $ sfp server org register-sandbox \                                        │
│       --sandbox-name uat \                                                   │
│       --production-username admin@production.com                             │
│                                                                              │
│   # Sandboxes (option B: direct registration)                                │
│   $ sf org login web --alias uat --instance-url https://test.salesforce.com  │
│   $ sfp server org register --targetorg uat                                  │
│                                                                              │
│   STEP 2: Create Environments                                                │
│   ───────────────────────────                                                │
│                                                                              │
│   $ sfp server environment create \                                          │
│       --repository myorg/salesforce-app \                                    │
│       --name UAT \                                                           │
│       --category test \                                                      │
│       --branch release/* \                                                   │
│       --targetusername admin@production--uat.sandbox.com                     │
│                                                                              │
│   STEP 3: Access Environments                                                │
│   ───────────────────────────                                                │
│                                                                              │
│   $ sfp server environment get \                                             │
│       --name UAT \                                                           │
│       --repository myorg/salesforce-app \                                    │
│       --auth-type accessToken \                                              │
│       --authenticate                                                         │
│                                                                              │
│   ✅ Ready to use: sfp install --targetorg UAT --artifactdir ./artifacts     │
│                                                                              │
└─────────────────────────────────────────────────────────────────────────────┘
```

### Step 1: Register Orgs

First, register your Salesforce orgs with sfp-server. See [Org Registration](org-registration.md) for complete details.

```bash
# List registered orgs
sfp server org list

# Verify registration
sfp server org test --username admin@production.com
```

### Step 2: Create Environments

Link registered orgs to repository branches.

## Creating Environments

### Basic Creation

```bash
sfp server environment create \
  --repository myorg/salesforce-app \
  --name UAT \
  --category test \
  --branch release/v2.0 \
  --description "User Acceptance Testing environment" \
  --targetusername admin@uat.sandbox.com
```

### Interactive Mode

If you don't specify `--targetusername`, sfp shows registered orgs for selection:

```bash
sfp server environment create \
  --repository myorg/salesforce-app \
  --name UAT \
  --category test \
  --branch release/v2.0 \
  --description "UAT environment"

# Interactive prompt:
# ? Select Salesforce org for this environment:
#   > admin@uat.sandbox.com (sandbox)
#     admin@sit.sandbox.com (sandbox)
#     admin@production.com (production)
```

### With Metadata and Tags

```bash
sfp server environment create \
  --repository myorg/salesforce-app \
  --name Production \
  --category release \
  --branch main \
  --description "Production environment" \
  --targetusername admin@production.com \
  --tags "critical,monitored" \
  --metadata '{"region": "US-WEST", "sla": "99.9%"}'
```

## Listing Environments

### List All

```bash
sfp server environment list --repository myorg/salesforce-app
```

Output:
```
┌─────────────┬──────────┬─────────────────┬────────────────────────────┬────────┐
│ Name        │ Category │ Branch          │ Salesforce Org             │ Active │
├─────────────┼──────────┼─────────────────┼────────────────────────────┼────────┤
│ Production  │ release  │ main            │ admin@production.com       │ Yes    │
│ Staging     │ release  │ release/*       │ admin@staging.sandbox.com  │ Yes    │
│ UAT         │ test     │ release/v2.0    │ admin@uat.sandbox.com      │ Yes    │
│ SIT         │ test     │ develop         │ admin@sit.sandbox.com      │ Yes    │
│ Dev         │ dev      │ feature/*       │ admin@dev.sandbox.com      │ Yes    │
└─────────────┴──────────┴─────────────────┴────────────────────────────┴────────┘
```

### Filter by Category

```bash
sfp server environment list --repository myorg/salesforce-app --category test
```

## Retrieving Environments

### Basic Retrieval

Get environment information without credentials:

```bash
sfp server environment get --name UAT --repository myorg/salesforce-app
```

### With Authentication

Retrieve the environment AND authenticate locally:

```bash
# Using auth-type for read-only access
sfp server environment get \
  --name UAT \
  --repository myorg/salesforce-app \
  --auth-type accessToken \
  --authenticate
```

This:
1. Fetches environment details from server
2. Retrieves Salesforce credentials
3. Authenticates locally with the alias matching the environment name

### Auth Type Selection

Choose between short-lived access tokens (preferred) or long-lived SFDX Auth URLs:

```bash
# Short-lived access token (default, more secure)
sfp server environment get \
  --name UAT \
  --repository myorg/salesforce-app \
  --authenticate \
  --auth-type accessToken

# Long-lived SFDX Auth URL (for extended operations)
sfp server environment get \
  --name UAT \
  --repository myorg/salesforce-app \
  --authenticate \
  --auth-type sfdxAuthUrl
```

| Auth Type | Lifetime | Best For |
|-----------|----------|----------|
| `accessToken` | ~2 hours | Short operations, better security |
| `sfdxAuthUrl` | Until revoked | Long-running pipelines, pools |

## Environment Properties

### Core Properties

| Property | Description |
|----------|-------------|
| `name` | Unique name within repository |
| `category` | dev, test, snapshot, release |
| `branch` | Git branch pattern |
| `description` | Human-readable description |
| `salesforceUsername` | Linked Salesforce org |
| `isActive` | Whether environment is active |
| `isDefault` | Default for its category |

### Extended Properties

| Property | Description |
|----------|-------------|
| `tags` | Searchable tags |
| `metadata` | Custom JSON metadata |
| `orchestrationOrder` | Deployment sequence |
| `devHubUsername` | Parent org (for sandboxes) |

### Lock Status

When an environment is locked:

```json
{
  "name": "UAT",
  "isLocked": true,
  "lockStatus": {
    "isLocked": true,
    "currentLock": {
      "lockedBy": "ci-pipeline",
      "lockReason": "Deployment in progress",
      "expiresAt": "2024-01-15T14:30:00Z",
      "expiresInSeconds": 1800
    },
    "queuedLocks": [
      {
        "position": 1,
        "requestedBy": "developer@company.com",
        "ticketId": "lock-abc123"
      }
    ]
  }
}
```

## Updating Environments

```bash
sfp server environment update \
  --name UAT \
  --repository myorg/salesforce-app \
  --description "Updated UAT for Q2 release" \
  --tags "critical,q2-release"
```

## Deleting Environments

```bash
sfp server environment delete \
  --name old-dev \
  --repository myorg/salesforce-app
```

## Credential Access Control

### Role-Based Access

| Role | Can View | Can Get Credentials | Can Modify |
|------|----------|---------------------|------------|
| Member | Yes | No | No |
| Owner | Yes | Yes | Yes |
| Application | Yes | Yes | Limited |

### Audit Trail

All credential access is logged:

```bash
# View access audit (owners only)
sfp server environment audit --name UAT --repository myorg/salesforce-app
```

## CI/CD Integration

### GitHub Actions

```yaml
jobs:
  deploy-to-uat:
    runs-on: ubuntu-latest
    env:
      SFP_SERVER_URL: ${{ secrets.SFP_SERVER_URL }}
      SFP_SERVER_TOKEN: ${{ secrets.SFP_SERVER_TOKEN }}
    steps:
      - uses: actions/checkout@v4

      - name: Authenticate to UAT
        run: |
          sfp server environment get \
            --name UAT \
            --repository ${{ github.repository }} \
            --auth-type accessToken \
            --authenticate

      - name: Deploy
        run: |
          sfp install --targetorg UAT --artifactdir ./artifacts
```

### Matrix Deployment

```yaml
jobs:
  deploy:
    strategy:
      matrix:
        environment: [SIT, UAT, Staging]
    runs-on: ubuntu-latest
    steps:
      - name: Authenticate
        run: |
          sfp server environment get \
            --name ${{ matrix.environment }} \
            --repository ${{ github.repository }} \
            --auth-type accessToken \
            --authenticate

      - name: Deploy to ${{ matrix.environment }}
        run: |
          sfp install --targetorg ${{ matrix.environment }} --artifactdir ./artifacts
```

## Environment Patterns

### Standard Pipeline

```
Repository: myorg/salesforce-app

Environments:
├── Dev (dev) ────────> feature branches
│
├── SIT (test) ────────> develop branch
│
├── UAT (test) ────────> release/* branches
│
├── Staging (release) ─> release/* branches (pre-prod)
│
└── Production (release) ─> main branch
```

### Multi-Region

```
Environments:
├── Production-US (release, tags: ["us", "primary"])
├── Production-EU (release, tags: ["eu", "gdpr"])
└── Production-APAC (release, tags: ["apac"])
```

### Feature Environments

```
Environments:
├── Feature-Auth (dev, branch: feature/auth-*)
├── Feature-Payments (dev, branch: feature/payments-*)
└── Feature-Reports (dev, branch: feature/reports-*)
```

## Related Topics

* [Server Authentication](../../authentication/server-authentication.md) - Authenticate with sfp-server
* [Org Registration](org-registration.md) - Register orgs for environments
* [Environment Locking](environment-locking.md) - Concurrent access control
* [Accessing Environments](accessing-environments.md) - Practical examples
* [JIT Sandbox Authentication](jit-sandbox.md) - On-demand sandbox credentials
