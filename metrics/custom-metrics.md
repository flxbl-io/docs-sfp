# Custom Metrics

sfp provides multiple ways to enhance your metrics with custom data:
1. Report custom metrics using the `metrics:report` command
2. Add organization-wide custom tags to all metrics (sfp-pro only)

### `sfp metrics report`

Report a custom metric to any sfp supported metric provider

```
USAGE
  $ @flxbl-io/sfp metrics report -m <value> -t gauge|counter|timer [-v <value>] [-g <value>] [--loglevel
    trace|debug|info|warn|error|fatal|TRACE|DEBUG|INFO|WARN|ERROR|FATAL]

FLAGS
  -g, --tags=<value>       tags for metric
  -m, --metric=<value>     (required) metrics to publish
  -t, --type=<option>      (required) type of metric
                           <options: gauge|counter|timer>
  -v, --value=<value>      value of metric
      --loglevel=<option>  [default: info] logging level for this command invocation
                           <options: trace|debug|info|warn|error|fatal|TRACE|DEBUG|INFO|WARN|ERROR|FATAL>

DESCRIPTION
  Report a custom metric to any sfp supported metric provider

EXAMPLES
  $ sfp metrics:report -m <metric> -t <type> -v <value>
```

## Custom Tags Configuration

|              | sfp-pro     | sfp (community) |
| ------------ | ----------- | --------------- |
| Availability | ✅           | ❌               |
| From         | December 24 |                 |

sfp-pro allows you to configure custom tags that will be automatically added to ALL metrics sent by sfp. This is useful for adding consistent organizational metadata without modifying individual commands.

### Setting Custom Tags

Use the `sfp config set` command with the `custom-tag.*` prefix:

```bash
# Set a team tag
sfp config set custom-tag.team platform-engineering

# Set an environment tag
sfp config set custom-tag.environment production

# Set a repository tag
sfp config set custom-tag.repository myorg/myrepo

# Set any custom tag
sfp config set custom-tag.<tagname> <value>
```

### Viewing Custom Tags

To see all configured custom tags:

```bash
# List all config including custom tags
sfp config list

# Filter for custom tags only
sfp config list | grep custom-tag
```

### Removing Custom Tags

```bash
# Unset a specific tag
sfp config unset custom-tag.team

# Unset all custom tags (Unix/Linux/Mac)
sfp config list | grep custom-tag | cut -d'=' -f1 | xargs -I {} sfp config unset {}
```

### Scope of Custom Tags

Custom tags follow the standard sfp configuration hierarchy:

- **Global tags**: Set without `--scope local`, applies to all projects
- **Project-specific tags**: Set with `--scope local`, applies only to current project
- Project tags override global tags when both are present

```bash
# Set a global tag
sfp config set custom-tag.organization acme-corp

# Set a project-specific tag
sfp config set custom-tag.project frontend --scope local
```

### How Custom Tags Work

1. **Automatic Inclusion**: Custom tags are automatically added to every metric sent by sfp
2. **Tag Precedence**: Command-specific tags override custom tags if there's a conflict
3. **GitHub Integration**: Repository tags are automatically added when running in GitHub Actions
4. **Backend Support**: Works with all supported metrics backends (StatsD, DataDog, NewRelic, Splunk)

### Example Workflow

```bash
# Configure organizational tags
sfp config set custom-tag.team devops
sfp config set custom-tag.environment staging
sfp config set custom-tag.cost-center engineering

# Run any sfp command - tags are automatically included
sfp build --branch main
# Metrics sent: sfpowerscripts.build.scheduled
# Tags: team=devops, environment=staging, cost-center=engineering, branch=main

sfp release --environment uat
# Metrics sent: sfpowerscripts.release.scheduled
# Tags: team=devops, environment=staging, cost-center=engineering, env=uat
```

### Use Cases

#### Team Attribution
Track metrics by team for resource allocation and performance monitoring:
```bash
sfp config set custom-tag.team platform-team
sfp config set custom-tag.squad alpha
```

#### Multi-Environment Tracking
Differentiate metrics across environments:
```bash
# In CI/CD pipeline for staging
sfp config set custom-tag.environment staging
sfp config set custom-tag.region us-west-2

# In CI/CD pipeline for production
sfp config set custom-tag.environment production
sfp config set custom-tag.region us-east-1
```

#### Cost Center Tracking
Associate metrics with cost centers for chargeback:
```bash
sfp config set custom-tag.cost-center CC-12345
sfp config set custom-tag.business-unit retail
```

#### Project Metadata
Add project-specific context:
```bash
sfp config set custom-tag.project customer-portal --scope local
sfp config set custom-tag.version v2.3.0 --scope local
sfp config set custom-tag.release-cycle Q1-2024 --scope local
```

### Best Practices

1. **Establish Naming Conventions**: Define standard tag names across your organization
2. **Use Hierarchical Tags**: Organize tags logically (e.g., `team`, `squad`, `project`)
3. **Automate Tag Configuration**: Set tags in CI/CD pipeline initialization
4. **Document Required Tags**: Maintain a list of required tags for your organization
5. **Regular Audits**: Periodically review and clean up unused tags

### CI/CD Integration

Configure custom tags in your CI/CD pipeline:

#### GitHub Actions
```yaml
- name: Configure Custom Tags
  run: |
    sfp config set custom-tag.team ${{ vars.TEAM_NAME }}
    sfp config set custom-tag.environment ${{ github.ref_name }}
    sfp config set custom-tag.pipeline github-actions
    sfp config set custom-tag.run-id ${{ github.run_id }}
```

#### Jenkins
```groovy
stage('Configure Metrics') {
    steps {
        sh """
            sfp config set custom-tag.team ${env.TEAM_NAME}
            sfp config set custom-tag.environment ${env.BRANCH_NAME}
            sfp config set custom-tag.pipeline jenkins
            sfp config set custom-tag.build-number ${env.BUILD_NUMBER}
        """
    }
}
```

#### Azure DevOps
```yaml
- script: |
    sfp config set custom-tag.team $(TeamName)
    sfp config set custom-tag.environment $(Build.SourceBranchName)
    sfp config set custom-tag.pipeline azure-devops
    sfp config set custom-tag.build-id $(Build.BuildId)
  displayName: 'Configure Custom Tags'
```

### Limitations

- Custom tags are loaded once at command initialization
- Tag values must be strings
- Tag names should follow your metrics backend's naming conventions
- Maximum number of tags depends on your metrics backend limitations

> **Note**: Custom tags are only available in sfp-pro edition. Community edition users must add tags manually to each command or upgrade to sfp-pro.

