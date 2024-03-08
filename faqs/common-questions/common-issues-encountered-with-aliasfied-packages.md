# Common Issues encountered with aliasfied packages

### 1. Alias Name Matching

* **Issue**: Deployment fails due to mismatch in the alias names.
* **Solution**: Ensure the alias name used in Salesforce CLI matches the intended org's alias exactly, including case sensitivity. Alias names are case sensitive, so `MyAlias` and `myalias` would be considered different.

### 2. Adding Parent Folder Name to Release YML File

* **Issue**: Confusion about defining directories or packages in the release definition file.
* **Solution**: In the release definition (`*.yml`) file, specify packages rather than directories. In some cases, the "path" and "package" may coincide, such as `src-env-specific-alias-pre`.

### 3. Building and Adding Packages to Artifact Directory

* **Issue**: "Aliasified" packages are skipped during the build process.
* **Solution**: Even though the template might skip "aliasified" packages, it's recommended to include them in the build process for the latest setup. Ensure these packages are built and added to the artifact directory.

### 4. Setting AlwaysDeploy Flag

* **Issue**: Packages not deploying as expected.
* **Solution**: Check if the `alwaysDeploy` flag is set in your configuration. This flag ensures that certain packages are always deployed, regardless of other conditions.

### 5. Using Multiple Force Ignore Feature

* **Issue**: Need to exclude specific files or directories from deployment selectively.
* **Solution**: Utilize the multiple force ignore feature to manage what gets included or excluded in your deployment package more granely.

### 6. Updating Force Ignore Files

* **Issue**: Alias packages are unintentionally included in builds or deployments.
* **Solution**: Create an additional `.forceignore` file to exclude alias packages specifically for build and quick build processes. Note that regular force ignore files are not applied during release, allowing everything to deploy as expected.

\
