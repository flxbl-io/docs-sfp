# Docker Installation

Prerequisites

* Ubuntu instance with sudo privileges
* SSH access to the instance

### 1. Install Docker

#### Update System and Install Prerequisites

```bash
# Update package index
sudo apt-get update

# Install required packages
sudo apt-get install -y \
    ca-certificates \
    curl \
    gnupg \
    lsb-release
```

#### Add Docker Repository

```bash
# Create directory for Docker GPG key
sudo mkdir -m 0755 -p /etc/apt/keyrings

# Download and add Docker's official GPG key
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | \
    sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg

# Add Docker repository to apt sources
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \
  $(lsb_release -cs) stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
```

#### Install Docker Engine

```bash
# Update package index with Docker packages
sudo apt-get update

# Install Docker Engine, CLI, and plugins
sudo apt-get install -y \
    docker-ce \
    docker-ce-cli \
    containerd.io \
    docker-buildx-plugin \
    docker-compose-plugin
```

### 2. Configure User Permissions

```bash
# Create docker group (if it doesn't exist)
sudo groupadd -f docker

# Add current user to docker group
sudo usermod -aG docker $USER

# Add ubuntu user to docker group (for EC2)
sudo usermod -aG docker ubuntu
```

### 3. Start and Enable Docker Service

```bash
# Start Docker daemon
sudo systemctl start docker

# Enable Docker to start on boot
sudo systemctl enable docker

# Verify Docker service status
sudo systemctl status docker
```

### 4. Apply Group Changes

```bash
# Apply new group membership without logging out
newgrp docker
```

**Note:** Alternatively, you can disconnect and reconnect to your SSH session:

```bash
exit
# Then SSH back into your instance
ssh -i your-key.pem ubuntu@your-ec2-ip
```

### 5. Verify Installation

```bash
# Check Docker version
docker --version

# Verify Docker works without sudo
docker ps

# Run test container
docker run hello-world
```

### 6. Verify Everything is Working

If successful, you should see:

* `docker ps` runs without permission errors
* `docker run hello-world` downloads and runs a test image
* Docker version information displays correctly

### Troubleshooting

#### If Permission Denied Still Occurs

```bash
# Check if user is in docker group
groups

# Check Docker socket permissions
ls -l /var/run/docker.sock

# Fix socket permissions if needed
sudo chown root:docker /var/run/docker.sock
sudo chmod 660 /var/run/docker.sock
```

#### If Docker Service Won't Start

```bash
# Check Docker logs
sudo journalctl -u docker.service

# Restart Docker
sudo systemctl restart docker
```

### Security Considerations for EC2

1. **Security Groups**: Ensure your AWS Security Group allows necessary ports:
   * Port 2375 (unencrypted) or 2376 (TLS) only if remote Docker access is needed
   * Keep these closed unless specifically required
2. **IAM Roles**: If your containers need AWS access, attach appropriate IAM roles to your EC2 instance
3. **Docker Group Warning**: Adding users to the docker group grants root-equivalent privileges. Only add trusted users.

### Quick One-Liner Installation

For a fresh Ubuntu EC2 instance, you can run all installation commands in sequence:

```bash
curl -fsSL https://get.docker.com -o get-docker.sh && \
sudo sh get-docker.sh && \
sudo usermod -aG docker $USER && \
sudo usermod -aG docker ubuntu && \
sudo systemctl start docker && \
sudo systemctl enable docker && \
newgrp docker
```

### Post-Installation Optional Steps

#### Install Docker Compose (Standalone)

```bash
# Download latest Docker Compose
sudo curl -L "https://github.com/docker/compose/releases/latest/download/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose

# Make executable
sudo chmod +x /usr/local/bin/docker-compose

# Verify installation
docker-compose --version
```

#### Configure Docker Logging

```bash
# Create Docker daemon configuration
sudo tee /etc/docker/daemon.json <<EOF
{
  "log-driver": "json-file",
  "log-opts": {
    "max-size": "10m",
    "max-file": "3"
  }
}
EOF

# Restart Docker to apply changes
sudo systemctl restart docker
```

### Useful Docker Commands

```bash
# List running containers
docker ps

# List all containers (including stopped)
docker ps -a

# List Docker images
docker images

# Remove unused data
docker system prune -a

# View Docker system info
docker info

# View Docker logs
docker logs [container-id]
```



