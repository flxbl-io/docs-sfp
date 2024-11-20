---
icon: ring-diamond
---

# Initializing SFP  server

|              | sfp-pro     | sfp (community) |
| ------------ | ----------- | --------------- |
| Availability | ✅           | ❌               |
| From         | December 24 |                 |

The `sfp server init` command initializes a new SFP server tenant with all necessary configurations, services, and secret management. This command sets up the complete infrastructure required for running an SFP server instance.

### Usage

```bash
sfp server init -t <tenant-name> [flags]
```

### Flags

| Flag                 | Required | Description                                                                   |
| -------------------- | -------- | ----------------------------------------------------------------------------- |
| `-t, --tenant`       | Yes      | Name of the tenant. Must contain only lowercase letters, numbers, and hyphens |
| `-m, --mode`         | No       | Environment mode: `dev` or `prod` (default: `dev`)                            |
| `-f, --force`        | No       | Force initialization even if tenant already exists                            |
| `-i, --interactive`  | No       | Enable/disable interactive mode for configuration (default: true)             |
| `-d, --domain`       | No       | Custom domain for the server. Required in prod mode                           |
| `--worker-counts`    | No       | Comma-separated worker counts for critical,normal,batch (default: "1,1,1")    |
| `--secrets-provider` | No       | Secret management provider to use (default: "infisical")                      |

### Secret Provider Options

The command supports multiple secret management providers, each with its own configuration flags:

#### Infisical

* `--infisical-token`: Authentication token for Infisical

#### Azure Key Vault

* `--keyvault-url`: URL of the Azure Key Vault
* `--keyvault-tenant-id`: Azure tenant ID
* `--keyvault-client-id`: Azure client ID
* `--keyvault-client-secret`: Azure client secret

#### AWS Secrets Manager

* `--aws-region`: AWS region
* `--aws-access-key-id`: AWS access key ID
* `--aws-secret-access-key`: AWS secret access key

#### HashiCorp Vault

* `--vault-url`: URL of the HashiCorp Vault
* `--vault-token`: Authentication token for Vault

### Examples

1. Initialize a development server:

```bash
sfp server init --tenant my-app
```

2. Initialize a production server with Infisical:

```bash
sfp server init --tenant my-app --mode prod --secrets-provider infisical --infisical-token mytoken
```

3. Initialize with Azure Key Vault:

```bash
sfp server init --tenant my-app --mode prod \
  --secrets-provider azure-keyvault \
  --keyvault-url https://myvault.vault.azure.net
```

4. Initialize with custom worker configuration:

```bash
sfp server init --tenant my-app --worker-counts 2,3,1
```

### Behavior

1. **Directory Structure**: Creates a structured directory layout under `./sfp-server/tenants/<tenant-name>` containing:
   * Configuration files
   * Secret management setup
   * Docker compose files
   * Service configurations
2. **Environment Setup**:
   * **Development Mode**: Sets up local development environment with default configurations
   * **Production Mode**: Configures production-ready setup with specified domain and secret management
3. **Services Configuration**:
   * Configures Docker services
   * Sets up database schema and migrations
   * Initializes worker processes
   * Configures reverse proxy (Caddy)
4. **Secret Management**:
   * Integrates with specified secrets provider
   * Sets up secure storage and access patterns
   * Configures necessary authentication

### Error Handling

The command will fail with appropriate error messages in the following scenarios:

* Invalid tenant name format
* Missing required provider-specific configuration
* Existing tenant without force flag
* Invalid worker count format
* Failed service initialization

### Post-Initialization

After successful initialization, the command outputs:

* Tenant URL and access details
* Available management commands
* Service status information

You can manage the initialized server using other `sfp server` commands:

```bash
sfp server logs <tenant>     # View server logs
sfp server status <tenant>   # Check server status
sfp server stop <tenant>     # Stop server
sfp server update <tenant>   # Update server configuration
```

> **Note**: For production deployments, ensure you have configured your domain DNS settings and have necessary SSL certificates before initialization.

> **Warning**: The `--force` flag will overwrite existing configurations. Use with caution in production environments.
