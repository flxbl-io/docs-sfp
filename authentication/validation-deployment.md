# Authentication for Validation & Deployment

This page explains how authentication works during sfp's validation and deployment operations. Understanding these flows helps you configure CI/CD pipelines correctly.

## Validation Authentication

Validation in sfp can use multiple orgs simultaneously:

```
┌─────────────────────────────────────────────────────────────────┐
│                    Validation Auth Flow                         │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│   sfp validate                                                  │
│        │                                                        │
│        ├──── DevHub ──────> Scratch org creation/fetching       │
│        │                                                        │
│        ├──── Target Org ──> Package deployment & testing        │
│        │                                                        │
│        └──── Pool ────────> Pre-created scratch orgs            │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

### Required Authentication

| Org | Purpose | Required? |
|-----|---------|-----------|
| **Target Org** | Deploy and test packages | Yes |
| **DevHub** | Create scratch orgs, version unlocked packages | When using scratch orgs or unlocked packages |
| **Pool** | Fetch pre-created scratch orgs | When using pools |

### Basic Validation

```bash
# Authenticate orgs first
sf org login web --alias targetOrg
sf org login web --alias devhub --set-default-dev-hub

# Run validation
sfp validate \
  --targetorg targetOrg \
  --devhubusername devhub
```

### Validation with Pools

```bash
# Authenticate DevHub (pool is on DevHub)
sf org login web --alias devhub --set-default-dev-hub

# Validate using pool - target org fetched automatically
sfp validate \
  --devhubusername devhub \
  --pools my-pool
```

### Validation Modes

| Mode | Target Org | Description |
|------|------------|-------------|
| **Individual** | Specified via `--targetorg` | Validate against a specific org |
| **Pool** | Fetched from pool | Auto-fetch scratch org for validation |
| **Org Pool** | From org pool | Use server-managed org pools (Pro) |

## Deployment Authentication

Deployment operations require authentication to the target org(s).

### Single Org Deployment

```bash
# Authenticate target
sf org login web --alias production

# Install artifacts
sfp install \
  --targetorg production \
  --artifactdir ./artifacts
```

### Release Deployment

Releases may deploy to multiple orgs sequentially:

```bash
# Authenticate all target orgs
sf org login web --alias uat
sf org login web --alias staging
sf org login web --alias production

# Release deploys based on release definition
sfp release \
  --targetorg production \
  --releasedefinition ./release-definition.yaml
```

### DevHub for Package Promotion

When promoting unlocked packages, DevHub authentication is required:

```bash
# Authenticate DevHub
sf org login web --alias devhub --set-default-dev-hub

# Install with package promotion
sfp install \
  --targetorg production \
  --devhubusername devhub \
  --artifactdir ./artifacts
```

## CI/CD Pipeline Authentication

### Environment Variables

sfp respects standard Salesforce CLI environment variables:

```bash
export SF_TARGET_ORG=myOrg           # Default target org
export SF_TARGET_DEV_HUB=myDevHub    # Default DevHub
```

### GitHub Actions Example

```yaml
name: Validate and Deploy

on:
  pull_request:
    branches: [main]

jobs:
  validate:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4

      - name: Install sfp
        run: npm install -g @flxbl-io/sfp

      - name: Authenticate DevHub
        run: |
          echo "${{ secrets.DEVHUB_AUTH_URL }}" | \
            sf org login sfdx-url --sfdx-url-stdin --alias devhub --set-default-dev-hub

      - name: Validate with Pool
        run: |
          sfp validate \
            --devhubusername devhub \
            --pools ci-pool \
            --coveragepercent 75

  deploy:
    needs: validate
    if: github.event_name == 'push'
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4

      - name: Install sfp
        run: npm install -g @flxbl-io/sfp

      - name: Authenticate Production
        run: |
          echo "${{ secrets.PRODUCTION_AUTH_URL }}" | \
            sf org login sfdx-url --sfdx-url-stdin --alias production

      - name: Authenticate DevHub
        run: |
          echo "${{ secrets.DEVHUB_AUTH_URL }}" | \
            sf org login sfdx-url --sfdx-url-stdin --alias devhub --set-default-dev-hub

      - name: Deploy
        run: |
          sfp install \
            --targetorg production \
            --devhubusername devhub \
            --artifactdir ./artifacts
```

### Azure DevOps Example

```yaml
trigger:
  - main

pool:
  vmImage: 'ubuntu-latest'

stages:
  - stage: Validate
    jobs:
      - job: ValidateChanges
        steps:
          - script: npm install -g @flxbl-io/sfp
            displayName: 'Install sfp'

          - script: |
              echo "$(DEVHUB_AUTH_URL)" | sf org login sfdx-url --sfdx-url-stdin --alias devhub --set-default-dev-hub
            displayName: 'Authenticate DevHub'

          - script: |
              sfp validate --devhubusername devhub --pools ci-pool
            displayName: 'Validate'

  - stage: Deploy
    dependsOn: Validate
    condition: and(succeeded(), eq(variables['Build.SourceBranch'], 'refs/heads/main'))
    jobs:
      - job: DeployToProduction
        steps:
          - script: |
              echo "$(PRODUCTION_AUTH_URL)" | sf org login sfdx-url --sfdx-url-stdin --alias production
              echo "$(DEVHUB_AUTH_URL)" | sf org login sfdx-url --sfdx-url-stdin --alias devhub --set-default-dev-hub
            displayName: 'Authenticate Orgs'

          - script: |
              sfp install --targetorg production --devhubusername devhub --artifactdir ./artifacts
            displayName: 'Deploy'
```

## Authentication Scoping

### What Each Operation Needs

| Operation | Target Org | DevHub | Pool |
|-----------|------------|--------|------|
| `sfp build` | - | Required for unlocked packages | - |
| `sfp validate` | Required (or pool) | Required for scratch/unlocked | Optional |
| `sfp install` | Required | Required for package promotion | - |
| `sfp release` | Required | Required for package promotion | - |
| `sfp deploy` | Required | - | - |

### Authentication Persistence

Authenticated orgs persist for the duration of the CI/CD job:

```yaml
# Authentication in step 1 is available in step 2
steps:
  - name: Authenticate
    run: sf org login sfdx-url --sfdx-url-stdin --alias myOrg

  - name: Use Authenticated Org
    run: sfp install --targetorg myOrg  # Works!
```

## Multi-Environment Deployments

### Sequential Deployment

```bash
# Authenticate all environments
for env in dev uat staging production; do
  echo "${!${env^^}_AUTH_URL}" | sf org login sfdx-url --sfdx-url-stdin --alias $env
done

# Deploy to each
for env in dev uat staging production; do
  sfp install --targetorg $env --artifactdir ./artifacts
done
```

### Parallel Deployment

With proper isolation, deploy to multiple orgs in parallel:

```yaml
# GitHub Actions matrix strategy
jobs:
  deploy:
    strategy:
      matrix:
        environment: [dev, uat, staging]
    runs-on: ubuntu-latest
    steps:
      - name: Authenticate
        run: |
          echo "${{ secrets[format('{0}_AUTH_URL', matrix.environment)] }}" | \
            sf org login sfdx-url --sfdx-url-stdin --alias targetOrg

      - name: Deploy
        run: sfp install --targetorg targetOrg --artifactdir ./artifacts
```

## Error Handling

### Authentication Failures

```bash
# Check if authentication succeeded
sf org login sfdx-url --sfdx-url-stdin --alias myOrg || {
  echo "Authentication failed"
  exit 1
}
```

### Org Availability

```bash
# Verify org is accessible before deployment
sf org display --target-org myOrg --json || {
  echo "Cannot access org"
  exit 1
}
```

## Security Best Practices

### Secret Management

* Store SFDX Auth URLs as secrets, not in code
* Use separate secrets for each environment
* Rotate credentials periodically

### Least Privilege

* Use service accounts with minimal permissions
* Don't share production credentials with dev pipelines
* Separate DevHub from deployment credentials

### Audit Trail

* Log which credentials are used for each deployment
* Track who has access to CI/CD secrets
* Review authentication patterns regularly

## Related Topics

* [Org Authentication](org-authentication.md) - Authentication methods
* [SFDX Auth URL](sfdx-auth-url.md) - Auth URL format
* [Environments](environments.md) - Centralized environment management (Pro)
* [Validating a Change](../validating-a-change/overview.md) - Validation concepts
