---
icon: ring-diamond
---

# Org Registration

{% hint style="info" %}
This feature requires **sfp-pro** with sfp-server
{% endhint %}

Org registration stores Salesforce credentials centrally on sfp-server, enabling team members and CI/CD pipelines to access orgs without managing individual credentials.

## Why Register Orgs?

| Without Registration                         | With Registration                     |
| -------------------------------------------- | ------------------------------------- |
| Each developer manages their own credentials | Credentials stored centrally          |
| CI/CD secrets for each org                   | Single server token for CI/CD         |
| No credential rotation coordination          | Centralized credential management     |
| Risk of stale/invalid credentials            | Server validates and refreshes tokens |

## Registration Flow

```
┌─────────────────────────────────────────────────────────────────┐
│                    Org Registration Flow                        │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│   1. Developer authenticates locally                            │
│      sf org login web --alias myOrg                             │
│                                                                 │
│   2. Register with server                                       │
│      sfp server org register --targetorg myOrg                  │
│                  │                                              │
│                  ▼                                              │
│   3. Server stores encrypted credentials                        │
│      ┌─────────────────────────────────┐                        │
│      │ sfp-server (Supabase)           │                        │
│      │ ┌─────────────────────────────┐ │                        │
│      │ │ sfp_salesforce_auth         │ │                        │
│      │ │ - username                  │ │                        │
│      │ │ - instance_url              │ │                        │
│      │ │ - sfdx_auth_url (encrypted) │ │                        │
│      │ │ - org_id                    │ │                        │
│      │ │ - is_devhub                 │ │                        │
│      │ └─────────────────────────────┘ │                        │
│      └─────────────────────────────────┘                        │
│                                                                 │
│   4. Team members can now access the org                        │
│      sfp server org login --username admin@myorg.com            │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

## Registering Orgs

### Basic Registration

First, authenticate locally, then register:

```bash
# Step 1: Authenticate to the org
sf org login web --alias production

# Step 2: Register with sfp-server
sfp server org register --targetorg production
```

### Register as DevHub

Mark an org as your DevHub for scratch org operations:

```bash
sfp server org register --targetorg devhub --is-devhub
```

### Register as Default

Set as the default org for a specific role:

```bash
sfp server org register --targetorg production --is-default
```

### Register with Metadata

Add custom metadata for organization:

```bash
sfp server org register \
  --targetorg production \
  --metadata '{"region": "US", "tier": "enterprise"}'
```

## Listing Registered Orgs

```bash
sfp server org list
```

Output:

```
┌──────────────────────────┬────────────────────────────────┬─────────┬─────────┐
│ Username                 │ Instance URL                   │ DevHub  │ Default │
├──────────────────────────┼────────────────────────────────┼─────────┼─────────┤
│ admin@production.com     │ https://myorg.my.salesforce.com│ No      │ Yes     │
│ admin@devhub.com         │ https://devhub.salesforce.com  │ Yes     │ Yes     │
│ admin@uat.sandbox.com    │ https://myorg--uat.sandbox.com │ No      │ No      │
└──────────────────────────┴────────────────────────────────┴─────────┴─────────┘
```

## Accessing Registered Orgs

### Login via Server

Team members can authenticate to registered orgs without having the original credentials:

```bash
# Authenticate locally using server-stored credentials
sfp server org login --username admin@production.com
```

This retrieves credentials from the server and authenticates locally.

### Get Default DevHub

```bash
sfp server org get-default-devhub
```

## Credential Storage Security

### Encryption

SFDX Auth URLs are encrypted before storage:

```sql
-- Stored encrypted using PGP symmetric encryption
sfdx_auth_url_encrypted = pgp_sym_encrypt(auth_url, encryption_key)
```

The encryption key is configured during sfp-server setup and never exposed.

### Access Control

* Only users with **owner** or **application** roles can retrieve credentials
* Members can see org metadata but not credentials
* All access is logged in the audit trail

### What's Stored

| Field         | Stored | Encrypted |
| ------------- | ------ | --------- |
| Username      | Yes    | No        |
| Instance URL  | Yes    | No        |
| Org ID        | Yes    | No        |
| SFDX Auth URL | Yes    | **Yes**   |
| Org Type      | Yes    | No        |
| Metadata      | Yes    | No        |

## Sandbox Registration

### Standard Sandbox Registration

```bash
# Authenticate to sandbox
sf org login web --alias uat --instance-url https://test.salesforce.com

# Register
sfp server org register --targetorg uat
```

### Register with Parent (for JIT)

Link a sandbox to its parent production org for JIT authentication:

```bash
sfp server org register-sandbox \
  --sandbox-name uat \
  --production-username admin@production.com
```

This enables [JIT Sandbox Authentication](jit-sandbox.md) - the sandbox can be authenticated on-demand via the production org.

## Updating Registrations

### Update Credentials

When org credentials change (e.g., after re-authentication):

```bash
# Re-authenticate locally
sf org login web --alias production

# Update server registration
sfp server org update --targetorg production
```

### Update Metadata

```bash
sfp server org update \
  --username admin@production.com \
  --metadata '{"region": "EU", "tier": "enterprise"}'
```

## Removing Registrations

```bash
sfp server org delete --username admin@oldorg.com
```

{% hint style="warning" %}
Deleting an org registration will break any environments linked to that org.
{% endhint %}

## CI/CD Integration

### Use Registered Orgs in Pipelines

```yaml
jobs:
  deploy:
    runs-on: ubuntu-latest
    env:
      SFP_SERVER_URL: ${{ secrets.SFP_SERVER_URL }}
      SFP_SERVER_TOKEN: ${{ secrets.SFP_SERVER_TOKEN }}
    steps:
      - name: Login to Production
        run: |
          sfp server org login --username admin@production.com --alias production

      - name: Deploy
        run: |
          sfp install --targetorg production --artifactdir ./artifacts
```

## Troubleshooting

### "Org not found"

```bash
# Verify org is registered
sfp server org list

# Re-register if needed
sfp server org register --targetorg myOrg
```

### "Unable to retrieve credentials"

* Verify you have owner or application role
* Check if the SFDX Auth URL is still valid
* Re-register the org if credentials expired

### "Invalid SFDX Auth URL"

The stored credentials may be stale:

```bash
# Re-authenticate locally
sf org login web --alias myOrg

# Update registration
sfp server org update --targetorg myOrg
```

## Related Topics

* [Server Authentication](../../authentication/server-authentication.md) - Authenticate with sfp-server
* [Environments](./) - Link registered orgs to environments
* [JIT Sandbox](jit-sandbox.md) - On-demand sandbox authentication
* [SFDX Auth URL](../../authentication/sfdx-auth-url.md) - Understanding auth URLs
