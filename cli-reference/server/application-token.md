# Application Token

## `sfp server application-token`

Manage application tokens for programmatic access to the SFP server

### Commands

* [`sfp server application-token create`](#sfp-server-application-token-create) - Create a new application token
* [`sfp server application-token list`](#sfp-server-application-token-list) - List all application tokens
* [`sfp server application-token revoke`](#sfp-server-application-token-revoke) - Revoke an application token

---

### `sfp server application-token create`

Create a new application token for CI/CD and automation use.

```
USAGE
  $ sfp server application-token create -n <value> [--json] [-x <value>] [--sfp-server-url
    <value>] [-e <value>] [-g <value>...] [--loglevel
    trace|debug|info|warn|error|fatal|TRACE|DEBUG|INFO|WARN|ERROR|FATAL]

FLAGS
  -n, --name=<value>                (required) Name of the token
  -x, --expires-in=<value>          [default: 30] Token expiration time in days
  -e, --email=<value>               Email address for the authenticated CLI user
  --sfp-server-url=<value>          URL of the SFP server
  --json                            Format output as json
  -g, --logsgroupsymbol=<value>...  Symbol used by CICD platform to group/collapse logs
  --loglevel=<option>               [default: info] logging level for this command invocation
                                    <options: trace|debug|info|warn|error|fatal|TRACE|DEBUG|INFO|WARN|ERROR|FATAL>

DESCRIPTION
  Create a new application token

  Application tokens are used for:
  - CI/CD pipeline authentication
  - Automated scripts and tools
  - Service-to-service communication
  - Non-interactive authentication scenarios

EXAMPLES
  $ sfp server application-token create --name "CI Token" --expires-in 30

  $ sfp server application-token create --name "GitHub Actions" --expires-in 90 --email admin@example.com

  $ sfp server application-token create --name "Jenkins Build" --expires-in 7

  $ sfp server application-token create --name "Production Deploy" --expires-in 365 --json
```

#### Token Creation Process

1. **Create the token**:
```bash
sfp server application-token create --name "GitHub Actions" --expires-in 90
```

2. **Save the token securely**:
```
Application token created successfully!

Token: sfp_app_1234567890abcdef...
Name: GitHub Actions
Expires: 2024-04-15T10:30:00Z

⚠️  Save this token securely. It cannot be retrieved again.
```

3. **Use in CI/CD**:
```yaml
# GitHub Actions example
env:
  SFP_APPLICATION_TOKEN: ${{ secrets.SFP_TOKEN }}
```

---

### `sfp server application-token list`

List all application tokens associated with your account.

```
USAGE
  $ sfp server application-token list [--json] [--sfp-server-url <value>] [-e <value>]
    [-g <value>...] [--loglevel
    trace|debug|info|warn|error|fatal|TRACE|DEBUG|INFO|WARN|ERROR|FATAL]

FLAGS
  -e, --email=<value>               Email address for the authenticated CLI user
  --sfp-server-url=<value>          URL of the SFP server
  --json                            Format output as json
  -g, --logsgroupsymbol=<value>...  Symbol used by CICD platform to group/collapse logs
  --loglevel=<option>               [default: info] logging level for this command invocation
                                    <options: trace|debug|info|warn|error|fatal|TRACE|DEBUG|INFO|WARN|ERROR|FATAL>

DESCRIPTION
  List all application tokens

  Shows:
  - Token name
  - Creation date
  - Expiration date
  - Status (active/expired)
  - Last used date

EXAMPLES
  $ sfp server application-token list

  $ sfp server application-token list --json

  $ sfp server application-token list --email admin@example.com
```

Output example:
```
Application Tokens:

  CI Token
    ID: tok_abc123
    Created: 2024-01-01T10:00:00Z
    Expires: 2024-01-31T10:00:00Z
    Status: Active
    Last Used: 2024-01-15T14:30:00Z

  GitHub Actions
    ID: tok_def456
    Created: 2023-12-01T09:00:00Z
    Expires: 2024-03-01T09:00:00Z
    Status: Active
    Last Used: 2024-01-15T16:45:00Z

  Jenkins Build (Expired)
    ID: tok_ghi789
    Created: 2023-11-01T08:00:00Z
    Expired: 2023-11-08T08:00:00Z
    Status: Expired
```

---

### `sfp server application-token revoke`

Revoke an existing application token.

```
USAGE
  $ sfp server application-token revoke -i <value> [--json] [--sfp-server-url <value>]
    [-e <value>] [-g <value>...] [--loglevel
    trace|debug|info|warn|error|fatal|TRACE|DEBUG|INFO|WARN|ERROR|FATAL]

FLAGS
  -i, --id=<value>                  (required) ID of the token to revoke
  -e, --email=<value>               Email address for the authenticated CLI user
  --sfp-server-url=<value>          URL of the SFP server
  --json                            Format output as json
  -g, --logsgroupsymbol=<value>...  Symbol used by CICD platform to group/collapse logs
  --loglevel=<option>               [default: info] logging level for this command invocation
                                    <options: trace|debug|info|warn|error|fatal|TRACE|DEBUG|INFO|WARN|ERROR|FATAL>

DESCRIPTION
  Revoke an application token

  Once revoked, the token cannot be used for authentication.
  This action is immediate and cannot be undone.

EXAMPLES
  $ sfp server application-token revoke --id tok_abc123

  $ sfp server application-token revoke --id tok_def456 --json

  $ sfp server application-token revoke --id tok_ghi789 --email admin@example.com
```

### Use Cases

#### CI/CD Pipeline Integration

**GitHub Actions**:
```yaml
name: Deploy to Production
on:
  push:
    branches: [main]

jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2
      - name: Deploy using SFP
        env:
          SFP_APPLICATION_TOKEN: ${{ secrets.SFP_APP_TOKEN }}
          SFP_SERVER_URL: https://sfp.example.com
        run: |
          sfp release --environment production
```

**Jenkins**:
```groovy
pipeline {
    agent any
    environment {
        SFP_APPLICATION_TOKEN = credentials('sfp-app-token')
        SFP_SERVER_URL = 'https://sfp.example.com'
    }
    stages {
        stage('Deploy') {
            steps {
                sh 'sfp release --environment staging'
            }
        }
    }
}
```

#### Automated Scripts

```bash
#!/bin/bash
# automated-deployment.sh

export SFP_APPLICATION_TOKEN="sfp_app_1234567890abcdef..."
export SFP_SERVER_URL="https://sfp.example.com"

# Run deployment
sfp build --branch main
sfp publish
sfp release --environment production
```

### Security Best Practices

1. **Token Rotation**:
   - Regularly rotate tokens (every 30-90 days)
   - Use shorter expiration for high-privilege tokens

2. **Secure Storage**:
   - Store tokens in secure vaults (HashiCorp Vault, AWS Secrets Manager)
   - Never commit tokens to version control
   - Use environment variables or secret management systems

3. **Principle of Least Privilege**:
   - Create separate tokens for different purposes
   - Limit token scope when possible

4. **Monitoring**:
   - Regularly review token usage with `list` command
   - Revoke unused tokens promptly
   - Monitor for suspicious activity

### Token Management Workflow

```bash
# 1. Create token for CI/CD
sfp server application-token create --name "Production CI" --expires-in 90

# 2. List and monitor tokens
sfp server application-token list

# 3. Revoke when no longer needed
sfp server application-token revoke --id tok_abc123
```

> **Security Warning**: Application tokens have the same permissions as the user who created them. Treat them as passwords and protect accordingly.

> **Note**: Tokens are automatically revoked upon expiration. Set appropriate expiration times based on your security requirements.