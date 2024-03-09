# Limiting artifacts to be built

Artifacts to be built can be limited by various mechanisms.  This section deals with various techniques to limit artifacts being built

### Limiting artifacts by domain

Artifacts to be built  can be restricted by [domain](../concepts/domains.md) during a build process in **sfp** by utilizing specific configurations. Consider the [release config ](../configuring-a-project/release-config.md)example provided in

```yaml
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

You can  use the path to the config file to  the build command to limit the artifacts being built as in the sample below

<pre><code><strong>// Limit build by release config
</strong><strong>sfp build -v devhub --branch=main --domain config/sales.yaml
</strong></code></pre>

### Limiting artifacts by packages

Artifacts to be built can be limited only to certain packages. This could be very helpful, in case you want to build  an artifact of only one package or a few while testing locally.

<pre><code><strong>// Limit build by certain packages
</strong><strong>sfp build -v devhub --branch=main -p sales-ui,sales-channels
</strong></code></pre>

### Limiting artifacts by ignoring packages

Packages can be ignored from the build command  by utilising an additional descriptor on the package in  project manifest (sfdx-project.json)



<pre><code>// Sample sfdx project json 
{
  "packageDirectories": [
    {
      "path": "./src-env-specific-pre",
      "package": "src-env-specific-pre",
      "versionNumber": "1.0.0.0",
      "<a data-footnote-ref href="#user-content-fn-1">ignoreOnStage</a>": [
        "build"
      ]
    },
    {
      "path": "./src/frameworks/feature-mgmt",
      "package": "feature-mgmt",
      "versionNumber": "1.0.0.NEXT"
    }
  ]
}
</code></pre>

In the above scenario, src-env-specific-pre will be ignored  while build command is invoked

[^1]: Add ignoreOnStage to ignore the package being built
