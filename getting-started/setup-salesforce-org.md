# Setup Salesforce Org

To keep setup to a minimum, the guide below will assume use of a Developer Edition Org only. Typically, the setup will be done in Production Orgs and Sandboxes/Scratch Orgs will be used but this guide is meant to introduce the basics to introduce the basics of sfp cli.

## A. Sign-Up and Create Developer Edition Org

1. Click on the following [link](https://developer.salesforce.com/signup) to sign up and create your own Salesforce Developer Edition. &#x20;

<figure><img src="../.gitbook/assets/image (4).png" alt=""><figcaption><p>Salesforce Developer Edition Sign-Up</p></figcaption></figure>

2. Click on the "Verify Account" from your email and login.

<figure><img src="../.gitbook/assets/image (21).png" alt=""><figcaption></figcaption></figure>

## B. Authenticate to Developer Edition Orgs via the SF CLI

1. Authorize your Developer Edition Org using the [web login flow](https://developer.salesforce.com/docs/atlas.en-us.sfdx\_cli\_reference.meta/sfdx\_cli\_reference/cli\_reference\_org\_commands\_unified.htm#cli\_reference\_org\_login\_web\_unified). The example below uses "**flxbl-demo**" as the alias for the org.

```bash
sf org web login -a flxbl-demo -r https://login.salesforce.com
```

2. Confirm your org is listed.

```bash
sf org list

Type   Alias             Username                     Org ID             Status    Expires
 ─ ────── ───────────────── ──────────────────────────── ────────────────── ───────── ───────
          flxbl-demo        admin@flxbldemo.dev          00Dau0000009aBcDEF Connected
```

3. Login to your org.

```
sf org open -o flxbl-demo 
```

<figure><img src="../.gitbook/assets/image (22).png" alt=""><figcaption><p>Sales Cloud Default Page</p></figcaption></figure>

## C. Enable Dev Hub

To enable modular package development, there are a few configurations in Salesforce that needs to be turned on to be able to create Scratch Orgs and Unlock Packages.

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



