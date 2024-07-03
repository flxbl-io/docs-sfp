# Field History & Feed  Tracking

<table><thead><tr><th width="229">Attribute</th><th>Type</th><th>Description</th><th>Package Types Applicable</th></tr></thead><tbody><tr><td>enableFHT</td><td>boolean</td><td>Enable  field history tracking for fields</td><td><p></p><ul><li>unlocked</li><li>org dependent unlocked</li></ul><p></p></td></tr><tr><td>enableFT</td><td>boolean</td><td>Enable Feed Tracking for fields</td><td><p></p><ul><li>unlocked</li></ul><ul><li>org dependent unlocked</li></ul></td></tr></tbody></table>

Salesforce has a strict limit on the number of fields that can be tracked. Of course, this limit can be increased by raising it to the Salesforce Account Executive (AE). However, it would be problematic if an unlocked package is deployed to orgs that do not have the limit increased. So don’t be surprised when you are working on a flxbl project and happen to find that the deployment of field history tracking from unlocked packages is disabled by Salesforce.

One workaround  is to keep a copy of all the fields that need to be tracked in a separate source package (field-history-tracking or similar) and deploy it as one of the last packages with the ‘alwaysDeploy’ option.

<figure><img src="https://miro.medium.com/v2/resize:fit:536/1*p43ndpkmL2HzW7qr4pHAuQ.png" alt="" height="246" width="268"><figcaption><p>Files to be maintained to enable field history tracking deployment before the Jan 23 Release</p></figcaption></figure>

However, this specific package has to be carefully aligned with the original source/unlocked packages, to which the fields originally belong. As the number of tracked fields increases in large projects, this package becomes larger and more difficult to maintain. In addition, since it’s often the case that the project does not own the metadata definition of fields from managed packages, it doesn’t make much sense to carry the metadata only for field history tracking purposes.

To resolve this, **sfp** features the ability to automate the deployment of field history tracking for both unlocked packaged and managed packages without having to maintain additional packages.

Two mechanisms are implemented to ensure that all the fields that need to be field history tracking enabled are properly deployed. Specifically, a YAML file that contains all the tracked fields is added to the package directory.

<figure><img src="https://miro.medium.com/v2/resize:fit:1002/1*mMeUFwmSJwiSvIRIz0DfkQ.png" alt="" height="244" width="501"><figcaption><p>Sample history-tracking.yml</p></figcaption></figure>

During deployment, the YAML file is examined and the fields to be set are stored in an internal representation inside the deployment artifact. Meanwhile, the components to be deployed are analyzed and the ones with ‘trackHistory’ on are added to the same artifact. This acts as a double assurance that any field that is missed in the YAML files due to human error will also be picked up. After the package installation, all the fields in the artifact are retrieved from the target org and, for those fields, the trackHistory is turned on before they are redeployed to the org.

<figure><img src="https://miro.medium.com/v2/resize:fit:692/1*ozMGWIZ9wNm7-IdlLfFPsA.png" alt="" height="51" width="346"><figcaption><p>Files to be maintained to enable field history tracking deployment after the Jan 23 Release</p></figcaption></figure>

In this way, the deployment of field history tracking is completely automated. One now has a declarative way of defining field history tracking (as well as feed tracking) without having to store the metadata in the repo. The only maintenance effort left for developers is to manage the YAML file.



```json
// Sample package 

{
  "packageDirectories": [
      {    
      "path": "src/core-crm",
      "package": "core-crm",
      "versionNumber": "2.0.10.NEXT",
      "enableFHT" : true,
      "enableFT" : true
    }

```
