# Patching Releases

|              | sfp-pro | sfp (community) |
| ------------ | ------- | --------------- |
| Availability | ✅       | ✅               |

## Overview

The `sfp repo:patch` command enables you to apply a specific release (or multiple releases) to a different branch by replacing the source code of packages with their corresponding versions from built artifacts. This is particularly useful for:

- **Hotfix scenarios** - Apply production fixes to development branches
- **Release synchronization** - Sync release branches with specific versions
- **Rollback preparation** - Create branches with previous release states
- **Environment alignment** - Ensure branches match deployed environments

## How It Works

The repo patch process:

1. Creates a temporary clone of your repository at the source branch
2. Creates a new target branch from the source
3. Fetches all artifacts specified in the release definition(s)
4. Replaces package directories with the exact code from the artifacts
5. Commits the changes to the target branch
6. Pushes the patched branch to the remote repository

```mermaid
graph LR
    A[Source Branch] --> B[Create Temp Clone]
    B --> C[Create Target Branch]
    C --> D[Fetch Artifacts]
    D --> E[Replace Package Code]
    E --> F[Commit Changes]
    F --> G[Push Target Branch]
```

## Command Usage

### Basic Usage

```bash
sfp repo:patch --releasedefinitions path/to/release.yaml \
  --sourcebranchname main \
  --targetbranchname hotfix/release-1.2.3
```

### With NPM Registry

```bash
sfp repo:patch --releasedefinitions path/to/release.yaml \
  --sourcebranchname main \
  --targetbranchname hotfix/release-1.2.3 \
  --npm \
  --scope mycompany
```

### With Custom Script (Without NPM)

When artifacts are stored in a custom registry or storage system (not npm), you can use a custom script:

```bash
sfp repo:patch --releasedefinitions path/to/release.yaml \
  --sourcebranchname main \
  --targetbranchname hotfix/release-1.2.3 \
  --scriptpath scripts/fetch-artifact.sh
```

The custom script receives three parameters:
1. Package name
2. Version
3. Target directory

Example script (`scripts/fetch-artifact.sh`):
```bash
#!/bin/bash
PACKAGE_NAME=$1
VERSION=$2
TARGET_DIR=$3

# Example: Download from S3
aws s3 cp s3://my-bucket/artifacts/${PACKAGE_NAME}-${VERSION}.zip ${TARGET_DIR}/
unzip ${TARGET_DIR}/${PACKAGE_NAME}-${VERSION}.zip -d ${TARGET_DIR}/${PACKAGE_NAME}

# Example: Download from Artifactory
curl -u ${ARTIFACTORY_USER}:${ARTIFACTORY_TOKEN} \
  -O ${TARGET_DIR}/${PACKAGE_NAME}.zip \
  https://artifactory.company.com/sfp-artifacts/${PACKAGE_NAME}/${VERSION}/${PACKAGE_NAME}.zip
unzip ${TARGET_DIR}/${PACKAGE_NAME}.zip -d ${TARGET_DIR}/${PACKAGE_NAME}
```

### Multiple Release Definitions

```bash
sfp repo:patch \
  --releasedefinitions release1.yaml \
  --releasedefinitions release2.yaml \
  --sourcebranchname develop \
  --targetbranchname release/sprint-23
```

## Command Flags

| Flag | Description | Required |
|------|-------------|----------|
| `-p, --releasedefinitions` | Path to release definition YAML file(s). Can be specified multiple times | Yes |
| `-s, --sourcebranchname` | Name of the source branch on which the alignment needs to be applied | Yes |
| `-t, --targetbranchname` | Name of the target branch to be created after the alignment | Yes |
| `-f, --scriptpath` | Path to script that authenticates and downloads artifacts from the registry (mutually exclusive with --npm) | No |
| `--npm` | Download artifacts from a pre-authenticated private npm registry (mutually exclusive with --scriptpath) | No |
| `--scope` | User or organization scope of the NPM package (required when using --npm) | No |
| `--npmrcpath` | Path to .npmrc file for authentication. Defaults to home directory | No |
| `--loglevel` | Logging level (trace, debug, info, warn, error, fatal) | No |

## Use Cases

### 1. Hotfix Application

When a production hotfix needs to be applied back to development:

```bash
# Apply production release back to develop branch
sfp repo:patch \
  --releasedefinitions releases/prod-hotfix-1.2.3.yaml \
  --sourcebranchname develop \
  --targetbranchname feature/apply-prod-hotfix
```

After patching, you can:
1. Review the changes
2. Create a pull request
3. Merge the hotfix back into development

### 2. Environment Synchronization

Ensure a branch matches exactly what's deployed in an environment:

```bash
# Sync UAT branch with UAT release
sfp repo:patch \
  --releasedefinitions releases/uat-release-2.0.0.yaml \
  --sourcebranchname main \
  --targetbranchname uat-sync
```

### 3. Rollback Preparation

Create a branch with a previous release state for potential rollback:

```bash
# Create rollback branch with previous release
sfp repo:patch \
  --releasedefinitions releases/previous-release.yaml \
  --sourcebranchname main \
  --targetbranchname rollback/v1.9.0
```

### 4. Release Branch Creation

Create a release branch with specific package versions:

```bash
# Create release branch with exact versions
sfp repo:patch \
  --releasedefinitions releases/sprint-23.yaml \
  --sourcebranchname develop \
  --targetbranchname release/sprint-23
```

## Integration with Release Workflow

The repo patch command fits into the release workflow at several points:

### After Production Release

```bash
# 1. Release to production
sfp release --path releases/prod-release.yaml \
  --targetusername prod

# 2. Patch the release back to develop
sfp repo:patch \
  --releasedefinitions releases/prod-release.yaml \
  --sourcebranchname develop \
  --targetbranchname feature/prod-sync

# 3. Create PR to merge changes
gh pr create --base develop --head feature/prod-sync
```

### During Hotfix Process

```bash
# 1. Create hotfix branch from production release
sfp repo:patch \
  --releasedefinitions releases/current-prod.yaml \
  --sourcebranchname main \
  --targetbranchname hotfix/urgent-fix

# 2. Make fixes on hotfix branch
git checkout hotfix/urgent-fix
# ... make changes ...

# 3. Build and release hotfix
sfp build --branch hotfix/urgent-fix
sfp release --path releases/hotfix.yaml --targetusername prod

# 4. Patch hotfix back to develop
sfp repo:patch \
  --releasedefinitions releases/hotfix.yaml \
  --sourcebranchname develop \
  --targetbranchname feature/apply-hotfix
```

## Important Considerations

### Package Structure
- The command replaces entire package directories
- Any local changes in the target branch packages will be overwritten
- Non-package files (configs, scripts) are preserved from the source branch

### Version Alignment
- Ensures exact version alignment with released artifacts
- Useful for maintaining version consistency across branches
- Helps prevent version drift between environments

### Git History
- Creates a clean commit with all package changes
- Preserves the relationship to the source branch
- Makes it easy to track what release was applied

## Troubleshooting

### Artifacts Not Found

```bash
# Ensure artifacts are available in the registry
sfp artifacts:query --npm --scope mycompany

# Or check local artifacts directory
ls -la artifacts/
```

### Package Not in Project

If a package exists in the release but not in the source branch:
- The package will be added to the project
- Update sfdx-project.json after patching if needed

### Merge Conflicts

After patching, if merging causes conflicts:
1. The patched branch has the exact release state
2. Carefully review conflicts
3. Generally, prefer the patched version for package code
4. Preserve local changes for configuration files

## Example Workflow

Here's a complete example of using repo patch in a release workflow:

```bash
# 1. Generate release definition for production
sfp releasedefinition:generate \
  --gitref main \
  --releaseconfig config/prod-release.yaml \
  --directory releases \
  --releasename "prod-v2.0.0"

# 2. Release to production
sfp release \
  --path releases/prod-v2.0.0.yaml \
  --targetusername production

# 3. Patch production release back to develop
sfp repo:patch \
  --releasedefinitions releases/prod-v2.0.0.yaml \
  --sourcebranchname develop \
  --targetbranchname feature/prod-v2-sync

# 4. Create and merge PR
git checkout feature/prod-v2-sync
git push origin feature/prod-v2-sync
gh pr create \
  --title "Sync develop with Production v2.0.0" \
  --body "Applying production release artifacts back to develop" \
  --base develop

# 5. After merge, continue development
git checkout develop
git pull origin develop
```

This ensures that your development branch always contains the exact code that's running in production, preventing drift between environments.