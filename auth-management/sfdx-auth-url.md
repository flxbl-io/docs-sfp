# SFDX Auth URL

The SFDX Auth URL is the credential format that enables automated Salesforce authentication. Understanding how it works helps you configure orgs correctly and troubleshoot authentication issues.

## What is an SFDX Auth URL?

An SFDX Auth URL is a URL-formatted credential containing OAuth2 tokens that allow non-interactive authentication with Salesforce. It's the standard credential format used by Salesforce CLI and sfp for automation.

### URL Format

```
force://<clientId>:<clientSecret>:<refreshToken>@<instanceUrl>
```

| Component | Description |
|-----------|-------------|
| `force://` | Protocol identifier |
| `clientId` | OAuth2 Connected App Client ID |
| `clientSecret` | OAuth2 Client Secret (often empty for CLI apps) |
| `refreshToken` | Long-lived token for obtaining access tokens |
| `instanceUrl` | Salesforce instance (e.g., `login.salesforce.com`) |

### Example

```
force://PlatformCLI::5Aep861_XXXXX.YYYYY@login.salesforce.com
```

* **clientId**: `PlatformCLI` (Salesforce CLI's default Connected App)
* **clientSecret**: Empty (not required)
* **refreshToken**: `5Aep861_XXXXX.YYYYY`
* **instanceUrl**: `login.salesforce.com`

## How sfp-server Uses SFDX Auth URLs

{% hint style="info" %}
This section applies to **sfp-pro** with sfp-server
{% endhint %}

sfp-server provides centralized, secure storage for SFDX Auth URLs. When you register orgs with the server, credentials are encrypted and stored in Supabase, then decrypted on-demand when needed.

### Credential Storage Architecture

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                    sfp-server Credential Architecture                        │
├─────────────────────────────────────────────────────────────────────────────┤
│                                                                              │
│   Registration Flow                                                          │
│   ─────────────────                                                          │
│                                                                              │
│   1. Developer authenticates to Salesforce org locally                       │
│      $ sf org login web --alias production                                   │
│                                                                              │
│   2. Register with sfp-server (uploads and encrypts credentials)             │
│      $ sfp server org register --targetorg production                        │
│                                                                              │
│   ┌─────────────┐     ┌─────────────┐     ┌─────────────────────────────┐   │
│   │  Local CLI  │────>│ sfp-server  │────>│ Supabase (sfp_salesforce_auth)│  │
│   │ sfdxAuthUrl │     │   API       │     │                             │   │
│   └─────────────┘     └─────────────┘     │  username                   │   │
│                                           │  instance_url               │   │
│                                           │  org_id                     │   │
│                                           │  sfdx_auth_url_encrypted ◄──│   │
│                                           │  (PGP symmetric encryption) │   │
│                                           └─────────────────────────────┘   │
│                                                                              │
│   Retrieval Flow                                                             │
│   ──────────────                                                             │
│                                                                              │
│   1. Request environment with authentication                                 │
│      $ sfp server environment get --name UAT --repository myorg/app \        │
│          --auth-type accessToken --authenticate                              │
│                                                                              │
│   ┌─────────────┐     ┌─────────────┐     ┌─────────────────────────────┐   │
│   │    CLI      │────>│ sfp-server  │────>│       Supabase              │   │
│   │             │     │             │     │                             │   │
│   └─────────────┘     └─────────────┘     │  SELECT get_auth_url(...)   │   │
│         │                   │             │  → Decrypt with key         │   │
│         │                   │<────────────│  → Return sfdxAuthUrl       │   │
│         │<──────────────────┘             └─────────────────────────────┘   │
│         │                                                                    │
│         │  accessToken    OR    sfdxAuthUrl                                  │
│         │  (short-lived)        (long-lived)                                 │
│         │                                                                    │
│         ▼                                                                    │
│   Local Salesforce authentication created (alias matches environment name)   │
│                                                                              │
└─────────────────────────────────────────────────────────────────────────────┘
```

### Database Schema

sfp-server stores credentials in the `sfp_salesforce_auth` table:

| Column | Description |
|--------|-------------|
| `username` | Salesforce username |
| `instance_url` | Salesforce instance URL |
| `org_id` | Salesforce Org ID |
| `org_type` | `production`, `sandbox`, or `devhub` |
| `sfdx_auth_url_encrypted` | **Encrypted** SFDX Auth URL |
| `is_devhub` | Whether org is a DevHub |
| `is_default` | Default org for its type |
| `parent_production_username` | Parent org (for JIT sandboxes) |
| `is_jit_registration` | Whether sandbox uses JIT auth |

### Encryption

SFDX Auth URLs are encrypted using PGP symmetric encryption before storage:

```sql
-- Storage (during registration)
sfdx_auth_url_encrypted = pgp_sym_encrypt(sfdx_auth_url, encryption_key)

-- Retrieval (via stored procedure)
sfdx_auth_url = pgp_sym_decrypt(sfdx_auth_url_encrypted, encryption_key)
```

The encryption key is configured during sfp-server setup and never leaves the server.

## Registering Orgs with sfp-server

### Production Orgs and DevHubs

Production orgs and DevHubs require an SFDX Auth URL during registration:

```bash
# Step 1: Authenticate locally
sf org login web --alias production

# Step 2: Register with sfp-server
sfp server org register --targetorg production

# For DevHub
sf org login web --alias devhub
sfp server org register --targetorg devhub --is-devhub --is-default
```

What happens during registration:
1. CLI extracts the sfdxAuthUrl from local authentication
2. Validates the connection by querying the org
3. Sends to sfp-server which encrypts and stores it

### Sandboxes (Two Options)

#### Option 1: Direct Registration (with SFDX Auth URL)

```bash
# Authenticate to sandbox directly
sf org login web --alias uat --instance-url https://test.salesforce.com

# Register with sfp-server
sfp server org register --targetorg uat
```

#### Option 2: JIT Registration (recommended)

JIT (Just-In-Time) sandboxes don't store credentials - they generate them on-demand via the parent production org:

```bash
# Register sandbox by name (no local auth needed)
sfp server org register-sandbox \
  --sandbox-name uat \
  --production-username admin@production.com
```

What's stored for JIT sandboxes:
```
sfp_salesforce_auth:
├── username: admin@production--uat.sandbox.com
├── org_type: sandbox
├── sfdx_auth_url_encrypted: NULL  ◄── No stored credentials!
├── parent_production_username: admin@production.com
└── is_jit_registration: true
```

When credentials are requested, sfp-server:
1. Retrieves the parent production org's sfdxAuthUrl
2. Calls Salesforce's sandbox auth API
3. Returns fresh credentials for the sandbox

See [JIT Sandbox Authentication](../environment-management/environments/jit-sandbox.md) for details.

## How Credentials are Returned

sfp-server always returns **short-lived access tokens** when you access environments. The full SFDX Auth URL (containing the refresh token) **never leaves the server**.

```bash
# Standard usage - always use accessToken
sfp server environment get \
  --name UAT \
  --repository myorg/app \
  --auth-type accessToken \
  --authenticate
```

### Access Token Generation

When you request credentials:

1. Server retrieves stored sfdxAuthUrl (encrypted)
2. Uses refresh token to generate new access token
3. Returns **only** the short-lived access token (~2 hours)
4. Refresh token stays on the server

```
┌──────────────────────────────────────────────────────────────────┐
│                    Access Token Generation                        │
├──────────────────────────────────────────────────────────────────┤
│                                                                   │
│   Stored (encrypted):    force://PlatformCLI::RefreshToken@...   │
│                                    │                              │
│                                    ▼                              │
│   Server calls:          POST /services/oauth2/token              │
│                          grant_type=refresh_token                 │
│                                    │                              │
│                                    ▼                              │
│   Returns to CLI:        { accessToken, instanceUrl }             │
│                          (NO refresh token exposed)               │
│                                                                   │
└──────────────────────────────────────────────────────────────────┘
```

### Exception: Scratch Orgs and Pool-Fetched Sandboxes

For scratch orgs fetched from pools and certain sandbox pool scenarios, the full SFDX Auth URL is returned. This is necessary because:

- Scratch org operations may require extended sessions
- Pool-fetched environments need independent credential lifecycle
- The orgs have limited lifespan anyway (scratch orgs expire)

These are the only scenarios where sfdxAuthUrl leaves the server.

## Community Edition: Direct SFDX Auth URL Usage

{% hint style="info" %}
Without sfp-server, you manage SFDX Auth URLs directly in CI/CD secrets.
{% endhint %}

### Generating an SFDX Auth URL

```bash
# Authenticate interactively
sf org login web --alias myOrg

# Export the auth URL
sf org display --target-org myOrg --verbose --json | jq -r '.result.sfdxAuthUrl'
# Output: force://PlatformCLI::5Aep861_XXXXX@login.salesforce.com
```

### Using in CI/CD

Store the SFDX Auth URL as a secret (e.g., `PRODUCTION_AUTH_URL`), then:

```yaml
# GitHub Actions example
- name: Authenticate to Salesforce
  run: |
    echo "${{ secrets.PRODUCTION_AUTH_URL }}" > /tmp/auth
    sfp org login --url-file /tmp/auth --alias production
    rm /tmp/auth
```

See [Community Edition Authentication](community-edition.md) for complete CI/CD examples.

## OAuth2 Behind the Scenes

### The OAuth2 Flow

When you run `sf org login web`:

1. **Authorization Request**: CLI opens browser to Salesforce authorization endpoint
2. **User Consent**: You log in and authorize the Connected App
3. **Authorization Code**: Salesforce redirects back with temporary code
4. **Token Exchange**: CLI exchanges code for tokens:
   * **Access Token**: Short-lived (~2 hours)
   * **Refresh Token**: Long-lived (until revoked)
5. **SFDX Auth URL Created**: CLI packages tokens into the auth URL format

### Token Lifecycle

```
┌─────────────────┐     ┌─────────────────┐     ┌─────────────────┐
│  Refresh Token  │────>│  Access Token   │────>│   API Calls     │
│  (in sfdxAuthUrl)│     │  (2 hours)      │     │                 │
└─────────────────┘     └─────────────────┘     └─────────────────┘
        │                       │
        │                       │ Expires
        │                       ▼
        │               ┌─────────────────┐
        └──────────────>│  New Access     │
           Auto-refresh │  Token          │
                        └─────────────────┘
```

The refresh token is what makes automation possible. sfp and Salesforce CLI automatically use it to obtain new access tokens when needed.

## Troubleshooting

### "Invalid SFDX Auth URL"

* Verify URL starts with `force://`
* Check for `undefined` in the refresh token position
* Ensure instance URL is valid

### "Refresh token expired"

* Re-authenticate: `sf org login web --alias myOrg`
* Check Connected App session policies
* Re-register with sfp-server

### "Decryption failed" (sfp-server)

* Encryption key mismatch between registration and retrieval
* Re-register the org with current encryption key

### "Unable to generate JIT auth"

* Verify parent production org is registered
* Check sandbox exists and is active
* Ensure user has access to the sandbox

## Related Topics

* [Connected Apps](connected-apps.md) - Understanding OAuth clients
* [Org Registration](../environment-management/environments/org-registration.md) - Register orgs with sfp-server
* [JIT Sandbox Authentication](../environment-management/environments/jit-sandbox.md) - On-demand sandbox credentials
* [Environments](../environment-management/environments/README.md) - Link orgs to environments
* [Community Edition](community-edition.md) - Managing credentials without sfp-server
