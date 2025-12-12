# Available Metrics

sfp is built with metrics on every key activity. The below table provides a list of metrics emitted from sfp.

## Metrics Destinations

### sfp Server (sfp-pro only)

|              | sfp-pro     | sfp (community) |
| ------------ | ----------- | --------------- |
| Availability | ✅           | ❌               |
| From         | January 26  |                 |

When using sfp-pro with `SFP_SERVER_URL` configured, all metrics are automatically sent to the built-in Victoria Metrics database. You can query these metrics using:

* `sfp metrics:query` - Ad-hoc queries with MetricsQL
* `sfp metrics:display` - Summary dashboards by category

See [sfp Server Metrics](configuring-collectors/sfp-server.md) for configuration details.

**Note:** Metrics ingestion requires an application token (used by CI/CD pipelines). User tokens are silently accepted but metrics are not stored.

### External Collectors

sfp also supports forwarding metrics to external observability platforms:

* [DataDog](configuring-collectors/datadog.md)
* [NewRelic](configuring-collectors/new-relic.md)
* [Splunk](configuring-collectors/splunk.md)
* [StatsD](configuring-collectors/statsd.md) - Connect to any StatsD-compatible collector

You can use multiple collectors simultaneously - metrics will be sent to all configured destinations.

## Metric Tags

All metrics include contextual tags that help you filter and aggregate data:

### Default Tags
- **branch**: Git branch name (when applicable)
- **package**: Package name (for package-specific metrics)
- **environment**: Target environment (for deployment/release metrics)
- **status**: Operation status (success/failure)
- **repo**: GitHub repository (automatically added in GitHub Actions)

### Custom Tags (sfp-pro only)
With sfp-pro, you can configure custom tags that are automatically added to ALL metrics. See [Custom Metrics](custom-metrics.md#custom-tags-configuration) for details on setting up organizational tags like team, cost-center, project, etc.

## Metrics Reference

| METRIC                                            | DESCRIPTION                                                                                                         | TYPE  |
| ------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------- | ----- |
| sfpowerscripts.deploy.failed                      | Number of times deploy command failed                                                                               | COUNT |
| sfpowerscripts.deploy.duration                    | Time spent on executing deploy command                                                                              | GAUGE |
| sfpowerscripts.deploy.scheduled                   | Number of times deployment was scheduled to run                                                                     | COUNT |
| sfpowerscripts.deploy.packages.scheduled          | Number of packages that were scheduled to be deployed by the deploy command                                         | GAUGE |
| sfpowerscripts.deploy.succeeded                   | Number of succeeded deploy executions                                                                               | COUNT |
| sfpowerscripts.deploy.succeeded.packages          | Number of packages that were successfully deployed                                                                  | GAUGE |
| sfpowerscripts.deploy.failed                      | Number of times deploy command failed to execute                                                                    | COUNT |
| sfpowerscripts.deploy.failed.packages             | Number and details of packages that failed to deploy                                                                | GAUGE |
| sfpowerscripts.build.scheduled                    | Number of times build was scheduled to run                                                                          | COUNT |
| sfpowerscripts.build.duration                     | Time spent on executing build command                                                                               | GAUGE |
| sfpowerscripts.build.scheduled.packages           | Number of packages being scheduled to build                                                                         | GAUGE |
| sfpowerscripts.build.succeeded.packages           | Number of packages successfully built                                                                               | GAUGE |
| sfpowerscripts.build.failed.packages              | Number of packages failed to build                                                                                  | GAUGE |
| sfpowerscripts.validate.scheduled                 | Number of scheduled validations                                                                                     | COUNT |
| sfpowerscripts.validate.succeeded                 | Number of successful validations                                                                                    | COUNT |
| sfpowerscripts.validate.failed                    | Number of time validate failed to execute                                                                           | COUNT |
| sfpowerscripts.validate.duration                  | Time spent on executing validate command                                                                            | GAUGE |
| sfpowerscripts.validate.packages.scheduled        | Number of packages scheduled for installation in a validation                                                       | GAUGE |
| sfpowerscripts.validate.packages.succeeded        | Number of successful package installations in a validation                                                          | GAUGE |
| sfpowerscripts.validate.packages.failed           | Number of failed package installations in a validation                                                              | GAUGE |
| sfpowerscripts.publish.duration                   | Time spent on executing publish command                                                                             | GAUGE |
| sfpowerscripts.publish.succeeded                  | Number of succeeded publish executions                                                                              | COUNT |
| sfpowerscripts.package.installation               | Number of times a package was installed                                                                             | COUNT |
| sfpowerscripts.package.installation.elapsed\_time | Time taken to install a package                                                                                     | GAUGE |
| sfpowerscripts.package.elapsed                    | Time taken to create a package                                                                                      | GAUGE |
| sfpowerscripts.package.created                    | Number of times a particular package was created                                                                    | COUNT |
| sfpowerscripts.package.metadatacount              | Number of metadata in a package                                                                                     | GAUGE |
| sfpowerscripts.package.testcoverage               | Test Coverage of a package                                                                                          | GAUGE |
| sfpowerscripts.apextests.triggered                | Number of times apex tests were triggered for a package                                                             | COUNT |
| sfpowerscripts.apextest.testtotal                 | Time taken for Apex Test Execution                                                                                  | GAUGE |
| sfpowerscripts.apextest.command.time              | Time taken for Apex Test Execution (Command Time)                                                                   | GAUGE |
| sfpowerscripts.prepare.succeededorgs              | Number of orgs that were succeeded during a run of prepare                                                          | GAUGE |
| sfpowerscripts.prepare.failedorgs                 | Number of orgs that failed during a run of prepare                                                                  | GAUGE |
| sfpowerscripts.prepare.duration                   | Time take to prepare a pool of scratchorgs                                                                          | GAUGE |
| sfpowerscripts.prepare.org.checkpointfailed       | Number of scratch orgs that failed on a checkpoint, during prepare                                                  | COUNT |
| sfpowerscripts.prepare.org.partial                | Number of scratch orgs that partially succeeded, during prepare                                                     | COUNT |
| sfpowerscripts.prepare.packages.scheduled         | Number of packages scheduled for installation when preparing scratch org pools                                      | GAUGE |
| sfpowerscripts.prepare.packages.succeeded         | Number of packages successfully installed when preparing scratch org pools                                          | GAUGE |
| sfpowerscripts.prepare.packages.failed            | Number of packages failed to install when preparing scratch org pools                                               | GAUGE |
| sfpowerscripts.so.packages.requested              | Number of packages requested to be installed to an individual scratch org in the pool                               | GAUGE |
| sfpowerscripts.so.package.installed               | Number of packages successfully installed to an individual scratch org in the pool                                  | GAUGE |
| sfpowerscripts.pool.available                     | Number of scratch orgs that are available in a pool after fetched by validate command                               | GAUGE |
| sfpowerscripts.pool.sandbox.succeededorgs         | Number of sandbox that was succesfully activated                                                                    | COUNT |
| sfpowerscripts.pool.sandbox.failedorgs            | Number of sandbox that failed to activate                                                                           | COUNT |
| sfpowerscripts.pool.sandbox.deleted               | Number of sandbox that was sucuesfully deleted                                                                      | COUNT |
| sfpowerscripts.pool.sandbox.deletefailed          | Number of sandbox that was failed to delete                                                                         | COUNT |
| sfpowerscripts.issueops.dev.sandbox.requested     | Number of dev sandboxes that were requested by raising an issue                                                     | COUNT |
| sfpowerscripts.issueops.dev.sandbox.failed        | Number of dev sandboxes that were failed during request                                                             | COUNT |
| sfpowerscripts.release.scheduled                  | Number of scheduled releases                                                                                        | COUNT |
| sfpowerscripts.release.succeeded                  | Number of successful releases                                                                                       | COUNT |
| sfpowerscripts.release.failed                     | Number of failed releases                                                                                           | COUNT |
| sfpowerscripts.release.duration                   | Time taken for a release                                                                                            | GAUGE |
| sfpowerscripts.release.packages.scheduled         | Number of packages scheduled for release                                                                            | GAUGE |
| sfpowerscripts.release.packages.succeeded         | Number of packages that were installed successfully in a release                                                    | GAUGE |
| sfpowerscripts.release.packages.failed            | Number of packages that failed to install in a release                                                              | GAUGE |
| sfpowerscripts.release.workitems                  | Aggregated count of workitems in a release (a release is identified by the identifier used in a release definition) | GAUGE |
| sfpowerscripts.release.commits                    | Aggregated count of commits in a release (a release is identified by the identifier used in a release definition)   | GAUGE |
