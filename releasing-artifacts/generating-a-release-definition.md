# Generating a release definition

In a high velocity project operating on a trunk such as a #flxbl project  and with substantial number of packages, manually generating release definition can be a chore. This can eased by using generating the release definition file automatically after every publish command. \
\
One can utilise [release config ](../configuring-a-project/release-config.md)along with release definition generate command to automate the process of generating release definitions.&#x20;

```bash
# Community Edition
sfp releasedefinition:generate \
  --gitref main \
  --configfile config/release-config.yaml \
  --releasename "Release-2.0.0" \
  --directory releases \
  --branchname releasedefns

# sfp-pro Server Edition
sfp server releasedefinition:generate \
  --gitref HEAD \
  --configfile config/release-config.yaml \
  --releasename "Release-2.0.0" \
  --repository myorg/myrepo \
  --branch main
```

## Command Attributes

### Community Edition (sfp releasedefinition:generate)

| Flag | Description | Required |
|------|-------------|----------|
| `-c, --gitref` | Git reference (branch/tag/commit) to use for artifact selection | Yes |
| `-f, --configfile` | Path to the release configuration YAML file | Yes |
| `-n, --releasename` | Name of the release for the definition | Yes |
| `-b, --branchname` | Repository branch to push the definition to | No |
| `-d, --directory` | Directory to write the release definition file | No |
| `--nopush` | Create definition locally without pushing to repository | No |
| `--forcepush` | Force push changes to the repository branch | No |
| `-m, --metadata` | Additional metadata to include in the release definition | No |

### sfp-pro Edition (sfp server releasedefinition:generate)

| Flag | Description | Required |
|------|-------------|----------|
| `-c, --gitref` | Git reference (branch/tag/commit) to use for artifact selection | Yes |
| `-f, --configfile` | Path to the release configuration YAML file | Yes |
| `-n, --releasename` | Name of the release for the definition | Yes |
| `-b, --branch` | Branch name for context (defaults to current branch) | No |
| `--repository` | Repository identifier in the format org/repo | Yes |
| `--skip-publish` | Generate definition without publishing to server | No |
| `-m, --metadata` | Additional metadata to include in the release definition | No |
| `--email` | Email of the user generating the release | No |

## How It Works

The command:
1. Reads the release configuration file to understand which packages to include
2. Uses the specified git reference to determine the latest artifact versions
3. Generates a release definition YAML with the correct versions
4. Either saves locally or publishes to the server/repository

For the community edition, if `--nopush` is not specified, the definition is pushed to the specified branch. For sfp-pro server edition, the definition is published to the server unless `--skip-publish` is used.

