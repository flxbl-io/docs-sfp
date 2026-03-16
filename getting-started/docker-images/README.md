---
metaLinks:
  alternates:
    - >-
      https://app.gitbook.com/s/YLI5Ts7pWhWQV9UaBn3H/getting-started/docker-images
---

# Docker Images

sfp Docker images are published from the flxbl Gitea registry at source.flxbl.io.

Two image variants are available:

| Image | Description |
| ----- | ----------- |
| **sfp-pro-lite** | sfp CLI without SF CLI bundled |
| **sfp-pro** | sfp CLI with SF CLI bundled |

### Pulling Images

```bash
# Login to the Gitea registry
docker login source.flxbl.io -u your-username

# Pull the sfp image with SF CLI
docker pull source.flxbl.io/flxbl/sfp-pro:version

# Pull the lite image (without SF CLI)
docker pull source.flxbl.io/flxbl/sfp-pro-lite:version
```

Version numbers can be found at [https://source.flxbl.io/flxbl/-/packages/container/sfp-pro/](https://source.flxbl.io/flxbl/-/packages/container/sfp-pro/)

### Using in CI/CD

```yaml
# GitHub Actions
jobs:
  build:
    runs-on: ubuntu-latest
    container:
      image: source.flxbl.io/flxbl/sfp-pro:latest
      credentials:
        username: ${{ secrets.GITEA_USER }}
        password: ${{ secrets.GITEA_PAT }}
```

For setting up automated image synchronization to your own registry, see [Automated Image Synchronization](sfp-pro/migrating-to-sfp-pro.md).
