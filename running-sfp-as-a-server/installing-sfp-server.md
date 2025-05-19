---
icon: ring-diamond
---

# Installing SFP Server

|              | sfp-pro     | sfp (community) |
| ------------ | ----------- | --------------- |
| Availability | ✅           | ❌               |
| From         | December 24 |                 |

&#x20;

This document outlines the system requirements and infrastructure needed to host an SFP server instance. The SFP server is a containerized application that provides API endpoints for Salesforce DevOps automation.

### System Requirements

#### Hardware Requirements

| Component | Minimum  | Recommended |
| --------- | -------- | ----------- |
| CPU       | 2 cores  | 4+ cores    |
| RAM       | 4 GB     | 8+ GB       |
| Storage   | 20 GB    | 50+ GB      |
| Network   | 100 Mbps | 1 Gbps      |

#### Software Requirements

| Component        | Requirement           | Notes                                     |
| ---------------- | --------------------- | ----------------------------------------- |
| Operating System | Linux (Ubuntu 20.04+) |                                           |
| Docker           | 20.10.0+              | Required for container orchestration      |
| Docker Compose   | 2.0.0+                | Required for multi-container applications |
| Node.js          | 16.x+                 | Required for CLI tools                    |

#### Network Requirements

| Port | Service    | Description                                |
| ---- | ---------- | ------------------------------------------ |
| 443  | HTTPS      | Primary port for API access (configurable) |
| 3029 | API Server | Internal port used by the API server       |
| 6379 | Redis      | Internal port used by Redis                |

### External Dependencies

#### Supabase

The SFP server requires a Supabase instance for data storage and authentication. Please refer to the [official Supabase documentation](https://supabase.com/docs) for setup and configuration instructions. We recommend to use a managed Supabase instance. You can also self host Supabase, please ensure you provision supabase in a dedicated instance

#### Docker Registry

Access to a Docker registry is required to pull container images:

* **Default**: Uses GitHub Container Registry (ghcr.io)
* **Custom**: Can configure a private registry

### Deployment Model

The SFP server is designed to run on a single server with all components deployed as Docker containers:

* **API Server**: Handles HTTP requests and business logic
* **Worker Containers**: Process asynchronous tasks (critical, normal, batch)
* **Redis**: Manages task queues and inter-service communication
* **Caddy**: Handles HTTPS termination and proxying

This single-server deployment is suitable for most teams and workloads. The server components are managed through Docker Compose for simplified orchestration.

### Domain and SSL

For production deployments:

* **Domain Name**: Required for production mode
* **SSL Certificate**: Automatically provisioned by Caddy
* **DNS Configuration**: A record pointing to the server IP

### Networking

#### Outbound Traffic

The server requires outbound access to:

* GitHub API (api.github.com)
* Docker Registry (ghcr.io or custom)
* Supabase instance
* Salesforce APIs (login.salesforce.com, test.salesforce.com)

### Required Secrets

The SFP server requires several secrets to be configured for proper operation. These secrets are used for authentication, database access, and container image retrieval.

#### Critical Secrets

The following secrets are required for server initialization:

**Docker Registry Access**

These secrets are required to pull container images from the registry:

* `DOCKER_REGISTRY`: The Docker registry URL (default: ghcr.io)
* `DOCKER_REGISTRY_TOKEN`: Authentication token for the Docker registry with read access

**Database Access**

This secret is required for database migrations during initialization:

* `SUPABASE_DB_URL`: The PostgreSQL connection URL including credentials (format: `postgresql://username:password@host:port/database`)

**Supabase Configuration**

These secrets are required for Supabase integration:

* `SUPABASE_URL`: URL of your Supabase instance
* `SUPABASE_SERVICE_KEY`: Service key for Supabase API access
* `SUPABASE_ANON_KEY`: Anonymous key for public Supabase access
* `SUPABASE_JWT_SECRET`: JWT secret for authentication token generation
* `SUPABASE_ENCRYPTION_KEY`: Key used for encrypting sensitive data in the database

**GitHub Integration**

These secrets are required for GitHub operations:

* `GITHUB_TOKEN`: GitHub personal access token with appropriate scopes
* `GITHUB_APP_ID`: GitHub App ID (if using GitHub Apps instead of PAT)
* `GITHUB_APP_PRIVATE_KEY`: GitHub App private key (if using GitHub Apps instead of PAT)

**Authentication Configuration**

These secrets are required for authentication:

* `AUTH_USE_GLOBAL_AUTH`: Whether to use global authentication service (true/false)
* `AUTH_SUPABASE_URL`: URL for authentication Supabase instance
* `AUTH_SUPABASE_ANON_KEY`: Anonymous key for authentication Supabase instance
* `GLOBALSUPABASE_JWTSECRET`: JWT secret for global authentication (if using global auth)

#### Secrets Management Options

The server supports different methods for providing these secrets:

1. **Environment Variables** (recommended)
   * Set secrets as environment variables before running the init command
   * Example: `export DOCKER_REGISTRY=ghcr.io && export DOCKER_REGISTRY_TOKEN=your-token && sfp server init --tenant my-app --secrets-provider custom`
2. **Interactive Mode**
   * The system will prompt for required secrets during initialization
   * Example: `sfp server init --tenant my-app --interactive`
3. **Configuration File**
   * Provide secrets in a JSON configuration file
   * Example: `sfp server init --tenant my-app --config-file ./server-config.json`
4. **Infisical Integration**
   * Use Infisical for secure secrets management
   * Example: `sfp server init --tenant my-app --secrets-provider infisical --infisical-token your-token --infisical-workspace your-workspace`

### Server Initialization

The `sfp server init` command is used to set up and configure an SFP server instance. This command creates the necessary directory structure, generates configuration files, and initializes the database.

#### Basic Usage

```bash
sfp server init --tenant my-app
```

This command will:

1. Create a directory structure for your tenant at `./sfp-server/tenants/my-app`
2. Generate Docker Compose and Caddy configuration files
3. Collect required secrets (prompting if needed)
4. Initialize the Supabase database with required schema
5. Start the server containers
6. Create a default admin user

#### Command Options

**Required Parameters**

* `--tenant`: Name of the tenant (required, must be lowercase alphanumeric with hyphens)

**Environment Configuration**

* `--mode`: Server mode, either 'dev' or 'prod' (default: 'prod')
* `--domain`: Domain name for the server (required in prod mode)

**Infrastructure Configuration**

* `--worker-counts`: Number of workers for critical,normal,batch queues (comma separated, default: 1,1,1)
* `--base-dir`: Base directory for the server (default: './sfp-server')

**Secrets Configuration**

* `--secrets-provider`: Type of secrets provider to use (options: 'custom', 'infisical')
* `--infisical-token`: Authentication token for Infisical
* `--infisical-workspace`: Workspace ID in Infisical

**Other Options**

* `--interactive`: Run in interactive mode to prompt for secrets (default: false)
* `--force`: Overwrite existing tenant configuration if it exists
* `--config-file`: Path to JSON config file containing server configuration

#### Example Usage

**Development Mode**

```bash
sfp server init --tenant my-app --mode dev --interactive
```

**Production Mode with Domain**

```bash
sfp server init --tenant my-app --mode prod --domain example.com --worker-counts 2,3,1
```

**Using Configuration File**

```bash
sfp server init --tenant my-app --config-file ./server-config.json
```

Example configuration file:

```json
{
  "domain": "example.com",
  "workerCounts": "2,3,1",
  "secrets": {
    "DOCKER_REGISTRY": "ghcr.io",
    "DOCKER_REGISTRY_TOKEN": "your-token",
    "SUPABASE_DB_URL": "postgresql://postgres:password@localhost:5432/postgres",
    "SUPABASE_URL": "https://your-project.supabase.co",
    "SUPABASE_SERVICE_KEY": "your-service-key",
    "SUPABASE_ANON_KEY": "your-anon-key",
    "SUPABASE_JWT_SECRET": "your-jwt-secret",
    "SUPABASE_ENCRYPTION_KEY": "your-encryption-key",
    "GITHUB_TOKEN": "your-github-token",
    "AUTH_USE_GLOBAL_AUTH": "false",
    "AUTH_SUPABASE_URL": "https://your-project.supabase.co",
    "AUTH_SUPABASE_ANON_KEY": "your-anon-key",
  }
}
```

