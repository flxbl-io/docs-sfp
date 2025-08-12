# Limiting Validation by Domain

Validation processes often need to synchronize the provided organization by installing packages that differ from those already installed. This task can become particularly time-consuming for large projects with hundreds of packages, especially in monorepo setups with multiple independent domains.

## Using Release Configurations

To streamline the validation process and focus it on specific domains, you can use release configurations with the `--releaseconfig` flag. This approach limits the scope of validation to only the packages defined in your release configuration, significantly enhancing efficiency and reducing validation time.

### Basic Usage

```bash
sfp validate org -o ci \
                 -v devhub \
                 --mode thorough \
                 --releaseconfig=config/release-domain-sales.yml
```

In this example, validation is limited to packages defined in the `release-domain-sales.yml` configuration file. Only packages that:
1. Are listed in the release configuration AND
2. Have changes compared to what's installed in the org

will be validated.

### Multiple Domain Configurations

For projects with multiple independent domains, you can specify multiple release configurations:

```bash
sfp validate org -o ci \
                 -v devhub \
                 --mode thorough \
                 --releaseconfig config/release-sales.yml \
                 --releaseconfig config/release-service.yml
```

## Benefits of Domain-Limited Validation

1. **Faster Feedback**: Validate only the relevant packages for your team's domain
2. **Reduced Dependencies**: Avoid failures from unrelated packages in other domains
3. **Parallel Development**: Multiple teams can work independently without blocking each other
4. **Optimized CI/CD**: Shorter validation times mean more efficient pipeline execution

## Example Release Configuration

```yaml
# config/release-sales.yml
name: Sales Domain
includeOnlyPackages:
  - sales-core
  - sales-ui
  - sales-integrations
  - opportunity-management
  - quote-management
skipPackages:
  - sales-deprecated
```

## Combining with Other Options

### With Diff Check
```bash
sfp validate org -o ci \
                 -v devhub \
                 --diffcheck \
                 --releaseconfig=config/release.yml
```

### With Individual Mode
```bash
sfp validate org -o ci \
                 -v devhub \
                 --mode individual \
                 --releaseconfig=config/release.yml
```

### With Branch References
```bash
sfp validate org -o ci \
                 -v devhub \
                 --ref feature-branch \
                 --baseRef main \
                 --releaseconfig=config/release.yml
```

## Best Practices

1. **Organize by Domain**: Create separate release configurations for each logical domain
2. **Keep Configurations Updated**: Regularly review and update package lists in release configs
3. **Use in CI/CD**: Automate domain-specific validation in your pipeline
4. **Document Dependencies**: Clearly document cross-domain dependencies in your configurations

> **Note**: The deprecated `--mode=thorough-release-config` and `--mode=ff-release-config` have been replaced by using the standard modes with the `--releaseconfig` flag. This provides the same functionality with a simpler, more consistent interface.



