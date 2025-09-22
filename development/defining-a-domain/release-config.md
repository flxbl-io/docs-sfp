# Release Config

Release configuration is a fundamental setup that outlines the organisation of packages within a project, streamlining across different lifecycle of your project, such as validating, building, deploying/release of artifacts. In flxbl projects, a release config is used to define the concept of a domain/subdomains.\
\
This configuration is instrumental when using sfp commands, as it allows for selective operations on specified packages defined by a configuration. By employing a release configuration, teams can efficiently manage a mono repository of packages  across various teams.&#x20;



```
---
​releaseName: core
pool: core_pool
includeOnlyArtifacts:
  - src-env-specific-pre
  - src-env-specific-alias-pre
  - core-crm
  - telephony-service
excludePackageDependencies:
  - Genesys Cloud for Salesforce
  - Marketing Cloud
releasedefinitionProperties:
  changelog:
    workItemFilters:
      -  BRO-[0-9]{3,4}
    workItemUrl: https://bro.atlassian.net/browse
    limit: 30
```

The below table list the options that are currently available for release configuration

<table><thead><tr><th>Parameter</th><th>Required</th><th width="134">Type</th><th>Description</th></tr></thead><tbody><tr><td>releaseName</td><td>No</td><td>String</td><td>Name of the release config, in flxbl project, this name is used as the name of the domain</td></tr><tr><td>pool</td><td>No</td><td>String</td><td>Name of the scratch org or sandbox pool associated with this release config during validation</td></tr><tr><td>excludeArtifacts</td><td>No</td><td>Array</td><td>An array of artifacts that need to be excluded while creating the release definition</td></tr><tr><td>includeOnlyArtifacts</td><td>No</td><td>Array</td><td>An array of artifacts that should only be included while creating the release definition</td></tr><tr><td>dependencyOn</td><td>No</td><td>Array</td><td>An array of packages  that denotes the dependency this configuration has.  The dependencies mentioned  will be used for synchronization in review sandboxes</td></tr><tr><td>excludePackageDependencies</td><td>No</td><td>Array</td><td>Exclude the mentioned package dependencies from the release definition</td></tr><tr><td>includeOnlyPackageDependencies</td><td>No</td><td>Array</td><td>Include only the mentioned package dependencies from the release definition</td></tr><tr><td>releasedefinitionProperties</td><td>No</td><td>Object</td><td>Properties of release definition that should be added to the generated release definition. See below</td></tr></tbody></table>

A release configuration also can contain additional options that can be used by certain sfp commands to generate [release definitions](../../releasing-artifacts/release-definitions.md). These properties in a release definiton alters the behaviour of deployment of artifacts during a release



### Release Definition Properties

| Parameter                                                        | Required | Type    | Description                                                                                                                                                    |
| ---------------------------------------------------------------- | -------- | ------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| releasedefinitionProperties.skipIfAlreadyInstalled               | No       | boolean | Skip installation of artifact if it's already installed in target org                                                                                          |
| releasedefinitionProperties.baselineOrg                          | No       | string  | The org used to decide whether to to skip installation of an artifact. Defaults to the target org when not provided                                            |
| releasedefinitionProperties.promotePackagesBeforeDeploymentToOrg | No       | string  | Promote packages before they are installed into an org that matches alias of the org                                                                           |
| releasedefinitionProperties.changelog.repoUrl                    | No       | Prop    | The URL of the version control system to push changelog files                                                                                                  |
| releasedefinitionProperties.changelog.workItemFilters            | No       | Prop    | An array of regular expression used to identify work items in your commit messages                                                                             |
| releasedefinitionProperties.changelog.workitemUrl                | No       | Prop    | The generic URL of work items, to which to append work item codes. Allows easy redirection to user stories by clicking on the work-item link in the changelog. |
| releasedefinitionProperties.changelog.limit                      | No       | Prop    | Limit the number of releases to display in the changelog markdown                                                                                              |
| releasedefinitionProperties.changelog.showAllArtifacts           | No       | Prop    | Whether to show artifacts that haven't changed between releases                                                                                                |
