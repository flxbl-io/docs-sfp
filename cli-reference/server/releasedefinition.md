# Release Definition

## `sfp server releasedefinition`

Generate and manage release definitions based on artifacts using git tags

### Commands

* [`sfp server releasedefinition generate`](#sfp-server-releasedefinition-generate) - Generate a release definition and publish to server

---

### `sfp server releasedefinition generate`

Generate a release definition based on artifacts using git tags and publish to server.

```
USAGE
  $ sfp server releasedefinition generate -c <value> -f <value> -n <value> [--json]
    [--repository <value>] [-e <value> | -t <value>] [--sfp-server-url <value>]
    [-b <value>] [-m <value>] [--skip-publish] [--loglevel
    trace|debug|info|warn|error|fatal|TRACE|DEBUG|INFO|WARN|ERROR|FATAL]

FLAGS
  -n, --releasename=<value>         (required) Set a release name on the release definition
  -c, --gitref=<value>              (required) Git reference (commit SHA, tag, or HEAD) to generate from
  -f, --configfile=<value>          (required) Path to the release config file
  -b, --branch=<value>              Branch name for the release definition
  -m, --metadata=<value>            Additional metadata in JSON format
  --repository=<value>              Repository identifier (e.g., owner/repo)
  --skip-publish                    Skip publishing the release definition to server
  
  AUTHENTICATION
  -e, --email=<value>               Email address for authenticated CLI user
  -t, --application-token=<value>   Application token for authentication (CI/CD)
  --sfp-server-url=<value>          URL of the SFP server
  
  OTHER OPTIONS
  --json                            Format output as json
  --loglevel=<option>               [default: info] logging level for this command invocation
                                    <options: trace|debug|info|warn|error|fatal|TRACE|DEBUG|INFO|WARN|ERROR|FATAL>

DESCRIPTION
  Generate a release definition based on artifacts using git tags and publish to server

  This command:
  1. Analyzes git tags at the specified reference
  2. Identifies artifacts and their versions
  3. Creates a release definition based on the config file
  4. Publishes the definition to the SFP server (unless --skip-publish)

EXAMPLES
  $ sfp server releasedefinition generate -n MyRelease -c HEAD -f config/release.yml

  $ sfp server releasedefinition generate -n MyRelease -c HEAD -b main -f config/release.yml

  $ sfp server releasedefinition generate -n MyRelease -c abc123 -f config/release.yml -m '{"env":"prod"}'

  $ sfp server releasedefinition generate -n MyRelease -c HEAD -b develop -f config/release.yml --skip-publish

  $ sfp server releasedefinition generate -n "v2.3.0" -c v2.3.0 -f config/prod-release.yml --repository myorg/myrepo
```

### Release Configuration File

The release configuration file defines how the release should be generated:

```yaml
# config/release.yml
name: Production Release
includeOnlyArtifacts:
  - core-package
  - auth-module
  - ui-components
excludeArtifacts:
  - test-package
  - dev-tools
baselineOrg: production
promotePackagesBeforeDeploymentToOrg: production
skipIfAlreadyInstalled: true
changelog:
  workItemFilters:
    - "JIRA-*"
    - "GH-*"
  limit: 200
  showAllArtifacts: false
```

### Common Scenarios

#### Production Release

```bash
# Generate release from latest main branch
sfp server releasedefinition generate \
  -n "Production-$(date +%Y%m%d)" \
  -c HEAD \
  -b main \
  -f config/production-release.yml \
  --repository myorg/myrepo
```

#### Staging Release

```bash
# Generate from specific commit
sfp server releasedefinition generate \
  -n "Staging-v2.3.0" \
  -c abc123def \
  -b develop \
  -f config/staging-release.yml \
  -m '{"environment":"staging","version":"2.3.0"}'
```

#### Release from Tag

```bash
# Generate from git tag
sfp server releasedefinition generate \
  -n "Release-v2.3.0" \
  -c v2.3.0 \
  -f config/release.yml \
  --repository myorg/myrepo
```

### Adding Metadata

Include additional metadata in the release definition:

```bash
sfp server releasedefinition generate \
  -n "MyRelease" \
  -c HEAD \
  -f config/release.yml \
  -m '{
    "environment": "production",
    "approvedBy": "release-manager@example.com",
    "ticketNumber": "REL-1234",
    "notes": "Q1 2024 Release"
  }'
```

### CI/CD Integration

#### GitHub Actions

```yaml
name: Generate Release Definition
on:
  push:
    tags:
      - 'v*'

jobs:
  release:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2
        with:
          fetch-depth: 0
      
      - name: Generate Release Definition
        env:
          SFP_APPLICATION_TOKEN: ${{ secrets.SFP_TOKEN }}
          SFP_SERVER_URL: ${{ secrets.SFP_SERVER_URL }}
        run: |
          sfp server releasedefinition generate \
            -n "Release-${{ github.ref_name }}" \
            -c ${{ github.sha }} \
            -b main \
            -f config/release.yml \
            --repository ${{ github.repository }} \
            -m '{"tag":"${{ github.ref_name }}","triggered_by":"${{ github.actor }}"}'
```

#### Jenkins Pipeline

```groovy
pipeline {
    agent any
    parameters {
        string(name: 'RELEASE_NAME', defaultValue: '', description: 'Release name')
        string(name: 'GIT_REF', defaultValue: 'HEAD', description: 'Git reference')
    }
    environment {
        SFP_APPLICATION_TOKEN = credentials('sfp-token')
        SFP_SERVER_URL = 'https://sfp.example.com'
    }
    stages {
        stage('Generate Release') {
            steps {
                sh """
                    sfp server releasedefinition generate \
                        -n "${params.RELEASE_NAME}" \
                        -c "${params.GIT_REF}" \
                        -b main \
                        -f config/release.yml \
                        --repository myorg/myrepo
                """
            }
        }
    }
}
```

### Output Formats

#### Standard Output

```
Generating release definition: MyRelease
Git reference: HEAD (abc123def456)
Branch: main
Config file: config/release.yml

Analyzing artifacts...
✓ Found 12 artifacts with git tags
✓ core-package: v1.2.3
✓ auth-module: v2.0.1
✓ ui-components: v3.1.0
...

Creating release definition...
✓ Release definition created

Publishing to server...
✓ Published successfully

Release Definition: MyRelease
ID: rel_abc123
Status: Published
URL: https://sfp.example.com/releases/rel_abc123
```

#### JSON Output

```json
{
  "releaseName": "MyRelease",
  "releaseId": "rel_abc123",
  "gitRef": "abc123def456",
  "branch": "main",
  "artifacts": {
    "core-package": "v1.2.3",
    "auth-module": "v2.0.1",
    "ui-components": "v3.1.0"
  },
  "metadata": {
    "environment": "production",
    "createdAt": "2024-01-15T10:30:00Z",
    "createdBy": "user@example.com"
  },
  "status": "published",
  "url": "https://sfp.example.com/releases/rel_abc123"
}
```

### Best Practices

1. **Use Semantic Versioning**: Name releases with semantic version tags
2. **Include Metadata**: Add relevant metadata for tracking and auditing
3. **Version Control Config**: Keep release config files in version control
4. **Automate Generation**: Integrate with CI/CD for consistent releases
5. **Test First**: Use `--skip-publish` to test generation before publishing

### Troubleshooting

#### No Artifacts Found

```bash
# Check git tags
git tag -l

# Ensure tags are pushed
git push --tags
```

#### Invalid Git Reference

```bash
# Verify the reference exists
git rev-parse <gitref>

# For tags, ensure they're fetched
git fetch --tags
```

> **Note**: The git reference must be accessible in the current repository. Ensure you have fetched all necessary tags and branches.

> **Tip**: Use `--skip-publish` during testing to validate the release definition generation without publishing to the server.