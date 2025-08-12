---
icon: ring-diamond
---

# sfp-pro Docker Images

SFP-Pro provides Docker images through our self-hosted Gitea registry at source.flxbl.io. These pre-built images are maintained and updated regularly with the latest features and security patches.

### Prerequisites

1. Access to source.flxbl.io (Gitea server)
2. Docker installed on your machine
3. Registry credentials from your welcome email

### Accessing the Images

1. Login to the Gitea registry:
```bash
docker login source.flxbl.io -u your-username
```

2. Pull the desired image:

The version numbers can be found at [https://source.flxbl.io/flxbl/-/packages/container/sfp-pro/](https://source.flxbl.io/flxbl/-/packages/container/sfp-pro/)

```bash
# For base sfp-pro image
docker pull source.flxbl.io/sfp-pro-lite:version

# For sfp-pro with SF CLI
docker pull source.flxbl.io/sfp-pro:version
```

3. (Optional) Tag for your registry:
```bash
# Tag for your registry
docker tag source.flxbl.io/sfp-pro:version your-registry/sfp-pro:version

# Push to your registry
docker push your-registry/sfp-pro:version
```

### Best Practices

1. Use specific version tags in production
2. Cache images in your private registry for better performance
3. Implement proper access controls in your registry
4. Document image versions used in your pipelines

### Building Docker Images

If you need to build the images yourself, you can access the source code from source.flxbl.io and follow these instructions:

#### Prerequisites

- Docker with BuildKit support
- GitHub Personal Access Token with `packages:read` permissions
- Node.js (for local development)

#### Building the Base Image (sfp-pro-lite)

```bash
# Create a file containing your GITEA token
echo "YOUR_GITEA_TOKEN" > .npmrc.token

# Build the base sfp-pro image (without SF CLI)
docker buildx build \
  --secret id=npm_token,src=.npmrc.token \
  --build-arg NODE_MAJOR=22 \
  --file dockerfiles/sfp-pro-lite.Dockerfile \
  --tag sfp-pro-lite:local .

# Remove the token file
rm .npmrc.token
```

#### Building the Image with SF CLI Bundled (sfp-pro)

```bash
# Create a file containing your GITEA token
echo "YOUR_GITEA_TOKEN" > .npmrc.token

# Build the sfp-pro image with SF CLI bundled
docker buildx build \
  --secret id=npm_token,src=.npmrc.token \
  --build-arg NODE_MAJOR=22 \
  --file dockerfiles/sfp-pro.Dockerfile \
  --tag sfp-pro:local .

# Remove the token file
rm .npmrc.token
```

#### Build Arguments

The following build arguments are supported:
- `NODE_MAJOR`: Node.js major version (default: 22)
- `SFP_VERSION`: Version of SFP Pro to build
- `GIT_COMMIT`: Git commit hash for versioning
- `SF_COMMIT_ID`: Salesforce commit ID

### Support

For issues or questions about Docker images, please contact flxbl support through your designated support channels.
