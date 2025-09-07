---
description: >-
  The page details the configurations that are required in your self hosted
  supabase instance for sfp-server to work effectively
---

# Self Hosted Supabase Configuration

### Overview

This guide helps you set up Supabase on your own server with GitHub login enabled for SFP tools.

> **Note**: This documentation extends the [official Supabase Self-Hosting with Docker guide](https://supabase.com/docs/guides/self-hosting/docker). Some steps may become outdated as Supabase evolves - refer to the official documentation for the most current information.

{% hint style="danger" %}
We recommend using  Supabase Cloud Hosted version using a Teams/Pro subscription. Please proceed if you have sufficitient in house capabilities for ongoing management of Supabase
{% endhint %}

### What You'll Need

* A server with at least 8GB RAM and 25GB SSD storage (EC2 with Ubuntu preferred, as this guide uses `apt` commands)
* A domain name (like `supabase.yourdomain.com`)
* Basic command line knowledge
* **For AWS EC2**: Security Group with ports 80, 443 (for HTTPS), and 8000 (for direct access) open for inbound traffic

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

# Install Caddy (REQUIRED for production - provides automatic HTTPS/SSL)
# Caddy automatically obtains and renews SSL certificates from Let's Encrypt
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
   * ANON\_KEY
   * SERVICE\_ROLE\_KEY

#### Step 4: Configure Supabase

Edit the `.env` file and update these values:

```bash
nano .env
```

Update the following values:

```bash
# IMPORTANT: Change ALL these from the defaults!

# Database password (make it strong!)
POSTGRES_PASSWORD='your-strong-password-here'

# Dashboard credentials (CHANGE THESE!)
DASHBOARD_USERNAME='your-dashboard-username'
DASHBOARD_PASSWORD='your-secure-dashboard-password'

# From step 3
JWT_SECRET='your-jwt-secret-here'
ANON_KEY='your-anon-key-here'
SERVICE_ROLE_KEY='your-service-role-key-here'

# Set your URL (use your server's public IP for initial testing)
SITE_URL=http://YOUR-PUBLIC-IP:8000
API_EXTERNAL_URL=http://YOUR-PUBLIC-IP:8000

# Enable dashboard access from outside localhost
SUPABASE_PUBLIC_URL=http://YOUR-PUBLIC-IP:8000

# Add this for GoTrue Auth to allow SFP CLI and Codev desktop app callbacks
GOTRUE_URI_ALLOW_LIST="io.flxbl.codev://auth/callback,http://localhost:54329/callback"
```

Save and exit (Ctrl+X, then Y, then Enter).

#### Step 5: Configure SSL with Caddy (Required for Production)

{% hint style="success" %}
If you are using your enterprise's mechanism for HTTPS termination, you can skip this step.&#x20;
{% endhint %}

**Why you need this**: GitHub OAuth and secure authentication require HTTPS. Running without SSL exposes your credentials and tokens in plain text. Never run production without HTTPS.

If you installed Caddy in Step 1, configure it now. If not, go back and install it first.

Edit the existing `/etc/caddy/Caddyfile`:

```bash
sudo nano /etc/caddy/Caddyfile
```

Add this block at the END of the file (after any existing :80 block):

```
supabase.yourdomain.com {
    reverse_proxy localhost:8000
}
```

Your Caddyfile should now have both the default :80 block AND your new Supabase domain block.

Then reload Caddy:

```bash
sudo systemctl reload caddy
sudo systemctl enable caddy
```

#### Step 6: Set Up GitHub Login

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

#### Pre-flight Check (AWS EC2)

If using AWS EC2, ensure the required ports are open:

1. Go to EC2 Console → Your instance → Security tab
2. Check current Security Groups - if none or only default, you need to add one
3. Click "Actions" → "Security" → "Change security groups"
4. Either modify existing or create new security group with:
   * Inbound rule: HTTP, Port 80, Source 0.0.0.0/0 (for Let's Encrypt certificate validation)
   * Inbound rule: HTTPS, Port 443, Source 0.0.0.0/0 (for secure access via domain)
   * Inbound rule: Custom TCP, Port 8000, Source 0.0.0.0/0 (for direct Supabase access during setup)
   * Inbound rule: SSH, Port 22, Source: Your IP (for SSH access)

#### Step 7: Start Supabase

After configuration, pull and start services:

```bash
cd /opt/supabase/docker/
# Pull the latest images
docker compose pull
```

```bash
# Start the services (in detached mode)
docker compose up -d
```

Wait for all services to start (about 30 seconds), then check their status:

```bash
# Check if all services are healthy
docker compose ps
```

All services should show status `running (healthy)`.

**Test if Supabase is running:**

```bash
# Check if the services are responding (will return an auth error, which is expected)
curl -I http://localhost:8000/rest/v1/
```

If you get a `401 Unauthorized` response, your Supabase instance is running correctly (the 401 just means you need authentication, which is normal).

You can now access Supabase Studio at `http://YOUR-PUBLIC-IP:8000` with the credentials you configured in Step 4 (DASHBOARD\_USERNAME and DASHBOARD\_PASSWORD).

> **Note**: If you made configuration changes to the `.env` file after starting Supabase, restart the services for the changes to take effect:
>
> ```bash
> docker compose down
> docker compose up -d
> ```

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

**Can't access Supabase from public IP?**

* **AWS EC2**: Check Security Group - ensure port 8000 is open for inbound traffic
* Verify Docker is running: `docker compose ps`
* Check if port is listening: `sudo netstat -tlnp | grep 8000`
* Test locally first: `curl http://localhost:8000/auth/v1/health`

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
