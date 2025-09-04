---
description: >-
  The page details the configurations that are required in your self hosted
  supabase instance for sfp-server to work effectively
---

# Self Hosted Supabase Configuration

### Overview

This guide helps you set up Supabase on your own server with GitHub login enabled for SFP tools.

> **Note**: This documentation extends the [official Supabase Self-Hosting with Docker guide](https://supabase.com/docs/guides/self-hosting/docker). Some steps may become outdated as Supabase evolves - refer to the official documentation for the most current information.

### What You'll Need

* A server with at least 8GB RAM (EC2 with Ubuntu preferred, as this guide uses `apt` commands)
* A domain name (like `supabase.yourdomain.com`)
* Basic command line knowledge

### Quick Start

#### Step 1: Install Prerequisites

```bash
# Update system
sudo apt update && sudo apt upgrade -y

# Install Docker and Docker Compose
curl -fsSL https://get.docker.com -o get-docker.sh
sudo sh get-docker.sh
sudo usermod -aG docker $USER

# The get-docker.sh script might install docker-ce only and not the compose plugin
# Install Docker Compose plugin
sudo apt update && sudo apt install -y docker-compose-plugin

# Log out and back in, then continue

# Install Caddy (for automatic SSL)
sudo apt install -y debian-keyring debian-archive-keyring apt-transport-https
curl -1sLf 'https://dl.cloudsmith.io/public/caddy/stable/gpg.key' | sudo gpg --dearmor -o /usr/share/keyrings/caddy-stable-archive-keyring.gpg
curl -1sLf 'https://dl.cloudsmith.io/public/caddy/stable/debian.deb.txt' | sudo tee /etc/apt/sources.list.d/caddy-stable.list
sudo apt update && sudo apt install caddy
```

#### Step 2: Get Supabase

```bash
cd /opt
sudo git clone --depth 1 https://github.com/supabase/supabase
sudo chown -R $USER:$USER supabase
cd supabase/docker
cp .env.example .env
```

#### Step 3: Generate Your Keys

1. Generate a JWT secret:

```bash
openssl rand -base64 32
# Save this output - you'll need it next
```

2. Go to the [JWT Generator in the official Supabase docs](https://supabase.com/docs/guides/self-hosting/docker#generate-api-keys)
3. Paste your JWT secret in the form and generate:
   * ANON_KEY
   * SERVICE_ROLE_KEY

#### Step 4: Configure Supabase

Edit the `.env` file and update these values:

```bash
nano .env
```

Update the following values:

```bash
# IMPORTANT: Change these from the defaults!

# Database password (make it strong!)
POSTGRES_PASSWORD='your-strong-password-here'

# From step 3
JWT_SECRET='your-jwt-secret-here'
ANON_KEY='your-anon-key-here'
SERVICE_ROLE_KEY='your-service-role-key-here'

# Set your domain
SITE_URL=https://supabase.yourdomain.com
API_EXTERNAL_URL=https://supabase.yourdomain.com

# Add this for GoTrue Auth to allow SFP CLI and Codev desktop app callbacks
GOTRUE_URI_ALLOW_LIST="io.flxbl.codev://auth/callback,http://localhost:54329/callback"
```

#### Step 5: Set Up GitHub Login

1. Go to GitHub Settings → Developer settings → OAuth Apps → New OAuth App
2. Fill in:
   * Application name: `Your Supabase`
   * Homepage URL: `https://supabase.yourdomain.com`
   * Callback URL: `https://supabase.yourdomain.com/auth/v1/callback`
3. Create and save the Client ID and Secret
4. Add to your `.env` file:

```bash
GOTRUE_EXTERNAL_GITHUB_ENABLED=true
GOTRUE_EXTERNAL_GITHUB_CLIENT_ID=your-github-client-id
GOTRUE_EXTERNAL_GITHUB_SECRET=your-github-client-secret
GOTRUE_EXTERNAL_GITHUB_REDIRECT_URI=https://supabase.yourdomain.com/auth/v1/callback
```

#### Step 6: Configure SSL with Caddy

Create `/etc/caddy/Caddyfile`:

```
supabase.yourdomain.com {
    reverse_proxy localhost:8000
    tls admin@yourdomain.com
}
```

Then:

```bash
sudo systemctl reload caddy
sudo systemctl enable caddy
```

#### Step 7: Start Supabase

```bash
cd /opt/supabase/docker
docker compose up -d
```

#### Step 8: Connect SFP Server

On your SFP Server, use these settings:

```bash
SUPABASE_URL=https://supabase.yourdomain.com
SUPABASE_ANON_KEY=your-anon-key
SUPABASE_SERVICE_KEY=your-service-key
```

### Verify Everything Works

```bash
# Check if Supabase is running
docker ps

# Test the API
curl https://supabase.yourdomain.com/auth/v1/health

# Check logs if needed
docker compose logs -f
```

### Maintenance

#### Start/Stop Supabase

```bash
cd /opt/supabase/docker
docker compose stop    # Stop
docker compose start   # Start
```

#### Update Supabase

```bash
cd /opt/supabase
git pull
cd docker
docker compose down
docker compose pull
docker compose up -d
```

#### View Logs

```bash
docker compose logs -f auth    # Auth logs
docker compose logs -f kong    # API gateway logs
```

### Troubleshooting

**Can't login with GitHub?**

* Check your GitHub OAuth app callback URL matches exactly
* Look at auth logs: `docker compose logs -f auth`

**SSL not working?**

* Make sure your domain points to your server's IP
* Check Caddy logs: `sudo journalctl -u caddy -f`

**Out of memory?**

* Your server needs at least 8GB RAM
* Check with: `free -h`

### Security Checklist

Before going live:

* [ ] Changed all default passwords
* [ ] Set strong database password
* [ ] Enabled firewall (ports 22, 80, 443 only)
* [ ] Regular backups configured

### Need Help?

* [Supabase Discord](https://discord.supabase.com)
* [Supabase GitHub Discussions](https://github.com/orgs/supabase/discussions)
* [SFP Documentation](https://docs.flxbl.io)
