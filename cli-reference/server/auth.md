# Auth

## `sfp server auth`

Authenticate with and manage authentication for the SFP server

### Commands

* [`sfp server auth login`](#sfp-server-auth-login) - Authenticate with the SFP server
* [`sfp server auth display`](#sfp-server-auth-display) - Display authentication token information
* [`sfp server auth list`](#sfp-server-auth-list) - List all auth tokens stored locally
* [`sfp server auth clear`](#sfp-server-auth-clear) - Clear all local authentication tokens

---

### `sfp server auth login`

Authenticate with the SFP server using various authentication strategies. This creates a JWT token stored securely in the keychain for subsequent commands.

```
USAGE
  $ sfp server auth login [--json] [--sfp-server-url <value>] [-e <value>]
    [-p <value>] [--strategy email|oauth] [-g <value>...] [--loglevel
    trace|debug|info|warn|error|fatal|TRACE|DEBUG|INFO|WARN|ERROR|FATAL]

FLAGS
  -e, --email=<value>               Email address for authentication
  -p, --password=<value>            Password for email authentication
  --strategy=<option>               [default: email] Authentication strategy to use
                                    <options: email|oauth>
  --sfp-server-url=<value>          URL of the SFP server
  --json                            Format output as json
  -g, --logsgroupsymbol=<value>...  Symbol used by CICD platform to group/collapse logs
  --loglevel=<option>               [default: info] logging level for this command invocation
                                    <options: trace|debug|info|warn|error|fatal|TRACE|DEBUG|INFO|WARN|ERROR|FATAL>

DESCRIPTION
  Authenticate with the SFP server

  Authentication tokens are stored securely in your system's keychain/credential manager
  and are automatically used for subsequent SFP server commands.

EXAMPLES
  $ sfp server auth login --email user@example.com

  $ sfp server auth login --strategy oauth

  $ sfp server auth login --email user@example.com --password mypassword

  $ sfp server auth login --sfp-server-url https://sfp.example.com --email user@example.com
```

#### Authentication Strategies

**Email Authentication** (Default):
```bash
# Interactive password prompt
sfp server auth login --email user@example.com

# With password (not recommended for security reasons)
sfp server auth login --email user@example.com --password yourpassword
```

**OAuth Authentication**:
```bash
sfp server auth login --strategy oauth
```
This will open your browser for OAuth authentication flow.

---

### `sfp server auth display`

Display information about the current authentication token.

```
USAGE
  $ sfp server auth display [--json] [-e <value>] [--sfp-server-url <value>]
    [-g <value>...] [--loglevel
    trace|debug|info|warn|error|fatal|TRACE|DEBUG|INFO|WARN|ERROR|FATAL]

FLAGS
  -e, --email=<value>               Email address to display token for
  --sfp-server-url=<value>          URL of the SFP server
  --json                            Format output as json
  -g, --logsgroupsymbol=<value>...  Symbol used by CICD platform to group/collapse logs
  --loglevel=<option>               [default: info] logging level for this command invocation
                                    <options: trace|debug|info|warn|error|fatal|TRACE|DEBUG|INFO|WARN|ERROR|FATAL>

DESCRIPTION
  Display authentication token information

  Shows details about the stored authentication token including:
  - User email
  - Token expiration
  - Server URL
  - Token validity status

EXAMPLES
  $ sfp server auth display

  $ sfp server auth display --email user@example.com

  $ sfp server auth display --json
```

---

### `sfp server auth list`

List all authentication tokens stored locally.

```
USAGE
  $ sfp server auth list [--json] [-g <value>...] [--loglevel
    trace|debug|info|warn|error|fatal|TRACE|DEBUG|INFO|WARN|ERROR|FATAL]

FLAGS
  --json                            Format output as json
  -g, --logsgroupsymbol=<value>...  Symbol used by CICD platform to group/collapse logs
  --loglevel=<option>               [default: info] logging level for this command invocation
                                    <options: trace|debug|info|warn|error|fatal|TRACE|DEBUG|INFO|WARN|ERROR|FATAL>

DESCRIPTION
  List all auth tokens of users and their status stored locally

  Displays a list of all stored authentication tokens with:
  - User email
  - Server URL
  - Token status (valid/expired)
  - Expiration date

EXAMPLES
  $ sfp server auth list

  $ sfp server auth list --json
```

Output example:
```
Authentication Tokens:
  
  user@example.com (https://sfp.example.com)
    Status: Valid
    Expires: 2024-02-15T10:30:00Z
    
  admin@company.com (https://sfp-staging.company.com)
    Status: Expired
    Expired: 2024-01-10T15:45:00Z
```

---

### `sfp server auth clear`

Clear all locally stored authentication tokens.

```
USAGE
  $ sfp server auth clear [--json] [-e <value>] [--all] [-g <value>...]
    [--loglevel
    trace|debug|info|warn|error|fatal|TRACE|DEBUG|INFO|WARN|ERROR|FATAL]

FLAGS
  -e, --email=<value>               Clear token for specific email address
  --all                             Clear all stored tokens
  --json                            Format output as json
  -g, --logsgroupsymbol=<value>...  Symbol used by CICD platform to group/collapse logs
  --loglevel=<option>               [default: info] logging level for this command invocation
                                    <options: trace|debug|info|warn|error|fatal|TRACE|DEBUG|INFO|WARN|ERROR|FATAL>

DESCRIPTION
  Clear all the local tokens created by sfp server auth login command

  This removes authentication tokens from your system's secure storage.
  You will need to authenticate again to use SFP server commands.

EXAMPLES
  $ sfp server auth clear --all

  $ sfp server auth clear --email user@example.com

  $ sfp server auth clear --all --json
```

### Best Practices

1. **Use OAuth when available**: OAuth provides better security than email/password authentication
2. **Avoid hardcoding passwords**: Use interactive prompts or secure environment variables
3. **Regularly rotate tokens**: Clear and re-authenticate periodically for security
4. **Check token validity**: Use `auth display` to verify token status before operations

### Token Storage

Authentication tokens are stored in:
- **macOS**: Keychain Access
- **Windows**: Windows Credential Manager
- **Linux**: Secret Service API (libsecret)

> **Note**: Tokens are stored securely and are not accessible in plain text.

> **Security**: Never share authentication tokens or store them in version control.