# Start

## `sfp server start`

Start a tenant's services

### `sfp server start`

Start or restart the services for a specified tenant.

```
USAGE
  $ sfp server start -t <value> [-j] [--daemon] [--no-browser] [--base-dir
    <value>] [-r] [--config-file <value>] [--passphrase <value>
    [--identity-file <value> --ssh-connection <value>]] [--infisical-token
    <value> --secrets-provider infisical|aws-secretsmanager|custom]
    [--aws-region <value>] [--aws-access-key-id <value>]
    [--aws-secret-access-key <value>] [-g <value>...] [--loglevel
    trace|debug|info|warn|error|fatal|TRACE|DEBUG|INFO|WARN|ERROR|FATAL]

FLAGS
  -t, --tenant=<value>              (required) Name of the tenant to start
  -j, --[no-]json                   Output in JSON format
  -r, --restart                     Restart server if already running
  --daemon                          Run server in daemon mode (detached)
  --no-browser                      Disable automatic browser opening
  --base-dir=<value>                [default: ./sfp-server] Base Directory which contains the sfp-server
  --config-file=<value>             Path to JSON config file containing server configuration values
  
  SSH OPTIONS
  --ssh-connection=<value>          SSH connection string in the format user@host[:port]
  --identity-file=<value>           Path to SSH private key file
  --passphrase=<value>              Passphrase for the SSH private key if required
  
  SECRETS MANAGEMENT
  --secrets-provider=<option>       [default: custom] Secret provider to use for managing secrets
                                    <options: infisical|aws-secretsmanager|custom>
  --infisical-token=<value>         Infisical API token (required when secrets-provider is "infisical")
  --aws-region=<value>              AWS region for Secrets Manager (required when secrets-provider is "aws-secretsmanager")
  --aws-access-key-id=<value>       AWS access key ID (optional, can use instance profile)
  --aws-secret-access-key=<value>   AWS secret access key (optional, can use instance profile)
  
  OTHER OPTIONS
  -g, --logsgroupsymbol=<value>...  Symbol used by CICD platform to group/collapse logs
  --loglevel=<option>               [default: info] logging level for this command invocation
                                    <options: trace|debug|info|warn|error|fatal|TRACE|DEBUG|INFO|WARN|ERROR|FATAL>

DESCRIPTION
  Start a tenant's services

  Secrets Management:
  This command supports multiple options for secrets management:
  - infisical: Use Infisical as a dedicated secrets manager
  - aws-secretsmanager: Use AWS Secrets Manager
  - custom: Use environment variables (recommended when using tools like "infisical run" or AWS CLI)

  For custom secrets provider, inject secrets as environment variables before running the command.

EXAMPLES
  $ sfp server start --tenant my-app

  $ sfp server start --tenant my-app --daemon --no-browser

  $ sfp server start --tenant my-app --restart

  $ sfp server start --tenant my-app --ssh-connection user@example.com --identity-file ~/.ssh/id_rsa

  $ sfp server start --tenant my-app --secrets-provider custom

  $ infisical run -- sfp server start --tenant my-app --secrets-provider custom

  $ sfp server start --tenant my-app --secrets-provider infisical --infisical-token your-token

  $ sfp server start --tenant my-app --secrets-provider aws-secretsmanager --aws-region us-east-1
```

### Modes of Operation

#### Daemon Mode
Run the server in the background (detached):
```bash
sfp server start --tenant my-app --daemon
```

#### Interactive Mode
Run the server in the foreground (default):
```bash
sfp server start --tenant my-app
```

### Secrets Management

#### Custom Provider (Default)
Use environment variables injected by external tools:
```bash
# Using Infisical CLI
infisical run -- sfp server start --tenant my-app --secrets-provider custom

# Using AWS CLI with SSM Parameter Store
aws ssm get-parameters-by-path --path /sfp/my-app --recursive | \
  jq -r '.Parameters[] | "export \(.Name | split("/") | last)=\(.Value)"' | \
  source /dev/stdin && \
  sfp server start --tenant my-app --secrets-provider custom
```

#### Infisical Provider
Direct integration with Infisical:
```bash
sfp server start --tenant my-app \
  --secrets-provider infisical \
  --infisical-token your-token
```

#### AWS Secrets Manager
Direct integration with AWS Secrets Manager:
```bash
sfp server start --tenant my-app \
  --secrets-provider aws-secretsmanager \
  --aws-region us-east-1
```

### Remote Server Management

Start a tenant on a remote server via SSH:
```bash
sfp server start --tenant my-app \
  --ssh-connection admin@production.example.com:2222 \
  --identity-file ~/.ssh/production_key
```

> **Note**: When using `--restart`, the server will be stopped and started again if it's already running.

> **Tip**: Use `--daemon` mode for production deployments to run the server in the background.