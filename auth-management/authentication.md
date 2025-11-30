# Overview

{% hint style="info" %}
This section covers how authentication is handled (both user and orgs)  for sfp cli. For Community Edition, see [Community Edition Authentication](community-edition.md).
{% endhint %}

sfp-pro uses a two-layer authentication model that separates **server access** from **Salesforce org access**. This allows teams to share access to Salesforce environments without sharing Salesforce credentials.

## Two-Layer Authentication Model

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                    Two-Layer Authentication                                  │
├─────────────────────────────────────────────────────────────────────────────┤
│                                                                              │
│   Layer 1: Authenticate to sfp-server                                        │
│   ────────────────────────────────────                                       │
│                                                                              │
│   Developer/CI ───> sfp-server (OAuth or Application Token)                  │
│                                                                              │
│   Layer 2: Server retrieves Salesforce credentials                           │
│   ────────────────────────────────────────────────                           │
│                                                                              │
│   sfp-server ───> Encrypted Storage ───> Salesforce                          │
│                                                                              │
│   Result: User gets short-lived access token, never sees stored credentials  │
│                                                                              │
└─────────────────────────────────────────────────────────────────────────────┘
```

**How this works in practice:**

1. An administrator registers Salesforce orgs once - credentials are encrypted and stored on sfp-server
2. Team members authenticate to sfp-server using OAuth (GitHub, SAML, etc.)
3. When accessing an environment, sfp-server decrypts stored credentials and returns a short-lived access token
4. The refresh token and SFDX Auth URL never leave the server

## Layer 1: Authenticating to sfp-server

### Developers (OAuth)

```bash
# Interactive login - opens browser for GitHub/SAML authentication
sfp server auth login --email your@email.com --provider github

# Verify authentication
sfp server auth display
```

Token is stored in your OS keychain (macOS Keychain, Windows Credential Manager, Linux Secret Service).

### CI/CD Pipelines (Application Token)

```yaml
env:
  SFP_SERVER_URL: ${{ secrets.SFP_SERVER_URL }}
  SFP_SERVER_TOKEN: ${{ secrets.SFP_SERVER_TOKEN }}
```

Application tokens are created by administrators and provide non-interactive access.

## Layer 2: Accessing Salesforce Orgs

Once authenticated to sfp-server, access Salesforce through environments:

```bash
# Request environment - server returns short-lived access token
sfp server environment get \
  --name UAT \
  --repository myorg/salesforce-app \
  --auth-type accessToken \
  --authenticate

# Org is now authenticated locally as "UAT"
sfp install --targetorg UAT --artifactdir ./artifacts
```

The `--auth-type accessToken` returns a token valid for \~2 hours (depending on the session expiry configured in the org). The stored SFDX Auth URL never leaves the server.

{% hint style="info" %}
**Exception**: Scratch orgs from pools and sandbox pools receive full SFDX Auth URLs since these orgs have limited lifespan and require extended sessions.
{% endhint %}

## Role-Based Access

Access to credentials is controlled by role:

| Role            | View Environments | Get Credentials | Register Orgs |
| --------------- | ----------------- | --------------- | ------------- |
| **Member**      | ✅                 | ❌               | ❌             |
| **Owner**       | ✅                 | ✅               | ✅             |
| **Application** | ✅                 | ✅               | Limited       |

* **Members** can list environments and see metadata, but cannot retrieve Salesforce credentials
* **Owners** have full access including org registration and credential retrieval
* **Applications** (CI/CD tokens) can retrieve credentials for automated deployments

## Setup Workflow

```
Administrator                          Team Members / CI/CD
─────────────                          ────────────────────

1. Register orgs with server
   $ sf org login web --alias prod
   $ sfp server org register --targetusername prod

2. Create environments
   $ sfp server environment create \
       --name UAT --repository myorg/app \
       --category test --branch main \
       --description "UAT environment" \
       --targetusername admin@prod--uat.sandbox.com

3. Create application token for CI/CD
   $ sfp server application-token create \
       --name ci-pipeline --expires-in 90
                                       4. Authenticate to server
                                          $ sfp server auth login --provider github

                                       5. Access environment
                                          $ sfp server environment get \
                                              --name UAT --repository myorg/app \
                                              --auth-type accessToken --authenticate

                                       6. Deploy
                                          $ sfp install --targetorg UAT
```

## Quick Reference

| Task               | Command                                                                                          |
| ------------------ | ------------------------------------------------------------------------------------------------ |
| Login to server    | `sfp server auth login --provider github`                                                        |
| Check auth status  | `sfp server auth display`                                                                        |
| List environments  | `sfp server environment list --repository REPO`                                                  |
| Access environment | `sfp server environment get --name ENV --repository REPO --auth-type accessToken --authenticate` |
| Create app token   | `sfp server application-token create --name NAME --expires-in 30`                               |

## Related Topics

* [Server Authentication](server-authentication.md) - Detailed authentication flows
* [SFDX Auth URL](sfdx-auth-url.md) - How credentials are stored and retrieved
* [Environments](../environment-management/environments/) - Environment setup and management
* [Accessing Environments](../environment-management/environments/accessing-environments.md) - Practical examples
