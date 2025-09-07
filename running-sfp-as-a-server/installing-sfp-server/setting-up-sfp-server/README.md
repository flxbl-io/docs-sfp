# Setting up SFP Server

This guide provides a streamlined process to get your SFP Server up and running quickly on any Linux server (AWS EC2, Hetzner, DigitalOcean, etc.).

## Prerequisites

Before starting, ensure you have:

- **Linux Server** with Docker and Docker Compose installed
- **Supabase Instance** (managed or self-hosted) 
- **GitHub App** configured for repository integration
- **Local Machine** with sfp-pro CLI installed

For detailed requirements, see the [Installing SFP Server](../README.md) guide.

## Step-by-Step Setup

#### Step 1: Generate Security Keys

Generate required secrets on your local machine:

```bash
# Generate Encryption Key (save this value)
openssl rand -base64 32
```

#### Step 2: Create Configuration File

Create `server.json` on your local machine. This file provides all the secrets needed for the SFP server to:

* Pull Docker images from the registry
* Connect to your Supabase database
* Authenticate with GitHub
* Configure HTTPS domain and worker processes

```json
{
  "domain": "sfp.yourcompany.com",
  "workerCounts": "1,2,1",
  "secrets": {
    "DOCKER_REGISTRY": "ghcr.io",
    "DOCKER_REGISTRY_TOKEN": "ghp_xxxxxxxxxxxx",
    "SUPABASE_DB_URL": "postgresql://postgres:password@db.project.supabase.co:5432/postgres",
    "SUPABASE_URL": "https://project.supabase.co",
    "SUPABASE_ANON_KEY": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
    "SUPABASE_SERVICE_KEY": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
    "SUPABASE_JWT_SECRET": "gAZgjd0n6yQxQkANH8DHRkgQg8Jyf2Z3QFZv...",
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

* **DOCKER\_REGISTRY\_TOKEN**: GitHub → Settings → Developer settings → Personal access tokens (classic) → Generate new token → Select `read:packages` scope
* **SUPABASE\_DB\_URL**: Supabase Dashboard → Click "Connect" button at top (Note: URL-encode password if it contains special characters like @ → %40)
* **SUPABASE\_URL**: Supabase Dashboard → Project overview → Project API section → Project URL
* **SUPABASE\_ANON\_KEY**: Supabase Dashboard → Project overview → Project API section → API Key (anon/public)
* **SUPABASE\_SERVICE\_KEY**: Supabase Dashboard → Project Settings → API Keys → service\_role key (keep this secret!)
* **SUPABASE\_JWT\_SECRET**: Supabase Dashboard → Project Settings → JWT Keys → JWT Secret
* **SUPABASE\_ENCRYPTION\_KEY**: Generated in Step 1 using `openssl rand -base64 32`
* **GITHUB\_APP\_ID**: GitHub → Settings → Developer settings → GitHub Apps → Your App → App ID (at top of page)
* **GITHUB\_APP\_PRIVATE\_KEY**: GitHub App settings → Scroll to "Private keys" section → Generate a private key → Download .pem file (replace newlines with `\n`)
* **AUTH\_SUPABASE\_URL** and **AUTH\_SUPABASE\_ANON\_KEY**: Same as SUPABASE\_URL and SUPABASE\_ANON\_KEY if not using global auth

> **Note**: For `GITHUB_APP_PRIVATE_KEY`, replace all line breaks with `\n` to make it a single line.

> **Validation**: The configuration will be validated when you run `sfp server init` in Step 4. Invalid credentials will cause the initialization to fail with specific error messages.

#### Step 3: Prepare Your Server

SSH into your server and run:

```bash
# Create directory
sudo mkdir -p /opt/sfp-server
sudo chown $USER:$USER /opt/sfp-server

# Login to Docker registry (if using private images)
echo "your-gitea-pat" | docker login source.flxbl.io -u your-username --password-stdin

# Exit back to local machine
exit
```

#### Step 4: Deploy SFP Server

From your **local machine**, run:

```bash
sfp server init \
  --tenant your-company \
  --mode prod \
  --config-file ./server.json \
  --base-dir /opt/sfp-server \
  --ssh-connection ubuntu@your-server-ip \
  --identity-file ~/.ssh/your-key.pem

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
5. Create default admin user (if Supabase credentials provided)

#### Step 5: Start the Server

After initialization, start the server containers:

```bash
sfp server start \
  --tenant your-company \
  --ssh-connection ubuntu@your-server-ip \
  --identity-file ~/.ssh/your-key.pem
```

#### Step 6: Verify Installation

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

#### Step 7: Enable Auto-Restart

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

* Verify Supabase URL is publicly accessible
* Check service key permissions
* Test connection: `curl -X GET "SUPABASE_URL/rest/v1/" -H "apikey: ANON_KEY"`

#### IPv6 Network Unreachable Error

If you see `network is unreachable` with an IPv6 address (`[2a05:d014:...]`):

* **Cause**: Your server lacks IPv6 connectivity (common on Hetzner)
* **Solution 1**: Use Supabase Session Pooler connection (recommended)
  * In Supabase Dashboard → Click "Connect" → Select "Session pooler"
  * Use the pooler connection string (port 6543) instead of direct connection
  * Update `SUPABASE_DB_URL` in your server.json
* **Solution 2**: Purchase IPv4 add-on from Supabase
* **Solution 3**: Enable IPv6 on your server (if supported by your host)

#### Domain Not Resolving

* Check DNS: `nslookup sfp.yourcompany.com`
* Verify A record configuration
* Allow up to 48 hours for propagation

#### Container Issues

```bash
# Check logs on the server
cd /opt/sfp-server/tenants/your-company
docker compose logs -f
```

### Security Best Practices

1.  **Firewall Configuration**

    ```bash
    # Allow only necessary ports
    sudo ufw allow 22/tcp   # SSH
    sudo ufw allow 80/tcp   # HTTP (redirects to HTTPS)
    sudo ufw allow 443/tcp  # HTTPS
    sudo ufw enable
    ```
2. **Regular Updates**
   * Enable automatic security updates
   * Rotate secrets quarterly
   * Monitor server logs regularly
3. **Backup Strategy**
   * Configure database backups
   * Backup `/opt/sfp-server` directory
   * Test restore procedures

### Next Steps

1. **Configure GitHub Integration**
   * Connect repositories
   * Set up webhooks
   * Configure CI/CD pipelines
2. **Set Up Monitoring**
   * Access metrics at `/metrics`
   * Configure alerting
   * Set up log aggregation
3. **User Management**
   * Create additional users via API
   * Configure teams and permissions
   * Set up SSO if required

### Getting Help

* **Documentation**: Full details in subsequent sections
* **Logs**: Check `sfp server logs` for debugging
* **Support**: Contact your SFP support channel
