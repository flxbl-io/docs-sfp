# Scratch Org Pool Authentication

Scratch org pools require special authentication handling. This page explains how authentication works when creating, storing, and fetching scratch orgs from pools.

## How Pool Authentication Works

Scratch org pools store authentication credentials in a custom object on the DevHub. This enables teams to share pre-created scratch orgs without sharing DevHub credentials.

```
┌─────────────────────────────────────────────────────────────────┐
│                    Pool Authentication Flow                     │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│   Pool Prepare                      Pool Fetch                  │
│   ───────────                       ──────────                  │
│                                                                 │
│   DevHub ──────> Create Scratch Org                             │
│      │                   │                                      │
│      │                   │                                      │
│      │                   ▼                                      │
│      │           ┌─────────────────┐                            │
│      └──────────>│ ScratchOrgInfo  │                            │
│                  │ (Custom Object) │                            │
│                  │                 │                            │
│                  │ SfdxAuthUrl__c  │◄────── Developer fetches   │
│                  │ Password__c     │        and authenticates   │
│                  │ Status__c       │                            │
│                  └─────────────────┘                            │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

## DevHub Authentication Requirements

### Valid Authentication Methods

Pool operations require the DevHub to be authenticated with methods that generate an SFDX Auth URL:

```bash
# Recommended methods
sf org login web --alias devhub --set-default-dev-hub
sf org login jwt --client-id ... --jwt-key-file ... --username ... --alias devhub
```

{% hint style="warning" %}
Pool prepare will fail if the DevHub was authenticated using methods that don't generate SFDX Auth URLs:
* `sf org login access-token`
* Some SSO configurations
{% endhint %}

### Validation Check

When running `sfp pool prepare`, sfp validates that the DevHub has a valid SFDX Auth URL:

```bash
# This validation happens automatically
sfp pool prepare --tag dev --config-file-path config/pool-config.json

# If validation fails, you'll see:
# Error: Pools have to be created using a DevHub authenticated with
#        auth:web or auth:store or auth:accesstoken:store
```

## ScratchOrgInfo Custom Object

Pool authentication relies on a custom object installed on the DevHub.

### Required Fields

| Field | Type | Purpose |
|-------|------|---------|
| `Pooltag__c` | Text | Pool identifier |
| `SfdxAuthUrl__c` | Text (Encrypted) | Authentication URL |
| `Password__c` | Text | Auto-generated password |
| `Allocation_status__c` | Picklist | Pool status tracking |
| `SignupUsername` | Text | Scratch org username |
| `ExpirationDate` | Date | Org expiration |

### Allocation Status Values

| Status | Meaning |
|--------|---------|
| `In Progress` | Scratch org being created |
| `Available` | Ready for fetch |
| `Allocate` | Being allocated |
| `Assigned` | Fetched by a user |
| `Return` | Being returned to pool |

### Setup Requirements

Before using pools, install the required custom fields. See [Setting up your Salesforce Org for Scratch Org Pools](../pools/setting-up-your-salesforce-org-for-scratch-org-pools.md).

## Pool Prepare Authentication

When preparing a pool, sfp:

1. **Validates DevHub authentication** - Ensures SFDX Auth URL exists
2. **Creates scratch orgs** - Uses DevHub credentials
3. **Generates passwords** - For web-based login
4. **Stores credentials** - In ScratchOrgInfo records

### Example

```bash
sfp pool prepare \
  --tag dev \
  --devhubusername devhub \
  --config-file-path config/pool-config.json
```

### What Gets Stored

For each scratch org created:

```
ScratchOrgInfo Record:
├── Pooltag__c = "dev"
├── SfdxAuthUrl__c = "force://PlatformCLI::5Aep8..@test.salesforce.com"
├── Password__c = "abc123XYZ"
├── SignupUsername = "test-xyz@example.com"
├── Allocation_status__c = "Available"
└── ExpirationDate = 2024-02-15
```

## Pool Fetch Authentication

When fetching from a pool, sfp:

1. **Queries available orgs** - Finds orgs with `Available` status
2. **Retrieves credentials** - Gets SFDX Auth URL from record
3. **Authenticates locally** - Uses the SFDX Auth URL
4. **Updates status** - Marks as `Assigned`

### Example

```bash
sfp pool fetch --tag dev --alias mydev --set-default
```

### Output

```
======== Scratch org fetched ========

Org Id          : 00D5f000000XXXXX
Login URL       : https://ability-ruby-1234-dev-ed.scratch.my.salesforce.com
Username        : test-xyz@example.com
Password        : abc123XYZ
Auth URL        : force://PlatformCLI::5Aep8...@test.salesforce.com
Expiry Date     : 2024-02-15

Use the following to authenticate and open the org:
echo force://PlatformCLI::5Aep8... > ./authfile
sf org login sfdx-url --sfdx-url-file=authfile
sf org open --target-org=test-xyz@example.com
```

The org is automatically authenticated locally - you can immediately use it:

```bash
sf org open --target-org mydev
```

## SFDX Auth URL Validation

sfp validates SFDX Auth URLs before storing them in the pool:

```typescript
// Valid formats
force://refreshToken@instance.salesforce.com
force://clientId:clientSecret:refreshToken@instance.salesforce.com
force://clientId::refreshToken@instance.salesforce.com

// Invalid - will be rejected
force://clientId:clientSecret:undefined@instance.salesforce.com
```

If a scratch org's auth URL is invalid, it won't be added to the pool.

## Security Considerations

### SFDX Auth URL Storage

The `SfdxAuthUrl__c` field contains sensitive credentials. Consider:

* **Field-level security**: Restrict access to system administrators
* **Encryption**: Enable Shield Platform Encryption if available
* **Audit**: Monitor access to ScratchOrgInfo records

### Password Generation

sfp auto-generates passwords for scratch orgs:

```bash
# Password is stored in Password__c field
# Enables web-based login without SFDX Auth URL
```

### Pool Cleanup

Expired and unused orgs should be cleaned up:

```bash
# Delete expired/orphaned orgs
sfp pool delete --tag dev --devhubusername devhub --all-scratch-orgs
```

## CI/CD Integration

### GitHub Actions Example

```yaml
jobs:
  validate:
    runs-on: ubuntu-latest
    steps:
      - name: Authenticate DevHub
        run: |
          echo "${{ secrets.DEVHUB_AUTH_URL }}" | sf org login sfdx-url --sfdx-url-stdin --alias devhub --set-default-dev-hub

      - name: Fetch Scratch Org from Pool
        run: |
          sfp pool fetch --tag ci --alias scratchorg --set-default

      - name: Validate
        run: |
          sfp validate --targetorg scratchorg --devhubusername devhub
```

### Pool Preparation Job

```yaml
jobs:
  prepare-pool:
    runs-on: ubuntu-latest
    steps:
      - name: Authenticate DevHub
        run: |
          echo "${{ secrets.DEVHUB_AUTH_URL }}" | sf org login sfdx-url --sfdx-url-stdin --alias devhub --set-default-dev-hub

      - name: Prepare Pool
        run: |
          sfp pool prepare \
            --tag ci \
            --devhubusername devhub \
            --config-file-path config/pool-config.json \
            --max-parallel 5
```

## Troubleshooting

### "Invalid SFDX Auth URL"

The DevHub authentication doesn't have a valid SFDX Auth URL:

```bash
# Re-authenticate the DevHub
sf org logout --target-org devhub
sf org login web --alias devhub --set-default-dev-hub
```

### "No scratch orgs available"

Pool is empty or all orgs are assigned:

```bash
# Check pool status
sfp pool list --tag dev --devhubusername devhub

# Prepare more orgs if needed
sfp pool prepare --tag dev --devhubusername devhub --config-file-path config/pool-config.json
```

### "Prerequisite values missing"

ScratchOrgInfo custom fields aren't set up:

```bash
# Error: Required Prerequisite values in ScratchOrgInfo is missing in the DevHub
```

Follow the [setup guide](../pools/setting-up-your-salesforce-org-for-scratch-org-pools.md) to install required fields.

### Authentication Fails After Fetch

The SFDX Auth URL may be expired or revoked:

```bash
# The scratch org may have been deleted or expired
# Fetch a different org from the pool
sfp pool fetch --tag dev --alias newdev
```

## Related Topics

* [SFDX Auth URL](sfdx-auth-url.md) - Understanding the auth URL format
* [Defining a Pool](../pools/defining-a-pool.md) - Pool configuration
* [Setting up DevHub for Pools](../pools/setting-up-your-salesforce-org-for-scratch-org-pools.md) - DevHub setup
* [Pool Operations](../pools/pool-operations/README.md) - Managing pools
