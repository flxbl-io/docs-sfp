# Connected Apps

A Connected App is the OAuth2 client configuration in Salesforce that enables external applications like sfp to authenticate and access Salesforce APIs. Understanding Connected Apps helps you control and secure how authentication works in your organization.

## What is a Connected App?

A Connected App defines:
* **Client credentials** (Client ID and Client Secret)
* **OAuth scopes** (what the app can access)
* **Security policies** (IP restrictions, session timeout)
* **Callback URLs** (where to redirect after authentication)

When you run `sf org login web`, Salesforce asks you to authorize a Connected App to access your org.

## The Default Connected App

sfp uses the Salesforce CLI's default Connected App, called **"Salesforce CLI"** (also known as `PlatformCLI`). This Connected App is:

* Pre-configured in all Salesforce orgs
* Managed by Salesforce
* Has broad API access scopes
* No setup required

### Why sfp Uses the Default Connected App

Rather than requiring customers to create and maintain their own Connected Apps, sfp leverages the existing Salesforce CLI infrastructure:

```
┌─────────────────────────────────────────────────────────────────┐
│                    Authentication Flow                          │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│   User                Salesforce CLI            Salesforce      │
│    │                       │                        │           │
│    │  sf org login web     │                        │           │
│    │──────────────────────>│                        │           │
│    │                       │   OAuth Request        │           │
│    │                       │   (PlatformCLI)        │           │
│    │                       │───────────────────────>│           │
│    │                       │                        │           │
│    │        Browser opens for authorization         │           │
│    │<───────────────────────────────────────────────│           │
│    │                       │                        │           │
│    │        User authorizes                         │           │
│    │───────────────────────────────────────────────>│           │
│    │                       │                        │           │
│    │                       │   Tokens returned      │           │
│    │                       │<───────────────────────│           │
│    │                       │                        │           │
│    │   Auth URL stored     │                        │           │
│    │<──────────────────────│                        │           │
│                                                                 │
│   sfp then uses these credentials via @salesforce/core          │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

### Benefits of Using the Default App

* **Zero configuration**: Works out of the box
* **Automatic updates**: Salesforce maintains security patches
* **Proven reliability**: Used by millions of developers
* **Consistent behavior**: Same experience across all orgs

## When to Use a Custom Connected App

While the default Connected App works for most scenarios, you may want a custom Connected App for:

| Scenario | Reason |
|----------|--------|
| **IP Restrictions** | Limit authentication to specific IP ranges |
| **Custom Session Policies** | Shorter session timeouts for security |
| **Audit Requirements** | Track usage separately from CLI activity |
| **JWT Bearer Flow** | Headless CI/CD without user interaction |
| **Limited Scopes** | Restrict API access to specific operations |

## Creating a Custom Connected App

### Step 1: Create the Connected App

1. Navigate to **Setup > App Manager**
2. Click **New Connected App**
3. Configure basic settings:
   * **Connected App Name**: `sfp-automation`
   * **API Name**: `sfp_automation`
   * **Contact Email**: Your email

### Step 2: Configure OAuth Settings

Enable OAuth and configure:

```
✓ Enable OAuth Settings

Callback URL:
  http://localhost:1717/OauthRedirect

Selected OAuth Scopes:
  - Access the identity URL service (id, profile, email, address, phone)
  - Manage user data via APIs (api)
  - Perform requests at any time (refresh_token, offline_access)
  - Access unique user identifiers (openid)
```

{% hint style="info" %}
The `refresh_token` scope is essential for generating SFDX Auth URLs that work in automation.
{% endhint %}

### Step 3: Configure Security Policies (Optional)

Under **OAuth Policies**:

* **Permitted Users**: Admin approved users are pre-authorized
* **IP Relaxation**: Enforce IP restrictions
* **Refresh Token Policy**: Configure token expiration

### Step 4: Retrieve Client Credentials

After saving, click **Manage Consumer Details** to view:
* **Consumer Key** (Client ID)
* **Consumer Secret**

## Using a Custom Connected App

### Web-based Flow

```bash
# Set environment variables
export SFDX_CLIENT_ID=your_client_id
export SFDX_CLIENT_SECRET=your_client_secret

# Authenticate
sf org login web --client-id $SFDX_CLIENT_ID
```

### JWT Bearer Flow (CI/CD)

The JWT Bearer Flow is ideal for CI/CD as it doesn't require user interaction:

1. **Generate a certificate**:
   ```bash
   openssl genrsa -out server.key 2048
   openssl req -new -x509 -sha256 -key server.key -out server.crt -days 365
   ```

2. **Upload certificate to Connected App**:
   * Edit the Connected App
   * Check **Use digital signatures**
   * Upload `server.crt`

3. **Pre-authorize the user**:
   * Go to **Setup > Manage Connected Apps**
   * Find your app and click **Manage**
   * Click **Edit Policies**
   * Set **Permitted Users** to "Admin approved users are pre-authorized"
   * Add profiles/permission sets

4. **Authenticate with JWT**:
   ```bash
   sf org login jwt \
     --client-id $SFDX_CLIENT_ID \
     --jwt-key-file ./server.key \
     --username admin@myorg.com \
     --alias myOrg
   ```

### SFDX Auth URL with Custom App

When using a custom Connected App, the SFDX Auth URL includes your client credentials:

```
force://YOUR_CLIENT_ID:YOUR_CLIENT_SECRET:REFRESH_TOKEN@instance.salesforce.com
```

## Connected App in SFDX Auth URLs

The SFDX Auth URL format reveals which Connected App was used:

```
force://<clientId>:<clientSecret>:<refreshToken>@<instanceUrl>
         ─────────
            │
            └─── "PlatformCLI" = Default Salesforce CLI app
                 Custom ID = Your Connected App
```

### Examples

**Default Connected App:**
```
force://PlatformCLI::5Aep861_XXXXX@login.salesforce.com
```

**Custom Connected App:**
```
force://3MVG9d8...:7EF234...:5Aep861_XXXXX@login.salesforce.com
```

## Security Best Practices

### For the Default Connected App

* Rely on user-level security (profiles, permission sets)
* Use Salesforce's session management to revoke access if needed
* Monitor login history in Setup

### For Custom Connected Apps

* **Rotate secrets periodically**: Regenerate the Consumer Secret
* **Use IP restrictions**: Limit to known CI/CD IP ranges
* **Minimize scopes**: Only request necessary permissions
* **Set session policies**: Appropriate timeout values
* **Monitor usage**: Review Connected App Usage reports

### JWT Flow Security

* **Protect the private key**: Store in secure secret management
* **Use short-lived certificates**: Rotate annually
* **Pre-authorize specific users**: Don't use "All users may self-authorize"

## Troubleshooting

### "Connected App not found"

* Ensure the Connected App is created in the correct org
* For sandbox authentication, the Connected App must exist in production (for JWT flow)

### "Invalid client credentials"

* Verify the Client ID and Secret
* Check if the Consumer Secret was regenerated

### "Refresh token expired"

* Check the Refresh Token Policy on the Connected App
* Re-authenticate to get a new refresh token

### "IP blocked"

* Verify your IP is in the allowed ranges
* Check **Trusted IP Ranges** on the Connected App

## Related Topics

* [SFDX Auth URL](sfdx-auth-url.md) - Understanding the auth URL format
* [Org Authentication](org-authentication.md) - Different authentication methods
* [Server Authentication](server-authentication.md) - Authentication with sfp-server (Pro)
