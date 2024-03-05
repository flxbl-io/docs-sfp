# Setup Salesforce Org

To fully leverage the capabilities of sfp, a few addition steps need to be configured in your Salesforce orgs,  Please find the steps below

## 1. Enable Dev Hub

To enable modular package development, there are a few configurations in Salesforce that need to be turned on in order to create Scratch Orgs and Unlock Packages.

[Enable Dev Hub](https://developer.salesforce.com/docs/atlas.en-us.sfdx\_dev.meta/sfdx\_dev/sfdx\_setup\_enable\_devhub.htm) in your Salesforce org so you can create and manage scratch orgs and second-generation packages. Scratch orgs are disposable Salesforce orgs to support development and testing.

1. Navigate to the **Setup** menu
2. Go to **Development > Dev Hub**
3. Toggle the button to on for **Enable Dev Hub**
4. &#x20;**Enable Unlocked Packages and Second-Generation Managed Packages**&#x20;

<figure><img src="../.gitbook/assets/image (6).png" alt=""><figcaption><p>Enable Dev Hub</p></figcaption></figure>

## 2. Install sfpowerscripts-artifact Unlocked Package

The sfpowerscripts-artifact package is a lightweight unlocked package consisting of a custom setting **SfpowerscriptsArtifact2\_\_c** that is used to keep a record of the artifacts that have been installed in the org. This enables package installation, using sfp, to be skipped if the same artifact version already exists in the target org.

```bash
sf package install --package 04t1P000000ka9mQAA -o <your_org_alias> --security-type=AdminsOnly --wait=120

Waiting 120 minutes for package install to complete.... done
Successfully installed package [04t1P000000ka9mQAA]
```

Once the command completes, confirm the unlocked package has been installed.

1. Navigate to the **Setup** menu
2. Go to **Apps > Packaging > Installed Packages**
3. Confirm the package **sfpowerscripts-artifact** is listed in the "**Installed Packages**"

<figure><img src="../.gitbook/assets/image (7).png" alt=""><figcaption><p>sfpowerscripts-artifact </p></figcaption></figure>

Ensure that you intall sfpowerscripts artifact unlocked package in all your target orgs that you intend to deploy using sfp

