# Preparing pools

sfp's prepare command helps you to build a pool of pre-built scratch orgs which can include managed packages as well as packages in your repository. This process allows you to cut down time in re-creating a scratch org during validation process when a scratch org is used as just-in-time environment or when being used as a developer environment.\
\
As you integrate more automated processes into Salesforce, incorporating third-party managed packages into your repository's configuration metadata and code becomes necessary. This integration increases the setup time for CI scratch orgs or developer environments for various tasks, such as running data loading scripts or assigning permission sets.



```
------------------------------------------------------------------------------------------
sfp-pro  -- ❤️ by flxbl.io ❤️ -Version:50.1.0 -Release:NOV 25 -Build:399 -Docker:50.1.0
------------------------------------------------------------------------------------------
Command  : sfp pool scratch init --targetdevhubusername=devhub --poolconfig=config/scratch-ci-pool.json --loglevel=info
CodevHub : N/A
User     : N/A
------------------------------------------------------------------------------------------
command: prepare
Pool Name: flxbl-io-scratch-ci-test-to-delete-1
Requested Count of Orgs: 6
Scratch Orgs to be submitted to pool in case of failures: true
All packages in the repo to be installed: true
Enable Source Tracking: true
Fetch artifacts from pre-authenticated NPM registry: true
------------------------------------------------------------------------------------------
Validating Org Authentication Mechanism..
Fetching Artifacts
Fetching checkpoints for prepare if any.....
Computing Allocation..

Current Allocation of ScratchOrgs in the pool flxbl-io-scratch-ci-test-to-delete-1: 2
Remaining Active scratchOrgs in the org: 10
ScratchOrgs to be allocated: 4
Generate Scratch Orgs..
Waiting for all scratch org request to complete, Please wait
Requesting Scratch Org SO1..
```

### Steps undertaken by prepare command

The prepare command does the following sequence of activities:

1. **Calculate the number of scratch orgs to be allocated** (Based on your requested number of scratch orgs and your org limits, we calculate what is the number of scratch orgs to be allocated at this point in time)
2. **Fetch the artifacts from registry if "fetchArtifacts" is defined in config, otherwise build all artifacts**
3. **Create the scratch orgs in parallel (respecting the timeout requested in the config), and update Allocation\_status\_c of each these orgs to "In Progress"**
4. **On each scratch org, in parallel, do the following activities:**
   * Install SFPOWERSCRIPTS\_ARTIFACT\_PACKAGE (04t1P000000ka9mQAA) for keeping track of all the packages which will be installed in the org. You can set an environment variable SFPOWERSCRIPTS\_ARTIFACT\_PACKAGE to override the installation with your own package id (the source code is available [here](https://github.com/dxatscale/sfpowerscripts-artifact))
   * Install all the dependencies of your packages, such as managed packages that are marked as dependencies in your sfdx-project.json
   * Install all the artifacts that is either built/fetched
   * If `enableSourceTracking` is specified in the configuration, the command will create and deploy "sourceTrackingFiles" using sfpowerscripts artifacts to the scratch org. To store local source tracking files, we re-create it when fetching a scratch org from a pool, using the [@salesforce/source-tracking](https://github.com/forcedotcom/source-tracking) library. Checkout the commit from which each sfpowerscripts artifact was created, and update the local source tracking using the package directory. The files are retrieved to the local ".sf" directory, when using `sfpowerscripts:pool:fetch` to fetch a scratch org, and allows users to deploy their changes only, through source tracking. Refer to the [decision log](https://github.com/dxatscale/sfpowerscripts/blob/main/decision%20records/prepare/001-prepare-source-tracking.md) for more details.
5. **Mark each completed scratch org as "Available", depending on the pool config \`succeedOnDeploymentErrors\` is true, else scratch orgs are deleted**

{% hint style="warning" %}
**Ensure that your DevHub is authenticated using** [**SFDX Auth URL**](https://developer.salesforce.com/docs/atlas.en-us.sfdx_cli_reference.meta/sfdx_cli_reference/cli_reference_auth_sfdxurl.htm) **and the auth URL is stored in a secure place (Key Management System or Secure Storage).**
{% endhint %}
