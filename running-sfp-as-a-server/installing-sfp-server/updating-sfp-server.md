# Updating SFP Server



This guide shows you how to set up automated server management and updates for your self-hosted SFP server instance using a ready-to-use management repository. Simply clone the template repository and configure it for your environment.



### Prerequisites

Before setting up automated server management, ensure you have:

* ✅ A running SFP server instance (see Installation Guide)
* ✅ GitHub account with repository creation permissions
* ✅ SSH access to your SFP server
* ✅ Access token from `source.flxbl.io` (your Gitea instance)
* ✅ Basic familiarity with GitHub Actions

### Step 1: Clone the SFP Server Management Template

#### Get the Template Repository

The SFP team provides a ready-to-use server management repository template that includes all necessary GitHub Actions workflows and configurations for updating your SFP server.

1.  **Clone the Template Repository**

    ```bash
    git clone  https://source.flxbl.io/flxbl/sfp-server-mangement-template.git
    cd sfp-server-management-template
    ```
2.  **Create Your Own Management Repository**

    * Create a new private repository in your GitHub organization
    * Name it something like `sfp-server-management` or `our-sfp-server-updates`
    * Push the cloned template to your new repository:

    ```bash
    git remote set-url origin https://github.com/your-org/sfp-server-management.git
    git push -u origin main
    ```

#### Repository Structure

The management template includes:

```
sfp-server-management/
├── .github/
│   └── workflows/
│       └── deploy.yml          # Server update workflow
├── README.md                   # Setup and usage instructions
└── .gitignore                  # Git ignore rules
```

### Step 2: Configure Repository Secrets

Navigate to your repository's Settings > Secrets and Variables > Actions, and add the following secrets:

#### Required Secrets

| Secret Name       | Description                       | Example                                  |
| ----------------- | --------------------------------- | ---------------------------------------- |
| `GITEA_TOKEN`     | Access token from source.flxbl.io | `xxxxxxxxxxxxxxxxxxxx`                   |
| `SSH_PRIVATE_KEY` | Private SSH key for server access | `-----BEGIN OPENSSH PRIVATE KEY-----...` |
| `SSH_HOST`        | Server hostname or IP address     | `sfp-server.company.com`                 |
| `SSH_USER`        | SSH username                      | `ubuntu`                                 |
| `TENANT_NAME`     | Your SFP server tenant name       | `company-sfp`                            |

#### Optional Secrets

| Secret Name | Description              | Default |
| ----------- | ------------------------ | ------- |
| `SSH_PORT`  | SSH port if not standard | `22`    |

#### How to Generate Secrets

**Gitea Token (source.flxbl.io)**

1. Log in to your `source.flxbl.io` instance
2. Go to Settings > Applications > Generate New Token
3. Select scopes: `read:packages`
4. Copy the generated token

**SSH Private Key**

Since you already have SSH access to your SFP server from the initial setup, use your existing SSH private key:

```bash
# Copy your existing private key content for GitHub secret
cat ~/.ssh/id_rsa
# or if you use a different key file
cat ~/.ssh/your-server-key
```

> **Note**: Use the same private key you used during server installation and management.

### Step 3: Understanding the GitHub Actions Workflow

The template repository includes a pre-configured workflow at `.github/workflows/deploy.yml` with the following key features:

#### Update Options

The workflow provides two main deployment types:

1. **Update to Latest** (`update-latest`)
   * Automatically pulls the newest stable SFP server version
   * Recommended for regular maintenance updates
2. **Update to Specific Version** (`deploy-specific`)
   * Deploy a specific version tag (e.g., `v48.3.0`)
   * Useful for controlled deployments or rollbacks to known versions

### Step 4: How to Execute Server Updates

#### Running Updates from GitHub

1. **Navigate to Your Management Repository**
   * Go to your `sfp-server-management` repository on GitHub
   * Click on the "Actions" tab
2. **Start a Server Update**
   * Click on "SFP Server Update" workflow
   * Click "Run workflow" button
   * Choose your update options:
     * **Update to Latest**: Automatically updates to the newest stable version
     * **Update to Specific Version**: Choose a specific version tag (e.g., v48.3.0)
     * **CLI Version**: Optionally specify SFP CLI version to use
3. **Monitor Progress**
   * Watch the workflow execution in real-time
   * Review health checks and update status
   * Get detailed deployment summary
   * Receive notifications on success or failure

### Step 5: When to Execute Updates

#### Regular Update Schedule

**Recommended Approach:**

* **Monthly Updates**: Check for new stable releases monthly
* **Security Updates**: Apply security updates within 48 hours
* **Major Releases**: Plan and test major version upgrades during maintenance windows

#### Update Triggers

1.  **Planned Updates**

    ```
    Trigger: Manual (workflow_dispatch)
    Type: update-latest
    Timing: During maintenance windows
    ```
2.  **Specific Version Deployment**

    ```
    Trigger: Manual (workflow_dispatch)
    Type: deploy-specific
    Version: Specific version tag (e.g., v48.3.0)
    ```

#### Best Practices

**Before Updates**

* [ ] Check [SFP Release Notes](https://github.com/flxbl-io/sfp/releases) for breaking changes
* [ ] Notify team members of planned maintenance
* [ ] Verify backup retention policy
* [ ] Ensure maintenance window availability

**During Updates**

* [ ] Monitor workflow progress in GitHub Actions
* [ ] Watch server logs during deployment
* [ ] Verify health checks pass
* [ ] Test critical functionality

**After Updates**

* [ ] Confirm all services are running
* [ ] Test key integrations
* [ ] Update internal documentation
* [ ] Clean up old backups

### Database Migration Considerations

#### Understanding Migration Limitations

**Important**: Database migrations are **forward-only** operations. When you update to a newer version of SFP server, database schema changes are applied that cannot be automatically reversed.

**Migration Failure Scenarios**

1. **Schema Conflicts**: Migration failures occur when manual database changes conflict with expected schema
2. **Data Inconsistency**: Direct database modifications may conflict with migration expectations
3. **Version Conflicts**: Attempting to apply migrations out of sequence or on modified schemas

**Manual Recovery Procedures**

If you encounter database issues after a rollback:

1.  **Check Server Logs**:

    ```bash
    sfp server logs --tenant my-tenant --tail 100
    ```
2.  **Manual Database Restoration** (if needed):

    ```bash
    # Restore from backup
    psql -h your-db-host -U username -d database_name < backup.sql

    # Restart services
    sfp server restart --tenant my-tenant
    ```
3. **Contact Support**: For complex migration issues, contact support with:
   * Server logs
   * Database error messages
   * Version numbers (before/after)
   * Migration failure details

### Troubleshooting

#### Common Issues

**Authentication Errors**

```
Error: Authentication failed with source.flxbl.io
```

**Solution:** Verify `GITEA_TOKEN` secret is valid and has `read:packages` scope.

**SSH Connection Issues**

```
Error: Permission denied (publickey)
```

**Solutions:**

* Verify SSH private key format in `SSH_PRIVATE_KEY` secret
* Ensure public key is added to server's `authorized_keys`
* Check SSH\_USER and SSH\_HOST values

**Version Mismatch**

```
Error: Version 'v1.2.3' not found in registry
```

**Solution:** Check available versions in `source.flxbl.io` registry or use `latest`.

**Database Migration Failures**

```
Error: Database migration failed: column "new_field" already exists
```

**Solutions:**

* Check for conflicting manual database changes
* Restore from backup if database is corrupted
* Review migration changes in release notes before updating

### Advanced Configuration

#### Multi-Environment Support

For organizations with multiple SFP server instances:

```yaml
strategy:
  matrix:
    environment: [staging, production]
    include:
      - environment: staging
        tenant: company-sfp-staging
        host: staging.sfp.company.com
      - environment: production  
        tenant: company-sfp-prod
        host: sfp.company.com
```

***

> **Tip**: Start with test deployments before using in production. The workflow includes safety features, but testing your specific environment is recommended.
