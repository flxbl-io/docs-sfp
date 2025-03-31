# Destructive Changes

sfp handles destructive changes according to the type of package. Here is a rundown on how the behaviour is according to various package types and modes

### Unlocked Packages

Salesforce handles destructive changes in unlocked packages / org dependent unlocked packages as part of the package upgrade process.\
\
From the Salesforce documentation ([https://developer.salesforce.com/docs/atlas.en-us.sfdx\_dev.meta/sfdx\_dev/sfdx\_dev\_unlocked\_pkg\_install\_pkg\_upgrade.htm?q=delete+metadata](https://developer.salesforce.com/docs/atlas.en-us.sfdx_dev.meta/sfdx_dev/sfdx_dev_unlocked_pkg_install_pkg_upgrade.htm?q=delete+metadata))

_Metadata that was removed in the new package version is also removed from the target org as part of the upgrade. Removed metadata is metadata not included in the current package version install, but present in the previous package version installed in the target org. If metadata is removed before the upgrade occurs, the upgrade proceeds normally. Some examples where metadata is deprecated and not deleted are:_

* _User-entered data in custom objects and fields are deprecated and not deleted. Admins can export such data if necessary._
* _An object such as an Apex class is deprecated and not deleted if it’s referenced in a Lightning component that is part of the package._

sfp utilizes `mixed` mode while installing unlocked packages to the target org.  So any metadata that can be deleted is removed from the target org.  If the component is deprecated, it has to be manually removed. \
Components that are hard deleted upon a version upgrade is found [here](https://developer.salesforce.com/docs/atlas.en-us.sfdx_dev.meta/sfdx_dev/sfdx_dev_unlocked_hard_deleted_components.htm).

### Source Packages &#x20;

Source packages support destructive changes using folder structure to demarcate components that need to be deleted. One can make use of `pre-destructive and` \``post-destructive` folders to mark components that need to be deleted

<pre><code>// Consider a source package feature-management
// with path as src/feature-management

└── feature-management
    ├── main
    ├──── default
    ├────────  &#x3C;metadata-contents>
    ├── <a data-footnote-ref href="#user-content-fn-1">post-destructive</a>
<strong>    ├────────  &#x3C;metadata-contents>
</strong>    ├──<a data-footnote-ref href="#user-content-fn-2"> pre-destructive</a>
    ├────────  &#x3C;metadata-contents>
    └── test
</code></pre>

The package installation is a single deployment transaction with components that are part of pre/post deployed along with destructive operation as specified in the folder structure.  This would allow one to refactor the current code to facilitate refactoring for the destructive changes to succeed, as often deletion is only allowed if there are no existing components in the org that have a reference to the component that is being deleted

{% hint style="info" %}
Destructive Changes support for source package is currently available only in **sfp (pro)** version.&#x20;
{% endhint %}

***

{% hint style="info" %}
## Things to look out for

* Test  destructive changes in your review environment thoroughly before merging your changes
* You will need to understand the dependency implications while dealing with destructive changes,  especially the follow on effects of a deletion in other packages, It is recommended you do a compile all of all apex classes ([https://salesforce.stackexchange.com/a/149955](https://salesforce.stackexchange.com/a/149955) & [https://salesforce.stackexchange.com/a/391614](https://salesforce.stackexchange.com/a/391614)) to detect any errors on apex classes or triggers
* After the version of package is installed across all the target orgs, you would need to merge another change which would remove the post-destructive or pre-destructive folders.   You do not need to rush through this , as sfp ignores any warning  associated with missing components in the org
{% endhint %}

***

### Data Packages

Data packages utilize sfdmu under the hood, and one can utilize any of the below approaches to remove data records.

**Approach 1: Combined Upsert and Delete Operations**

One effective method involves configuring SFDMU to perform both upsert and delete operations in sequence for the same object. This approach ensures comprehensive data management—updating and inserting relevant records first, followed by removing outdated entries based on specific conditions.

**Upsert Operation**: Updates or inserts records based on a defined external ID, aligning the Salesforce org with new or updated data from a source file.

```json
{
  "name": "CustomObject__c",
  "operation": "Upsert",
  "externalId": "External_Id__c",
  "query": "SELECT Id, Name, IsActive__c FROM CustomObject__c WHERE SomeCondition = true"
}
```

**Delete Operation**: Deletes specific records that meet certain criteria, such as being marked as inactive, to ensure the org only contains relevant and active data.

```json
{
  "name": "CustomObject__c",
  "operation": "Delete",
  "query": "SELECT Id FROM CustomObject__c WHERE IsActive__c = false"
}
```

**Approach 2: Utilizing `deleteOldData`**

Another approach involves using the `deleteOldData` parameter. This parameter is particularly useful when needing to clean up old data that no longer matches the current dataset in the source before inserting or updating new records.

* **Delete Old Data**: Before performing data insertion or updates, SFDMU can be configured to remove all existing records that no longer match the new dataset criteria, thus simplifying the maintenance of data freshness and relevance in the target org

```
// Use of deleteOldData
{
  "name": "CustomObject__c",
  "operation": "Upsert",
  "externalId": "External_Id__c",
  "deleteOldData": true
}

```

[^1]: Metadata components  in this folder will be deleted from the org after the components in the package are deployed

[^2]: Metafata components in the folder will be deleted before the components in the package are deployed
