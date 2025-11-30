# Accessing Environments: A Practical Guide

{% hint style="info" %}
This feature requires **sfp-pro** with sfp-server
{% endhint %}

This guide walks through common scenarios for accessing environments with sfp-server, from local development to production deployments.

## Complete Flow: From Login to Deployment

Here's the complete end-to-end flow for a developer to access an environment and deploy:

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                    End-to-End Authentication Flow                            │
├─────────────────────────────────────────────────────────────────────────────┤
│                                                                              │
│   STEP 1: Authenticate to sfp-server (one-time until token expires)         │
│   ─────────────────────────────────────────────────────────────────         │
│                                                                              │
│   $ sfp server auth login --email developer@company.com --provider github    │
│                                                                              │
│   ┌────────────┐                                                             │
│   │  Browser   │  → GitHub OAuth → Authorize → Callback to CLI               │
│   └────────────┘                                                             │
│         │                                                                    │
│         ▼                                                                    │
│   ┌────────────┐                                                             │
│   │  Keychain  │  Token stored securely (valid ~24h)                         │
│   └────────────┘                                                             │
│                                                                              │
│   STEP 2: Get environment and authenticate locally                           │
│   ────────────────────────────────────────────────                           │
│                                                                              │
│   $ sfp server environment get \                                             │
│       --name UAT \                                                           │
│       --repository myorg/salesforce-app \                                    │
│       --auth-type accessToken \                                              │
│       --authenticate                                                         │
│                                                                              │
│   ┌────────────┐     ┌─────────────┐     ┌─────────────┐                    │
│   │  Keychain  │────>│ sfp-server  │────>│  Salesforce │                    │
│   │ (get token)│     │   API       │     │   Auth      │                    │
│   └────────────┘     └─────────────┘     └─────────────┘                    │
│         │                   │                   │                            │
│         │                   │  Decrypt &        │                            │
│         │                   │  return creds     │                            │
│         │<──────────────────┘                   │                            │
│         │                                       │                            │
│         └───────────────────────────────────────┘                            │
│                    Create local SF auth (alias: "UAT")                       │
│                                                                              │
│   STEP 3: Use sfp commands with the authenticated org                        │
│   ───────────────────────────────────────────────────                        │
│                                                                              │
│   $ sfp install --targetorg UAT --artifactdir ./artifacts                    │
│   $ sfp release --targetorg UAT --releasedefinition ./release.yaml           │
│                                                                              │
│   ✅ The org "UAT" is now available for all sfp and sf commands              │
│                                                                              │
└─────────────────────────────────────────────────────────────────────────────┘
```

### Quick Start Commands

```bash
# 1. Login to sfp-server (interactive, opens browser)
sfp server auth login --email your-email@company.com --provider github

# 2. Verify you're authenticated
sfp server auth display

# 3. List available environments
sfp server environment list --repository myorg/salesforce-app

# 4. Access an environment (this authenticates you to the Salesforce org)
sfp server environment get \
  --name UAT \
  --repository myorg/salesforce-app \
  --auth-type accessToken \
  --authenticate

# 5. Use the authenticated org
sfp install --targetorg UAT --artifactdir ./artifacts
```

## Scenario 1: Local Development

As a developer, you need to access a development environment for daily work.

### One-time Setup

```bash
# 1. Authenticate with sfp-server (one-time or when token expires)
sfp server auth login --provider github

# Verify authentication
sfp server auth display
```

### Daily Workflow

```bash
# Get your dev environment
sfp server environment get \
  --name Dev \
  --repository myorg/salesforce-app \
  --auth-type accessToken \
  --authenticate

# Start developing - org is now authenticated locally as "Dev"
sf project deploy start --source-dir force-app --target-org Dev

# Push changes
sf project deploy start --source-dir force-app --target-org Dev
```

### Switching Environments

```bash
# Switch to UAT for testing
sfp server environment get \
  --name UAT \
  --repository myorg/salesforce-app \
  --auth-type accessToken \
  --authenticate

# Deploy to UAT
sfp install --targetorg UAT --artifactdir ./artifacts
```

## Scenario 2: CI/CD Pipeline Deployment

Setting up a GitHub Actions workflow that deploys to multiple environments.

### Repository Secrets Setup

Configure these secrets in your repository:
* `SFP_SERVER_URL` - Your sfp-server URL
* `SFP_SERVER_TOKEN` - Application token for CI/CD

### Basic Deployment Workflow

```yaml
name: Deploy to UAT

on:
  push:
    branches: [develop]

jobs:
  deploy:
    runs-on: ubuntu-latest
    env:
      SFP_SERVER_URL: ${{ secrets.SFP_SERVER_URL }}
      SFP_SERVER_TOKEN: ${{ secrets.SFP_SERVER_TOKEN }}

    steps:
      - uses: actions/checkout@v4

      - name: Install sfp
        run: npm install -g @flxbl-io/sfp

      - name: Get UAT Environment
        run: |
          sfp server environment get \
            --name UAT \
            --repository ${{ github.repository }} \
            --auth-type accessToken \
            --authenticate

      - name: Deploy Artifacts
        run: |
          sfp install \
            --targetorg UAT \
            --artifactdir ./artifacts
```

### Production Deployment with Locking

```yaml
name: Production Release

on:
  release:
    types: [published]

jobs:
  deploy-production:
    runs-on: ubuntu-latest
    env:
      SFP_SERVER_URL: ${{ secrets.SFP_SERVER_URL }}
      SFP_SERVER_TOKEN: ${{ secrets.SFP_SERVER_TOKEN }}

    steps:
      - uses: actions/checkout@v4

      - name: Install sfp
        run: npm install -g @flxbl-io/sfp

      - name: Request Lock
        id: lock
        run: |
          RESULT=$(sfp server environment lock \
            --name Production \
            --repository ${{ github.repository }} \
            --reason "Release ${{ github.ref_name }}" \
            --lease-duration 60 \
            --json)
          echo "ticket_id=$(echo $RESULT | jq -r '.ticketId')" >> $GITHUB_OUTPUT

      - name: Acquire Lock and Authenticate
        run: |
          sfp server environment get \
            --name Production \
            --repository ${{ github.repository }} \
            --lock-ticket-id ${{ steps.lock.outputs.ticket_id }} \
            --authenticate

      - name: Deploy to Production
        run: |
          sfp release \
            --targetorg Production \
            --releasedefinition ./release-definition.yaml

      - name: Release Lock
        if: always()
        run: |
          sfp server environment unlock \
            --name Production \
            --repository ${{ github.repository }} \
            --lock-ticket-id ${{ steps.lock.outputs.ticket_id }}
```

## Scenario 3: Multi-Environment Pipeline

A complete pipeline that progresses through SIT → UAT → Staging → Production.

```yaml
name: Progressive Deployment

on:
  workflow_dispatch:
    inputs:
      environment:
        description: 'Target environment'
        required: true
        type: choice
        options:
          - SIT
          - UAT
          - Staging
          - Production

jobs:
  deploy:
    runs-on: ubuntu-latest
    environment: ${{ github.event.inputs.environment }}
    env:
      SFP_SERVER_URL: ${{ secrets.SFP_SERVER_URL }}
      SFP_SERVER_TOKEN: ${{ secrets.SFP_SERVER_TOKEN }}

    steps:
      - uses: actions/checkout@v4

      - name: Install sfp
        run: npm install -g @flxbl-io/sfp

      - name: Request Lock
        id: lock
        run: |
          RESULT=$(sfp server environment lock \
            --name ${{ github.event.inputs.environment }} \
            --repository ${{ github.repository }} \
            --reason "Deployment triggered by ${{ github.actor }}" \
            --json)
          echo "ticket_id=$(echo $RESULT | jq -r '.ticketId')" >> $GITHUB_OUTPUT

      - name: Authenticate
        run: |
          sfp server environment get \
            --name ${{ github.event.inputs.environment }} \
            --repository ${{ github.repository }} \
            --lock-ticket-id ${{ steps.lock.outputs.ticket_id }} \
            --authenticate

      - name: Deploy
        run: |
          sfp install \
            --targetorg ${{ github.event.inputs.environment }} \
            --artifactdir ./artifacts

      - name: Run Tests
        run: |
          sfp test run \
            --targetorg ${{ github.event.inputs.environment }}

      - name: Release Lock
        if: always()
        run: |
          sfp server environment unlock \
            --name ${{ github.event.inputs.environment }} \
            --repository ${{ github.repository }} \
            --lock-ticket-id ${{ steps.lock.outputs.ticket_id }}
```

## Scenario 4: Team Access Setup

Setting up environments for a team with different access levels.

### Initial Setup (Admin/Owner)

```bash
# 1. Authenticate as admin
sfp server auth login --admin --email admin@company.com

# 2. Register the production org
sf org login web --alias production
sfp server org register --targetorg production

# 3. Register DevHub
sf org login web --alias devhub
sfp server org register --targetorg devhub --is-devhub --is-default

# 4. Register sandboxes with JIT
sfp server org register-sandbox --sandbox-name uat --production-username admin@production.com
sfp server org register-sandbox --sandbox-name sit --production-username admin@production.com

# 5. Create environments
sfp server environment create \
  --repository myorg/salesforce-app \
  --name Production \
  --category release \
  --branch main \
  --description "Production environment" \
  --targetusername admin@production.com

sfp server environment create \
  --repository myorg/salesforce-app \
  --name UAT \
  --category test \
  --branch release/* \
  --description "User Acceptance Testing" \
  --targetusername admin@production--uat.sandbox.com

sfp server environment create \
  --repository myorg/salesforce-app \
  --name SIT \
  --category test \
  --branch develop \
  --description "System Integration Testing" \
  --targetusername admin@production--sit.sandbox.com
```

### Developer Access

```bash
# Developer authenticates with their GitHub account
sfp server auth login --provider github

# Developer can view environments
sfp server environment list --repository myorg/salesforce-app

# Developer accesses their assigned environment
sfp server environment get \
  --name SIT \
  --repository myorg/salesforce-app \
  --auth-type accessToken \
  --authenticate

# Developer can now work with the SIT environment
sf project deploy start --source-dir force-app --target-org SIT
```

### CI/CD Service Account

```bash
# Admin creates application token for CI/CD
sfp server token create --name ci-deployment --expiry 90d

# Output: sfp_at_xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
# Store this in CI/CD secrets as SFP_SERVER_TOKEN
```

## Scenario 5: Handling Sandbox Refreshes

When a sandbox is refreshed, JIT authentication handles it automatically.

### Before Refresh

```bash
# Verify current environment works
sfp server environment get \
  --name UAT \
  --repository myorg/salesforce-app \
  --auth-type accessToken \
  --authenticate

sfp org display --targetorg UAT
```

### After Refresh

```bash
# Same command works - JIT re-authenticates via production
sfp server environment get \
  --name UAT \
  --repository myorg/salesforce-app \
  --auth-type accessToken \
  --authenticate

# New sandbox is ready to use
sf org display --target-org UAT
```

No credential updates needed!

## Scenario 6: Debugging Access Issues

### Check Your Authentication

```bash
# View server auth status
sfp server auth display

# List available orgs
sfp server org list

# List environments
sfp server environment list --repository myorg/salesforce-app
```

### Verify Environment Details

```bash
# Get environment info (without authenticate to avoid credential issues)
sfp server environment get \
  --name UAT \
  --repository myorg/salesforce-app \
  --json
```

### Test Credential Access

```bash
# Test if you can get credentials (requires owner/app role)
sfp server environment get \
  --name UAT \
  --repository myorg/salesforce-app \
  --authenticate \
  --auth-type accessToken

# If this fails with permission error, you may need elevated access
```

### Common Issues and Solutions

**"Environment not found"**
```bash
# Check exact name and repository
sfp server environment list --repository myorg/salesforce-app
```

**"Unauthorized"**
```bash
# Re-authenticate
sfp server auth clear
sfp server auth login --provider github
```

**"Cannot retrieve credentials"**
```bash
# Check your role - only owners and applications can get credentials
# Contact your admin for elevated access
```

## Quick Reference

### Environment Access Commands

| Task | Command |
|------|---------|
| List environments | `sfp server environment list --repository REPO` |
| Get environment info | `sfp server environment get --name NAME --repository REPO` |
| Authenticate to env | `sfp server environment get --name NAME --repository REPO --authenticate` |
| Lock environment | `sfp server environment lock --name NAME --repository REPO --reason "..."` |
| Unlock environment | `sfp server environment unlock --name NAME --repository REPO --lock-ticket-id ID` |

### Common Flags

| Flag | Purpose |
|------|---------|
| `--authenticate` | Also authenticate the org locally |
| `--auth-type` | `accessToken` (short-lived) or `sfdxAuthUrl` (long-lived) |
| `--lock-ticket-id` | Use with locked environments |
| `--json` | Output as JSON for scripting |

## Related Topics

* [Environments](environments.md) - Environment concepts
* [Environment Locking](environment-locking.md) - Concurrency control
* [Server Authentication](server-authentication.md) - Auth with sfp-server
* [Org Registration](org-registration.md) - Register orgs
