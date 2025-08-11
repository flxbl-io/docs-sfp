---
description: >-
  This guide helps organizations set up automated synchronization of sfp pro
  images from Flxbl's registry to their own container registry, with optional
  customization capabilities
icon: ring-diamond
---

# Automated Image Synchronization to Your Registry

## Why Synchronize to Your Registry?

While you can pull directly from `source.flxbl.io`, maintaining your own synchronized copy provides:

* **Centralized version control** across all teams
* **Reduced external dependencies** during CI/CD runs
* **Ability to add organization-specific customizations**
* **Improved pull performance** from your own registry
* **Compliance** with internal security policies

### Setting Up Automated Synchronization

#### Step 1: Create a Dedicated Repository

Create a GitHub repository in your organization specifically for Docker image management (e.g., `docker-images` or `sfp-docker`).

#### Step 2: Configure Repository Secrets

Add the following secrets to your repository (Settings → Secrets and variables → Actions):

| Secret Name  | Description                     | Value                                                                                                         |
| ------------ | ------------------------------- | ------------------------------------------------------------------------------------------------------------- |
| `GITEA_USER` | Your Gitea username             | From your welcome email                                                                                       |
| `GITEA_PAT`  | Personal Access Token for Gitea | Generate at source.flxbl.io (Settings → Applications → Personal Access Tokens) with `read:package` permission |

#### Step 3: Create Synchronization Workflow

Create `.github/workflows/sync-sfp-pro.yml` in your repository:

```yaml
name: Sync SFP Pro Images

on:
  workflow_dispatch:
    inputs:
      sfp_version:
        description: 'SFP Pro version (leave empty for latest)'
        required: false
        type: string
      include_sf_cli:
        description: 'Also sync SF CLI variant'
        required: false
        type: boolean
        default: true

env:
  REGISTRY: ghcr.io
  IMAGE_PREFIX: ${{ github.repository }}

jobs:
  sync-images:
    runs-on: ubuntu-latest
    permissions:
      contents: read
      packages: write
    
    steps:
      - name: Checkout repository
        uses: actions/checkout@v4
        
      - name: Login to source.flxbl.io
        run: |
          echo "${{ secrets.GITEA_PAT }}" | docker login source.flxbl.io \
            -u ${{ secrets.GITEA_USER }} \
            --password-stdin

      - name: Login to GitHub Container Registry
        run: |
          echo "${{ secrets.GITHUB_TOKEN }}" | docker login ghcr.io \
            -u ${{ github.actor }} \
            --password-stdin

      - name: Determine version
        id: version
        run: |
          if [ -n "${{ github.event.inputs.sfp_version }}" ]; then
            echo "version=${{ github.event.inputs.sfp_version }}" >> $GITHUB_OUTPUT
          else
            # Fetch latest version from your version strategy
            echo "version=latest" >> $GITHUB_OUTPUT
          fi

      - name: Sync base SFP-Pro image
        run: |
          SOURCE_IMAGE="source.flxbl.io/flxbl/sfp-pro:${{ steps.version.outputs.version }}"
          TARGET_IMAGE="${{ env.REGISTRY }}/${{ env.IMAGE_PREFIX }}/sfp-pro"
          
          docker pull ${SOURCE_IMAGE}
          docker tag ${SOURCE_IMAGE} ${TARGET_IMAGE}:${{ steps.version.outputs.version }}
          docker tag ${SOURCE_IMAGE} ${TARGET_IMAGE}:latest
          
          docker push ${TARGET_IMAGE}:${{ steps.version.outputs.version }}
          docker push ${TARGET_IMAGE}:latest

      - name: Sync SFP-Pro SF CLI image
        if: github.event.inputs.include_sf_cli != 'false'
        run: |
          SOURCE_IMAGE="source.flxbl.io/flxbl/sfp-pro-sf-cli:${{ steps.version.outputs.version }}"
          TARGET_IMAGE="${{ env.REGISTRY }}/${{ env.IMAGE_PREFIX }}/sfp-pro-sf-cli"
          
          docker pull ${SOURCE_IMAGE}
          docker tag ${SOURCE_IMAGE} ${TARGET_IMAGE}:${{ steps.version.outputs.version }}
          docker tag ${SOURCE_IMAGE} ${TARGET_IMAGE}:latest
          
          docker push ${TARGET_IMAGE}:${{ steps.version.outputs.version }}
          docker push ${TARGET_IMAGE}:latest
```

### Creating Custom Images

If you need to add organization-specific tools or configurations, create a `Dockerfile`:

#### For base sfp pro:

```dockerfile
ARG BASE_VERSION=latest
FROM source.flxbl.io/flxbl/sfp-pro:${BASE_VERSION}

# Add your customizations
RUN apt-get update && apt-get install -y \
    jq \
    your-custom-tools \
    && rm -rf /var/lib/apt/lists/*

# Copy custom scripts or configurations
# COPY scripts/ /usr/local/bin/
# COPY config/ /etc/your-app/
```

#### For sfp proro with SF CLI:

```dockerfile
ARG BASE_VERSION=latest
FROM source.flxbl.io/flxbl/sfp-pro-sf-cli:${BASE_VERSION}

# Your customizations here
```

Then modify the workflow to build and push your custom image:

```yaml
      - name: Build and push custom image
        run: |
          docker build \
            --build-arg BASE_VERSION=${{ steps.version.outputs.version }} \
            -f Dockerfile \
            -t ${{ env.REGISTRY }}/${{ env.IMAGE_PREFIX }}/sfp-pro-custom:${{ steps.version.outputs.version }} \
            -t ${{ env.REGISTRY }}/${{ env.IMAGE_PREFIX }}/sfp-pro-custom:latest \
            .
          
          docker push ${{ env.REGISTRY }}/${{ env.IMAGE_PREFIX }}/sfp-pro-custom:${{ steps.version.outputs.version }}
          docker push ${{ env.REGISTRY }}/${{ env.IMAGE_PREFIX }}/sfp-pro-custom:latest
```

### Using Synchronized Images in Your Pipelines

Update your project workflows to use images from your registry:

#### GitHub Actions:

```yaml
jobs:
  build:
    runs-on: ubuntu-latest
    container:
      image: ghcr.io/your-org/docker-images/sfp-pro:latest
      credentials:
        username: ${{ github.actor }}
        password: ${{ secrets.GITHUB_TOKEN }}
```

#### GitLab CI:

```yaml
image: ghcr.io/your-org/docker-images/sfp-pro:latest

before_script:
  - echo "$CI_REGISTRY_PASSWORD" | docker login ghcr.io -u "$CI_REGISTRY_USER" --password-stdin
```

#### Azure DevOps:

```yaml
resources:
  containers:
  - container: sfp
    image: ghcr.io/your-org/docker-images/sfp-pro:latest
    endpoint: your-service-connection

jobs:
- job: Build
  container: sfp
```

### Verification

After running the workflow, verify the synchronization:

```bash
# List available images in your registry
docker search ghcr.io/your-org/docker-images

# Pull and test the synchronized image
docker pull ghcr.io/your-org/docker-images/sfp-pro:latest
docker run --rm ghcr.io/your-org/docker-images/sfp-pro:latest sfp --version
```

### Troubleshooting

#### Authentication Issues

If you encounter authentication errors:

1. Verify your PAT has `read:package` permission
2. Check that secrets are correctly set in repository settings
3. Ensure your Gitea username is correct

#### Image Not Found

If the source image cannot be pulled:

1. Check the version exists at https://source.flxbl.io/flxbl/-/packages/container/sfp-pro/
2. Verify your network can reach source.flxbl.io
3. Confirm your credentials are valid

#### Push Failures to GitHub Container Registry

1. Ensure the workflow has `packages: write` permission
2. Verify the repository name in `IMAGE_PREFIX` is correct
3. Check GitHub Packages settings for your repository
