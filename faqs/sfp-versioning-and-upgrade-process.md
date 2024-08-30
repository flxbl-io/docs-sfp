# sfp versioning and upgrade Process

## Versioning Strategy

sfp follows Semantic Versioning 2.0.0 (SemVer) for version numbering. This means version numbers follow the format MAJOR.MINOR.PATCH, where:

* MAJOR version changes indicate incompatible API changes
* MINOR version changes add functionality in a backwards-compatible manner
* PATCH version changes make backwards-compatible bug fixes

sfp uses release-please to manage releases, ensuring consistent and automated version management.

## Release Channels and Frequency

sfp is available in two editions:

1. sfp Community Edition: Released quarterly
2. sfp Pro: Released monthly (for licensed users only)

Both editions will receive immediate releases for critical issues such as security vulnerabilities or blockers due to Salesforce upgrades.

sfp offers three release channels:

1. alpha: Early access to new features, may contain bugs
2. beta: More stable than alpha, but still in testing
3. latest: The stable release channel

## Upgrading sfp

#### Recommended Upgrade Method

The recommended method for using and upgrading sfp is through Docker images. This approach ensures consistency across different environments and simplifies the upgrade process.

For CI/CD workflows:

```yaml
# Example Docker image reference in a CI/CD configuration
image: ghcr.io/flxbl-io/sfp:X.Y.Z  # Replace X.Y.Z with the specific version number
```

If using npm (not recommended for production environments):

```bash
npm install -g @flxbl-io/sfp@X.Y.Z  # Replace X.Y.Z with the specific version number
```

Always use specific version numbers rather than tags like "latest" to ensure reproducibility.

## Upgrade Considerations

1. **Review Release Notes**: Before upgrading, always review the release notes for any breaking changes or new features. These can be found in the GitHub repository.
2. **Test Before Deployment**: For large projects, it's recommended to test the new version in a separate test project before rolling it out to your main development environment.
3. **CI/CD Environments**: When upgrading in CI/CD environments, update the Docker image reference to the new version number. Ensure all jobs and pipelines are updated simultaneously to maintain consistency.
4. **Backup**: While not strictly necessary due to the nature of sfp as a CLI tool, it's always a good practice to ensure your project files are backed up before performing any significant upgrades.
5. **Compatibility**: There are no automated tools to check for compatibility issues. Refer to the release notes for any known compatibility concerns.

## Changelog and Notifications

* Changelogs are published on the sfp GitHub repository for each release.
* Users will be notified of new releases and important changes in the Flxbl Slack community.

## Long-Term Support (LTS)

Currently, sfp does not have a formal Long-Term Support policy. Users are encouraged to stay up-to-date with the latest stable release.

## Rollback Procedure

If issues are encountered after an upgrade:

1. Revert to the previous version by specifying the old version number in your Docker image reference or npm install command.
2. Report the issue on the sfp GitHub repository or Flxbl Slack community.

Remember, using Docker images with specific version numbers makes it easy to switch between versions if needed.
