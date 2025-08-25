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

---

## Quick Start Guide

This guide provides a streamlined path to get your SFP Server up and running quickly on any Linux server (AWS EC2, Hetzner, DigitalOcean, etc.).

### Prerequisites Checklist

Before starting, ensure you have:

- [ ] **Linux Server** (Ubuntu 20.04+ or similar) with:
  - Minimum 4GB RAM, 2 vCPUs
  - Docker 20.10+ and Docker Compose 2.0+ installed
  - SSH access configured
  - Public IP address
  
  **Quick check**: `ssh your-server "free -h | grep Mem && nproc && docker --version && docker compose version"`

- [ ] **Supabase Instance** with:
  - Project URL (e.g., https://xxxxx.supabase.co)
  - Service Key (for API access)
  - Anon Key (for public access) 
  - JWT Secret (from project settings)
  - Database URL (PostgreSQL connection string)

  **Setup options**: [Supabase Cloud](https://supabase.com) or [Self-Hosted Guide](./self-hosted-supabase-configuration.md)
  
  **How to get these values from Supabase Dashboard**:
  1. Go to your project → **Settings** → **API**
     - **Project URL**: Copy the URL under "Project URL"
     - **Anon Key**: Copy from "Project API keys" → anon/public
     - **Service Key**: Copy from "Project API keys" → service_role (keep secret!)
  2. Go to **Settings** → **Database**
     - **Database URL**: Copy the connection string from "Connection string" → URI
  3. **JWT Secret**: Go to **Settings** → **API** → Scroll to "JWT Settings" → Copy the JWT Secret
  
  **Quick check**: `curl -s -o /dev/null -w "%{http_code}" YOUR_SUPABASE_URL/rest/v1/ -H "apikey: YOUR_ANON_KEY" # Should return 200`

- [ ] **GitHub App** configured with:
  - App ID
  - Private Key (.pem file)
  - Appropriate repository permissions

- [ ] **Domain Name** (for production):
  - DNS A record pointing to server IP
  - e.g., `sfp.yourcompany.com`

- [ ] **Local Machine** with:
  - sfp-pro CLI installed
  - SSH key for server access

### Step-by-Step Setup

#### Step 1: Generate Security Keys

Generate required secrets on your local machine:

```bash
# Generate JWT Secret (save this value)
openssl rand -hex 32

# Generate Encryption Key (save this value)
openssl rand -base64 32
```

#### Step 2: Create Configuration File

Create `server.json` on your local machine. This file provides all the secrets needed for the SFP server to:
- Pull Docker images from the registry
- Connect to your Supabase database  
- Authenticate with GitHub
- Configure HTTPS domain and worker processes

```json
{
  "domain": "sfp.yourcompany.com",
  "workerCounts": "1,2,1",
  "secrets": {
    "DOCKER_REGISTRY": "ghcr.io",
    "DOCKER_REGISTRY_TOKEN": "ghp_xxxxxxxxxxxx",
    "SUPABASE_DB_URL": "postgresql://postgres:password@db.project.supabase.co:5432/postgres",
    "SUPABASE_URL": "https://project.supabase.co",
    "SUPABASE_SERVICE_KEY": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
    "SUPABASE_ANON_KEY": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
    "SUPABASE_JWT_SECRET": "your-jwt-secret-from-step-1",
    "SUPABASE_ENCRYPTION_KEY": "your-encryption-key-from-step-1",
    "GITHUB_TOKEN": "your-github-token",
    "GITHUB_APP_ID": "123456",
    "GITHUB_APP_PRIVATE_KEY": "-----BEGIN RSA PRIVATE KEY-----\nMIIEpAIBAAKCAQ...\n-----END RSA PRIVATE KEY-----",
    "AUTH_USE_GLOBAL_AUTH": "false",
    "AUTH_SUPABASE_URL": "https://project.supabase.co",
    "AUTH_SUPABASE_ANON_KEY": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..."
  }
}
```

**Where to get each value:**

- **DOCKER_REGISTRY_TOKEN**: GitHub Personal Access Token with `read:packages` scope from [GitHub Settings → Developer settings → Personal access tokens](https://github.com/settings/tokens)
- **SUPABASE_DB_URL**: Supabase Dashboard → Settings → Database → Connection string → URI (use the full PostgreSQL connection string)
- **SUPABASE_URL**: Supabase Dashboard → Settings → API → Project URL
- **SUPABASE_SERVICE_KEY**: Supabase Dashboard → Settings → API → service_role key (keep this secret!)
- **SUPABASE_ANON_KEY**: Supabase Dashboard → Settings → API → anon/public key
- **SUPABASE_JWT_SECRET**: Supabase Dashboard → Settings → API → JWT Settings → JWT Secret
- **SUPABASE_ENCRYPTION_KEY**: Generated in Step 1 using `openssl rand -base64 32`
- **GITHUB_APP_ID**: GitHub App settings page → App ID
- **GITHUB_APP_PRIVATE_KEY**: Download from GitHub App settings → Private keys section (replace newlines with `\n`)
- **AUTH_SUPABASE_URL** and **AUTH_SUPABASE_ANON_KEY**: Same as SUPABASE_URL and SUPABASE_ANON_KEY if not using global auth

> **Note**: For `GITHUB_APP_PRIVATE_KEY`, replace all line breaks with `\n` to make it a single line.

> **Validation**: The configuration will be validated when you run `sfp server init` in Step 4. Invalid credentials will cause the initialization to fail with specific error messages.

#### Step 3: Prepare Your Server

SSH into your server and run:

```bash
# Create directory
sudo mkdir -p /opt/sfp-server
sudo chown $USER:$USER /opt/sfp-server

# Login to Docker registry (if using private images)
echo "your-github-pat" | docker login ghcr.io -u your-username --password-stdin

# Exit back to local machine
exit
```

#### Step 4: Deploy SFP Server

From your **local machine**, run:

```bashsfp server init \
  --tenant your-company \
  --mode prod \
  --config-file ./server.json \
  --base-dir /opt/sfp-server \
  --ssh-connection ubuntu@your-server-ip \
  --identity-file ~/.ssh/your-key.pem
# For remote deployment

```

**Alternative for local deployment:**
```bash
# If running directly on the server
sfp server init --tenant your-company --mode prod --config-file ./server.json --base-dir /opt/sfp-server
```

The initialization process will:
1. Create directory structure
2. Generate Docker Compose configuration
3. Configure Caddy for automatic HTTPS
4. Initialize database schema
5. Start all containers
6. Create default admin user

#### Step 5: Verify Installation

```bash
# Check server status
sfp server status \
  --tenant your-company \
  --ssh-connection ubuntu@your-server-ip \
  --identity-file ~/.ssh/your-key.pem

# Test the API endpoint
curl https://sfp.yourcompany.com/health
```

Expected response:
```json
{"status": "healthy", "version": "x.x.x"}
```

#### Step 6: Enable Auto-Restart

SSH to your server and configure auto-restart:

```bash
# Enable Docker on boot
sudo systemctl enable docker

# Set restart policy
docker update --restart=unless-stopped $(docker ps -q)
```

### Post-Installation Management

#### Common Management Commands

All commands can be run remotely from your local machine:

```bash
# View logs
sfp server logs --tenant your-company \
  --ssh-connection ubuntu@your-server-ip \
  --identity-file ~/.ssh/your-key.pem

# Stop server
sfp server stop --tenant your-company \
  --ssh-connection ubuntu@your-server-ip \
  --identity-file ~/.ssh/your-key.pem

# Start server
sfp server start --tenant your-company \
  --ssh-connection ubuntu@your-server-ip \
  --identity-file ~/.ssh/your-key.pem

# Update to latest version
sfp server update --tenant your-company \
  --ssh-connection ubuntu@your-server-ip \
  --identity-file ~/.ssh/your-key.pem
```

### Troubleshooting Quick Fixes

#### Docker Registry Authentication Issues
```bash
# On the server
docker logout ghcr.io
echo "your-pat" | docker login ghcr.io -u username --password-stdin
```

#### Supabase Connection Failed
- Verify Supabase URL is publicly accessible
- Check service key permissions
- Test connection: `curl -X GET "SUPABASE_URL/rest/v1/" -H "apikey: ANON_KEY"`

#### Domain Not Resolving
- Check DNS: `nslookup sfp.yourcompany.com`
- Verify A record configuration
- Allow up to 48 hours for propagation

#### Container Issues
```bash
# Check logs on the server
cd /opt/sfp-server/tenants/your-company
docker compose logs -f
```

### Security Best Practices

1. **Firewall Configuration**
   ```bash
   # Allow only necessary ports
   sudo ufw allow 22/tcp   # SSH
   sudo ufw allow 80/tcp   # HTTP (redirects to HTTPS)
   sudo ufw allow 443/tcp  # HTTPS
   sudo ufw enable
   ```

2. **Regular Updates**
   - Enable automatic security updates
   - Rotate secrets quarterly
   - Monitor server logs regularly

3. **Backup Strategy**
   - Configure database backups
   - Backup `/opt/sfp-server` directory
   - Test restore procedures

### Next Steps

1. **Configure GitHub Integration**
   - Connect repositories
   - Set up webhooks
   - Configure CI/CD pipelines

2. **Set Up Monitoring**
   - Access metrics at `/metrics`
   - Configure alerting
   - Set up log aggregation

3. **User Management**
   - Create additional users via API
   - Configure teams and permissions
   - Set up SSO if required

### Getting Help

- **Documentation**: Full details in subsequent sections
- **Logs**: Check `sfp server logs` for debugging
- **Support**: Contact your SFP support channel

