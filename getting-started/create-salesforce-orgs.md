# Create Salesforce Orgs

To keep setup to a minimum, the guide below will assume use of Developer Edition Orgs only. Typically, the setup will be done in Production Orgs and Sandboxes/Scratch Orgs will be used but this guide is meant to introduce the basics to introduce the basics of sfp cli before deep dive later on.

* Create 2 developer Edition Orgs
* [https://developer.salesforce.com/signup](https://developer.salesforce.com/signup)

<figure><img src="../.gitbook/assets/image (4).png" alt=""><figcaption><p>Salesforce Developer Edition Sign-Up</p></figcaption></figure>







* One Serve as DEV
* One Serve as TEST
  * Dual function of your Dev Hub as well, enable this, and release environment, as well as initial deployment environment.



## A. Authenticate to Developer Edition Orgs via CLI

Authorize your production instance and/or Developer Edition Org using the [web login flow](https://developer.salesforce.com/docs/atlas.en-us.sfdx\_cli\_reference.meta/sfdx\_cli\_reference/cli\_reference\_auth\_web.htm). The example below uses "**DevHub**" as the alias for the instance where you will use it to create Unlock Packages and manage Scratch Orgs.

```
sf org web login -a dev -r https://login.salesforce.com

sf org web login -a test -r https://login.salesforce.com

confirm after with

sf org list
```

## B. Enable Dev Hub

To enable modular package development, there are a few configurations in Salesforce as a System Administrator that needs to be turned on to be able to create Scratch Orgs and Unlock Packages.

[Enable Dev Hub](https://developer.salesforce.com/docs/atlas.en-us.sfdx\_dev.meta/sfdx\_dev/sfdx\_setup\_enable\_devhub.htm) in your Salesforce org so you can create and manage scratch orgs and second-generation packages. Scratch orgs are disposable Salesforce orgs to support development and testing.

{% hint style="info" %}
Do the following on the TEST org
{% endhint %}

1. Navigate to the **Setup** menu
2. Go to **Development > Dev Hub**
3. Toggle the button to on for **Enable Dev Hub**

<figure><img src="../.gitbook/assets/image (6).png" alt=""><figcaption><p>Enable Dev Hub</p></figcaption></figure>

## C. Install sfpowerscripts-artifact Unlocked Package in DevHub and Lower Existing Sandboxes

The [sfpowerscripts-artifact package](https://github.com/dxatscale/sfpowerscripts-artifact) is a lightweight unlocked package consisting of a custom setting **SfpowerscriptsArtifact2\_\_c** that is used to keep a record of the artifacts that have been installed in the org. This enables package installation, using sfpowerscripts, to be skipped if the same artifact version already exists in the target org.

```
sf package install --package 04t1P000000ka9mQAA -o <OrgAlias> --security-type=AdminsOnly --wait=120
```

Once the command completes, confirm the unlocked package has been installed.

1. Navigate to the **Setup** menu
2. Go to **Apps > Packaging > Installed Packages**
3. Confirm the package **sfpowerscripts-artifact** is listed in the "**Installed Packages**"

<figure><img src="../.gitbook/assets/image (7).png" alt=""><figcaption><p>sfpowerscripts-artifact </p></figcaption></figure>

Repeat the steps above for the other "Dev" sandbox.

## Future Updates

The following items are more for best practices for enterprises that can be documented in the flxmod gitbook.&#x20;

* [ ] Enable Unlocked Packages and Second-Generation Managed Packages
* [ ] Create Service Account for Deployments
*



