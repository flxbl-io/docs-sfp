# Authentication for Community Edition

This guide covers authentication setup for sfp community edition, which doesn't include sfp-server. You'll manage credentials locally using standard Salesforce CLI authentication patterns.

{% hint style="info" %}
**Consider upgrading to sfp-pro** for centralized credential management, team access control, environment locking, and JIT sandbox authentication. [Learn more about sfp-pro](../getting-started/install-sfp/install-sfp-pro.md)
{% endhint %}

## How Community Edition Authentication Works

Without sfp-server, you manage SFDX Auth URLs directly:

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                    Community Edition Authentication                          │
├─────────────────────────────────────────────────────────────────────────────┤
│                                                                              │
│   LOCAL DEVELOPMENT                                                          │
│   ─────────────────                                                          │
│                                                                              │
│   ┌─────────────┐     ┌─────────────┐     ┌─────────────┐                   │
│   │  Developer  │────>│  Salesforce │────>│ Local CLI   │                   │
│   │             │     │  OAuth      │     │ Storage     │                   │
│   └─────────────┘     └─────────────┘     └─────────────┘                   │
│                                                                              │
│   $ sf org login web --alias myOrg                                           │
│   $ sfp install --targetorg myOrg --artifactdir ./artifacts                  │
│                                                                              │
│   ✅ Credentials stored locally in ~/.sfdx/                                  │
│                                                                              │
│   CI/CD PIPELINES                                                            │
│   ───────────────                                                            │
│                                                                              │
│   ┌─────────────┐     ┌─────────────┐     ┌─────────────┐                   │
│   │  CI/CD      │────>│  Secrets    │────>│  sfp org    │                   │
│   │  Pipeline   │     │  Store      │     │  login      │                   │
│   └─────────────┘     └─────────────┘     └─────────────┘                   │
│         │                                        │                           │
│         │                                        ▼                           │
│         │                               ┌─────────────┐                      │
│         │                               │ Salesforce  │                      │
│         └──────────────────────────────>│ API         │                      │
│                                         └─────────────┘                      │
│                                                                              │
│   $ sfp org login --url-file /tmp/auth --alias UAT                           │
│   $ sfp install --targetorg UAT --artifactdir ./artifacts                    │
│                                                                              │
│   ✅ Each environment needs its own secret (SFDX Auth URL)                   │
│                                                                              │
└─────────────────────────────────────────────────────────────────────────────┘
```

### Key Differences from Pro Edition

| Aspect | Community Edition | Pro Edition |
|--------|------------------|-------------|
| Credential Storage | CI/CD secrets (per env) | Encrypted in sfp-server |
| Access Control | Share secrets directly | Role-based access |
| Sandbox Refresh | Re-generate auth URL | Automatic via JIT |
| Environment Locking | Not available | Built-in |
| Audit Trail | CI/CD logs only | Complete access logging |

## Overview

Without sfp-server, authentication is handled through:
* Local Salesforce CLI credential storage
* SFDX Auth URLs stored as CI/CD secrets
* Manual credential rotation and management

## Authentication Methods

### Web-based OAuth (Local Development)

For interactive local development:

```bash
# Authenticate to a production/developer org
sfp org login --url-file <(sf org display --target-org myOrg --verbose --json | jq -r '.result.sfdxAuthUrl')

# Or use sf directly for web auth, then use with sfp
sf org login web --alias myOrg
```

### SFDX Auth URL (CI/CD)

For automated pipelines, generate and store SFDX Auth URLs:

#### Generate Auth URL

```bash
# First authenticate interactively
sf org login web --alias myOrg

# Export the auth URL
sf org display --target-org myOrg --verbose --json | jq -r '.result.sfdxAuthUrl'
# Output: force://PlatformCLI::5Aep861_XXXXX@login.salesforce.com
```

#### Store as Secret

Store the auth URL in your CI/CD platform:
* **GitHub Actions**: Repository Secret
* **Azure DevOps**: Pipeline Variable (secret)
* **GitLab CI**: CI/CD Variable (masked)

#### Use in Pipeline

```bash
# Authenticate from secret
sfp org login --url-file <(echo "$SFDX_AUTH_URL") --alias targetOrg

# Or from stdin
echo "$SFDX_AUTH_URL" | sfp org login --url-stdin --alias targetOrg
```

## CI/CD Setup

### GitHub Actions

```yaml
name: Deploy

on:
  push:
    branches: [main]

jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4

      - name: Install sfp
        run: npm install -g @flxbl-io/sfp

      - name: Authenticate DevHub
        run: |
          echo "${{ secrets.DEVHUB_AUTH_URL }}" > /tmp/devhub_auth
          sfp org login --url-file /tmp/devhub_auth --alias devhub --set-default-dev-hub
          rm /tmp/devhub_auth

      - name: Authenticate Target Org
        run: |
          echo "${{ secrets.TARGET_AUTH_URL }}" > /tmp/target_auth
          sfp org login --url-file /tmp/target_auth --alias targetOrg
          rm /tmp/target_auth

      - name: Build
        run: sfp build --devhubusername devhub

      - name: Deploy
        run: sfp install --targetorg targetOrg --artifactdir ./artifacts
```

### Azure DevOps

```yaml
trigger:
  - main

pool:
  vmImage: 'ubuntu-latest'

steps:
  - script: npm install -g @flxbl-io/sfp
    displayName: 'Install sfp'

  - script: |
      echo "$(DEVHUB_AUTH_URL)" > /tmp/devhub_auth
      sfp org login --url-file /tmp/devhub_auth --alias devhub --set-default-dev-hub
      rm /tmp/devhub_auth
    displayName: 'Authenticate DevHub'

  - script: |
      echo "$(TARGET_AUTH_URL)" > /tmp/target_auth
      sfp org login --url-file /tmp/target_auth --alias targetOrg
      rm /tmp/target_auth
    displayName: 'Authenticate Target'

  - script: sfp install --targetorg targetOrg --artifactdir ./artifacts
    displayName: 'Deploy'
```

## Managing Multiple Environments

Without sfp-server, you need separate secrets for each environment:

```
Repository Secrets:
├── DEVHUB_AUTH_URL          # DevHub for scratch orgs
├── DEV_AUTH_URL             # Development sandbox
├── SIT_AUTH_URL             # SIT sandbox
├── UAT_AUTH_URL             # UAT sandbox
├── STAGING_AUTH_URL         # Staging sandbox
└── PRODUCTION_AUTH_URL      # Production org
```

### Environment-specific Deployment

```yaml
jobs:
  deploy:
    strategy:
      matrix:
        include:
          - environment: SIT
            secret_name: SIT_AUTH_URL
          - environment: UAT
            secret_name: UAT_AUTH_URL
    runs-on: ubuntu-latest
    steps:
      - name: Authenticate
        run: |
          echo "${{ secrets[matrix.secret_name] }}" > /tmp/auth
          sfp org login --url-file /tmp/auth --alias ${{ matrix.environment }}
          rm /tmp/auth

      - name: Deploy
        run: sfp install --targetorg ${{ matrix.environment }} --artifactdir ./artifacts
```

## Scratch Org Pools

Pool authentication works the same in community edition, using the DevHub:

```bash
# Authenticate DevHub
sfp org login --url-file devhub_auth.json --alias devhub --set-default-dev-hub

# Prepare pool
sfp pool prepare --tag dev --devhubusername devhub --config-file-path config/pool.json

# Fetch scratch org
sfp pool fetch --tag dev --alias scratchOrg
```

See [Scratch Org Pool Authentication](scratch-org-pools.md) for details.

## Credential Rotation

Without centralized management, you must manually rotate credentials:

1. **Re-authenticate locally**:
   ```bash
   sf org login web --alias myOrg
   ```

2. **Generate new auth URL**:
   ```bash
   sf org display --target-org myOrg --verbose --json | jq -r '.result.sfdxAuthUrl'
   ```

3. **Update CI/CD secret** with new auth URL

4. **Repeat for each environment**

## Limitations vs Pro Edition

| Feature | Community | Pro |
|---------|-----------|-----|
| Credential storage | Local + CI secrets | Centralized server |
| Team access | Share secrets | Role-based access |
| Credential rotation | Manual per env | Update once |
| Environment locking | Not available | Built-in |
| JIT sandbox auth | Not available | Automatic |
| Audit trail | Not available | Complete logging |
| Sandbox refresh handling | Re-authenticate | Automatic via JIT |

## Upgrading to Pro

When ready for centralized management:

1. [Install sfp-pro](../getting-started/install-sfp/install-sfp-pro.md)
2. [Setup sfp-server](../cli-reference/server/init.md)
3. [Register your orgs](org-registration.md)
4. [Create environments](environments.md)
5. Update pipelines to use `sfp server environment get --authenticate`

## Related Topics

* [SFDX Auth URL](sfdx-auth-url.md) - Understanding auth URLs
* [Connected Apps](connected-apps.md) - OAuth configuration
* [Scratch Org Pools](scratch-org-pools.md) - Pool authentication
