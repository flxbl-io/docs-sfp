# Reconciling Profiles

<table><thead><tr><th width="229">Attribute</th><th>Type</th><th>Description</th><th>Package Types Applicable</th></tr></thead><tbody><tr><td>reconcileProfiles</td><td>boolean</td><td>Reconcile profiles to only apply permissions  to objects, fields and features that are present in the target org.</td><td><p></p><ul><li>source</li><li>diff</li></ul></td></tr></tbody></table>

In order to prevent deployment errors to target Salesforce Orgs, enabling **reconcileProfiles** to`true` ensures and extra steps are taken to ensure the profile metadata is cleansed of any missing attributes prior to deployment. &#x20;

During the reconcilement process, you can provide the follow flags to the command:

* Folder - path to the folder which contains the profiles to be reconciled, if project contain multiple package directories, please provide a comma separated list, if
* Profile List - list of profiles to be reconciled. If omitted, all the profiles components will be reconciled.
* Source Only - set this flag to reconcile profiles only against component available in the project only. Configure ignored permissions in sfdx-project.json file in the array
* Destination Folder - the destination folder for reconciled profiles, if omitted existing profiles will be reconciled and will be rewritten in the current location

For more details on the`sfp profile reconcile` command, [click here](../../cli-reference/utilities/profile.md).

```
// Sample package 

{
  "packageDirectories": [
      {    
      "path": "packages/access-mgmt",
      "package": "access-mgmt",
      "default": false,
      "versionName": "access-mgmt",
      "versionNumber": "1.0.0.0",
      "reconcileProfiles": "true"
    }

```



{% hint style="info" %}
For more a more detailed understanding of the ProfileReconcile library, visit the GitHub repository "[sfprofiles](https://github.com/flxbl-io/sfprofiles)" for details of on the [`source`](https://github.com/flxbl-io/sfprofiles/blob/main/src/impl/source/profileReconcile.ts).&#x20;
{% endhint %}

