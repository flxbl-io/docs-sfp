---
icon: ring-diamond
---

# JIT Sandbox Authentication

{% hint style="info" %}
This feature requires **sfp-pro** with sfp-server
{% endhint %}

JIT (Just-In-Time) sandbox authentication generates sandbox credentials on-demand via the parent production org, eliminating the need to store and manage individual sandbox credentials. This feature only works provided the sandbox is refreshed /created using the same user that is registered as production org in the sfp server

## How JIT Authentication Works

Instead of storing credentials for each sandbox, sfp-server:

1. Stores the parent production org's credentials
2. Uses Salesforce's sandbox auth API to generate credentials when needed
3. Returns short-lived credentials for the requested sandbox

```
┌─────────────────────────────────────────────────────────────────┐
│                    JIT Authentication Flow                      │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│   Request sandbox access                                        │
│          │                                                      │
│          ▼                                                      │
│   sfp-server checks: Is this a JIT sandbox?                     │
│          │                                                      │
│          │ Yes                                                  │
│          ▼                                                      │
│   Retrieve parent production credentials                        │
│          │                                                      │
│          ▼                                                      │
│   Connect to production org                                     │
│          │                                                      │
│          ▼                                                      │
│   Call Salesforce Sandbox Auth API                              │
│   POST /services/data/vXX.0/tooling/sandboxAuth                 │
│          │                                                      │
│          ▼                                                      │
│   Receive sandbox auth fields                                   │
│          │                                                      │
│          ▼                                                      │
│   Return credentials to user                                    │
│   (accessToken + instanceUrl OR sfdxAuthUrl)                    │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

## Benefits of JIT Authentication

| Traditional Approach                  | JIT Approach                      |
| ------------------------------------- | --------------------------------- |
| Store credentials for each sandbox    | Only store production credentials |
| Re-authenticate after sandbox refresh | Automatic - uses production auth  |
| Manage N sandbox credentials          | Manage 1 production credential    |
| Risk of stale credentials             | Always fresh credentials          |

## Setting Up JIT Sandboxes

### Step 1: Register the Production Org

```bash
# Authenticate to production
sf org login web --alias production

# Register with sfp-server
sfp server org register --targetorg production
```

### Step 2: Register Sandbox with Parent

```bash
sfp server org register-sandbox \
  --sandbox-name uat \
  --production-username admin@production.com
```

This creates a "JIT registration" - the sandbox is registered but no credentials are stored:

```
Sandbox Registration:
├── username: admin@production--uat.sandbox.com
├── is_jit_registration: true
├── parent_production_username: admin@production.com
└── sfdx_auth_url_encrypted: NULL  (no stored credentials)
```

### Step 3: Create Environment (Optional)

Link the JIT sandbox to an environment:

```bash
sfp server environment create \
  --repository myorg/salesforce-app \
  --name UAT \
  --category test \
  --branch release/* \
  --description "UAT environment with JIT auth" \
  --targetusername admin@production--uat.sandbox.com
```

## Using JIT Sandboxes

### Direct Sandbox Access

```bash
# This triggers JIT authentication
sfp server org login --username admin@production--uat.sandbox.com
```

Behind the scenes:

1. Server sees this is a JIT sandbox
2. Retrieves production credentials
3. Calls sandbox auth API
4. Returns fresh sandbox credentials

### Via Environment

```bash
# Get environment with JIT sandbox
sfp server environment get \
  --name UAT \
  --repository myorg/salesforce-app \
  --auth-type accessToken \
  --authenticate
```

## Sandbox Refresh Handling

JIT authentication automatically handles sandbox refreshes:

```
Before Refresh:
├── Production: admin@production.com
└── Sandbox: admin@production--uat.sandbox.com
    └── JIT auth works ✓

After Refresh (sandbox recreated):
├── Production: admin@production.com
└── Sandbox: admin@production--uat.sandbox.com  (new sandbox)
    └── JIT auth still works ✓  (uses production to authenticate)
```

No manual credential update needed!

## When JIT Auth is Used

sfp-server automatically uses JIT authentication when:

1. The org is registered with `is_jit_registration = true`
2. No stored credentials exist (`sfdx_auth_url_encrypted = NULL`)
3. A `parent_production_username` is set

## CI/CD Integration

### Standard Usage

```yaml
jobs:
  deploy-to-sandbox:
    runs-on: ubuntu-latest
    env:
      SFP_SERVER_URL: ${{ secrets.SFP_SERVER_URL }}
      SFP_SERVER_TOKEN: ${{ secrets.SFP_SERVER_TOKEN }}
    steps:
      - name: Authenticate to UAT (JIT)
        run: |
          # JIT authentication happens automatically
          sfp server environment get \
            --name UAT \
            --repository ${{ github.repository }} \
            --auth-type accessToken \
            --authenticate

      - name: Deploy
        run: |
          sfp install --targetorg UAT --artifactdir ./artifacts
```

### Multi-Sandbox Pipeline

```yaml
jobs:
  deploy:
    strategy:
      matrix:
        sandbox: [SIT, UAT, Staging]
    runs-on: ubuntu-latest
    steps:
      - name: Authenticate to ${{ matrix.sandbox }}
        run: |
          # All sandboxes use JIT auth via production
          sfp server environment get \
            --name ${{ matrix.sandbox }} \
            --repository ${{ github.repository }} \
            --auth-type accessToken \
            --authenticate

      - name: Deploy
        run: |
          sfp install --targetorg ${{ matrix.sandbox }} --artifactdir ./artifacts
```

## Mixing JIT and Traditional Auth

You can have both JIT and traditionally-registered sandboxes:

```
Registered Orgs:
├── admin@production.com (traditional - credentials stored)
├── admin@devhub.com (traditional - credentials stored)
├── admin@production--uat.sandbox.com (JIT - via production)
├── admin@production--sit.sandbox.com (JIT - via production)
└── admin@legacy.sandbox.com (traditional - credentials stored)
```

## Troubleshooting

### "Parent production org not found"

The parent org isn't registered:

```bash
# Register the production org first
sf org login web --alias production
sfp server org register --targetorg production

# Then register the sandbox
sfp server org register-sandbox \
  --sandbox-name uat \
  --production-username admin@production.com
```

### "Unable to generate JIT auth"

* Verify the production org credentials are valid
* Check that the sandbox exists and is active
* Ensure the user has access to the sandbox

### "Sandbox not found"

The sandbox may have been refreshed with a different name:

```bash
# Re-register with correct sandbox name
sfp server org register-sandbox \
  --sandbox-name new-uat-name \
  --production-username admin@production.com
```

### JIT Auth Slow

JIT authentication involves an API call to production. If consistently slow:

* Check production org API limits
* Consider using traditional auth for high-frequency sandboxes

## Limitations

* **Requires Production Access**: User must have access to the parent production org
* **API Call Required**: Each JIT auth makes an API call to production
* **Sandbox Must Exist**: JIT can't authenticate to non-existent sandboxes
* **Full Sandboxes Only**: JIT works with sandboxes, not scratch orgs

## Related Topics

* [Org Registration](org-registration.md) - Register orgs with server
* [Environments](./) - Environment management
* [Server Authentication](../../auth-management/server-authentication.md) - Authenticate with sfp-server
