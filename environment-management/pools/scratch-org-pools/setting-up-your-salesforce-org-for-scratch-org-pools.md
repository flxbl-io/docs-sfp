# Setting up your Salesforce Org for Scratch Org Pools

## &#x20;Install sfpowerscripts Scratch Org Pooling Unlocked Package in DevHub

The Scratch Org Pooling Unlocked Package adds additional custom fields, validation rules, and workflow to the standard object "**ScratchOrgInfo**" in the DevHub to enable associated scratch org pool commands to work for the pipeline.

```bash
sf package install -p 04t1P000000katQQAQ -o <Your_DevHub_Username> -r -a package -s AdminsOnly -w 30
```



## Generate SFDX auth URL for Pipeline Authentication

In order for pools command to work effectively, ensure that you have authenticated to DevHub using SFDX Auth URL instead of other authentication strategies where you are executing the pool operations

```bash
sf org display -o <orgAlias> --verbose --json > authFile.json
cat authFile.json
> {
  "status": 0,
  "result": {
    "id": "XXXXYYY",
    "accessToken": "00D8G0000009g7h!uhuRfGKbvPeubTZKztmFWgrykDuuVdxbffzjjVTqjMyRcV{wb+2JtxsevgKfGiGXRz02jY83uNBsD4CuWHwv.b21KZdFxbTi",
    "instanceUrl": "https://your.salesforce.com",
    "username": "vu.ha@dxatscale.io.dxatscale.shareddev",
    "clientId": "PlatformCLI",
    "connectedStatus": "Connected",
    "sfdxAuthUrl": "force://PlatformCLI::Cq$QLeQvDxpvUoNKgiDkoTqyVHdeoMupiZvkgHYcdVHsfMaDpqKJNbg#8ZtUpfBuIdVaUD0B21cFav5X2Pzv5X2@yoursalesforce.com",
    "alias": "SharedDev"
  }
}
```

{% hint style="info" %}
Save only the following part of the **sfdxAuthUrl** to secret storate and  use **sf org login sfdx-url**

`force://PlatformCLI::Cq$QLeQvDxpvUoNKgiDkoTqyVHdeoMupiZvkgHYcdVHsfMaDpqKJNbg#8ZtUpfBuIdVaUD0B21cFav5X2Pzv5X2@yoursalesforce.com`
{% endhint %}

## Public Group and Sharing Rules Creation for Developer Access to Scratch Org Pools

For developers (who are on limited access license) to access scratch orgs created by the CI service user, for their local development, a sharing setting needs to be created on the **ScratchOrgInfo** object. The sharing setting should grant read/write access to the ScratchOrgInfo records owned by a public group consisting of the CI service user and a public group consisting of the developer users.

1.  Create Public Groups **(Setup > Users > Public Groups)**

    * CI Users (Admin users/ CI users who creates scratch orgs in pool)

    <figure><img src="../../../.gitbook/assets/image (32).png" alt=""><figcaption></figcaption></figure>

    * Developers (developers who are allowed to fetch scratch orgs from pool)

    \


    <figure><img src="../../../.gitbook/assets/image (33).png" alt=""><figcaption></figcaption></figure>
2. Create Sharing Rule **"ScratchOrgInfo RW to Developers"** **(Setup > Security > Sharing Settings)**
   * Grant Read/Write access to the ScratchOrgInfos records owned by the CI Users to Developers&#x20;

<figure><img src="../../../.gitbook/assets/image (34).png" alt=""><figcaption></figcaption></figure>

1.  Assign Users to Public Groups **(Setup > Security > Sharing Settings)**

    * CI Users
    * Developers



<figure><img src="../../../.gitbook/assets/image (35).png" alt=""><figcaption></figcaption></figure>

## J. Permission Set Creation for Developer Access to ScratchOrgInfo Object

The developers must also have object-level and FLS permissions on the ScratchOrgInfo object. One way to achieve this is to assign a permission set that has Read, Create, Edit and Delete access on ScratchOrgInfos, as well as Read and Edit access to the custom fields used for scratch org pooling: `Allocation_status__c`, `Password__c`, `Pooltag__c` and `SfdxAuthUrl__c`

**Permission Set Name:** "Scratch Org Developer"

**Object:** Scratch Org Info

1. Object Permissions
   * Read, Create, Edit and Delete
2. Field Permissions
   *   Read, Edit for Custom Fields

       * `Allocation_status__c`
       * `Password__c`
       * `Pooltag__c`
       * `SfdxAuthUrl__c`

       <figure><img src="../../../.gitbook/assets/image (36).png" alt=""><figcaption></figcaption></figure>

**System Permissions:**

* API Enabled = True
* API Only User = False
* Create and Update Second-Generation Packages = True

<figure><img src="../../../.gitbook/assets/image (37).png" alt=""><figcaption></figcaption></figure>

## **K. Profile and Permission Set Assignment for Developers in Production**

To onboard new developers, the following Profiles and Permission Set will need to be assigned to the new Developer User Account in Salesforce.

**Profile** (Choose 1 Only)

1. Minimum Access - Salesforce
   * Licence Type - Salesforce
2. Limited Access User
   * Licence Type - Salesforce Limited Access - Free

**Permission Set**&#x20;

* Scratch Org Developer

**Public Groups**

* Developers - Add Users to "Developers" Group
