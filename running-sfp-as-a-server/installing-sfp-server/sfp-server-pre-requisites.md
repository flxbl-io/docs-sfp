# SFP Server - Pre Requisites

This guide provides a streamlined path to get your SFP Server up and running quickly on any Linux server (AWS EC2, Hetzner, DigitalOcean, etc.).

### Prerequisites Checklist

Before starting, ensure you have:

*   [ ] **Linux Server** (Ubuntu 20.04+ or similar) with:

    * Minimum 4GB RAM, 2 vCPUs
    * Docker 20.10+ and Docker Compose 2.0+ installed
    * SSH access configured
    * Public IP address

    **Quick check**: `ssh your-server "free -h | grep Mem && nproc && docker --version && docker compose version"`
*   [ ] **Supabase Instance** with:

    * Project URL (e.g., https://xxxxx.supabase.co)
    * Service Key (for API access)
    * Anon Key (for public access)
    * JWT Secret (from project settings)
    * Database URL (PostgreSQL connection string)

    **Setup options**: [Supabase Cloud](https://supabase.com) or [Self-Hosted Guide](self-hosted-supabase-configuration.md)

    **Quick check**: `curl -s -o /dev/null -w "%{http_code}" YOUR_SUPABASE_URL/rest/v1/ -H "apikey: YOUR_ANON_KEY" # Should return 200`
* [ ] **GitHub App** configured with:
  * App ID
  * Private Key (.pem file)
  * Appropriate repository permissions
* [ ] **Domain Name** (for production):
  * DNS A record pointing to server IP
  * e.g., `sfp.yourcompany.com`
* [ ] **Local Machine** with:
  * sfp-pro CLI installed
  * SSH key for server access
