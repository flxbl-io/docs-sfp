# Defining a pool

Pools are defined by creating a pool definition file.  The file can be placed in a directory within your repository. \


The below configuration details a pool with tag 'DEV-POOL'  allocated with 20 scratch orgs.  Read on below to understand other configurations that are available &#x20;

```
{
    "tag": "DEV-POOL",
    "maxAllocation": 20,
    "expiry": 10,
    "batchSize": 10,
    "configFilePath": "config/project-scratch-def.json",
    "relaxAllIPRanges": true,
    "installAll": true,
    "enableSourceTracking": true,
    "retryOnFailure": true,
    "succeedOnDeploymentErrors": true,
    "fetchArtifacts": {
        "npm": {
            "scope": "myproject",
    }
}
```





The latest JSON schema for scratch org pool configuration can be found[ here.](https://github.com/dxatscale/sfpowerscripts/blob/main/packages/core/resources/pooldefinition.schema.json)​\
\
The `pool prepare` command accepts a JSON configuration file that defines settings and attributes of the scratch org pool. The properties accepted by the configuration file are shown in the table below.

| Property                            | Type    | Description                                                                                                           |
| ----------------------------------- | ------- | --------------------------------------------------------------------------------------------------------------------- |
| tag                                 | string  | Name used to identify the scratch org pool                                                                            |
| waitTime                            | int     | Minutes the command should wait for a scratch org to be created, Default is 6 minutes                                 |
| expiry                              | int     | Number of days for which the scratch orgs are active                                                                  |
| maxAllocation                       | int     | Maximum capacity of the pool                                                                                          |
| batchSize                           | int     | Number of processes for creating scratch orgs in parallel                                                             |
| configFilePath                      | string  | Path to scratch org definition JSON file                                                                              |
| succeedOnDeploymentErrors           | boolean | Whether to persist scratch org to the pool for a deployment error, default:true                                       |
| snapshotPool                        | string  | Name of the earlier prepared scratch org pool that can be utilized by this pool, to prepare pools in multiple stages. |
| installAll                          | boolean | Install all package artifacts, in addition to the managed package dependencies                                        |
| releaseConfigFile                   | string  | Path to a release config file to create pools with selected packages. Use in conjunction with installAll              |
| enableSourceTracking                | boolean | Enable source tracking by deploying packages using source:push and persisting source tracking files                   |
| disableSourcePackageOverride        | boolean | Disable overriding unlocked packages as source packages, Rather install unlocked packages as unlocked                 |
| relaxAllIPRanges                    | boolean | Relax all IP addresses, allowing all global access to scratch orgs                                                    |
| ipRangesToBeRelaxed                 | array   | Range of IP addresses that can access the scratch orgs                                                                |
| retryOnFailure                      | boolean | Retry installation of a package on a failed deployment                                                                |
| maxRetryCount                       | number  | Maximum number of times a package should be retried while deploying to a scratchorg, The default is 2                 |
| preDependencyInstallationScriptPath | string  | Path to a script file that need to be executed before dependent packages are installed in a scratch org               |
| postDeploymentScriptPath            | string  | Path to a script file that need to be exectued after all the packages (dependencies+repository) is installed          |
| enableVlocity                       | boolean | Enable vlocity settings and config deployment. Please note it doesnt install vlocity managed package"                 |
| fetchArtifacts                      | object  | Fetch artifacts, to be deployed to scratch orgs, from an artifact registry                                            |
| fetchArtifacts.artifactFetchScript  | string  | Path to the shell script containing logic for fetching artifacts from a universal registry, if not using npm          |
| fetchArtifacts.npm                  | object  | Fetch artifacts from NPM registry                                                                                     |
| fetchArtifacts.npm.scope            | string  | Scope of the NPM package                                                                                              |



