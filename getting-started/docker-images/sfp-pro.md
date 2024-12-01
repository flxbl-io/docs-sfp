---
icon: ring-diamond
---

# sfp-pro

sfp pro provides two Docker images optimized for different use cases, both available through the GitHub Container Registry (ghcr.io).

### Available Images

#### 1. sfp pro

**Image**: `ghcr.io/flxbl-io/sfp-pro`

A streamlined version containing:

* sfp pro CLI
* Essential build tools
* Basic development dependencies

Perfect for:

* Development environments
* Basic build scenarios
* Environments where minimal footprint is desired

#### 2. sfp pro with SF CLI

**Image**: `ghcr.io/flxbl-io/sfp-pro-sf-cli`

This image includes:

* sfp pro CLI
* Salesforce CLI and essential plugins
* Build tools and dependencies
* JDK 21
* Browser automation tools for web UI testing
* Additional utilities for CI/CD pipelines

Ideal for:

* CI/CD environments
* Build servers
* Complete development environments
* Environments requiring browser automation

### Image Tags

Both images use the following tagging convention:

* `development`: Latest development build
* `beta`: Beta release version
* `latest`: Latest stable release
* `x.y.z-{build}`: Specific version tags

### Getting Access

The Docker images are available through the GitHub Container Registry. To access them:

1. Contact flxbl support to obtain a GitHub Personal Access Token with the necessary permissions
2. Login to the GitHub Container Registry using the provided token:

```bash
echo $GITHUB_TOKEN | docker login ghcr.io -u USERNAME --password-stdin
```

3. Pull the desired image:

```bash
# For sfp pro
docker pull ghcr.io/flxbl-io/sfp-pro:latest

# For sfp pro with SF CLI
docker pull ghcr.io/flxbl-io/sfp-pro-sf-cli:latest
```

### Security

All images are:

* Automatically built in GitHub Actions
* Signed using Cosign for security verification
* Updated regularly with security patches

### Best Practices

1. Always specify a version tag in production environments
2. Use sfp pro image unless you specifically need the SF CLI version
3. Regularly update to the latest version for security updates
4. Consider using beta tags for testing new features

### Using in CI/CD Workflows

#### GitHub Actions

1. Store your provided GitHub token as a repository secret (e.g., `SFP_REGISTRY_TOKEN`)
2. Basic workflow example:

```yaml
name: SFP Build

jobs:
  build:
    runs-on: ubuntu-latest
    container:
      image: ghcr.io/flxbl-io/sfp-pro:latest
      credentials:
         username: ${{ github.actor }}
         password: ${{ secrets.SFP_REGISTRY_TOKEN }}
    
    steps:
      - uses: actions/checkout@v4
      
      - name: Run sfp commands
        run: |
          sfp build -v devhub --branch main

```

3. Using with service containers:

```yaml
name: SFP Build with Services

jobs:
  build:
    runs-on: ubuntu-latest
    
    services:
      sfp:
        image: ghcr.io/flxbl-io/sfp-pro-sf-cli:latest
        credentials:
          username: ${{ github.actor }}
          password: ${{ secrets.SFP_REGISTRY_TOKEN }}
```

#### GitLab CI

1. Store your provided GitHub token as a CI/CD variable (e.g., `SFP_REGISTRY_TOKEN`)
2. Basic pipeline example:

```yaml
image:
  name: ghcr.io/flxbl-io/sfp-pro:latest
  username: $GITLAB_USER
  password: $SFP_REGISTRY_TOKEN

before_script:
  - echo $SFP_REGISTRY_TOKEN | docker login ghcr.io -u $GITLAB_USER --password-stdin

build:
  script:
    - sfp build -v devhub --branch main
```

3. Using service containers:

```yaml
services:
  - name: ghcr.io/flxbl-io/sfp-pro-sf-cli:latest
    alias: sfp-cli

variables:
  DOCKER_AUTH_CONFIG: '{"auths":{"ghcr.io":{"auth":"'$SFP_REGISTRY_TOKEN'"}}}'
```

#### Azure Pipelines

1. Store your provided GitHub token as a pipeline variable or key vault secret
2. Basic pipeline example:

```yaml
pool:
  vmImage: 'ubuntu-latest'

container:
  image: ghcr.io/flxbl-io/sfp-pro:latest
  endpoint: github  # Docker registry service connection
  env:
    DOCKER_REGISTRY_TOKEN: $(SFP_REGISTRY_TOKEN)

steps:
- script: |
    echo $DOCKER_REGISTRY_TOKEN | docker login ghcr.io -u $(GITHUB_USER) --password-stdin
    sfp build -v devhub --branch main
```

#### Important Security Notes

1. Always store registry tokens as encrypted secrets/variables
2. Use environment-specific credentials when possible
3. Consider using temporary/ephemeral tokens for CI/CD environments
4. Regularly rotate credentials following your security policies

### Support

For issues or questions about the Docker images, please contact flxbl support through your designated support channels based on your support tier.
