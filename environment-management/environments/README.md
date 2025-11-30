---
icon: ring-diamond
---

# Environments

{% hint style="info" %}
This feature requires **sfp-pro** with sfp-server
{% endhint %}

Environments are the central concept in a Flxbl project. An environment links a repository branch to a registered Salesforce org, providing controlled access to credentials for team members and CI/CD pipelines.

## Orgs vs Environments

Understanding the distinction between **Orgs** and **Environments** is essential:

| Concept | Description | Scope |
|---------|-------------|-------|
| **Org** | A registered Salesforce org (production, sandbox, scratch org) with stored credentials | Global - shared across all repositories |
| **Environment** | A deployment target that links a repository + branch to a registered org | Repository-specific |

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                         Orgs vs Environments                                 │
├─────────────────────────────────────────────────────────────────────────────┤
│                                                                              │
│   REGISTERED ORGS (Global)           ENVIRONMENTS (Per Repository)          │
│   ────────────────────────           ─────────────────────────────          │
│                                                                              │
│   admin@production.com    ─────────► Production (myorg/app-1, main)         │
│                           └────────► Production (myorg/app-2, main)         │
│                                                                              │
│   admin@prod--uat.sandbox ─────────► UAT (myorg/app-1, release/*)           │
│                           └────────► QA (myorg/app-2, develop)              │
│                                                                              │
│   admin@devhub.com        ─────────► (DevHub for scratch org pools)         │
│                                                                              │
└─────────────────────────────────────────────────────────────────────────────┘
```

**Key differences:**

| Org Commands (`sfp server org`) | Environment Commands (`sfp server environment`) |
|--------------------------------|------------------------------------------------|
| Register Salesforce credentials | Link orgs to repository branches |
| Store/update auth details | Control access via locking |
| JIT sandbox registration | Provide deployment targets |
| Direct org access via `org login` | Credential access requires lock or `--auth-type` |

## What is an Environment?

An environment represents a deployment target - the combination of:

* **Repository**: Which codebase (e.g., `myorg/salesforce-app`)
* **Branch**: Which code branch (e.g., `main`, `develop`)
* **Salesforce Org**: Which org to install artifacts
* **Category**: The environment type (dev, test, release)

```
Environment: "UAT"
├── Repository: myorg/salesforce-app
├── Branch: release/v2.0
├── Salesforce Org: admin@uat.sandbox.com  (must be registered first)
├── Category: test
└── Metadata: { "region": "US", "owner": "qa-team" }
```

## Environment Categories

| Category   | Purpose     | Typical Use                        |
| ---------- | ----------- | ---------------------------------- |
| `dev`      | Development | Feature development, local testing |
| `test`     | Testing     | UAT, SIT, QA environments          |
| `snapshot` | Snapshots   | Point-in-time environment copies   |
| `release`  | Production  | Production and staging releases    |

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
│   $ sfp server org register --targetusername production                      │
│                                                                              │
│   # DevHub (if using scratch orgs)                                           │
│   $ sf org login web --alias devhub                                          │
│   $ sfp server org register --targetusername devhub --devhub --default       │
│                                                                              │
│   # Sandboxes (option A: JIT - recommended)                                  │
│   $ sfp server org register-sandbox \                                        │
│       --sandboxname uat \                                                    │
│       --productionusername admin@production.com                              │
│                                                                              │
│   # Sandboxes (option B: direct registration)                                │
│   $ sf org login web --alias uat --instance-url https://test.salesforce.com  │
│   $ sfp server org register --targetusername uat                             │
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

### Basic Retrieval (No Credentials)

Get environment information without credentials:

```bash
sfp server environment get --name UAT --repository myorg/salesforce-app
```

### Credential Access Methods

There are two ways to get credentials for an environment:

#### Method 1: Lock-Based Access (For Deployments)

{% hint style="success" %}
**Recommended for CI/CD and deployments.** Locking prevents concurrent deployments from conflicting with each other.
{% endhint %}

```bash
# Step 1: Request a lock
sfp server environment lock \
  --name UAT \
  --repository myorg/salesforce-app \
  --duration 30 \
  --reason "Deploying release v2.0"

# Output: Ticket ID: lock-abc123

# Step 2: Use the ticket to get credentials and authenticate
sfp server environment get \
  --name UAT \
  --repository myorg/salesforce-app \
  --lock-ticket-id lock-abc123 \
  --authenticate

# Step 3: After deployment, release the lock
sfp server environment unlock \
  --name UAT \
  --repository myorg/salesforce-app \
  --ticket-id lock-abc123
```

#### Method 2: Direct Access with `--auth-type` (No Locking)

{% hint style="warning" %}
**For testing and read-only operations only.** Do not use for deployments - without locking, concurrent operations may conflict.
{% endhint %}

```bash
sfp server environment get \
  --name UAT \
  --repository myorg/salesforce-app \
  --auth-type accessToken \
  --authenticate
```

This retrieves a short-lived access token (~2 hours) and authenticates locally.

#### When to Use Which Method

| Scenario | Method | Why |
|----------|--------|-----|
| CI/CD deployments | Lock-based | Prevents concurrent deployments |
| Running tests | `--auth-type` | No locking overhead needed |
| Quick data queries | `--auth-type` | Read-only, no conflict risk |
| Long-running operations | Lock with `--wait` | Ensures exclusive access |
| Parallel pipeline jobs | Lock-based | Queue management |

### Auth Type Selection

| Auth Type     | Lifetime      | Use Case                          |
| ------------- | ------------- | --------------------------------- |
| `accessToken` | ~2 hours      | Short operations, better security |
| `sfdxAuthUrl` | Until revoked | Scratch org pools, extended sessions |

## Environment Properties

### Core Properties

| Property             | Description                   |
| -------------------- | ----------------------------- |
| `name`               | Unique name within repository |
| `category`           | dev, test, snapshot, release  |
| `branch`             | Git branch pattern            |
| `description`        | Human-readable description    |
| `salesforceUsername` | Linked Salesforce org         |
| `isActive`           | Whether environment is active |
| `isDefault`          | Default for its category      |

### Extended Properties

| Property             | Description                |
| -------------------- | -------------------------- |
| `tags`               | Searchable tags            |
| `metadata`           | Custom JSON metadata       |
| `orchestrationOrder` | Deployment sequence        |
| `devHubUsername`     | Parent org (for sandboxes) |

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

| Role        | Can View | Can Get Credentials | Can Modify |
| ----------- | -------- | ------------------- | ---------- |
| Member      | Yes      | No                  | No         |
| Owner       | Yes      | Yes                 | Yes        |
| Application | Yes      | Yes                 | Limited    |

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
├── SIT (test) ────────> main
│
├── UAT (test) ────────> main
│
├── Staging (release) ─> main 
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

* [Server Authentication](../../auth-management/server-authentication.md) - Authenticate with sfp-server
* [Org Registration](org-registration.md) - Register orgs for environments
* [Environment Locking](environment-locking.md) - Concurrent access control
* [Accessing Environments](accessing-environments.md) - Practical examples
* [JIT Sandbox Authentication](jit-sandbox.md) - On-demand sandbox credentials
