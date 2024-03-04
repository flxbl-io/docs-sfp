# Fetching Artifacts

The fetch command provides you with the ability to fetch artifacts from your artifact registry to your local file system. Artifacts once fetched from your registry can the be used for installation or for further analysis\
\
The fetch command requires a release definition file as the input.  \
\
Let's assume you have the following release definition file with the name of the file as `Sprint2-13-11.yaml`

```
release: Sprint-2-13-11-6844956242
skipIfAlreadyInstalled: true
skipArtifactUpdate: false
artifacts:
  feature-management: 1.0.19-6844956242
promotePackagesBeforeDeploymentToOrg: prod
changelog:
  workItemFilters:
    - (FGK|FFK)-[0-9]{3,4}
  workItemUrl: https://flxbl.atlassian.net/browse
  limit: 30
```

One can use the fetch command to fetch all the artifacts by issuing the command as shown below

```
sfp artifacts fetch -p Sprint2-13-11.yaml --npm --scope flxbl 
```

Please note the scope flag, the scope flag should match exactly the scope that was provided during publish command
