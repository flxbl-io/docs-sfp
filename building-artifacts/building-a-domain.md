# Building a domain

Assume you have a domain 'sales' as defined  by release config `sales.yaml` provided as shown in this example here.

```
# release-config for sales
releaseName: sales
pool: sales-pool
excludeAllPackageDependencies: true 
includeOnlyArtifacts:
  - sales-ui
  - sales-channels
releasedefinitionProperties:
  skipIfAlreadyInstalled: true
  usePackageDiffDeploymentMode: true
  promotePackagesBeforeDeploymentToOrg: prod
  changelog:
    workItemFilters:
      -  (AKG|GIK)-[0-9]{2,5}
    workItemUrl: https://example.atlassian.net/browse
    limit: 30
```

In order to build the artifact of the packages defined by the above release config, you would use the  the build command with the flags as described here.

```
sfp build --releaseconfig sales.yaml -v devhub --branch main 
```

If you require only to build packages that's changed form the last published packages, you would add an additional diffcheck flag.&#x20;

```
sfp build --releaseconfig sales.yaml -v devhub --branch main  --diffcheck
```

{% hint style="warning" %}
diffcheck will work accurately only if the build command is able to access the latest tag in the repository. In certain CI system if the command is operated on a repository where only the head commit is checked out, diffchek will result in building all the artifacts for all packages within the domain
{% endhint %}
