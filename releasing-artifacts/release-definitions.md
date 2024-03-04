# Release Definitions

A release definition is a YAML file that carries attributes of artifacts and other details. A release definition is the main input for a release command.&#x20;

```
release: Sprint-2-13-11-6844956242
skipIfAlreadyInstalled: true
artifacts:
  feature-management: 1.0.19-6844956242
  apex-logger: 1.0.20-89
promotePackagesBeforeDeploymentToOrg: prod
changelog:
  workItemFilters:
    - (FGK|FFK)-[0-9]{3,4}
  workItemUrl: https://flxbl.atlassian.net/browse
  limit: 30
```

Lets examine closely the above release defintion file and understand the various attributes and it purposes

```
release: Sprint-2-13-11-6844956242
```

This defines the name of the release. this value will be utilised to generate changelogs or as an identification for other systems to understand what release went into an org&#x20;

```
skipIfAlreadyInstalled: true
```

This attribute determines whether artifacts should be skipped from installing if the same version number is already installed in the target org

```
artifacts:
  feature-management: 1.0.19-6844956242
  apex-logger: 1.0.20-89
```

This attribute determines which artifacts constitute the provided release.&#x20;

{% hint style="info" %}
Please note, when using release definition with exact version numbers. The format of the version number would be \<packageName>: X.Y.Z-BuildNumber, where the version number adopts the following semantics\
X - Major\
Y - Minor\
Z - Patch\
BuildNumber - The build number that produced the package\
\
The version exactly follows the semantics used by the artifact registry (such as Github Packages). However the format is slightly different from the version that is tagged in the git repository where it follows X.Y.Z.BuildNumber

**Example**:\
A package that has the version core\_crm\_v3.0.0.6936, tagged in git should be referenced\
as core\_crm: 3.0.0-6936&#x20;
{% endhint %}

```
promotePackagesBeforeDeploymentToOrg: <targetOrgAlias>
```

The above attribute determines whether an artifact of type unlocked package should be promoted before installing into the target org as provided by the alias. If the alias of the requested org and the targeOrgAlias matches, the unlocked packages are promoted before installing into the org

```
changelog:
  workItemFilters:
    - (FGK|FFK)-[0-9]{3,4}
  workItemUrl: https://flxbl.atlassian.net/browse
  limit: 30
```

The reminder of the attributes are about generating change log, which are explained [here](generating-a-changelog.md)



Here is a run down of all attributes that make up a release definition

| Parameter                            | Required | Type    | Description                                                                                                                                                    |
| ------------------------------------ | -------- | ------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| release                              | Yes      | string  | Name of the release                                                                                                                                            |
| skipIfAlreadyInstalled               | No       | boolean | Skip installation of artifact if it's already installed in target org                                                                                          |
| baselineOrg                          | No       | string  | The org used to decide whether or not to skip installation of an artifact. Defaults to the target org when not provided.                                       |
| artifacts                            | Yes      | Object  | Map of artifacts to deploy and their corresponding version                                                                                                     |
| promotePackagesBeforeDeploymentToOrg | No       | string  | Promote packages before they are installed into an org that matches alias of the org                                                                           |
| packageDependencies                  | No       | Object  | Packages dependencies (e.g. managed packages) to install as part of the release. Provide the 04t subscriber package version Id.                                |
| changelog.repoUrl                    | No       | Prop    | The URL of the version control system to push changelog files                                                                                                  |
| changelog.workItemFilters            | No       | Prop    | An array of regular expression used to identify work items in your commit messages                                                                             |
| changelog.workitemUrl                | No       | Prop    | The generic URL of work items, to which to append work item codes. Allows easy redirection to user stories by clicking on the work-item link in the changelog. |
| changelog.limit                      | No       | Prop    | Limit the number of releases to display in the changelog markdown                                                                                              |
| changelog.showAllArtifacts           | No       | Prop    | Whether to show artifacts that haven't changed between releases                                                                                                |
