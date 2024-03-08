# Build & Install an Artifact

In the earlier section, we looked at how we configured the project directory for sfp,  Now lets walk through some core commands.

### A. Build an artifact for a package

The build command will generate a zipped artifact file for each package you have defined in the sfdx-project.json file.  The artifact file will contain metadata and the source code at the point build creation for you to use to install.

1. Open up a terminal within your Salesforce project directory and enter the following command:

```bash
sfp build -v <DevHubAlias/DevHubUsername>
```

You will see the logs with details of your package creation, for instance here is a sample output

<figure><img src="../.gitbook/assets/image (43).png" alt=""><figcaption><p>Build Outputs</p></figcaption></figure>

2.  A new "artifacts" folder will be generated within your source project containing a zipped artifact file for each package defined in your sfdx-project.json file.\
    \
    For example, the artifact files will contain the following naming convention with the "\_sfpowerscript\_artifact\_" in between the package name and version number.\
    \
    `package-name_sfpowerscripts_artifact_1.0.0-1.zip` \
    \


    <figure><img src="../.gitbook/assets/image (38).png" alt=""><figcaption><p>artifacts folder</p></figcaption></figure>
3. If you want to explore the contents of the artifact file, extract it and it will consist of the following files and directory.&#x20;

* `artifact_metadata.json`
* `changelog.json`
* `source Folder`

### B. Install the artifact to a target org

Ensure you have authenticated your salesforce cli to a org first.  If you haven't, please find the instructions [here](https://developer.salesforce.com/docs/atlas.en-us.sfdx\_dev.meta/sfdx\_dev/sfdx\_dev\_auth\_web\_flow.htm).

1. Once you have the authenticated to the sandbox, you could execute the installation command as below

```bash
sfp install -o <TargetOrgAlias/TargetOrgUsername>
```

<figure><img src="../.gitbook/assets/image (41).png" alt=""><figcaption><p>Install Outputs</p></figcaption></figure>

2. Navigate to your target org and confirm that the package is now installed with the expected changes from your artifact.  In this example above, a new custom field has been added to the Account Standard Object.

{% hint style="info" %}
Depending on the type of packages,[ sfp will issue the equivalent test classes](../concepts/supported-package-types/) with in the package directory and it could result in failures during installation,  Please fix the issues in your code and repeat till you get a sucessful installation.   If your packaged doesn't have sufficient test coverage, you may need to use the all tests in the org to get your package installed. Refer to the material [here](../building-artifacts/configuring-installation-behaviour-of-a-package/optimized-installation.md)
{% endhint %}
