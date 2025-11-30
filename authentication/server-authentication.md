# Server Authentication

{% hint style="info" %}
This feature requires **sfp-pro** with sfp-server
{% endhint %}

sfp-server provides centralized authentication management for teams. This page explains how to authenticate with sfp-server itself, which is separate from authenticating with Salesforce orgs.

## Authentication Overview

sfp-server uses a two-layer authentication model:

```
┌─────────────────────────────────────────────────────────────────┐
│                    Authentication Layers                        │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│   Layer 1: Server Authentication                                │
│   ─────────────────────────────────                             │
│   You ───> sfp-server                                           │
│   Methods: OAuth (GitHub/SAML), Admin login, Impersonation      │
│   Result: Server access token (stored locally)                  │
│                                                                 │
│   Layer 2: Salesforce Authentication                            │
│   ──────────────────────────────────                            │
│   sfp-server ───> Salesforce Orgs                               │
│   Methods: Registered org credentials, JIT authentication       │
│   Result: Access to Salesforce APIs via server                  │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

## Complete Authentication Flow

Here's the step-by-step flow when a user authenticates to sfp-server and accesses an environment:

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                    Complete Authentication Flow                              │
├─────────────────────────────────────────────────────────────────────────────┤
│                                                                              │
│   Step 1: CLI Login to sfp-server                                            │
│   ────────────────────────────────                                           │
│                                                                              │
│   $ sfp server auth login --email user@company.com --provider github         │
│                                                                              │
│   ┌──────────┐     ┌──────────────┐     ┌─────────────┐     ┌─────────────┐ │
│   │   CLI    │────>│ Local Server │────>│   Browser   │────>│   GitHub    │ │
│   │          │     │ (port 54329) │     │             │     │   OAuth     │ │
│   └──────────┘     └──────────────┘     └─────────────┘     └─────────────┘ │
│        │                  │                    │                    │        │
│        │                  │<───────────────────┴────────────────────┘        │
│        │                  │         OAuth callback with token                │
│        │<─────────────────┘                                                  │
│        │           Token stored in system keychain                           │
│        │                                                                     │
│   ✅ Token stored securely in OS keychain (via keytar)                       │
│                                                                              │
│   Step 2: Access Environment                                                 │
│   ──────────────────────────                                                 │
│                                                                              │
│   $ sfp server environment get --name UAT --repository myorg/app \           │
│       --auth-type accessToken --authenticate                                 │
│                                                                              │
│   ┌──────────┐     ┌─────────────┐     ┌─────────────┐     ┌─────────────┐  │
│   │   CLI    │────>│  Keychain   │────>│ sfp-server  │────>│  Supabase   │  │
│   │          │     │ (get token) │     │   API       │     │  (decrypt)  │  │
│   └──────────┘     └─────────────┘     └─────────────┘     └─────────────┘  │
│        │                                     │                    │          │
│        │                                     │<───────────────────┘          │
│        │                                     │     Decrypted credentials     │
│        │<────────────────────────────────────┘                               │
│        │           Salesforce credentials returned                           │
│        │                                                                     │
│   ┌──────────┐     ┌─────────────┐                                          │
│   │   CLI    │────>│  Salesforce │  Local authentication created             │
│   │          │     │   CLI       │  (alias: "UAT")                           │
│   └──────────┘     └─────────────┘                                          │
│                                                                              │
│   ✅ Org authenticated locally - ready for sfp commands                      │
│                                                                              │
└─────────────────────────────────────────────────────────────────────────────┘
```

### Step-by-Step Walkthrough

#### 1. Initial Server Authentication

```bash
# First time: Login to sfp-server
sfp server auth login --email user@company.com --provider github
```

What happens:
1. CLI starts a local HTTP server on port 54329
2. Opens your browser to GitHub OAuth authorization page
3. You authorize the sfp application on GitHub
4. GitHub redirects back to localhost:54329/callback
5. CLI receives the OAuth token from GitHub via Supabase
6. Token is stored securely in your OS keychain (macOS Keychain, Windows Credential Manager, or Linux Secret Service)

#### 2. Verify Authentication Status

```bash
# Check your current auth status
sfp server auth display
```

Output:
```
┌──────────────────────────────────────────────────────────────────┐
│                    Server Authentication Status                   │
├──────────────────────────────────────────────────────────────────┤
│ Email:      user@company.com                                     │
│ Status:     active                                               │
│ Auth Type:  oauth                                                │
│ Expires In: 23h 45m                                              │
│ Server:     https://sfp.mycompany.com                            │
└──────────────────────────────────────────────────────────────────┘
```

#### 3. Access an Environment

```bash
# Get environment and authenticate to the Salesforce org
sfp server environment get \
  --name UAT \
  --repository myorg/salesforce-app \
  --auth-type accessToken \
  --authenticate
```

What happens:
1. CLI retrieves your server token from the OS keychain
2. Sends request to sfp-server API with your token
3. Server verifies your identity and role permissions
4. Server decrypts the stored Salesforce credentials
5. Returns credentials to CLI (accessToken or sfdxAuthUrl based on `--auth-type`)
6. CLI creates local Salesforce authentication with alias "UAT"

#### 4. Use the Authenticated Org

```bash
# Now you can use sfp commands with the authenticated org
sfp install --targetorg UAT --artifactdir ./artifacts

# Or use sf commands
sf project deploy start --source-dir force-app --target-org UAT
```

### Token Storage Details

sfp stores authentication tokens securely using the operating system's native credential storage:

| OS | Storage Location | Service Name |
|----|------------------|--------------|
| macOS | Keychain Access | `sfp-pro` |
| Windows | Credential Manager | `sfp-pro` |
| Linux | Secret Service (libsecret) | `sfp-pro` |

The token key format is: `supabase-token-{email}`

### Authentication Lifecycle

```
┌─────────────────────────────────────────────────────────────────┐
│                    Token Lifecycle                               │
├─────────────────────────────────────────────────────────────────┤
│                                                                  │
│   Login                   Active                    Expiring     │
│   ──────                  ──────                    ────────     │
│                                                                  │
│   sfp server    ───>    Token valid    ───>    Token expires     │
│   auth login            for ~24 hours          soon (<1 hour)    │
│        │                     │                       │           │
│        │                     │                       │           │
│        ▼                     ▼                       ▼           │
│   ┌─────────┐          ┌─────────┐            ┌─────────────┐   │
│   │ Keychain│          │Commands │            │ Re-auth     │   │
│   │ stored  │          │ work    │            │ prompted    │   │
│   └─────────┘          └─────────┘            └─────────────┘   │
│                                                                  │
│   Expired                Clear                                   │
│   ───────                ─────                                   │
│                                                                  │
│   Token invalid  ───>   sfp server auth clear                    │
│   Must re-login         Removes all stored tokens                │
│                                                                  │
└─────────────────────────────────────────────────────────────────┘
```

## Server Authentication Methods

### OAuth (GitHub)

The most common method for team members. Uses GitHub for identity verification.

```bash
sfp server auth login --provider github
```

This will:
1. Open a browser window to GitHub
2. Authenticate with your GitHub account
3. Store the server token locally

### OAuth (SAML)

For organizations using SAML-based SSO:

```bash
sfp server auth login --provider saml --sso-domain mycompany.okta.com
```

### Admin Login

For server administrators, username/password authentication:

```bash
sfp server auth login --admin --email admin@company.com
```

You'll be prompted for the password, or provide it via file:

```bash
sfp server auth login --admin --email admin@company.com --password-file ./password.txt
```

### Impersonation (Advanced)

For administrative tasks requiring user impersonation. Requires the `SUPABASE_JWT_SECRET` environment variable:

```bash
export SUPABASE_JWT_SECRET=your-jwt-secret
sfp server auth login --impersonate --email user@company.com
```

## Authentication Storage

Server credentials are stored locally and used for subsequent commands:

```bash
# View current auth status
sfp server auth display

# List stored credentials
sfp server auth list

# Clear stored credentials
sfp server auth clear
```

### Token Location

Tokens are stored in the sfp configuration directory:
* **macOS/Linux**: `~/.sfp/`
* **Windows**: `%USERPROFILE%\.sfp\`

## Environment Variables

For CI/CD pipelines, use environment variables instead of interactive login:

```bash
# Server URL
export SFP_SERVER_URL=https://sfp.mycompany.com

# Server token (from service account or application token)
export SFP_SERVER_TOKEN=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...
```

With these set, sfp commands automatically authenticate with the server:

```bash
# No manual login needed
sfp server environment list
```

## Application Tokens

For CI/CD and automation, application tokens provide secure, non-interactive authentication.

### Creating an Application Token

```bash
# Create a new application token
sfp server token create --name ci-pipeline --expiry 90d
```

Output:
```
Application Token Created:
Name: ci-pipeline
Token: sfp_at_xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
Expires: 2024-05-15

Store this token securely - it won't be shown again!
```

### Using Application Tokens

```bash
# Set as environment variable
export SFP_SERVER_TOKEN=sfp_at_xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx

# Or pass directly
sfp server environment list --token sfp_at_xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
```

### Managing Application Tokens

```bash
# List tokens
sfp server token list

# Revoke a token
sfp server token revoke --name ci-pipeline
```

## Role-Based Access

Server authentication grants different capabilities based on user roles:

| Role | Capabilities |
|------|-------------|
| **Member** | View environments, fetch orgs (no credentials) |
| **Owner** | Full access including credentials, org registration |
| **Application** | API access for automation |

### Credential Access

Only certain roles can retrieve Salesforce credentials:

```bash
# This works for owners and applications
sfp server environment get \
  --name uat \
  --repository myorg/myrepo \
  --auth-type accessToken \
  --authenticate

# Members can view environment info but not retrieve auth credentials
```

## CI/CD Integration

### GitHub Actions

```yaml
jobs:
  deploy:
    runs-on: ubuntu-latest
    env:
      SFP_SERVER_URL: ${{ secrets.SFP_SERVER_URL }}
      SFP_SERVER_TOKEN: ${{ secrets.SFP_SERVER_TOKEN }}
    steps:
      - name: Get Environment
        run: |
          sfp server environment get \
            --name production \
            --repository ${{ github.repository }} \
            --auth-type accessToken \
            --authenticate

      - name: Deploy
        run: |
          sfp install --targetorg production --artifactdir ./artifacts
```

### Azure DevOps

```yaml
variables:
  SFP_SERVER_URL: $(SFP_SERVER_URL)
  SFP_SERVER_TOKEN: $(SFP_SERVER_TOKEN)

steps:
  - script: |
      sfp server environment get \
        --name production \
        --repository $(Build.Repository.Name) \
        --auth-type accessToken \
        --authenticate
    displayName: 'Authenticate Environment'
```

## Global Auth Mode

sfp-server can be configured for global authentication, routing users to their appropriate tenant:

```bash
sfp server auth login --global-auth
```

This is useful for multi-tenant deployments where a single authentication endpoint serves multiple organizations.

## Session Management

### Session Timeout

Server sessions have configurable timeouts. Re-authenticate when expired:

```bash
# Check if session is valid
sfp server auth display

# Re-authenticate if needed
sfp server auth login --provider github
```

### Multiple Servers

If working with multiple sfp-server instances:

```bash
# Authenticate to different servers
SFP_SERVER_URL=https://dev.sfp.company.com sfp server auth login
SFP_SERVER_URL=https://prod.sfp.company.com sfp server auth login
```

## Security Best Practices

### For Team Members

* Use OAuth (GitHub/SAML) for interactive login
* Don't share personal tokens
* Log out when switching contexts

### For CI/CD

* Use application tokens, not personal credentials
* Set appropriate token expiration
* Rotate tokens periodically
* Use separate tokens per pipeline

### For Administrators

* Limit who can create application tokens
* Monitor token usage
* Revoke unused tokens
* Use impersonation sparingly

## Troubleshooting

### "Authentication failed"

```bash
# Clear existing credentials and re-authenticate
sfp server auth clear
sfp server auth login --provider github
```

### "Token expired"

```bash
# Re-authenticate
sfp server auth login

# Or create a new application token
sfp server token create --name new-token
```

### "Unauthorized"

* Verify your role has the required permissions
* Check if the application token is still valid
* Ensure `SFP_SERVER_URL` is correct

### "Cannot connect to server"

```bash
# Verify server is running
sfp server health

# Check server URL
echo $SFP_SERVER_URL
```

## Related Topics

* [Org Registration](org-registration.md) - Register Salesforce orgs with server
* [Environments](environments.md) - Environment management
* [Application Tokens](../cli-reference/server/application-token.md) - CLI reference
