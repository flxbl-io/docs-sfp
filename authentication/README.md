# Authentication

Authentication in sfp connects your CLI to Salesforce orgs for validation, deployment, and environment management. With sfp-pro and sfp-server, you get centralized credential management where **no one needs to handle Salesforce credentials directly** - not developers, not CI/CD pipelines.

## The Key Benefit: No Credentials in CI/CD or Local Machines

With sfp-server, Salesforce credentials (SFDX Auth URLs) are stored **only on the server**:

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                    Why sfp-server Matters                                    │
├─────────────────────────────────────────────────────────────────────────────┤
│                                                                              │
│   WITHOUT sfp-server (Community Edition)                                     │
│   ──────────────────────────────────────                                     │
│                                                                              │
│   ┌─────────────┐     ┌─────────────┐     ┌─────────────┐                   │
│   │  CI/CD      │────>│ CI Secrets  │────>│  Salesforce │                   │
│   │             │     │ (Auth URLs) │     │             │                   │
│   └─────────────┘     └─────────────┘     └─────────────┘                   │
│                                                                              │
│   ⚠️ Each environment needs its own secret                                   │
│   ⚠️ Credentials must be rotated manually                                    │
│   ⚠️ No access control - anyone with CI access has all credentials          │
│   ⚠️ Sandbox refresh requires re-generating secrets                          │
│                                                                              │
│   ═══════════════════════════════════════════════════════════════════════   │
│                                                                              │
│   WITH sfp-server (Pro Edition)                                              │
│   ─────────────────────────────                                              │
│                                                                              │
│   ┌─────────────┐     ┌─────────────┐     ┌─────────────┐     ┌──────────┐  │
│   │  CI/CD      │────>│ sfp-server  │────>│  Encrypted  │────>│Salesforce│  │
│   │             │     │  Token      │     │  Storage    │     │          │  │
│   └─────────────┘     └─────────────┘     └─────────────┘     └──────────┘  │
│                                                                              │
│   ✅ ONE token for CI/CD (SFP_SERVER_TOKEN) - accesses all environments     │
│   ✅ Salesforce credentials never leave the server                          │
│   ✅ Role-based access - members can deploy, only owners manage credentials │
│   ✅ Sandbox refresh handled automatically via JIT authentication          │
│   ✅ Complete audit trail of all credential access                          │
│                                                                              │
└─────────────────────────────────────────────────────────────────────────────┘
```

## How It Works

### Step 1: Admin Registers Orgs (One-Time)

An admin authenticates to Salesforce orgs locally and registers them with sfp-server. The credentials are encrypted and stored on the server.

```bash
# Admin authenticates locally (only admin needs to do this)
sf org login web --alias production

# Admin registers with sfp-server
sfp server org register --targetorg production
```

After this, **no one else needs the Salesforce credentials**.

### Step 2: Admin Creates Environments

Environments link repository branches to registered orgs:

```bash
sfp server environment create \
  --repository myorg/salesforce-app \
  --name UAT \
  --category test \
  --branch release/* \
  --targetusername admin@production--uat.sandbox.com
```

### Step 3: Team Members Access Environments

Developers and CI/CD access environments using their server credentials. They never see or handle Salesforce credentials:

```bash
# Developer logs in with GitHub OAuth
sfp server auth login --email developer@company.com --provider github

# Developer accesses environment (server handles Salesforce auth behind the scenes)
sfp server environment get \
  --name UAT \
  --repository myorg/salesforce-app \
  --auth-type accessToken \
  --authenticate

# Developer deploys (org is now authenticated locally)
sfp install --targetorg UAT --artifactdir ./artifacts
```

### Step 4: CI/CD Uses Application Token

CI/CD pipelines use a single application token to access all environments:

```yaml
# GitHub Actions
env:
  SFP_SERVER_URL: ${{ secrets.SFP_SERVER_URL }}
  SFP_SERVER_TOKEN: ${{ secrets.SFP_SERVER_TOKEN }}  # ONE token for all envs!

steps:
  - name: Deploy to UAT
    run: |
      sfp server environment get \
        --name UAT \
        --repository ${{ github.repository }} \
        --auth-type accessToken \
        --authenticate
      sfp install --targetorg UAT --artifactdir ./artifacts
```

## Role-Based Access Control

sfp-server enforces role-based access to credentials:

| Role | Can List Environments | Can Deploy | Can Get Credentials | Can Register Orgs |
|------|----------------------|------------|---------------------|-------------------|
| **Guest** | ❌ | ❌ | ❌ | ❌ |
| **Member** | ✅ | ✅ | ❌ | ❌ |
| **Owner** | ✅ | ✅ | ✅ | ✅ |
| **Application** | ✅ | ✅ | ✅ | Limited |

### What This Means

- **Members** (developers) can view environments and deploy to them, but never see actual Salesforce credentials
- **Owners** (admins) can register orgs and access raw credentials when needed
- **Applications** (CI/CD tokens) can access credentials for deployment but can't manage org registrations

When a member requests credentials:
```
Member requests: sfp server environment get --name UAT --auth-type accessToken --authenticate

Server checks:
├── Is user a member of this team? ✅
├── Can members access credentials? ❌ (only owners/applications can)
└── Result: 403 Forbidden - "Access to authentication credentials requires owner or application role"
```

When CI/CD (application token) requests credentials:
```
Application requests: sfp server environment get --name UAT --auth-type accessToken --authenticate

Server checks:
├── Is this a valid application token? ✅
├── Can applications access credentials? ✅
├── Decrypt stored sfdxAuthUrl
├── Generate short-lived access token
└── Return access token (refresh token stays on server)
```

## Complete Setup Flow

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                    Complete Setup Flow                                       │
├─────────────────────────────────────────────────────────────────────────────┤
│                                                                              │
│   1. SETUP SFP-SERVER (Admin)                                                │
│   ──────────────────────────                                                 │
│   $ sfp server init                                                          │
│   $ sfp server start                                                         │
│                                                                              │
│   2. REGISTER ORGS (Admin)                                                   │
│   ────────────────────────                                                   │
│   # Production (requires local auth)                                         │
│   $ sf org login web --alias production                                      │
│   $ sfp server org register --targetorg production                           │
│                                                                              │
│   # DevHub                                                                   │
│   $ sf org login web --alias devhub                                          │
│   $ sfp server org register --targetorg devhub --is-devhub --is-default      │
│                                                                              │
│   # Sandboxes (JIT - no local auth needed!)                                  │
│   $ sfp server org register-sandbox \                                        │
│       --sandbox-name uat \                                                   │
│       --production-username admin@production.com                             │
│                                                                              │
│   3. CREATE ENVIRONMENTS (Admin)                                             │
│   ──────────────────────────────                                             │
│   $ sfp server environment create \                                          │
│       --repository myorg/salesforce-app \                                    │
│       --name UAT \                                                           │
│       --category test \                                                      │
│       --branch release/* \                                                   │
│       --targetusername admin@production--uat.sandbox.com                     │
│                                                                              │
│   4. CREATE CI/CD TOKEN (Admin)                                              │
│   ─────────────────────────────                                              │
│   $ sfp server token create --name ci-pipeline --expiry 90d                  │
│   # Output: sfp_at_xxxxxxxx (store as SFP_SERVER_TOKEN)                      │
│                                                                              │
│   5. TEAM MEMBERS USE ENVIRONMENTS                                           │
│   ────────────────────────────────                                           │
│   # Login with GitHub (no Salesforce auth needed)                            │
│   $ sfp server auth login --provider github                                  │
│                                                                              │
│   # Access environment                                                       │
│   $ sfp server environment get \                                             │
│       --name UAT \                                                           │
│       --repository myorg/salesforce-app \                                    │
│       --auth-type accessToken \                                              │
│       --authenticate                                                         │
│                                                                              │
│   # Deploy                                                                   │
│   $ sfp install --targetorg UAT --artifactdir ./artifacts                    │
│                                                                              │
└─────────────────────────────────────────────────────────────────────────────┘
```

## Quick Start

### For Admins

```bash
# 1. Initialize and start sfp-server
sfp server init
sfp server start

# 2. Register your orgs
sf org login web --alias production
sfp server org register --targetorg production

# 3. Register sandboxes via JIT (no auth needed)
sfp server org register-sandbox --sandbox-name uat --production-username admin@production.com

# 4. Create environments
sfp server environment create \
  --repository myorg/salesforce-app \
  --name UAT \
  --category test \
  --branch release/* \
  --targetusername admin@production--uat.sandbox.com

# 5. Create CI/CD token
sfp server token create --name ci-pipeline --expiry 90d
```

### For Developers

```bash
# 1. Login to sfp-server (GitHub OAuth)
sfp server auth login --email your@email.com --provider github

# 2. Access environment
sfp server environment get \
  --name UAT \
  --repository myorg/salesforce-app \
  --auth-type accessToken \
  --authenticate

# 3. Deploy
sfp install --targetorg UAT --artifactdir ./artifacts
```

### For CI/CD Pipelines

```yaml
env:
  SFP_SERVER_URL: ${{ secrets.SFP_SERVER_URL }}
  SFP_SERVER_TOKEN: ${{ secrets.SFP_SERVER_TOKEN }}

steps:
  - name: Authenticate
    run: |
      sfp server environment get \
        --name Production \
        --repository ${{ github.repository }} \
        --auth-type accessToken \
        --authenticate

  - name: Deploy
    run: sfp install --targetorg Production --artifactdir ./artifacts
```

## What's Next?

* [Server Authentication](server-authentication.md) - Complete auth flow details
* [SFDX Auth URL](sfdx-auth-url.md) - How credentials work under the hood
* [Org Registration](org-registration.md) - Register orgs with server
* [Environments](environments.md) - Create and manage environments
* [Accessing Environments](accessing-environments.md) - Practical examples

---

{% hint style="info" %}
**Using Community Edition?** Without sfp-server, you'll manage SFDX Auth URLs directly in CI/CD secrets. See [Community Edition Authentication](community-edition.md).
{% endhint %}
